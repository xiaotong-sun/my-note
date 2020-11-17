## Python 直接赋值、浅拷贝和深度拷贝解析



- **直接赋值：**其实就是对象的引用（别名）。
- **浅拷贝(copy)：**拷贝父对象，不会拷贝对象的内部的**子对象**。
- **深拷贝(deepcopy)：** copy 模块的 deepcopy 方法，**完全拷贝了父对象及其子对象**。



<u>**浅拷贝和深拷贝只当存在父子对象时要注意区分**</u>



### 1.浅拷贝

#### 实例

```python
a = {1: [1, 2, 3], 'name': 'hello'}
b = a.copy()
b[1].remove(2)			#修改值（原地址修改，不是替换 // 或者说对子对象修改值），原字典会改变
b['name'] = 'world'		#替换值，原字典不受影响
print(a)
print(b)
```

#### 结果

```
{1: [1, 3], 'name': 'hello'}
{1: [1, 3], 'name': 'world'}
```



### 2.深度拷贝

深度拷贝需要引入 copy 模块：

#### 实例

```
import copy
a = {1: [1, 2, 3], 'name': 'hello'}
b = copy.deepcopy(a)
b[1].remove(2)
b['name'] = 'world'
print(a)
print(b)
```

#### 结果

```python
{1: [1, 2, 3], 'name': 'hello'}	#a保持不变
{1: [1, 3], 'name': 'world'}	#只有b改变
```



### 3.解析

1、**b = a:** 赋值引用，a 和 b 都指向同一个对象。

<img src="python.photo/py_copy1.png" style="zoom:80%;" />

**2、b = a.copy():** 浅拷贝, a 和 b 是一个独立的对象，但他们的子对象还是指向统一对象（是引用）。

<img src="python.photo/py_copy2.png" style="zoom:80%;" />

**b = copy.deepcopy(a):** 深度拷贝, a 和 b 完全拷贝了父对象及其子对象，两者是完全独立的。

<img src="python.photo/py_copy3.png" style="zoom:80%;" />



