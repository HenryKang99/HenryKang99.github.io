## 1. Python3.x

> Python 是一个交互式的解释型语言，免费、开源，支持面向对象。

### 1.1 基础语法

#### 格式

- 默认以 utf8 编码。
- 单引号和双引号作用相同。
- 标识符以字母或下划线开头，可包含数字，大小写敏感。

- 注释 `#` 与 `"""` 或 `'''`

  ```python
  # 单行注释
  
  '''
  多行注释
  '''
  
  """
  多行注释
  """
  ```

- 缩进：控制代码块

  ```python
  # 空格数可以改变但统一代码块中必须保持一致
  if True:
      print ("True")
  else:
      print ("False")
    print('error') # 这句会报错
  ```

- 多行语句

  - 每行结束不需要写分号，但一行有多条语句时，需要分号；
  - 使用反斜杠 `\` 可以将一行语句拆分为多行，被括号包裹的代码拆分不需要反斜杠。

- 导入
  - `import xxx`
  - `from xxx import xxx`

#### 基本数据类型

> Number、String、Tuple 均为不可变类型，即不能企图通过索引来更改赋值。

1. Number 
   - 包括：int flot bool complex
   - / 浮点除  // 向下取整除  % 取余   \** 乘方

2. String
   - 索引方法：`str[头下标:尾下标:步长]`

3. Tuple
   - 使用`()`声明：tuple = ('a', 'b', 'c', 'd')

4. List
   - 使用`[]`声明： list = ['a', 'b', 'c', 'd']；
   - 元素可变，且类型可以不同。

5. Set
   - 使用`{}`声明，构造函数set()，自动去重；
   - 集合运算：交集 `&`，并集 `|`， 差集 `-`，异或 `^` 。

6. Dictionary
   - 键值对，也使用`{}`声明，构造函数dict(k=v, k=v)，key 必须唯一；
   - `{}` 表示一个空 Dictionary，而不是空 Set，空 Set 用 set() 表示；
   - xxx.`keys()` 获得所有 key，`values()`获得所有 value。

#### 运算符

- 逻辑运算符：`and`、`or`、`not`；
- 位运算符：`&`、`|`、`~`、`^` ；
- 成员运算符：`in`、`not in`，判断一个序列中是否含有指定元素；
- 身份运算符：`is`、`is not`，判断两个引用是否指向同一个对象；
  - 等价于 id(x) == id(y)，`id(x)`用于获取 x 的内存地址；
  - `==` 用于判断值是否相等。

### 1.2 I/O



### 1.3 链接MySQL



## 2. 爬虫

### 2.1 Requests 库



### 2.2 Beautiful Soup 库



### 2.3 RE 库

```python
import requests

def getHTMLText(url):
    try:
        kv = {'user-agent':'Mozilla/5.0'}
        r = requests.get(url, timeout=30, headers=kv)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return "产生异常！！！"
```



```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(demo, "html.parser")
print(soup.prettify())
```



