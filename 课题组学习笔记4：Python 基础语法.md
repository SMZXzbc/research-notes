# 课题组学习笔记4：Python 基础语法

下面总结一下在课题组代码里用到的 Python 基本操作。

主要是学会基本语法怎么写，怎么用在课题组需要的 prior 生成、检查、编码里。

因为有些和c++很类似，这些部分就不再写出。

重点是整理和c++不一样的地方。

---

## 一.变量

##### 例子

```python
case_id = "DU_001"
frequency_GHz = 28
condition = "LOS"
```

意思是：

```text
case_id 保存样本编号
frequency_GHz 保存频率
condition 保存传播条件
```

---

## 二.字典 dict

字典适合保存：

```text
名称 → 数值
```

这种对应关系。

##### 例子

```python
mechanism_weights = {
    "los": 0.03,
    "reflection": 0.62,
    "diffraction": 0.10,
    "scattering": 0.20,
    "penetration": 0.05
}
```

这里表示五种传播机制的重要程度。

---

#### 从字典中取值

###### 例子：

```python
mechanism_weights["reflection"]
```

意思是：

取出 reflection 对应的值。

结果是：

```text
0.62
```

---

## 三.列表 list

列表就是一组数据。

###### 例子

```python
DOMINANT_MECHANISM_ORDER = [
    "LOS",
    "reflection",
    "diffraction",
    "scattering",
    "penetration",
    "mixed"
]
```

在课题里，列表常用于：

```text
保存机制顺序
保存字段名
保存多条 prior
```

---

## 四.if 判断

##### 例子

```python
if passed:
    print("权重检查通过")
else:
    print("权重检查失败")
```

意思是：如果 passed 是 True，输出通过。

否则输出失败。



---

## 四.函数 def

##### 例子

```python
def check_weight_sum(self):
    w = self.mechanism_weights

    total = (
        w.los
        + w.reflection
        + w.diffraction
        + w.scattering
        + w.penetration
    )

    return abs(total - 1.0) < 0.05, total
```

这个函数的作用是：

```text
检查五个传播机制权重加起来是否接近 1。
```

---

## 五.return

##### 例子

```python
return abs(total - 1.0) < 0.05, total
```

意思是：

函数返回两个值：

```text
是否通过
权重总和
```

例如：

```text
True, 1.0
```

---

## 六.for 循环

###### 例子

```python
for item in priors:
    prior = RadioMapPriorV2(**item)
```

意思是：

对 priors 里的每一条 prior，依次进行处理。

##### 为什么重要

因为实际项目里不会只检查一条数据，

而是要批量检查很多条 prior。

---

## 七.import

###### 例子

```python
import json
import sys
from pathlib import Path
```

作用：

导入 Python 工具包。

在当前项目里：

```text
json 用来读取 JSON 文件
sys 用来修改 Python 搜索路径
Path 用来处理文件路径
```

---

## 八.with open 读取文件

##### 例子

```python
with open(data_path, "r", encoding="utf-8") as f:
    priors = json.load(f)
```

作用：

打开 JSON 文件，并把里面的数据读到 Python 里。

##### encoding="utf-8"：

因为文件里可能有中文。加上：

```python
encoding="utf-8"
```

可以避免中文乱码。

---

## 九.try / except 异常处理

###### 例子

```python
try:
    prior = RadioMapPriorV2(**item)
    print("schema 检查通过")
except Exception as e:
    print("schema 检查失败")
    print(e)
```

作用：

如果某条 prior 有问题，程序不会直接崩溃，

而是输出错误原因。

---

## 十.json.load 和 json.dump

##### 读取 JSON

```python
raw_priors = json.load(f)
```

作用：

把 JSON 文件读进 Python。

##### 写入 JSON

```python
json.dump(encoded_results, f, indent=2, ensure_ascii=False)
```

作用：

把 Python 数据保存成 JSON 文件。

其中：

```text
indent=2：让 JSON 文件更好看
ensure_ascii=False：保留中文，不转义成乱码
```

---

# 总结：

之前学c++无论是算法还是数据结构都基本上还是停留在解题上面，

而现在学python真的在起了一个工具的作用，

是为了完成这个流程：

```text
读取 GPT 输出的 JSON
↓
用 Pydantic 检查
↓
检查权重
↓
批量处理
↓
把 prior 编码成 vector
↓
保存结果
```


