# Python学习入门2

---

## 控制流

### 比较运算符

- 比较数字

```python
10 > 3
10 >= 3
10 < 20
10 <= 20
print(10 == 10)     #True
print(10 != "10")   #True
print(10 == "10")   #False
```

- 比较字符串

```python
print("bag" > "apple")     #True
print("bag" == "BAG")      #False
```

1. 比较字符串时，先比第一个字符的ASCII值，若不同得出结果

2. 若相同，则再比第二个字符，以此类推（码点中：空格 < ASCII < 所有汉字）

3. 若一个字符串是另一个的前缀，那么短串更小

> 可以利用`ord("a")函数查看对应ASCII值`



### 条件语句

```python
temperature = input("Today's temperature: ")
if int(temperature) > 30:        #不要忘记冒号！
    print("It's a hot day.")
elif int(temperature) > 20:
    print("It's a nice day.")
else:
    print("It's a cold day.")
print("Done")
```

- 先if再elif再else
- 条件先严格再宽松，一层层筛过去

> if语句简洁写法（用变量描述）

```python
age = 12
message = "Eligible" if age >= 18 else "Not eligible"
print(message)
```

注：如果只有if/else那可以三元表达式，如果又多了elif就不能简化为三元

如果没有else是什么逻辑？——

```python
n = start
while n <= end:
    # 判断偶数：能被 2 整除即可
    if n % 2 == 0:
        count += 1
        print(n)
    # 步长设为 1.0，可根据需要改成更小步长
    n += 1.0
```

1. 判断`n % 2 == 0`
   - 若成立 → count加1，打印print
   - 若不成立 → 直接跳过
2. 无论上一步是否跳过，都会执行 n += 1



### 逻辑运算符

```python
high_income = False
good_credit = True
student = False

if (high_income or good_credit) and not student:
    print("Eligible")
else:
    print("Not eligible")
```

善用3个逻辑运算符 `and / or / not`

- `and`→ 全对
- `or`→ 至少一个对

```python
age = 22
if age >= 18 and age <65:
    print("Eligible")

if 18 <= age < 65:
    print("Eligible")
```

> 连接起两个比较运算符，更加简洁



### for循环

重复一个任务并重复几次时，使用for循环

```python
for number in range(1, 4):
    print("Attempt", number, number * ".")   #, 后面加空格，加减乘除左右各空格
```

- range为整数序列，左闭右开，步长也得是整数（3个参数都为整数，不能带小数点哪怕0.0都不可以）
- number为`range(1, 4)`依次产生的每一个整数
- `range(start, stop, step)` 中三个参数取任意整数，可以为负数
- start和stop可以相同，但此时range输出空序列（什么都没有）
- start > stop且step>0时输出空序列，step＜0时输出正常递减序列

```python
successful = False
for i in range(3):
    print("Attempt")
    if successful:
        print("successful")
        break
else:
    print("Attempted 3 times and failed")
```

`for-else`循环——当for循环完了后开始执行else

> 嵌套循环

```python
for x in range(5):
    for y in range(3):
        print(f"({x}, {y})")
```

从上到下执行，先轮x，x每个对应的y也轮一遍（即迭代）

> for的迭代性

```python
print(type(5))                #int
print(type(range(5)))         #range
```

`range`函数返回的也是`range`类型

```python
for i in range(5):
    print(i)
#i会迭代每一个range里的数

for i in range(5):
    print("i")
#会打印字符串i，会重复5次

for i in "Python":
    print(i)
#i迭代python里的每一个字符

for x in [1, 2, 3, 4]:
    print(x)
```

for也可以计算出范围内的偶数

```python
count = 0
for number in range(1, 10):
    if number % 2 == 0:
        count += 1
        print(number)
print(f"We have {count} even numbers")
```



---

### while循环

只要条件为真，就一直重复——不是遍历迭代，而是评估

```python
number = 100
while number > 0:
    print(number)
    number //= 2
```

```python
command = ""                        #初始化定义让command可以通过while条件判断
while command.lower() != "quit":
    command = input(">")            #存在漏洞，这一次能否输入取决于上一次是否成功
    print("ECHO", command)

command = input("> ")
while command.lower() != "quit":
    print("ECHO", command)
    command = input("> ")     #这一行保证下一轮继续开始
print("BYE")                  #循环结束时才会执行这个

##第二种写法
while True:                   #创建了无限循环一遍又一遍执行，直到被打断
    command = input("> ")
    if command.lower() == "quit":     #一个等号为赋值，两个等号才是比较
        break
    print("ECHO", command)
print("BYE")                  #当输入quit时break跳出整个while true循环，继续往下走
```

`while`语句就像是戈娅大招的漩涡，成立就一直在这几行来回转，直到条件不成立或者break



## 函数

### 基本运用

用小写字母命名，下划线分隔单词。def前后都要隔两行再写

```python
def greet():
    print("Hi there")
    print("Welcome aboard")

 
greet()
```

以上为简单的函数调用，如果我们加上了外界的输入呢？

```python
def greet(first_name, last_name):
    print(f"Hi {first_name} {last_name}")
    print("Welcome aboard")
    
   
greet("Mosh", "Smith")
```

当定义函数时指定参数，这些参数就变成了这个函数内部的**局部变量**。调用引入外界值时，Mosh和Smith被对应赋值给变量first_name和last_name（赋完值运行后就不再占内存）

——局部变量只存在于这个函数内部，别的地方都不存在。而全局变量存在于文件任何地方

为了提高代码可读性和可维护性可重用性，提倡多使用局部变量，少使用全局变量

> 如果参数不对应时python就会报错

我们还可以让用户输入决定我们的实参

```python
def greet(first_name, last_name):
    print(f"Hi {first_name} {last_name}")
    print("Welcome aborad")
    
    
first_name = input("请输入你的名")
last_name = input("请输入你的姓")

greet(first_name, last_name)     #调用函数
```

函数分为两类

- 执行一个任务
- 返回一个值（更好的重复利用函数）——默认返回None

```python
def greet(name):
    print(f"Hi {name}")
    
    
greet("Mosh")           #Hi Mosh
print(greet("Mosh"))    #None (打印返回值)
```

return返回一个值后储存在一个变量里，再对这个变量处理

——以下为一些常用化简方法

```python
def increment(number, by):
    return number + by

#最直接写法
result = increment(2, 1)
print(result)

#第二种写法——打印返回值
print(increment(2, 1))

#也可以用参数增强可读性
print(increment(number=2, by=1))

#设置默认参数值
def increment(number, by=1):
    return number + by


print(increment(2))
#默认值可以被修改
print(increment(2, 5))
```

def参数也可以为多个即将可变数量的参数传递给函数

```python
def multiply(*numbers):
    print(numbers)
    
    
multiply(2, 3, 4, 5)     #(2, 3, 4, 5)
```

输出的为**元组**，元组可以迭代，也可以访问某一项如`number[0]`等

```python
def multiply(*numbers):
    total = 1
    for number in numbers:
        total *= number
    return total


print(multiply(2. 3, 4, 5))
```

位置参数：仅传递值，函数根据位置与参数名匹配（元组必须用位置）

关键字参数：传递`参数名=值`，指明将哪个值赋给哪个参数（字典必须用关键字）

> 只有位置参数列举完了后才能给关键字参数

```python
def user(**user):
    print(user)
    
    
user(id=1, name="John", age=22)
#输出 {'id':1, 'name':'John', 'age':22}
```

必须要用键如`user["name"]` 来访问特定的项，不可以用[0]这样的访问

### Debug

在运行和调试页面选择文件

按F9或者左键在行数左边增加断点

- 断点：断点为在这一行暂停，**这一行不会被执行！**断点可以编辑为某条件为真时才断
- 按F5开始调试，按shift+F5结束调试。调试只能往下走不能重新往上走
- 按F10执行单步跳过，执行当前行代码，停在下一行。如果当前行为一个函数调用，那么会执行完整个函数
- 按F11执行单步进入，执行当前代码，如果当前行不是函数调用就到下一行。如果是函数调用就会进入该函数内部并停在第一行
- 按shift+F11执行单步跳出，如果处在某个函数内部，这命令会执行完当前函数剩余部分，返回到调用该函数的地方（即返回原来按F11的地方）

F10为不关心内部函数是怎么跑的只需要结果，F11为需要知道内部函数是否出了问题，shift+F11为觉得不需要再检查内部函数了直接回去

按F11回到函数内部后再往下走会回到调用函数的地方，再按F10/F11会继续往下走不会回到函数了

在左侧可以查看当前变量及其值，也可以监视特定的变量和表达式，在编辑器中把鼠标放在变量上就会显示变量当前值

> 核心：设置断点到自己想看的地方，按F5/10/11向前跑，观察左侧的变量和监视表达式来查看哪里出错了



### 快捷操作

Home：回到这一行的开头

End：回到这一行的末尾

Ctrl+Home：回到文件的开头

Ctrl+End：回到文件的末尾

alt+↑/↓：移动某行

shift+alt+↓：复制某行

ctrl+/：将选中的变成注释/将注释变为内容

快捷补充：任意输出函数的某几个字符，再按↑↓选择，最后enter

F2：点击变量，再按F2，即可把所有的这个名字的变量全部改名
