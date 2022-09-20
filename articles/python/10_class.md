# 类

## 类的基本定义

> self(或者定义为其他名字，一般用self) 必须要写，python显示传递self，必须要有一个参数承接self

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def run(self):
        print(f'{self.name} run')

    def howOld(self):
        print(f'I\'m {self.age} years old')
       
person = Person('lisi', 18)
print(person.name)
print(person.age)
person.run()
person.howOld()
```

## 继承

```python
# 类的继承
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(f'Animal {self.name} eat')

# 继承
class Dog(Animal):
    def __init__(self, leg):
        super().__init__('旺财')
        self.leg = leg
    # 重写    
    def eat(self):
        print(f'Dog {self.name} eat, I have {self.leg} legs')

dog = Dog(4)
dog.eat()
dog.run()
```

