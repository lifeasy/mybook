# 单元测试

## 测试单个方法

```python
# tool.py
def mySum(a: int, b: int):
    return a + b + b 

# unittest.py
import unittest
from tool import mySum

# 测试单个方法
# 编写测试用例，需要继承unittest.TestCase
class SumTestCase(unittest.TestCase):
  
  # test_开头的方法，会被自动执行
  def test_mySum_function(self):
    num = mySum(3, 5)
    self.assertEqual(num, 13)
    
# 如果py文件作为入口文件执行，__name__等于'__main__'
if __name__ == '__main__':
  # 执行 unittest.main()，会执行所有test_开头的方法
  unittest.main()
```

## 测试类

```python
# tool.py
class Helper:
    def __init__(self, name) -> None:
        self.name = name

    def roll(self, num):
        return self.name + str(num)

    def run(self, speed):
        return self.name + 'speed is' + str(speed)
      
# unittest.py
import unittest
from tool import Helper

# 测试类
class HeplerTestCase(unittest.TestCase):
    # 可以在setUp中做一些全局的初始化动作，只执行一次
    def setUp(self):
        na = 'lily'
        self.helper = Helper(na)

    def test_roll(self):
        n = 3
        ret = self.helper.roll(n)
        self.assertEqual(ret, self.helper.name + str(n))
    
    def test_run(self):
        speed = 14
        ret = self.helper.run(speed)
        self.assertEqual(ret, self.helper.name + 'speed is' + str(speed))

if __name__ == '__main__':
    unittest.main()
```



## 