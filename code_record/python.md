1. 类属性与方法

   ```python
   __private_attrs: # 两个下划线开头表示类的私有属性
   __private_method: # 类的私有方法
   # 以下为类的专有方法：
   __init__: # 构造函数，在生成对象时调用
   __del__: # 析构函数，释放对象时调用
   __repr__: # 打印，转换，命令行输入a,直接输出它的返回值
   __str__: # 命令行使用print(a) 输出的值
   __setitem__: # 按照索引赋值
   __getitem__: # 按照索引获取值
   __len__: # 获得长度
   __cmp__: # 比较运算
   __call__: # 函数调用
   __add__: # 加运算
   __sub__: # 减运算
   __mul__：# 乘运算
   __truediv__: # 除运算
   __mod__: # 求余运算
   __pow__：# 乘方
   ```

2. 命名空间/作用域

   三种命名空间：`内置名称` 、`全局名称` 、`局部名称` 

   四种作用域：

   `L` : 最内层，包含局部变量，比如一个函数/方法内部

   `E` : 包含了非局部也非全局的变量，比如函数中嵌套一个函数

   `G` : 当前脚本的最外层，比如当前模块的全局变量

   `B` : 内建关键字(dir(builtins)) == 154

   在Python中只有模块，类和函数才能引入新的作用域，其他代码块如 `if else`  `try except` 等

3. 错误和异常: 错误一般是指：语法错误；异常一般指运行期间的错误

   `assert exp`  当exp为False/0/[]/None时，报`AssertionError`  错误

   一个 `try` 可以接多个 `except`  但最多执行1个 `except` 语句

   `else` 必须放在所有的 `except` 之后，当没有异常时会被执行，这样设计的好处是可以避免一些意想不到而 `except` 又无法捕捉到的异常。

   `finally` 无论是否有异常都会执行的语句

   `raise Exception(message)` 主动抛出异常

4. from module import * ,不会将module里的_varName 引入到命名空间里

5. python数据结构

   `列表list`

   ```python
   a = [1,2]
   a[len(a):] = [2,3] # a==[1,2,2,3]
   ```

   ![image-20201106201635746](E:\md\resources\list方法.png)

   ```python
   # 将列表当做栈
   stack = [1,2,3]
   stack.append(4) #向栈顶追加元素
   stack.pop() #从栈顶取出元素
   # 将列表当做队列，由于涉及到数组元素的移动，所有效率不是很高
   # collections中的deque在list的基础上实现的
   from collections import deque
   queue = deque(['a','b','c'])
   queue.append('d')
   queue.append('e')
   queue.popleft() # 'a'
   queue #deque(['b','c','d','e'])
   # 列表推导式
   vec = [1,2,3,4]
   [i for i in vec if i>3] #[4]
   vec1 = [1,2,3]
   vec2 = [4,5,6]
   # 双重循环
   [x*y for x in vec1 for y in vec2] #[4,5,6,8,10,12,12,15,18]
   # del 删除列表中的切片或索引指定的值
   del a[0]
   del a[1:2]
   ```

   `元组和序列` 

   ```python
   t = 1,2,3
   t # (1,2,3)
   ```

   `集合` ：是一个无序不重复元素的集。基本功能包括关系测试和消除重复元素。

   ```python
   #创建一个集合
   basket = {'apple','orange','pear','banana'}
   basket # {'apple','orange','pear','banana'}
   'apple' in basket # True
   #创建一个空集合
   basket = set() # {}或set() 不可以使用basket = {}
   #集合之间的计算
   a = set('abracadabra')
   b = set('alacazam')
   a # {'a', 'r', 'b', 'c', 'd'}
   a-b # 在a中的字母但不在b中
   a | b # 在a中或在b中
   a & b # 在a中和b中都有
   a ^ b # 在a或b中，但不同时在a和b中
   ```

   `字典` 

   ```python
   #定义字典
   dictionary_0 = {'key':value}
   dictionary_1 = dict([('k1':v1),('k2':v2),('k3':v3)])
   dictionary_2 = dict(k1=v1,k2=v2,k3=v3)
   ```

   `遍历技巧`

   ```python
   #字典遍历
   for k,v in dictionary.items()
   #数组遍历
   for i,v in enumerate([2,4,6])
   #同时遍历两个数组
   questions = [1,2,3]
   answers = ['a','b','c']
   for q,a in zip(questions,answers)
   ```

   


