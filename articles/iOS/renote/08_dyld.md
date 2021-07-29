# dyld&Load&initialize&objc_messageSend

## 自己引入的动态库的加载流程（Load方法执行）

* 在没有依赖关系的情况下，动态库的加载顺序由`Link Binary With Libraries`中的拖入顺序决定，可以通过`Link Binary With Libraries`来控制动态库的加载顺序，先拖入先执行
* 在有依赖的关系下，被依赖的子节点优先加载。

**总结动态库的加载顺序为：先根据拖入配置的链接顺序加载，如有依赖的先递归式加载依赖。**

**注入的代码在主工程之前，但是在自己的动态库之后执行**

## 单个Image文件中的Load加载顺序

* **有继承关系的类的`+load`方法的执行顺序，是从基类到子类的；没有继承关系的两个类的`+load`方法的执行顺序是与编译顺序有关的（Build Phases -> Compile Sources中的顺序）**。

* 所有分类的`+load`方法都要在所有类的`+load`方法之后执行
* **所有分类的`+load`方法的执行顺序与编译顺序有关，与是谁的分类无关，也与一个类有几个分类无关**

具体流程：

从`load_images`中的`call_load_methods`开始：

1. 循环调用`call_class_loads`方法，直到没有可执行的`+load`方法
2. 调用`call_category_loads`方法
3. 重复1->2，直到所有的类和分类的`+load`方法都执行完毕

## Load方法总结

1. +load方法是在dyld阶段的执行初始化方法步骤中执行的，其调用为load_images -> call_load_methods
2. 一个类在代码中不主动调用+load方法的情况下，其类、子类实现的+load方法都会分别执行一次
3. 父类的+load方法执行在前，子类的+load方法在后
4. 在同一镜像中，所有类的+load方法执行在前，所有分类的+load方法执行在后
5. 同一镜像中，没有关系的两个类的执行顺序与编译顺序有关（Compile ources中的顺序）
6. 同一镜像中所有的分类的+load方法的执行顺序与编译顺序有关（Compile Sources中的顺序），与是谁的分类，同一个类有几个分类无关
7. 同一镜像中主工程的+load方法执行在前，静态库的+load方法执行在后。有多个静态库时，静态库之间的执行顺序与编译顺序有关（Link Binary With Libraries中的顺序）
8. 不同镜像中，动态库的+load方法执行在前，主工程的+load执行在后，多个动态库的+load方法的执行顺序编译顺序有关（Link Binary With Libraries中的顺序）。

## initialize

initialize在类或者其子类的第一个方法被调用前调用。即使类文件被引用进项目,但是没有使用,initialize不会被调用。由于是系统自动调用，也不需要再调用 [super initialize] ，否则父类的initialize会被多次执行。假如这个类放到代码中，而这段代码并没有被执行，这个函数是不会被执行的。

1. 父类的initialize方法会比子类先执行
2. 当子类未实现initialize方法时,会调用父类initialize方法,子类实现initialize方法时,会覆盖父类initialize方法.
3. 当有多个Category都实现了initialize方法,会覆盖类中的方法,只执行一个(会执行Compile Sources 列表中最后一个Category 的initialize方法)

## OC消息发送

整个方法查找的过程，可以简单的概括为以下几个步骤

1. 实现、初始化对应的类
2. 根据是否支持垃圾回收机制(GC)判断是否忽略当前的方法调用
3. 从cache中查找方法
4. cache中没有找到对应的方法，则到方法列表中查，查到则缓存
5. 如果本类中查询到没有结果，则遍历所有父类重复上面的查找过程
6. 最后都没有找到的方法的话，则执行 `_class_resolveMethod` 让调用者动态添加方法，并重复一轮查询方法的过程
7. 若第六步没有完成动态添加方法，则把 _objc_msgForward_impcache 作为对应 SEL 的方法进行缓存，然后调用 _objc_msgForward_impcache 方法

