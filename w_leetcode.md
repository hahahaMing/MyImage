[TOC]


# 预备知识

##  一、算法知识

### 1.1 算法的时间复杂度和空间复杂度详解

刷第一道题，看懂了算法却比较不出好坏，故学习。

[算法的时间复杂度和空间复杂度详解](https://blog.csdn.net/yjclsx/article/details/86612659)

读这篇文章，简练一下：

**1 时间复杂度：**

1. 找出算法中的基本语句；

　　算法中执行次数最多的那条语句就是基本语句，**通常是最内层循环的循环体。**

2. 计算基本语句的执行次数的数量级；
3. 用大Ο记号表示算法的时间性能。 
4. 程序复杂时的计算法则：
   1. 输入、输出、赋值  O(1) 
   2. 顺序结构 求和
   3. 选择结构 主要是选择后的时间，检验条件时间 O(1) 
   4. 循环结构 乘法法则

 **2 空间复杂度：**

1. 存储算法本身所占用的存储空间
2. 算法的输入输出数据所占用的存储空间
3. 算法在运行过程中临时占用的存储空间

### 1.2 广度优先搜索

问题出在 [695. 岛屿的最大面积](## [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/))

这个帖子牛逼，可以做成更直观的看，[图的广度优先搜索（BFS）和深度优先搜索（DFS）算法解析](https://blog.csdn.net/weixin_40953222/article/details/80544928)

 ![DFS 与 BFS](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/725e473003c35e3be67ac6177cc6744fa04b0466795b5e69c7d673f626206b86-file_1583293748397.jpg)

如何写（最短路径的） BFS 代码

我们都知道 BFS 需要使用**队列**(进出在两个地方），代码框架是这样子的（伪代码）：

```python
while queue 非空:
	node = queue.pop()
    for node 的所有相邻结点 m:
        if m 未访问过:
            queue.push(m)
```

### 1.3 快排

先取一个数，让其左边都比他小，右边都比他大，然后把左右各当成数组重复上述过程递归。

 ![快排图](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/b7003af33a87e950707fdf2110385343fbf2b416-1584683286835.gif)

```python
def quick_sort(data):    
    """快速排序"""    
    if len(data) >= 2:  # 递归入口及出口        
        mid = data[len(data)//2]  # 选取基准值，也可以选取第一个或最后一个元素        
        left, right = [], []  # 定义基准值左右两侧的列表        
        data.remove(mid)  # 从原始数组中移除基准值        
        for num in data:            
            if num >= mid:                
                right.append(num)            
            else:                
                left.append(num)        
        return quick_sort(left) + [mid] + quick_sort(right)    
    else:        
        return data
 
# 示例：
array = [2,3,5,7,1,4,6,15,5,2,7,9,10,15,9,17,12]
print(quick_sort(array))
# 输出为[1, 2, 2, 3, 4, 5, 5, 6, 7, 7, 9, 9, 10, 12, 15, 15, 17]
```

### 1.4 插入排序

 ![img](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/d0c8a786c9177f3eee69fa7e70cf3bc79f3d5667.gif)




## 二、一些语法--python

### 2.1 代码阅读

> 一般只存在于代码的阅读中，暂时不常用，稍作记录，后面如果觉得可以，就把他应用后面两项。

#### 2.1.1 [python3——“->”的含义](https://www.cnblogs.com/gaoquanquan/p/9455952.html) 		

`->:`标记返回函数注释,信息作为`.__annotations__`属性提供,`__annotations__`属性是字典。键`return`是用于在箭头后检索值的键。但是在Python中`3.5`，[PEP 484 - Type Hints](https://www.python.org/dev/peps/pep-0484/)附加了一个含义：`->`用于指示函数返回的类型。它似乎也将在未来版本中强制执行。

例子:

　　输入：

```python
def test() -> [1, 2, 3, 4, 5]:
    pass


print(test.__annotations__)
```

　　输出：

　　　　{'return': [1, 2, 3, 4, 5]}

#### 2.1.2 `str[-1:1]`为啥是空字符

可能是因为不知道啥意思吧：

一些尝试：

```python
>>> h = 'sdfsf'
>>> h[-1:1]
''
>>> h[-1:]
'f'
>>> h[-1:2]
''
>>> h[-1:0]
''
>>> h[-1:n]
Traceback (most recent call last):
  File "<pyshell#15>", line 1, in <module>
    h[-1:n]
NameError: name 'n' is not defined
>>> h[-1:-2]
''
>>> h[-2:1]
''
>>> h[-3:]
'fsf'
>>> h[-2:]
'sf'
>>> h[-2:0]
''
```

#### 2.1.3 820题代码阅读

```python
class Solution:
    def minimumLengthEncoding(self, words: List[str]) -> int:
        good = set(words)# set建立无重复序列
        for word in words:
            for k in range(1, len(word)):
                good.discard(word[k:])# disgard删除（）里的元素

        return sum(len(word) + 1 for word in good)
```

```python
class Solution:
    def minimumLengthEncoding(self, words: List[str]) -> int:
        words = list(set(words)) #remove duplicates
        #Trie is a nested dictionary with nodes created
        # when fetched entries are missing
        Trie = lambda: collections.defaultdict(Trie)#初始化字典，对没见过的key自动添加默认值
        trie = Trie()

        #reduce(..., S, trie) is trie[S[0]][S[1]][S[2]][...][S[S.length - 1]]
        nodes = [reduce(dict.__getitem__, word[::-1], trie)
                 for word in words]
        #看结果就是建立了字典树。。。

        #Add word to the answer if it's node has no neighbors
        return sum(len(word) + 1
                   for i, word in enumerate(words)
                   if len(nodes[i]) == 0)
```



### 2.2 代码提速

#### 2.2.1 位操作会快很多

1. 移位除2：`a>>1`等价于`a//2`但会快很多;
2. 与取余：`a&1`$\Harr$`a%2`;

### 2.3 代码整洁

#### 2.3.1 print不换行

```python
print('输出内容',end='')
```

#### 2.3.2 快速建立给定长度0数组

```python
p = [0 for _ in range(t_len)]
```

#### 2.3.4  三目运算 

```python
#一般的写法
 if (x == y):
     print("两数相同！")
 elif(x > y):
     print("较大的数为：",x)
 else:
     print("较大的数为：",y)
            
# 三目运算符写法
print(x if(x>y) else y)
```

#### 2.3.5 enumerate()

`for i, l in enumerate(目标数组)`

可以同时取出index和值

#### 2.3.6 用list

`stack = [(i, j)]`可以建立一个（i，j）为元素的数组

#### 2.3.7 for else

```python
for i in ():
    。。。
else：# 若for循环没有正常执行完成就执行下面语句（比如中途有break）
	。。。
```

c++里也有，不建议总用，好像对代码阅读不友好。

#### 2.3.8 别忘了库

`math.gcd`求两个数的最大公约数，返回整数；

`collections.Counter`统计字符串（数字）种类及数量，返回字典；

`functools.reduce`逐次对上次函数结果与当前序列元素应用函数；

- `reduce(function, sequence)`
- 例如 reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) 计算为((((1+2)+3)+4)+5)

### 2.4 常用语法

#### 2.4.1 字典操作

[collections.defaultdict()的使用](https://blog.csdn.net/yangsong95/article/details/82319675)可以避免 【当Key不存在时，会引发‘KeyError’异常 】

> - 字典查找速度快
> - 字典是无序的；（python 3.6以上版本有序）
> - 字典支持乘加、成员检查、长度、最小值、最大值、嵌套；
> - 字典值不支持列表、元组、索引、切片、元素赋值跟切片赋值；
> - 字典通过大括号表示；
> - 字典的内容是项；项由键和值组成，中间用冒号隔开；项和项之间用逗号隔开；需要注意键必须是唯一的；
> - 字典的意义是让用户能够快速的找到特定的单词（键），以获悉其定义（值）；
> - 字典通过键来进行查看值的内容
> - 字典的值可以是字符串、数字、字典

1. 字典的赋值

```
dict1 = {'key1':'value1', 'key2':'value2'}
```

2. 字典的添加

```
dic1 = {'name': 'liangxiao', 'age': 24}
dic1.setdefault('work', 'IT')            # 原有key存在值，则不操作

dic1 = {'name': 'liangxiao', 'age': 24}
dic1['work'] = 'IT'                      # 原有key存在值，则覆盖
```

3. 字典的更新

```
dic1 = {'name': 'liangxiao'}
dic2 = {'age': 18}
dic2.update(dic1)        　　　　　　　　# 将dic1里面的内容更新到dic2里面
```

```
dic1 = {'name': 'liangxiao', 'age': 24}
dic1['name'] = 'LIANGXIAO'            # 更新value的内容
```

4. 字典的删除

```
dic1 = {'name': 'liangxiao', 'age': 24}
dic1.pop('name')            # 根据key进行键值对删除，可设置返回值，没有找到相应的key默认会报错

dic1 = {'name': 'liangxiao', 'age': 24}
del dic1                    # 删除字典

dic1 = {'name': 'liangxiao', 'age': 24}
dic1.clear()                # 清空字典

dic1 = {'name': 'liangxiao', 'age': 24}
dic1.popitem()              # 随机删除任意一个键值对
```

5. 通过列表转换字典

```
items = [('name', 'xiao'), ('age', 25)]
Dict_ = dict(items)
```

6. 字典的查看

```
dic1.values()　　　　　　  # 查看所有的value
dic1.keys()　　　　　　　　 # 查看所有的key
print(dict)              # 打印字典所有
dic1.get('name')         # 查找指定的key的value，没有则返回None
dic1.items()             # 一组一组的查找所有内容
```

## 三、一些软件用法

### 3.1 pycharm

1. 无法debug可能是程序没加断点。

2. [pycharm添加文件头注释](https://www.cnblogs.com/yellow-hgy/p/10208759.html)

3. #### pycharm中批量替换                               

   ctrl /command + f 搜索
   ctrl /command + r 批量替换

## 四、一些疑问

### 4.1 发现两次提交会差很多时间？



## 五、C++记录

### 5.1 string操作

1 赋值

```c++
string s="";
string a="abcdefg";

//将字符串a完全赋值给新字符串s
s.assign(a);

//将字符串a的一部分赋值给新的字符串s
//start是截取字符串的首位置，len是截取字符串的长度
s.substr(start,len);
s.assign(a,start,len);

//对字符串s赋相同的n个初值
s.assing(n,'x'); 
//如给s赋10个字符a写法如下：
s.assign(10,'a');
```



---

---



# 刷 leetcode 记录

## 1 两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```
找到的暂时最佳答案
```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        hashmap = {}
        for index, num in enumerate(nums):
#enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标
            another_num = target - num
            if another_num in hashmap:
                return [hashmap[another_num], index]
            hashmap[num] = index
        return None
```

## 2 两数相加

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

**思路**：

重点在于需要递归

为什么呢，因为正常的循环会出现先后矛盾的问题，所以需要递归，

递归就是在函数体里缩进n次，重点看：

1. 函数的重要执行部分的执行顺序（本例：链表的头插）、
2. 函数代入的值（具体到每个变量都是几）、
3. 以及最后一步函数如何有限化（讲退出函数有点容易让人误会，本例：用if使不满足条件时不调用自身，总执行数有限）

**自己做的答案**

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def adding2(l1: ListNode, l2: ListNode, carry) -> ListNode:
    if l1 != None or l2 != None or carry != 0:
        if l1 == None: l1 = ListNode(0)
        if l2 == None: l2 = ListNode(0)
        node_ = ListNode((l1.val + l2.val + carry) % 10)
        node_.next = adding2(l1.next,l2.next,(l1.val + l2.val + carry) // 10)
        return node_

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        return adding2(l1,l2,0)
```

> 执行用时 :116 ms, 在所有 Python3 提交中击败了7.73% 的用户
>
> 内存消耗 :13.2 MB, 在所有 Python3 提交中击败了55.17%的用户
>

菜，，但是看了别人的，思路都差不多，就是链表这个东西在python里不是很友好，很多内存管理很绕。

时间上可以继续优化

1. 利用判断（>10）来进位，比较比整除算得快
2. 把比较改为取反

尝试把比较改为取反
```python
def adding2(l1: ListNode, l2: ListNode, carry) -> ListNode:
    if l1 or l2 or carry:
        if not l1: l1 = ListNode(0)
        if not l2: l2 = ListNode(0)
        node_ = ListNode((l1.val + l2.val + carry) % 10)
        node_.next = adding2(l1.next,l2.next,(l1.val + l2.val + carry) // 10)
        return node_
```

> 执行用时 :64 ms, 在所有 Python3 提交中击败了94.58% 的用户
>
> 内存消耗 :13.3 MB, 在所有 Python3 提交中击败了55.17%的用户

PS: 验证了取反比直接比较快，利用如下代码
```python
import datetime

a = None
start = datetime.datetime.now()
for i in range(100000000):
    if a is not None:
        a = 1
    else:
        a = None

end = datetime.datetime.now()
print(end - start)

start = datetime.datetime.now()
for i in range(100000000):
    if not a:
        a = 1
    else:
        a = None
end = datetime.datetime.now()
print(end - start)
```

结果：

```
0:00:08.103332
0:00:07.248612
```

## 3. 无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

示例 2:

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

示例 3:

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

**思路**：

参考题目1，建立字典，达到单次遍历做完这件事。

具体实现：建立一个front变量存放节点，使得新遍历的字符添加后，无重复最长字串长度只能为:
$$
MAX(添加前记录的最长长度，添加后长度-front)
$$

front定位为复现过字符的、最靠后的、上一次复现位置。

**自己做的答案**:

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        dic = {}
        lol = 0
        front = -1
        for index, char in enumerate(s):
            if char in dic:
                cg =  dic.get(char)
                if cg>front:
                    front = cg
            wt = index - front 
            if wt> lol:
                lol = wt
            dic[char] = index
        return lol
```

>执行用时 :48 ms, 在所有 Python3 提交中击败了99.05% 的用户
>
>内存消耗 :13.2 MB, 在所有 Python3 提交中击败了55.78%的用户

### **发现**：

1. 先把计算结果存起来再多次复用会大大降低运行时间
2. enumerate( str )生成的 index 从 **0** 开始

## 4. 寻找两个有序数组的中位数

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

示例 1:

```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

示例 2:

```
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

**思路**：

1. 从头开始建立新数组，建立一半就找到中位数了
2. 省时间的做法，可以到中位数位置的时候记录一下当时的数字，避免无用的读写

**自己做的答案**：

```python
class Solution:
    def findMedianSortedArrays(self, nums1: [int], nums2: [int]) -> float:
        c1 = 0
        c2 = 0
        l1 = nums1.__len__()
        l2 = nums2.__len__()
        mid = (l1 + l2) // 2
        sig = True if l1 + l2 - mid * 2 else False  # 判断总数是否为奇数
        nums1_emt = not nums1
        nums2_emt = not nums2
        num = 0
        while num <= mid:
            if nums1_emt:
                num = num + 1
                if num == mid + 1:
                    n2 = nums2[c2]
                if num == mid:
                    n1 = nums2[c2]
                if c2 == l2 - 1:
                    break
                else:
                    c2 = c2 + 1
            if nums2_emt:
                num = num + 1
                if num == mid + 1:
                    n2 = nums1[c1]
                if num == mid:
                    n1 = nums1[c1]
                if c1 == l1 - 1:
                    break
                else:
                    c1 = c1 + 1
            if not nums1_emt and not nums2_emt:
                if nums1[c1] < nums2[c2]:
                    num = num + 1
                    if num == mid + 1:
                        n2 = nums1[c1]
                    if num == mid:
                        n1 = nums1[c1]
                    if c1 == l1 - 1:
                        nums1_emt = True
                    else:
                        c1 = c1 + 1
                else:
                    num = num + 1
                    if num == mid + 1:
                        n2 = nums2[c2]
                    if num == mid:
                        n1 = nums2[c2]
                    if c2 == l2 - 1:
                        nums2_emt = True
                    else:
                        c2 = c2 + 1
        if sig:
            return n2 * 1.0
        else:
            return (n1 + n2) / 2.0
```

>执行用时 :108 ms, 在所有 Python3 提交中击败了70.54% 的用户
>
内存消耗 :13.2 MB, 在所有 Python3 提交中击败了51.77%的用户

​	这个时间复杂度不对

网上有个很简洁的做法，用了移位除2

```python
class Solution:
    def findMedianSortedArrays(self, nums1: [int], nums2: [int]) -> float:
        l = nums1 + nums2
        l.sort()
        ln = len(l)
        if ln % 2:
            return l[ln >> 1]
        return (l[ln >> 1] + l[(ln >> 1) - 1]) / 2
```

提交了一下试试也就快那么一点



- [x] **二分法待编写**

思想：
    通过寻找数组1的一个缝，由中位数位置确定数组二对应的一个缝；
    使得
    两缝前的所有数都小于两缝后的数
    找到了就搞定了，算法复杂度符合题目要求
编写成果：

```python
class Solution:
    def findMedianSortedArrays(self, nums1: [int], nums2: [int]) -> float:
        l1 = nums1.__len__()
        l2 = nums2.__len__()
        la = l1 + l2
        mid = la >> 1
        sig = la & 1  # 判断总数是否为奇数
        if l1 > l2:  # let nums1 the shorter one
            l1, l2 = l2, l1
            nums2, nums1 = nums1, nums2
        c1 = l1 >> 1  # 定义为缝
        c1f = 0
        c1b = l1
        while True:
            c2 = mid - c1
            if not c1:
                if c1 == l1:  # nums1 is empty
                    if sig:
                        return nums2[mid] * 1.0
                    else:
                        return (nums2[mid - 1] + nums2[mid]) / 2
                if c2 == l2:  # search is done & l1 == l2 so:
                    if sig:
                        return nums2[c2 - 1] * 1.0
                    else:
                        return (nums2[c2 - 1] + nums1[0]) / 2

                b1 = nums1[c1]
                f2 = nums2[c2 - 1]
                b2 = nums2[c2]
                if f2 > b1:  # c1 move right
                    c1f = c1
                    if c1 == c1b - 1:
                        c1 = c1b
                    else:
                        c1 = (c1b + c1) >> 1
                else:
                    if sig:
                        return min(b1, b2) * 1.0
                    else:
                        return (f2 + min(b1, b2)) / 2
                continue

            if c1 == l1:  # if nums1 is empty ,has been returned when c1 == 0
                if not c2:  # search is done & l1 == l2 so:
                    if sig:
                        return nums2[0] * 1.0
                    else:
                        return (nums1[c1 - 1] + nums2[0]) / 2
                f1 = nums1[c1 - 1]
                f2 = nums2[c2 - 1]
                b2 = nums2[c2]
                if f1 > b2:
                    c1b = c1
                    c1 = (c1f + c1) >> 1
                else:
                    if sig:
                        return b2 * 1.0
                    else:
                        return (max(f1, f2) + b2) / 2
                continue

            if c1 and c1 != l1:
                f1 = nums1[c1 - 1]
                b1 = nums1[c1]
                f2 = nums2[c2 - 1]
                b2 = nums2[c2]
                if f1 > b2:  # c1 move left
                    c1b = c1
                    c1 = (c1f + c1) >> 1
                if f2 > b1:  # c1 move right
                    c1f = c1
                    if c1 == c1b - 1:
                        c1 = c1b
                    else:
                        c1 = (c1b + c1) >> 1
                if f1 <= b2 and f2 <= b1:
                    if sig:
                        return min(b1, b2) * 1.0
                    else:
                        return (max(f1, f2) + min(b1, b2)) / 2
```

结果。。。几乎没有时间变化







### **反思：**

当前提交通过率太低，究其原因有两点：

1. 对题意有理解漏洞，经常出现没想到的输入情况
2. 代码有bug
3. 笔误

可以通过以下方法解决：

1. 做题目前把多种情况考虑好，凭经验给出几个特殊的输入进行实验
2. 用自己的编译器跑通再上传
3. 同2

##  5. 最长回文子串

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：
```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```
示例 2：
```
输入: "cbbd"
输出: "bb"
```

特例：

`"abababa" -> "abababa"`

`"cbbb" -> "bbb"`

`'' -> ''`

**思路**：

1. 回文：正看反看都一样
2. 暴力法

代码：
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        l = s.__len__()
        ss=""
        for i in range(0,l):
            for j in range(0,i+1):
                ss = s[j:l-i+j+1]
                sv = ss[::-1]
                if ss == sv:
                    return ss
        return ss
```

>执行用时 :5440 ms, 在所有 Python3 提交中击败了16.11% 的用户
>
>内存消耗 :12.9 MB, 在所有 Python3 提交中击败了63.25%的用户

### **Manacher 算法**

参考这个：[动态规划、Manacher 算法](https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zhong-xin-kuo-san-dong-tai-gui-hua-by-liweiwei1419/)
**自己写的代码**：


```python
def add_char(s,c):
    ss = c
    for _ in s:
        ss = ss + _ + c
    return ss

def delete_char(ss):
    size = len(ss)
    s=''
    for i in range(1,size):
        if i&1:s = s+ss[i]
    return s

def center_spread(s, center, start):
    size = len(s)
    i = center - 1 - start
    j = center + 1 + start
    step = start
    while i >= 0 and j < size and s[i] == s[j]:
        step += 1
        i = i - 1
        j = j + 1
    return step


class Solution:

    def longestPalindrome(self, s: str) -> str:
        # todo insert'#'
        ss = add_char(s,'#')
        # form 1 to l-1, calculate p (this char as center the longestP's length
        l = len(ss)
        p = [0] * l
        maxRight = 0
        center = 0
        p_max = 0
        longest_center = 0
        for i in range(1, l - 1):  # use a complicated way to traverse
            if i >= maxRight:
                p[i] = center_spread(ss, i, 0)
                if p[i] + i > maxRight:
                    center = i
                    maxRight = i + p[i]
            else:
                mirror = 2 * center - i
                if p[mirror] < maxRight - i:
                    p[i] = p[mirror]
                if p[mirror] == maxRight - i:
                    p[i] = center_spread(ss, i, p[mirror])  # start spread from a specific value
                    if p[i] + i > maxRight:
                        center = i
                        maxRight = i + p[i]
                if p[mirror] > maxRight - i:
                    p[i] = maxRight - i
            if p[i] > p_max:
                p_max = p[i]
                longest_center = i
        # minus "#" and return
        sss = ss[longest_center - p[longest_center]:longest_center + p[longest_center]+1]
        return delete_char(sss)
```
>执行用时 :116 ms, 在所有 Python3 提交中击败了90.77% 的用户
内存消耗 :13.5 MB, 在所有 Python3 提交中击败了52.65%的用户

**反思**：

1. `p = [0 for _ in range(t_len)]`快速建立给定长度0数组
2. 写的那个delete太蠢了，正解应该通过center推算答案在原字符串的起点与终点位置

## 6. Z 字形变换

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
```
L   C   I   R
E T O E S I I G
E   D   H   N
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);

示例 1:
```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```
示例 2:
```
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:

L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

**思路：**

建立字符串数组，逐个往里放就得了

代码：
```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s
        num = 0
        flag = True
        ss = []
        sss = ''
        for i in range(0,numRows):
            ss.append('')
        for i in s:
            ss[num] = ss[num] + i
            if num == numRows-1:
                flag = False
            if num == 0:
                flag = True
            if flag:
                num = num + 1
            else:
                num = num - 1
        for j in ss:
            sss = sss+j
        return sss
```
>执行用时 :76 ms, 在所有 Python3 提交中击败了39.18% 的用户
>内存消耗 :13.1 MB, 在所有 Python3 提交中击败了51.78%的用户


## 7. 整数反转

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1:

```
输入: 123
输出: 321
```

 示例 2:

```
输入: -123
输出: -321
```

示例 3:

```
输入: 120
输出: 21
```

注意:

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为$ [−2^{31},  2^{31} − 1]$。请根据这个假设，如果反转后整数溢出那么就返回 0。

暴力解法：

```python
class Solution:
    def reverse(self, x: int) -> int:
        num = 0
        if x <0:
            x = -x
            s = str(x)
            s = s[::-1]
            num = -int(s)
        else:
            s = str(x)
            s = s[::-1]
            num = int(s)
        if num > pow(2,31)-1 or num <-pow(2,31):
            num = 0
        return num
```

> 执行用时 :52 ms, 在所有 Python3 提交中击败了12.27% 的用户
>
> 内存消耗 :13 MB, 在所有 Python3 提交中击败了52.88%的用户

## 8.字符串转整数（`atoi`）

请你来实现一个 `atoi` 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

说明：

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

示例 1:

```
输入: "42"
输出: 42
```

示例 2:

```
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```

示例 3:

```
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```

示例 4:

```
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```

示例 5:

```
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```

测试用例：

```python
s = ' -01 dfaf'
s = ' + 1'
s = '-'
s = '42'
s = '-42'
s = 'words in 944'
s = "-91283472332"
s = "4193 with words"
s = '3.14159'
```

代码：

```
class Solution:
    def myAtoi(self, str: str) -> int:
        j = 0
        num = 0
        for i in str:
            if i is not ' ':
                str = str[j:]
                break
            j = j + 1
        if str.isnumeric():
            num = int(str)
        else:
            if str.__len__()>1:
                j = 0
                for i in str[1:]:
                    j = j + 1
                    if not i.isnumeric():
                        str = str[:j]
                        break
                if str.isnumeric(): num = int(str)
                if str.__len__() > 1:
                    if str[0] == '+': num = int(str[1:])
                    if str[0] == '-': num = int(str)
        if num > pow(2, 31) - 1:
            return 2147483647
        if num < -pow(2, 31):
            return -2147483648
        return num

```


>执行用时 :56 ms, 在所有 Python3 提交中击败了11.22% 的用户
>
>内存消耗 :13 MB, 在所有 Python3 提交中击败了49.12%的用户

题解：

自动机

 ![fig1](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/8_fig1.PNG)

建一个类，把所有情况列表输入，代码清晰工整。
```python
INT_MAX = 2 ** 31 - 1
INT_MIN = -2 ** 31

class Automaton:
    def __init__(self):
        self.state = 'start'
        self.sign = 1
        self.ans = 0
        self.table = {
            'start': ['start', 'signed', 'in_number', 'end'],
            'signed': ['end', 'end', 'in_number', 'end'],
            'in_number': ['end', 'end', 'in_number', 'end'],
            'end': ['end', 'end', 'end', 'end'],
        }
        
    def get_col(self, c):
        if c.isspace():
            return 0
        if c == '+' or c == '-':
            return 1
        if c.isdigit():
            return 2
        return 3

    def get(self, c):
        self.state = self.table[self.state][self.get_col(c)]
        if self.state == 'in_number':
            self.ans = self.ans * 10 + int(c)
            self.ans = min(self.ans, INT_MAX) if self.sign == 1 else min(self.ans, -INT_MIN)
        elif self.state == 'signed':
            self.sign = 1 if c == '+' else -1

class Solution:
    def myAtoi(self, str: str) -> int:
        automaton = Automaton()
        for c in str:
            automaton.get(c)
        return automaton.sign * automaton.ans
```

虽然很长，但可读性强，很工整。


## 9. 回文数

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

> 示例 1:
>

```
输入: 121
输出: true
```

示例 2:

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数
```

示例 3:

```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```

进阶:

你能不将整数转为字符串来解决这个问题吗？

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        s  = str(x)
        return s == s[::-1]
```

> 执行用时 :32 ms, 在所有 Python3 提交中击败了100.00% 的用户
>
> 内存消耗 :13.1 MB, 在所有 Python3 提交中击败了47.36%的用户

## [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。

```
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
```

所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。

说明:

    s 可能为空，且只包含从 a-z 的小写字母。
    p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。

示例 1:

```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```

示例 2:

```
输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```

示例 3:

```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```

示例 4:

```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```

示例 5:

```
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
```

思考：

如果只有`'.'`，问题十分简单，一次遍历就可以。

所以先把`'*'`变成字母和`'.'`；

先前尝试从前往后遍历会出问题，因为无法考虑到后面重复字符带来的问题。

所以只能统筹考虑

1. 用`'*'`分割字符串得到`box[]`;

2. 将分割后的字符串存到两个数组中，`sld[]`存放固定字符，`rpt[]`存放可重复字符;

   > 则后续为比较`s[] `与` sld[0]+rpt[0]*rns[0]+...+sld[n-1]+rpt[n-1]*rns[n-1]+sld[n]`;

3. 计算`s[]`中字符个数，减去`sld[]`中总字符数，剩余为可填补位置数（小于零直接输出`False`）；

4. 将`rpt[]`中的字符重复数重排列填入相应位置，形成对比数组，此数组中只含有字母和`'.'`；

   > 由于循环嵌套次数未知，使用递归进行循环

5. 对比`s[]`和生成的数组，如果匹配输出`True`，遍历结束无匹配项输出`False`；



递归：

原来问题为：给定n个位置，有m+1个字符有顺序、可重复的填入

问题可简化为：给定n个位置，在里面切m🔪得到的排列方案，可以重复切一个位置，可切头尾。
```python
class record():
    rcd = []
    len = 0


def show(m, n):
    if record.len < m:
        record.len = m
        record.rcd = [0] * m
    if m == 1:
        record.rcd[record.len - m] = n
        print(record.rcd)
    else:
        for i in range(0, n):
            record.rcd[record.len - m] = i
            show(m - 1, n - i)
show(3, 4)
```
输出：
```python
[0, 0, 4]
[0, 1, 3]
[0, 2, 2]
[0, 3, 1]
[1, 0, 3]
[1, 1, 2]
[1, 2, 1]
[2, 0, 2]
[2, 1, 1]
[3, 0, 1]
```

下一步是用来替换print的代码：
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Time    : 2020/2/16 10:08
# @Author  : Eiya_ming
# @Email   : eiyaming@163.com
# @File    : q10_test2.py


class Record():
    rcd = []
    len = 0

    def clean(self):
        self.rcd = []
        self.len = 0


def compare(s, p_rf):
    l = len(s)
    if len(p_rf) != l:
        return False
    else:
        for i in range(0, l):
            if s[i] == p_rf[i] or p_rf[i] == '.':
                continue
            else:
                return False
        return True


class Contrast():
    def __init__(self, s, p):
        self.s = s
        self.p = p
        self.box = p.split('*')
        self.lb = len(self.box)
        self.ls = len(s)
        self.sld = []  # 存放不可重复字符串
        self.rpt = []  # 存放可重复字符
        self.rpt_left = self.ls
        self.result = False
        for i in range(0, self.lb - 1):
            if not self.box[i]:
                self.sld.append('')
                self.rpt.append('')
            else:
                self.sld.append(self.box[i][:-1])
                self.rpt.append(self.box[i][-1])
                self.rpt_left = self.rpt_left - len(self.box[i][:-1])
        self.sld.append(self.box[-1])
        self.rpt_left = self.rpt_left - len(self.box[-1])

    def convert(self, nums: [int]) -> str:
        p_c = ''
        for i in range(0, self.lb - 1):
            p_c = p_c + self.sld[i] + self.rpt[i] * nums[i]
        p_c = p_c + self.sld[self.lb - 1]
        print(p_c)
        return p_c

    def show(self, m: int, n: int):
        """
        给定n个位置，在里面切m🔪得到的排列方案，可以重复切一个位置，可切头尾。
        :param m: 切m🔪       (m>=0)
        :param n: n个位置     (n>=0)
        :return: None
        """
        if self.result: return
        if not m:  # 没刀切，直接比较
            Record.rcd = [n]
            self.result = compare(self.s, self.p)
            # print(Record.rcd)
            return
        if Record.len < m:
            Record.len = m
            Record.rcd = [0] * m
        if m == 1:
            Record.rcd[Record.len - m] = n
            # print(Record.rcd)
            # 把数组输入字符串
            # 比较两个字符串
            if compare(self.s, self.convert(Record.rcd)):
                self.result = True
                return
        else:
            for i in range(0, n + 1):
                Record.rcd[Record.len - m] = i
                self.show(m - 1, n - i)


class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        # 初始化solve
        solve = Contrast(s, p)
        if solve.rpt_left < 0: return False
        # 进入递归函数，逐个比较
        solve.show(solve.lb - 1, solve.rpt_left)
        Record.clean(Record)
        return solve.result


def main():
    s = "aa"
    p = "a"
    print(Solution.isMatch(Solution, s, p))
    s = "aaa"
    p = "ab*a*c*a"
    print(Solution.isMatch(Solution, s, p))
    s = "aa"
    p = ".*"
    print(Solution.isMatch(Solution, s, p))
    s = "ab"
    p = ".*c"
    print(Solution.isMatch(Solution, s, p))
    s = "aaaaaaaaaaaaab"
    p = "a*a*a*a*a*a*a*a*a*a*a*a*b"
    print(Solution.isMatch(Solution, s, p))
    s = "acabacbaaabacba"
    p = "bc*c*.b*b*.*a*a*.*"
    print(Solution.isMatch(Solution, s, p))


if __name__ == '__main__':
    main()

```

这个代码跑通没问题，就是会超时，肉眼可见的处理速度是不行滴。

所以需要修改，使每次遍历的每个步骤都做比较，不行马上停，节省时间（不好写。。）

```python
class Record():
    rcd = []
    len = 0

    def clean(self):
        self.rcd = []
        self.len = 0


def compare(s, p_rf):
    l = len(s)
    if len(p_rf) != l:
        return False
    else:
        for i in range(0, l):
            if s[i] == p_rf[i] or p_rf[i] == '.':
                continue
            else:
                return False
        return True


def step_cp(s, p_rf):
    l = len(p_rf)
    for i in range(0, l):
        if s[i] == p_rf[i] or p_rf[i] == '.':
            continue
        else:
            return False
    return True


class Contrast():
    def __init__(self, s, p):
        self.s = s
        self.p = p
        self.box = p.split('*')
        self.lb = len(self.box)
        self.ls = len(s)
        self.sld = []  # 存放不可重复字符串
        self.rpt = []  # 存放可重复字符
        self.rpt_left = self.ls
        self.result = False
        for i in range(0, self.lb - 1):
            if not self.box[i]:
                self.sld.append('')
                self.rpt.append('')
            else:
                self.sld.append(self.box[i][:-1])
                self.rpt.append(self.box[i][-1])
                self.rpt_left = self.rpt_left - len(self.box[i][:-1])
        self.sld.append(self.box[-1])
        self.rpt_left = self.rpt_left - len(self.box[-1])
        # todo 简单判断 sld 是否满足要求：
        # todo '.' 怎么办，先把sld中的点都去掉
        sld_dd = []
        for i in self.sld:
            sld_dd = sld_dd + i.split('.')
        sld_temp = self.s
        for i in sld_dd:
            if i :
                if i in sld_temp:
                    sld_temp = sld_temp.split(i, 1)[1]
                else:
                    self.rpt_left = -1
                    break

    def convert(self, nums: [int]) -> str:
        p_c = ''
        for i in range(0, self.lb - 1):
            p_c = p_c + self.sld[i] + self.rpt[i] * nums[i]
        p_c = p_c + self.sld[self.lb - 1]
        # print(p_c)
        # print('****************************')
        return p_c

    def temp(self, nums, m):
        p_c = self.sld[0]
        for i in range(0, self.lb - 1 - m):
            p_c = p_c + self.rpt[i] * nums[i] + self.sld[i + 1]
        # print(p_c, self.lb - 1 - m)
        # print(step_cp(self.s, p_c))
        return step_cp(self.s, p_c)

    def show(self, m: int, n: int):
        """
        给定n个位置，在里面切m🔪得到的排列方案，可以重复切一个位置，可切头尾。
        :param m: 切m🔪       (m>=0)
        :param n: n个位置     (n>=0)
        :return: None
        """
        if not m:  # 没刀切，直接比较
            Record.rcd = [n]
            self.result = compare(self.s, self.p)
            # print(Record.rcd)
            return
        if Record.len < m:
            Record.len = m
            Record.rcd = [0] * m
        if m == 1:
            Record.rcd[Record.len - m] = n
            # print(Record.rcd)
            # 把数组输入字符串
            # 比较两个字符串
            if compare(self.s, self.convert(Record.rcd)):
                self.result = True
                return
        else:
            for i in range(0, n + 1):
                if self.result:
                    return
                if not self.temp(Record.rcd, m):  # 利用 m 实现成型过程打印,现在对不上，打印一个 False
                    break
                Record.rcd[Record.len - m] = i
                self.show(m - 1, n - i)


class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        print("--------------------------------------------------------------")
        print("s = ", s)
        print("p = ", p)
        # 初始化solve
        solve = Contrast(s, p)
        if solve.rpt_left < 0: return False

        # 进入递归函数，逐个比较
        solve.show(solve.lb - 1, solve.rpt_left)
        Record.clean(Record)
        return solve.result
```
>执行用时 :828 ms, 在所有 Python3 提交中击败了39.05% 的用户
内存消耗 :13.6 MB, 在所有 Python3 提交中击败了35.50%的用户

舒服了~~~~



### **反思**：

出现如下问题：

1. 输出超出限制
2. 时间超出限制
3. 多次执行变量未随之多次初始化
4. 本次做题尝试时间太多，要合理利用答案
5. 把print去掉，没有什么实质性变化

对应解决方案：

1. 每次提交，把print都检查一遍，（实际也可以不检查，一般超出输出限制那个时间必然惨不忍睹），取消相应的print。
2. 这就算法问题，思路不对或者有死循环
3. 测试的时候进行重复调用测试
4. **下面这个是重点：**

### 以后的刷题流程：

1. 读题，思路，选最优写简单代码，一次尝试

2. 不管尝试结果，记录下来

3. 看答案，选一个最优的答案，自己敲一遍，上传到成功，记录

   

## [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

给定 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *ai*) 。在坐标内画 *n* 条垂直线，垂直线 *i* 的两个端点分别为 (*i*, *ai*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器，且 *n* 的值至少为 2。

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

 

**示例:**

```
输入: [1,8,6,2,5,4,8,3,7]
输出: 49
```

思路：

两端向内遍历：

重点如下：

若向内移动短板，水槽的短板 $min(h[i],h[j]))$可能变大，因此水槽面积 $S(i,j)$可能增大。
若向内移动长板，水槽的短板 $min(h[i],h[j])$ 不变或变小，下个水槽的面积一定小于当前水槽面积。

![容器盛水过程](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/容器盛水过程.gif)

这样遍历完成，记录的最大面积即为所求。

代码：
```python
class Solution:
    def maxArea(self, height: [int]) -> int:
        i, j, s_max = 0, len(height) - 1, 0
        while i != j:
            s_temp = min(height[i], height[j]) * (j - i)
            if s_temp > s_max: s_max = s_temp
            if height[i] < height[j]:
                i = i + 1
            else:
                j = j - 1
        return s_max
```

### **反思**：

1. `is not`和`!=`不一样

## [12. 整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)

罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

**示例 1:**

```
输入: 3
输出: "III"
```

**示例 2:**

```
输入: 4
输出: "IV"
```

**示例 3:**

```
输入: 9
输出: "IX"
```

**示例 4:**

```
输入: 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.
```

**示例 5:**

```
输入: 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

思路：建立一个表，直接查表输出，感觉最快了。
```python
class Solution:
    p = ['' for _ in range(34)]
    for i in range(4):
        p[i] = 'I'*i
        p[i+5] = 'V'+'I'*i
        p[i+10] = 'X' * i
        p[i + 15] = 'L' + 'X' * i
        p[i + 20]='C' * i
        p[i + 25]='D' + 'C' * i
        p[i + 30]='M' * i
    p[4],p[9],p[14],p[19],p[24],p[29]='IV','IX','XL','XC','CD','CM'
    def intToRoman(self, num: int) -> str:
        nums = []
        i = 0
        roman = ''
        while num > 0:
            nums = nums + [num % 10 +10*i]
            num = num // 10
            i += 1
        nums.reverse()
        for j in nums:
            roman += self.p[j]
        return roman
```
>执行用时 :44 ms, 在所有 Python3 提交中击败了94.58% 的用户
内存消耗 :13.5 MB, 在所有 Python3 提交中击败了36.26%的用户

### **贪心算法：**

 在以前还使用现金购物的时候，**如果我们不想让对方找钱**，付款的时候我们会**尽量选择面值大的纸币**给对方，这样才会使得我们给对方的纸币张数最少，对方点钱的时候也最方便。 

![贪心算法（罗马数字转换）](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/贪心算法（罗马数字转换）.gif)

时间复杂度和空间复杂度都低（只遍历哈希表），但没有上一种快，代码也会简单一点。

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # 使用哈希表，按照从大到小顺序排列
        hashmap = {1000:'M', 900:'CM', 500:'D', 400:'CD', 100:'C', 90:'XC', 50:'L', 40:'XL', 10:'X', 9:'IX', 5:'V', 4:'IV', 1:'I'}
        res = ''
        for key in hashmap:
            if num // key != 0:
                count = num // key  # 比如输入4000，count 为 4
                res += hashmap[key] * count 
                num %= key
        return res
```

## [13. 罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

**示例 1:**

```
输入: "III"
输出: 3
```

**示例 2:**

```
输入: "IV"
输出: 4
```

**示例 3:**

```
输入: "IX"
输出: 9
```

**示例 4:**

```
输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```

**示例 5:**

```
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

思路：

直接数个数

细节上，先判断俩字符，再判断其他
```python
class Solution:
    def romanToInt(self, s: str) -> int:
        s = s.replace('CM', 'D' + 'C' * 4)
        s = s.replace('CD', 'C' * 4)
        s = s.replace('XC', 'L' + 'X' * 4)
        s = s.replace('XL', 'X' * 4)
        s = s.replace('IX', 'V' + 'I' * 4)
        s = s.replace('IV', 'I' * 4)
        return (s.count('M') * 1000 + s.count('D') * 500 + s.count('C') * 100 +
                s.count('L') * 50 + s.count('X') * 10 + s.count('V') * 5 + s.count('I'))

```

>执行用时 :48 ms, 在所有 Python3 提交中击败了83.66% 的用户

> 内存消耗 :13.5 MB, 在所有 Python3 提交中击败了34.39%的用户

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        d = {'I':1, 'IV':3, 'V':5, 'IX':8, 'X':10, 'XL':30, 'L':50, 'XC':80, 'C':100, 'CD':300, 'D':500, 'CM':800, 'M':1000}
        return sum(d.get(s[i-1:i+1], d[n]) for i, n in enumerate(s))
```

>执行用时 :84 ms, 在所有 Python3 提交中击败了12.18% 的用户

> 内存消耗 :13.4 MB, 在所有 Python3 提交中击败了34.39%的用户

美观，但慢，思路清奇。

## [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1:**

```
输入: ["flower","flow","flight"]
输出: "fl"
```

**示例 2:**

```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

**说明:**

所有输入只包含小写字母 `a-z` 。

思路：

挨个遍历就行：

有特殊情况

1. 到某个字符串数完了
2. 返回空

具体流程：

1. 判断是否为空数组
2. 判断第一个字符串是否为空
3. 取第一个字符串的字符char遍历每个字符串
   1. 若字符串长度不够，返回rst
   2. 若字符串对应位置字符不对应，返回rst
   3. 若对应，continue
   4. 遍历完一遍strs操作
      1. rst+=char
      2. 如果还能取，取char=下一个字符
      3. 如果不能，返回
```python
class Solution:
    def longestCommonPrefix(self, strs: [str]) -> str:
        if not strs or not strs[0]: return ''
        rst = ''
        i = 0
        char = strs[0][0]
        while True:
            for s in strs:
                if s.__len__() <= i: return rst
                if s[i] != char: return rst
            rst += char
            i += 1
            if len(strs[0]) == i:
                return rst
            char = strs[0][i]
```

>执行用时 :44 ms, 在所有 Python3 提交中击败了35.93% 的用户

> 内存消耗 :13.5 MB, 在所有 Python3 提交中击败了32.82%的用户

看答案：

一种利用如下公式，有水平，分治和二分（如图）几种方法。

$$
LCP(S_1…S_n)=LCP(LCP(S_1…S_k),LCP(S_{k+1}…S_n))
$$
 ![使用二分法寻找最长公共前缀](https://pic.leetcode-cn.com/e41778494b56890e2bb7616504e2a0169bbdb409710262eaf5250c635adab9d6-file_1555694009677) 

最后有种字典树的方法：

 ![使用字典树寻找最长公共前缀](https://pic.leetcode-cn.com/093a52aeacfa1f4b5489bbee3a6d0de22c9dcde6dd72a1c1887f3b75f3eec749-file_1555694178934) 

### **反思**：

1. 以后自己想法实现之后要计算时间与空间复杂度。

## [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

给定一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？找出所有满足条件且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

**示例：**

```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

暴力，超时了。。。

```python
class Solution:
    def threeSum(self, nums: [int]) -> [[int]]:
        l = len(nums)
        if l < 3: return []
        rst = []
        nums.sort()

        for i in range(0,l-1):
            for j in range(i+1,l):
                temp = -nums[i]-nums[j]
                if temp in nums[j+1:l]:
                    tt = [nums[i],nums[j],temp]
                    if tt not in rst:
                        rst.append([nums[i],nums[j],temp])
        return rst
```

看了眼答案，没啥牛逼解法，和自己想法都差不多，同时表现为代码能力有待提高

细看他这个双指针思路比我清晰多了

copy一个

```python
class Solution:
    def threeSum(self, nums: [int]) -> [[int]]:
        n = len(nums)
        res = []
        if n < 3: return []
        nums.sort()
        res = []
        for i in range(n):
            if (nums[i] > 0):
                return res
            if (i > 0 and nums[i] == nums[i - 1]):
                continue
            L = i + 1
            R = n - 1
            while (L < R):
                if (nums[i] + nums[L] + nums[R] == 0):
                    res.append([nums[i], nums[L], nums[R]])
                    while (L < R and nums[L] == nums[L + 1]):
                        L = L + 1
                    while (L < R and nums[R] == nums[R - 1]):
                        R = R - 1
                    L = L + 1
                    R = R - 1
                elif (nums[i] + nums[L] + nums[R] > 0):
                    R = R - 1
                else:
                    L = L + 1
        return res
```

> 执行用时 :1164 ms, 在所有 Python3 提交中击败了43.62% 的用户
>
> 内存消耗 :16.1 MB, 在所有 Python3 提交中击败了73.01%的用户

## [16. 最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)

给定一个包括 *n* 个整数的数组 `nums` 和 一个目标值 `target`。找出 `nums` 中的三个整数，使得它们的和与 `target` 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```
例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).
```

参考上面的思路:

```python
class Solution:
    def threeSumClosest(self, nums: [int], target: int) -> int:
        n = len(nums)
        if n < 3: return
        nums.sort()
        minus = max(pow(sum(nums[i] for i in range(3)) - target, 2),
                    pow(sum(nums[i] for i in range(3)) - target, 2)) + 1
        for i in range(n - 2):
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            L = i + 1
            R = n - 1
            while L < R:
                temp = nums[i] + nums[L] + nums[R]
                m_temp = pow(temp - target, 2)
                if m_temp < minus:
                    res = temp
                    minus = m_temp
                if temp == target:
                    return target
                elif temp > target:
                    R = R - 1
                else:
                    L = L + 1
        return res
```

> 执行用时 :136 ms, 在所有 Python3 提交中击败了60.29% 的用户
>
> 内存消耗 :13.3 MB, 在所有 Python3 提交中击败了24.87%的用户

计算时间与空间复杂度：

时间：O(n2)

空间：O(1)

看了题解，思路基本相同，有些优化主要思路就是减少双指针的移动次数。

## [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/17_telephone_keypad.png)

**示例:**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**说明:**
 尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

想法：递归遍历没啥好说的感觉

```python
class Solution:
    dict = {'2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl', '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'}

    def digui(self, n):
        if n == self.digits.__len__():
            self.rsts.append(self.rst)
        else:
            for i in self.dict[self.digits[n]]:
                self.rst = self.rst[:n] + i + self.rst[n + 1:]
                self.digui( n + 1)

    def letterCombinations(self, digits: str) -> [str]:
        if not digits:return []
        self.rsts = []
        self.rst = '.' * len(digits)
        self.digits = digits
        self.digui( 0)
        return self.rsts
```

> 执行用时 :40 ms, 在所有 Python3 提交中击败了33.11% 的用户
>
> 内存消耗 :13.4 MB, 在所有 Python3 提交中击败了25.68%的用户

感觉极限了呀，可能切片耽误时间了

学习一下官方代码，干净但是没快多少

```python
class Solution:
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        phone = {'2': ['a', 'b', 'c'],
                 '3': ['d', 'e', 'f'],
                 '4': ['g', 'h', 'i'],
                 '5': ['j', 'k', 'l'],
                 '6': ['m', 'n', 'o'],
                 '7': ['p', 'q', 'r', 's'],
                 '8': ['t', 'u', 'v'],
                 '9': ['w', 'x', 'y', 'z']}

        def backtrack(combination, next_digits):
            # if there is no more digits to check
            if len(next_digits) == 0:
                # the combination is done
                output.append(combination)
            # if there are still digits to check
            else:
                # iterate over all letters which map
                # the next available digit
                for letter in phone[next_digits[0]]:
                    # append the current letter to the combination
                    # and proceed to the next digits
                    backtrack(combination + letter, next_digits[1:])

        output = []
        if digits:
            backtrack("", digits)
        return output
```

递归要存储的东西可以作为输入，方便处理

## [18. 四数之和](https://leetcode-cn.com/problems/4sum/)

给定一个包含 *n* 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 *a，**b，c* 和 *d* ，使得 *a* + *b* + *c* + *d* 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

**注意：**

答案中不可以包含重复的四元组。

**示例：**

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```
利用之前的三数之和，加一层循环

```python
class Solution:
    def threeSum(self, nums: [int], target) -> [[int]]:
        n = len(nums)
        if n < 3: return []
        # nums.sort()
        res = []
        for i in range(n):
            if (nums[i] > target//3):
                return res
            if (i > 0 and nums[i] == nums[i - 1]):
                continue
            L = i + 1
            R = n - 1
            while (L < R):
                if (nums[i] + nums[L] + nums[R] == target):
                    res.append([nums[i], nums[L], nums[R]])
                    while (L < R and nums[L] == nums[L + 1]):
                        L = L + 1
                    while (L < R and nums[R] == nums[R - 1]):
                        R = R - 1
                    L = L + 1
                    R = R - 1
                elif (nums[i] + nums[L] + nums[R] > target):
                    R = R - 1
                else:
                    L = L + 1
        return res

    def fourSum(self, nums: [int], target: int) -> [[int]]:
        n = len(nums)
        if n < 4: return []
        nums.sort()
        res = []
        last_head = nums[0]-1
        for i in range(n):
            if (nums[i] > target>>2):
                return res
            if nums[i]==last_head: continue
            temp = self.threeSum( nums[i + 1:], target - nums[i])
            last_head = nums[i]
            for j in temp:
                res.append([nums[i]] + j)
        return res
```

> 执行用时 :720 ms, 在所有 Python3 提交中击败了54.15% 的用户
>
> 内存消耗 :13.5 MB, 在所有 Python3 提交中击败了21.35%的用户

看了别的解法：

1. 依旧使用双指针，然后两重循环，比上面可能简单一些。
2. 还有就是用python的库函数，有点吊后面可以研究一下。

## [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**说明：**

给定的 *n* 保证是有效的。

**进阶：**

你能尝试使用一趟扫描实现吗？

思路：

扫描一趟然后记录到数组里，再操作

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        nodes = []
        while head:
            nodes += [head]
            head = head.next
        # todo remove the head node
        l = len(nodes)
        if n == l:
            if l >1:return nodes[1]
            else:return None
        if n==1:
            nodes[-2].next = None
        else:
            nodes[-n-1].next = nodes[-n+1]
        return nodes[0]
```

> 执行用时 :40 ms, 在所有 Python3 提交中击败了47.91% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了7.60%的用户

看了题解，其实用两个相差n的指针向前移动就好了。

  ![Remove the nth element from a list](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/4e134986ba59f69042b2769b84e3f2682f6745033af7bcabcab42922a58091ba-file_1555694482088.png)

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        if not head.next: return None
        n1 = head
        n2 = head
        for i in range(n + 1):
            if not n2: return head.next
            n2 = n2.next
        while n2:
            n1 = n1.next
            n2 = n2.next
        n1.next = n1.next.next
        return head

```

> 执行用时 :56 ms, 在所有 Python3 提交中击败了11.55% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了7.45%的用户

不知道为啥这么慢，python链表就这样吧，思路清晰就好。看了一个据说超%99的题解，我自己跑超32.09%，这玩意应该就是看点。

## [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

**示例 1:**

```
输入: "()"
输出: true
```

**示例 2:**

```
输入: "()[]{}"
输出: true
```

**示例 3:**

```
输入: "(]"
输出: false
```

**示例 4:**

```
输入: "([)]"
输出: false
```

**示例 5:**

```
输入: "{[]}"
输出: true
```
据说是栈的思路，反正我也这么想的
```python
class Solution:
    def isValid(self, s: str) -> bool:
        dict = {'(': ')', '[': ']', '{': '}'}
        temp = []
        for i in s:
            if i in dict:
                temp = [i]+temp
            else:
                if not temp:return False
                if i == dict[temp[0]]:
                    temp = temp[1:]
                else:return False
        return not temp
```

出来贼慢，可能没用pop吧

官方题解，充分利用pop以及pop返回值，快不少

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """

        # The stack to keep track of opening brackets.
        stack = []

        # Hash map for keeping track of mappings. This keeps the code very clean.
        # Also makes adding more types of parenthesis easier
        mapping = {")": "(", "}": "{", "]": "["}

        # For every bracket in the expression.
        for char in s:

            # If the character is an closing bracket
            if char in mapping:

                # Pop the topmost element from the stack, if it is non empty
                # Otherwise assign a dummy value of '#' to the top_element variable
                top_element = stack.pop() if stack else '#'

                # The mapping for the opening bracket in our hash and the top
                # element of the stack don't match, return False
                if mapping[char] != top_element:
                    return False
            else:
                # We have an opening bracket, simply push it onto the stack.
                stack.append(char)

        # In the end, if the stack is empty, then we have a valid expression.
        # The stack won't be empty for cases like ((()
        return not stack

```

## [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

数字 *n* 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

 

**示例：**

```
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```

算暴力法吧。

```python
class Solution:
    def generateParenthesis(self, n: int) -> [str]:
        def flag(j):
            f = 0
            c=0
            for i in j:
                if i == '(':
                    f += 1
                    c += 1
                else:
                    f -= 1
            return f,c

        def recurtion(k, n):
            if n == 0: return []
            if k == 1: return ['(']
            arr = []
            if k == n * 2:
                for i in recurtion(k - 1, n):
                    arr.append(i + ')')
                return arr
            for i in recurtion(k - 1, n):
                f,c = flag(i)
                if c < n:
                    arr.append(i + '(')
                if f > 0:
                    arr.append(i + ")")
            return arr

        return recurtion(n * 2, n)
```

> 执行用时 :40 ms, 在所有 Python3 提交中击败了66.09% 的用户
>
> 内存消耗 :13.9 MB, 在所有 Python3 提交中击败了5.03%的用户

学习一下官方思路：

任何一个括号序列都一定是由 ( 开头，并且第一个 ( 一定有一个唯一与之对应的 )。这样一来，每一个括号序列可以用 (a)b 来表示，其中 a 与 b 分别是一个合法的括号序列（可以为空）。

那么，要生成所有长度为 2 * n 的括号序列，我们定义一个函数 generate(n) 来返回所有可能的括号序列。那么在函数 generate(n) 的过程中：

    我们需要枚举与第一个 ( 对应的 ) 的位置 2 * i + 1；
    递归调用 generate(i) 即可计算 a 的所有可能性；
    递归调用 generate(n - i - 1) 即可计算 b 的所有可能性；
    遍历 a 与 b 的所有可能性并拼接，即可得到所有长度为 2 * n 的括号序列。

为了节省计算时间，我们在每次 generate(i) 函数返回之前，把返回值存储起来，下次再调用 generate(i) 时可以直接返回，不需要再递归计算。

```python
class Solution:
    @lru_cache(None)
    def generateParenthesis(self, n: int) -> List[str]:
        if n == 0:
            return ['']
        ans = []
        for c in range(n):
            for left in self.generateParenthesis(c):
                for right in self.generateParenthesis(n-1-c):
                    ans.append('({}){}'.format(left, right))
        return ans
```



## [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

给定 *n* 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![img](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/rainwatertrap.png)

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 **感谢 Marcos** 贡献此图。

**示例:**

```
输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6
```

思路：

双指针，从两边向中间:

看到1，向内走，看到比1小的就加上1和它的差，看到比1大的就更新1为2，直到头尾指针重合。

```python
class Solution:
    def trap(self, height: [int]) -> int:
        if not height:return 0
        L, R = 0, len(height) - 1
        h = 0
        rst = 0
        while L<=R:
            if height[L] > h and height[R] > h:
                h = min(height[L], height[R])
            if height[L] <= h:
                rst += h - height[L]
                L += 1
            if L>R:break
            if height[R] <= h:
                rst += h - height[R]
                R -= 1
        return rst
```

> 执行用时 :48 ms, 在所有 Python3 提交中击败了81.04% 的用户
>
> 内存消耗 :14.1 MB, 在所有 Python3 提交中击败了5.04%的用户

看题解：



## [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```

**示例 2:**

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

思路：

记录：

1. 前几天最大利润
2. 前几天最低价

```python
class Solution:
    def maxProfit(self, prices: [int]) -> int:
        if not prices: return 0
        low = prices[0]
        mP = 0
        for i in prices:
            mT = i - low
            if mT > mP: mP = mT
            if i < low: low = i
        return mP
```

> 执行用时 :36 ms, 在所有 Python3 提交中击败了98.43% 的用户
>
> 内存消耗 :14.3 MB, 在所有 Python3 提交中击败了18.08%的用户

高兴，先不看题解。

## [151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

给定一个字符串，逐个翻转字符串中的每个单词。

 

**示例 1：**

```
输入: "the sky is blue"
输出: "blue is sky the"
```

**示例 2：**

```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```

**示例 3：**

```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

 

**说明：**

- 无空格字符构成一个单词。
- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

 

**进阶：**

请选用 C 语言的用户尝试使用 *O*(1) 额外空间复杂度的原地解法。

**思路：**

遍历到一个单词结束，记录头尾，移动到最前加空格；

过程中看到空格，删除紧挨着的空格；

最后尾部有空格，删除。

```c++
class Solution {
public:
    std::string reverseWords(std::string &s) {
        int head, lw;
        bool space = true, first = true;
        int l = s.length();
        int i = 0;
        while (i < s.length()) {
            if (s[i] == ' ') {
                if (!space) { space = true; }
                s.erase(i, 1);
            } else {
                if (space) { head = i; }
                if (i + 1 == s.length() or s[i + 1] == ' ') {
                    lw = i - head + 1;
                    //todo 挪到最前
                    if (first) {
                        s = (s.substr(head, lw) + s).erase(head + lw, lw);
                        first = false;
                    } else {
                        s = (s.substr(head, lw) + " " + s).erase(head + lw + 1, lw);
                        i++;
                    }
                }
                space = false;
                i++;
            }
        }
        return s;
    }
};
```

> 执行用时 :68 ms, 在所有 C++ 提交中击败了5.25% 的用户
>
> 内存消耗 :115.6 MB, 在所有 C++ 提交中击败了23.61%的用户

菜。。

看题解：

 ![fig](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/mutable2.png)

```c++
std::string reverseWords(std::string s) {
        // 反转整个字符串
        reverse(s.begin(), s.end()); //需要include "algorithm"

        int n = s.size();
        int idx = 0;// 表示更新后的单词尾部，最后以此结尾删除多余空格
        for (int start = 0; start < n; ++start) {
            if (s[start] != ' ') {
                // 填一个空白字符然后将idx移动到下一个单词的开头位置
                if (idx != 0) s[idx++] = ' ';

                // 循环遍历至单词的末尾
                int end = start;
                while (end < n && s[end] != ' ') s[idx++] = s[end++];//s[idx++] = s[end++]是为了填之前多余的空格

                // 反转整个单词
                reverse(s.begin() + idx - (end - start), s.begin() + idx);

                // 更新start，去找下一个单词
                start = end;
            }
        }
        s.erase(s.begin() + idx, s.end());
        return s;
    }
```





## [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

给定一个大小为 *n* 的数组，找到其中的多数元素。多数元素是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1:**

```
输入: [3,2,3]
输出: 3
```

**示例 2:**

```
输入: [2,2,1,1,1,2,2]
输出: 2
```

思路：建立hash表，满足条件就输出

```python
class Solution:
    def majorityElement(self, nums: [int]) -> int:
        hash = {}
        n2 = len(nums)//2
        for i in nums:
            if i in hash:
                hash[i] += 1
            else:
                hash[i] = 1
            if hash[i] == n2+1:
                return i
```

> 执行用时 :92 ms, 在所有 Python3 提交中击败了78.21% 的用户
>
> 内存消耗 :15.2 MB, 在所有 Python3 提交中击败了5.04%的用户

看题解：

1.数组排序，中间肯定是题解，牛逼

```python
class Solution:
    def majorityElement(self, nums: [int]) -> int:
        nums.sort()
        return nums[len(nums)//2]
```

> 执行用时 :52 ms, 在所有 Python3 提交中击败了90.33% 的用户
>
> 内存消耗 :15.1 MB, 在所有 Python3 提交中击败了5.04%的用户



### **摩尔投票法**

众数出现的次数>其他数字出现次数之和

    初始化res=0，count=0
    
    遍历数组：
    若count==0，则将res更新为nums[i]，并令count=1
    否则：
    若nums[i]==res，令count+=1
    若nums[i]!=res，令count−=1
    
    若最终count>0，则返回res

复杂度分析

时间复杂度：O(n)
**空间复杂度：O(1)**

自己写一遍

```python
class Solution:
    def majorityElement(self, nums: [int]) -> int:
        count = 0
        for i in nums:
            if not count:
                res = i
            if i == res:
                count += 1
            else:
                count -= 1
        return res
```

> 执行用时 :44 ms, 在所有 Python3 提交中击败了95.58% 的用户
>
> 内存消耗 :15.2 MB, 在所有 Python3 提交中击败了5.04%的用户

## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

反转一个单链表。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**进阶:**
 你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        ## plan1: iteration
        rst = None
        while head:
            temp=head.next
            head.next = rst
            rst = head
            head = temp
        return rst

        ## plan2: recursion
        def recursion(node,rst):
            if not node:
                return rst
            else:
                temp = node.next
                node.next = rst
                rst = node
                return recursion(temp,rst)
        return recursion(head,None)
        pass
```

> 执行用时 :44 ms, 在所有 Python3 提交中击败了49.58% 的用户
>
> 内存消耗 :19.4 MB, 在所有 Python3 提交中击败了5.00%的用户

看题解，对递归还是不敏感。

## [225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

使用队列实现栈的下列操作：

- push(x) -- 元素 x 入栈
- pop() -- 移除栈顶元素
- top() -- 获取栈顶元素
- empty() -- 返回栈是否为空

**注意:**

- 你只能使用队列的基本操作-- 也就是 `push to back`, `peek/pop from front`, `size`, 和 `is empty` 这些操作是合法的。
- 你所使用的语言也许不支持队列。 你可以使用 list 或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
- 你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 pop 或者 top 操作）。

```python
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.data = []


    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        self.data.append(x)


    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        return self.data.pop()


    def top(self) -> int:
        """
        Get the top element.
        """
        return self.data[-1]


    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return self.data ==[]


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```

> 执行用时 :40 ms, 在所有 Python3 提交中击败了42.68% 的用户
>
> 内存消耗 :13.4 MB, 在所有 Python3 提交中击败了5.13%的用户

栈结构不熟，多做题去熟悉他。

## [289. 生命游戏](https://leetcode-cn.com/problems/game-of-life/)

根据 [百度百科](https://baike.baidu.com/item/生命游戏/2926434?fr=aladdin) ，生命游戏，简称为生命，是英国数学家约翰·何顿·康威在 1970 年发明的细胞自动机。

给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态：1 即为活细胞（live），或 0 即为死细胞（dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

1. 如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
2. 如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
3. 如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
4. 如果死细胞周围正好有三个活细胞，则该位置死细胞复活；

根据当前状态，写一个函数来计算面板上所有细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。

 

**示例：**

```
输入： 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
输出：
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

 

**进阶：**

- 你可以使用原地算法解决本题吗？请注意，面板上所有格子需要同时被更新：你不能先更新某些格子，然后使用它们的更新后的值再更新其他格子。
- 本题中，我们使用二维数组来表示面板。原则上，面板是无限的，但当活细胞侵占了面板边界时会造成问题。你将如何解决这些问题？

思路，存储上一行和上一个

```python
class Solution:
    def gameOfLife(self, board: [[int]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        if not board: return
        m = len(board)
        n = len(board[0])
        calist = []
        for i in range(3):
            for j in range(3):
                if i == 1 and j == 1: continue
                calist.append([-1 + j, -1 + i])
        # print(calist)  # [[-1, -1], [0, -1], [1, -1], [-1, 0], [1, 0], [-1, 1], [0, 1], [1, 1]]
        temp_raw = [0] * n
        temp = 0
        c = 0
        for y in range(m):
            temp_raw2 = board[y].copy()
            for x in range(n):
                life_count = 0
                for i, j in calist:
                    a, b = x + i, y + j
                    if 0 <= a < n and 0 <= b < m:
                        if j == -1:
                            life_count += temp_raw[a]
                        elif j == 0 and i == -1:
                            life_count += temp
                        else:
                            life_count += board[b][a]
                temp = board[y][x]
                if board[y][x]:
                    if life_count < 2 or life_count > 3: board[y][x] = 0
                else:
                    if life_count == 3: board[y][x] = 1
            temp_raw = temp_raw2
```

> 执行用时 :40 ms, 在所有 Python3 提交中击败了60.31% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了10.53%的用户

题解是用两次遍历，在python下，没有省多少空间。。。。

## [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

给定一个无序的整数数组，找到其中最长上升子序列的长度。

**示例:**

```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

**说明:**

- 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
- 你算法的时间复杂度应该为 O(*n2*) 。

**进阶:** 你能将算法的时间复杂度降低到 O(*n* log *n*) 吗?



看这个指定是用到二分了，然而，，我没想到咋用

可以先排序，然后找逆序的，逆序的指定不行，排除之后剩下最多的那个就对了吧



思路：

O(n2)的话，那就是遍历再遍历，建一个表，存每一个数他后面比他大的数

我傻了，居然用递归遍历了一个

看题解有两种：

### 1.**动态规划**

自己理解就是找递推关系，随着遍历进行变化，使一次遍历可以解决整个问题。

dp表示前i个数中的最长上升子序长度

![动态规划300](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/动态规划300.gif)

### 2.**动态+贪心+二分法**

整个算法流程为：
设当前已求出的最长上升子序列的长度为len（初始时为1），从前往后遍历数组nums，在遍历到nums[i]时：

- ​	如果nums[i]>d[len]，则直接加入到d数组末尾，并更新len=len+1；
- ​	否则，在d数组中二分查找，找到第一个比nums[i]小的数d[k]，并更新d[k+1]=nums[i]。

以输入序列 [0,8,4,12,2] 为例：

    第一步插入 0，d=[0]；
    
    第二步插入 8，d=[0,8]；
    
    第三步插入 4，d=[0,4]；
    
    第四步插入 12，d=[0,4,12]；
    
    第五步插入 2，d=[0,2,12]。

最终得到最大递增子序列长度为 3。



## [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

**示例 1:**

```
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```

**示例 2:**

```
输入: coins = [2], amount = 3
输出: -1
```

**说明**:
 你可以认为每种硬币的数量是无限的。

思路：

贪心放大值硬币，同时记录所有可以正好匹配的情况，更新硬币数最小值

使用递归，当子函数中，整除最大值都比记录的最小值小的时候及时停止，节约时间。

```python
class Solution:
    def coinChange(self, coins: [int], amount: int) -> int:
        if not coins:return -1
        coins.sort()
        print(coins[::-1],amount)
        def recursion(coins: [int], rest: int, nums: [int],least):
            """
            :param num: the sum of num of coins used before
            :return: the sum of num of coins
            """
            trigger = False
            if not coins:
                # print('bad',end='')
                # print(nums)
                return -1
            n = rest // coins[0]
            if sum(nums)+n>least:
                return -1
            if len(coins) == 1:
                if rest % coins[0]:
                    # print('bad', end='')
                    # print(nums)
                    return -1
                else:
                    nums+=[n]
                    print(nums,sum(nums))
                    return sum(nums)
            else:

                next = rest % coins[0]
                for i in range(n + 1):
                    rst = recursion(coins[1:], next + coins[0] * i, nums + [n - i],least)
                    if rst != -1 and rst<least:
                        trigger = True
                        least = rst
            # return least
            if trigger:return least
            return -1
        return recursion(coins[::-1], amount, [],amount//coins[0]+1)
```

> 执行用时 :932 ms, 在所有 Python3 提交中击败了92.39% 的用户
>
> 内存消耗 :13.4 MB, 在所有 Python3 提交中击败了25.16%的用户

牛逼就完了，不看题解先，膨胀。

## [365. 水壶问题](https://leetcode-cn.com/problems/water-and-jug-problem/)

有两个容量分别为 *x*升 和 *y*升 的水壶以及无限多的水。请判断能否通过使用这两个水壶，从而可以得到恰好 *z*升 的水？

如果可以，最后请用以上水壶中的一或两个来盛放取得的 *z升* 水。

你允许：

- 装满任意一个水壶
- 清空任意一个水壶
- 从一个水壶向另外一个水壶倒水，直到装满或者倒空

**示例 1:** (From the famous [*"Die Hard"* example](https://www.youtube.com/watch?v=BVtQNK_ZUJg))

```
输入: x = 3, y = 5, z = 4
输出: True
```

**示例 2:**

```
输入: x = 2, y = 6, z = 5
输出: False
```
思路：转化成求最大公约数问题，然后辗转相除
列举各种情况就能推出来了。

```python
def Toss_Divide(a, b):
    if a < b: a, b = b, a
    while b > 0:
        a, b = b, a % b
    return a


class Solution:
    def canMeasureWater(self, x: int, y: int, z: int) -> bool:
        if not x or not y: return z == x or z == y
        if z < 0 or z > x + y: return False
        c = Toss_Divide(x, y)
        return not (z % c)
```

> 执行用时 :32 ms, 在所有 Python3 提交中击败了81.73% 的用户
>
> 内存消耗 :13.5 MB, 在所有 Python3 提交中击败了14.58%的用户

看题解：math有库求最大公约数，没快多少，但是干净。

一开始想枚举，但是这玩意你去模拟有无数种盛水的方法，所以题解给了用搜索的方法，搜到就停下。



## [409. 最长回文串](https://leetcode-cn.com/problems/longest-palindrome/)

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 `"Aa"` 不能当做一个回文字符串。

**注意:**
 假设字符串的长度不会超过 1010。

**示例 1:** 

```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```

思路：遍历并存入hash，当出现次数为偶数，rst+=2，如果出现的有奇数，就返回rst+1

或者记录总共出现的奇数次数，这估计怎么都要两次遍历？

```python
class Solution:
    def longestPalindrome(self, s: str) -> int:
        single = 0
        hash = {}
        for i in s:
            if i in hash:
                hash[i] += 1
            else:
                hash[i] = 1
        for i in hash:
            if hash[i] & 1:
                single += 1
        if single:
            return len(s) - single + 1
        else:
            return len(s)
```

>执行用时 :36 ms, 在所有 Python3 提交中击败了70.67% 的用户
>
>内存消耗 :13.5 MB, 在所有 Python3 提交中击败了5.32%的用户

题解思路和我一样。。。

## [460. LFU缓存](https://leetcode-cn.com/problems/lfu-cache/)

设计并实现[最不经常使用（LFU）](https://baike.baidu.com/item/缓存算法)缓存的数据结构。它应该支持以下操作：`get` 和 `put`。

`get(key)` - 如果键存在于缓存中，则获取键的值（总是正数），否则返回 -1。
 `put(key, value)` - 如果键不存在，请设置或插入值。当缓存达到其容量时，它应该在插入新项目之前，使最不经常使用的项目无效。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，**最近**最少使用的键将被去除。

一个项目的使用次数就是该项目被插入后对其调用 get 和 put 函数的次数之和。使用次数会在对应项目被移除后置为 0。

**进阶：**
 你是否可以在 **O(1)** 时间复杂度内执行两项操作？

**示例：**

```
LFUCache cache = new LFUCache( 2 /* capacity (缓存容量) */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回 1
cache.put(3, 3);    // 去除 key 2
cache.get(2);       // 返回 -1 (未找到key 2)
cache.get(3);       // 返回 3
cache.put(4, 4);    // 去除 key 1
cache.get(1);       // 返回 -1 (未找到 key 1)
cache.get(3);       // 返回 3
cache.get(4);       // 返回 4
```

思路：

双向链表



# 题目改完心态崩了，我是个弟弟，没做出来，抄题解拿积分。





## [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

**示例 :**
 给定二叉树

```
          1
         / \
        2   3
       / \     
      4   5    
```

返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

**注意：**两结点之间的路径长度是以它们之间边的数目表示。

思路：就是遍历，然后取左右两个作为root最大深度的和，再取最大值。

学习官方题解

```python
class Solution(object):
    def diameterOfBinaryTree(self, root):
        self.ans = 0
        def depth(node):
            # 访问到空节点了，返回0
            if not node: return 0
            # 左儿子为根的子树的深度
            L = depth(node.left)
            # 右儿子为根的子树的深度
            R = depth(node.right)
            # 计算d_node即L+R+1 并更新ans
            self.ans = max(self.ans, L+R)
            # 返回该节点为根的子树的深度
            return max(L, R)+1

        depth(root)
        return self.ans
```

同时了解了一个python的二叉树的库 [binarytree · PyPI](https://pypi.org/project/binarytree/)  

### **反思**：

递归从简单程序开始写起，比如本题应该就改自求最大深度的简单程序。

## [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

给定一个包含了一些 0 和 1的非空二维数组 `grid` , 一个 **岛屿** 是由四个方向 (水平或垂直) 的 `1` (代表土地) 构成的组合。你可以假设二维矩阵的四个边缘都被水包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为0。)

**示例 1:**

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

对于上面这个给定矩阵应返回 `6`。注意答案不应该是11，因为岛屿只能包含水平或垂直的四个方向的‘1’。

**示例 2:**

```
[[0,0,0,0,0,0,0,0]]
```

对于上面这个给定的矩阵, 返回 `0`。

**注意:** 给定的矩阵`grid` 的长度和宽度都不超过 50。

思路：暴力求所有岛，递归求单个岛面积
```python
class Solution:
    def maxAreaOfIsland(self, grid: [[int]]) -> int:
        # todo： 四周补零
        for i in range(len(grid)):
            grid[i] = [0] + grid[i] + [0]
        lx = len(grid[0])
        grid = [[0] * lx] + grid + [[0] * lx]
        ly = len(grid)
        # todo： 编写递归
        LEFT = 4
        RIGHT = 6
        UP = 8
        DOWN = 2
        # printGrid(grid)
        def recursion(x, y, area, drct):
            area += 1
            grid[y][x] = 0
            if drct != LEFT and grid[y][x - 1]:     area += recursion(x - 1, y, 0, RIGHT)
            if drct != RIGHT and grid[y][x + 1]:    area += recursion(x + 1, y, 0, LEFT)
            if drct != UP and grid[y - 1][x]:       area += recursion(x, y - 1, 0, DOWN)
            if drct != DOWN and grid[y + 1][x]:     area += recursion(x, y + 1, 0, UP)
            return area
        max_area = 0
        for y in range(1, ly - 1):
            for x in range(1, lx - 1):
                if grid[y][x]:
                    temp = recursion(x, y, 0, LEFT)
                    # printGrid(grid)
                    # print(temp)
                    if temp > max_area: max_area = temp
        # print('the answer is: ',max_area)
        return max_area
```
> 执行用时 :204 ms, 在所有 Python3 提交中击败了28.82% 的用户
>
> 内存消耗 :16.4 MB, 在所有 Python3 提交中击败了7.64%的用户

学习了栈的方法，可以去除递归

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        ans = 0
        for i, l in enumerate(grid):
            for j, n in enumerate(l):
                cur = 0
                stack = [(i, j)]
                while stack:
                    cur_i, cur_j = stack.pop()
                    if cur_i < 0 or cur_j < 0 or cur_i == len(grid) or cur_j == len(grid[0]) or grid[cur_i][cur_j] != 1:
                        continue
                    cur += 1
                    grid[cur_i][cur_j] = 0
                    for di, dj in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
                        next_i, next_j = cur_i + di, cur_j + dj
                        stack.append((next_i, next_j))
                ans = max(ans, cur)
        return ans
```

这个看懂了，而且有很多值得学习的地方。

## [820. 单词的压缩编码](https://leetcode-cn.com/problems/short-encoding-of-words/)

给定一个单词列表，我们将这个列表编码成一个索引字符串 `S` 与一个索引列表 `A`。

例如，如果这个列表是 `["time", "me", "bell"]`，我们就可以将其表示为 `S = "time#bell#"` 和 `indexes = [0, 2, 5]`。

对于每一个索引，我们可以通过从字符串 `S` 中索引的位置开始读取字符串，直到 "#" 结束，来恢复我们之前的单词列表。

那么成功对给定单词列表进行编码的最小字符串长度是多少呢？

 

**示例：**

```
输入: words = ["time", "me", "bell"]
输出: 10
说明: S = "time#bell#" ， indexes = [0, 2, 5] 。
```

 

**提示：**

1. `1 <= words.length <= 2000`
2. `1 <= words[i].length <= 7`
3. 每个单词都是小写字母 。



思路1：

1. 或者定义一个”尾包含“

2. 遍历整个列表，如果被包含，就不加rst；如果包含，rst+=本词条长度-被包含词条长度

```python
class Solution:
    def minimumLengthEncoding(self, words: [str]) -> int:
        def backCover(w_short, w_long) -> int:
            """
            :param w_short:
            :param w_long:
            :return: if cover:l_long-l_short;else:-1
            """
            l = len(w_short)
            for i in range(l):
                if w_short[-1 - i] != w_long[-1 - i]:
                    return -1
            return len(w_long) - l

        rst, codes = 0, []
        for i in words:
            append = True
            if not codes:
                rst += len(i)
                codes.append(i)
                continue
            for id, j in enumerate(codes):
                if len(i) <= len(j):  # 判断是否被包含
                    if backCover(i, j) != -1: append = False
                else:  # 判断是否包含
                    if backCover(j, i) != -1:
                        codes[id] = i
                        rst += len(i) - len(j)
                        append = False
            if append:
                codes.append(i)
                rst+=len(i)
        return rst+len(codes)
```

超时了。。

思路2：

```python
class Solution:
    def minimumLengthEncoding(self, words: [str]) -> int:
        code = ''
        for i in range(7):
            for j in words:
                if len(j) == 7 - i and j+'#' not in code:
                    code = code + j + '#'
        return len(code)
```

> 执行用时 :336 ms, 在所有 Python3 提交中击败了21.74% 的用户
>
> 内存消耗 :14 MB, 在所有 Python3 提交中击败了8.33%的用户

题解：字典树（前缀树）

python实现用字典套字典

## [836. 矩形重叠](https://leetcode-cn.com/problems/rectangle-overlap/)

矩形以列表 `[x1, y1, x2, y2]` 的形式表示，其中 `(x1, y1)` 为左下角的坐标，`(x2, y2)` 是右上角的坐标。

如果相交的面积为正，则称两矩形重叠。需要明确的是，只在角或边接触的两个矩形不构成重叠。

给出两个矩形，判断它们是否重叠并返回结果。

 

**示例 1：**

```
输入：rec1 = [0,0,2,2], rec2 = [1,1,3,3]
输出：true
```

**示例 2：**

```
输入：rec1 = [0,0,1,1], rec2 = [1,0,2,1]
输出：false
```

 

**提示：**

1. 两个矩形 `rec1` 和 `rec2` 都以含有四个整数的列表的形式给出。
2. 矩形中的所有坐标都处于 `-10^9` 和 `10^9` 之间。
3. `x` 轴默认指向右，`y` 轴默认指向上。
4. 你可以仅考虑矩形是正放的情况。

思路：

1. 判断xy方向中心距小于长宽一半
2. 判断一矩形一边是否在另一矩形的两边之间

```python
class Solution:
    def isRectangleOverlap(self, rec1: [int], rec2: [int]) -> bool:
        # method 1
        dx2 = pow(rec1[0] + rec1[2] - rec2[0] - rec2[2], 2)
        dy2 = pow(rec1[1] + rec1[3] - rec2[1] - rec2[3], 2)
        addx2 = pow(rec1[2] - rec1[0] + rec2[2] - rec2[0], 2)
        addy2 = pow(rec1[3] - rec1[1] + rec2[3] - rec2[1], 2)
        return dx2<addx2 and dy2<addy2
		# method 2
        return (rec1[0] <= rec2[0] < rec1[2] or
                rec2[0] <= rec1[0] < rec2[2]) and \
               (rec1[1] <= rec2[1] < rec1[3] or
                rec2[1] <= rec1[1] < rec2[3])
		# method 2
        x11, y11, x12, y12 = rec1
        x21, y21, x22, y22 = rec2
        return (x11 <= x21 < x12 or x21 <= x11 < x22 ) and (
                    y11 <= y21 < y12 or y21 <= y11 < y22 )
```

内存稳定很多，用时不稳定最快过70%

看题解：

```python
class Solution:
    def isRectangleOverlap(self, rec1: [int], rec2: [int]) -> bool:
        return not (rec1[0] >= rec2[2] or rec2[0] >= rec1[2] or rec1[1] >= rec2[3] or rec2[1] >= rec1[3])
```

### **反证法**很六

可惜时间空间都没降下来

## [876. 链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

给定一个带有头结点 `head` 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

 

**示例 1：**

```
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
```

**示例 2：**

```
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。
```

 

**提示：**

- 给定链表的结点数介于 `1` 和 `100` 之间。

思路：遍历同时更新前几个节点的中间节点。

```python
class Solution:
    def middleNode(self, head: ListNode) -> ListNode:
        mid = head
        count = 1
        while head:
            if not (count & 1):
                mid = mid.next
            head = head.next
            count += 1
        return mid
```

> 执行用时 :32 ms, 在所有 Python3 提交中击败了74.96% 的用户
>
> 内存消耗 :13.5 MB, 在所有 Python3 提交中击败了5.55%的用户

题解：快慢指针法，思路相同但是优雅很多。

```python
class Solution:
    def middleNode(self, head: ListNode) -> ListNode:
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

> 执行用时 :28 ms, 在所有 Python3 提交中击败了90.57% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了5.55%的用户

## [887. 鸡蛋掉落](https://leetcode-cn.com/problems/super-egg-drop/)

你将获得 `K` 个鸡蛋，并可以使用一栋从 `1` 到 `N` 共有 `N` 层楼的建筑。

每个蛋的功能都是一样的，如果一个蛋碎了，你就不能再把它掉下去。

你知道存在楼层 `F` ，满足 `0 <= F <= N` 任何从高于 `F` 的楼层落下的鸡蛋都会碎，从 `F` 楼层或比它低的楼层落下的鸡蛋都不会破。

每次*移动*，你可以取一个鸡蛋（如果你有完整的鸡蛋）并把它从任一楼层 `X` 扔下（满足 `1 <= X <= N`）。

你的目标是**确切地**知道 `F` 的值是多少。

无论 `F` 的初始值如何，你确定 `F` 的值的最小移动次数是多少？

**示例 1：**

```
输入：K = 1, N = 2
输出：2
解释：
鸡蛋从 1 楼掉落。如果它碎了，我们肯定知道 F = 0 。
否则，鸡蛋从 2 楼掉落。如果它碎了，我们肯定知道 F = 1 。
如果它没碎，那么我们肯定知道 F = 2 。
因此，在最坏的情况下我们需要移动 2 次以确定 F 是多少。
```

**示例 2：**

```
输入：K = 2, N = 6
输出：3
```

**示例 3：**

```
输入：K = 3, N = 14
输出：4
```

 

**提示：**

1. `1 <= K <= 100`
2. `1 <= N <= 10000`

思路：

观察发现当答案为x时，k情况下最多的楼层N为：
$$
N_k(x)=\begin{cases}
x+\sum_1^{x-1}{N_{k-1}(i)}&k>1\\
x&k=1
\end{cases}
$$
有递推关系:
$$
N_k(x)=N_K(x-1)+1+N_{k-1}(x-1)
$$
利用这些只要找到：
$$
N_K(x)\leqslant n \leqslant N_K(x-1)
$$
x即为结果。

下面需要减少迭代次数，蛋足够的情况，n最小值为$log_2(n)$。

```c++
class Solution {
private:
    int max_N(int k, int x) {
        if (k == 1) return x;
        int temp = x;
        for (int i = 1; i < x; i++) temp += max_N(k - 1, i);
        return temp;
    }

public:
    int superEggDrop(int K, int N) {
        int x = log(double(N)) / log(2);
        while (N > max_N(K, x)) x++;
        return x;
    }
    }
};
```

> 双百，我太牛逼了。

## [892. 三维形体的表面积](https://leetcode-cn.com/problems/surface-area-of-3d-shapes/)

在 `N * N` 的网格上，我们放置一些 `1 * 1 * 1 ` 的立方体。

每个值 `v = grid[i][j]` 表示 `v` 个正方体叠放在对应单元格 `(i, j)` 上。

请你返回最终形体的表面积。

**示例 1：**

```
输入：[[2]]
输出：10
```

**示例 2：**

```
输入：[[1,2],[3,4]]
输出：34
```

**示例 3：**

```
输入：[[1,0],[0,2]]
输出：16
```

**示例 4：**

```
输入：[[1,1,1],[1,0,1],[1,1,1]]
输出：32
```

**示例 5：**

```
输入：[[2,2,2],[2,1,2],[2,2,2]]
输出：46
```

 

**提示：**

- `1 <= N <= 50`
- `0 <= grid[i][j] <= 50`

思路：

先周围补零，然后遍历每个立方体，立方体表面积，减去周围被遮挡的部分

```python
class Solution:
    def surfaceArea(self, grid: [[int]]) -> int:
        if not grid: return 0
        col = len(grid[0])
        for i in range(col):
            grid[i] = [0] + grid[i] + [0]
        raw = len(grid)
        l = raw + 2
        grid = [[0] * l] + grid + [[0] * l]
        print(grid)
        rst = 0
        for x in range(1, col + 1):
            for y in range(1, 1 + raw):
                h = grid[x][y]
                u, d, l, r = grid[x - 1][y], grid[x + 1][y], grid[x][y - 1], grid[x][y + 1]
                if h:s = 4 * h + 2
                else:s = 0
                for i in [u, d, l, r]:
                    if i < h:
                        s -= i
                    else:
                        s -= h
                rst += s
        return rst
```

> 执行用时 :96 ms, 在所有 Python3 提交中击败了87.16% 的用户
>
> 内存消耗 :13.8 MB, 在所有 Python3 提交中击败了5.77%的用户

本以为时间炸了，结果还挺快。

题解也就那么回事，没啥牛逼算法。

## [914. 卡牌分组](https://leetcode-cn.com/problems/x-of-a-kind-in-a-deck-of-cards/)

给定一副牌，每张牌上都写着一个整数。

此时，你需要选定一个数字 `X`，使我们可以将整副牌按下述规则分成 1 组或更多组：

- 每组都有 `X` 张牌。
- 组内所有的牌上都写着相同的整数。

仅当你可选的 `X >= 2` 时返回 `true`。

 

**示例 1：**

```
输入：[1,2,3,4,4,3,2,1]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]
```

**示例 2：**

```
输入：[1,1,1,2,2,2,3,3]
输出：false
解释：没有满足要求的分组。
```

**示例 3：**

```
输入：[1]
输出：false
解释：没有满足要求的分组。
```

**示例 4：**

```
输入：[1,1]
输出：true
解释：可行的分组是 [1,1]
```

**示例 5：**

```
输入：[1,1,2,2,2,2]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[2,2]
```


 **提示：**

1. `1 <= deck.length <= 10000`
2. `0 <= deck[i] < 10000`

思路：

建立哈希表记录牌出现得次数，找最大公约数，若不小于2，就True

```python
import math


class Solution:
    def hasGroupsSizeX(self, deck: [int]) -> bool:
        hash = {}
        for i in deck:
            if i not in hash:
                hash[i] = 1
            else:
                hash[i] += 1
        x = 0
        for i in hash:
            if not x:
                x = hash[i]
                if x==1:return False
                continue
            x = math.gcd(x, hash[i])
            if x == 1:
                return False
        return True
```

> 执行用时 :56 ms, 在所有 Python3 提交中击败了83.26% 的用户
>
> 内存消耗 :13.9 MB, 在所有 Python3 提交中击败了5.38%的用户

看题解：还可以遍历X，牛逼

官方这题解给爷看傻了。

### **从官方题解学习python语法**

python3代码如下所示

```python
def hasGroupsSizeX(self, deck):
    from math import gcd
    from collections import Counter
    from functools import reduce
    vals = Counter(deck).values()
    return reduce(gcd, vals) >= 2
```

`math.gcd`求两个数的最大公约数，返回整数；

`collections.Counter`统计字符串（数字）种类及数量，返回字典；

`functools.reduce`逐次对上次函数结果与当前序列元素应用函数；

- `reduce(function, sequence)`
- 例如 reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) 计算为((((1+2)+3)+4)+5)

## [945. 使数组唯一的最小增量](https://leetcode-cn.com/problems/minimum-increment-to-make-array-unique/)

给定整数数组 A，每次 *move* 操作将会选择任意 `A[i]`，并将其递增 `1`。

返回使 `A` 中的每个值都是唯一的最少操作次数。

**示例 1:**

```
输入：[1,2,2]
输出：1
解释：经过一次 move 操作，数组将变为 [1, 2, 3]。
```

**示例 2:**

```
输入：[3,2,1,2,1,7]
输出：6
解释：经过 6 次 move 操作，数组将变为 [3, 4, 1, 2, 5, 7]。
可以看出 5 次或 5 次以下的 move 操作是不能让数组的每个值唯一的。
```

**提示：**

1. `0 <= A.length <= 40000`
2. `0 <= A[i] < 40000`

思路：

建立hash，如果有一样的就一直加到不一样。

超时了。。时间复杂度O(n2)。。。答案是多少我就加了几次

简化的方法应该是一次加不止1

一次遍历，遍历出以下结果：

1. 有重复的地方
2. 有空的地方

成功了。。

```python
class Solution:
    def minIncrementForUnique(self, A: [int]) -> int:
        if not A:return 0
        same = []
        places = []
        A.sort()
        temp = A[0] - 1
        for i in A:
            if i == temp:
                same.append(i)
            if i - temp > 1:
                places += [j for j in range(temp + 1, i)]
            temp = i
        l = len(same)
        places += [i for i in range(A[-1]+1,A[-1]+l+1)]
        n1, n2 = 0, 0
        count = 0
        while n1 < l:
            if same[n1] < places[n2]:
                count += places[n2] - same[n1]
                n1 += 1
                n2 += 1
            else:
                n2 += 1
        return count
```

> 执行用时 :388 ms, 在所有 Python3 提交中击败了71.59% 的用户
>
> 内存消耗 :18.9 MB, 在所有 Python3 提交中击败了15.38%的用户

题解基本和我一个思路，但是他

### **利用了给定范围**。

## [994. 腐烂的橘子](https://leetcode-cn.com/problems/rotting-oranges/)

在给定的网格中，每个单元格可以有以下三个值之一：

- 值 `0` 代表空单元格；
- 值 `1` 代表新鲜橘子；
- 值 `2` 代表腐烂的橘子。

每分钟，任何与腐烂的橘子（在 4 个正方向上）相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 `-1`。

 

**示例 1：**

**![img](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/oranges.png)

```
输入：[[2,1,1],[1,1,0],[0,1,1]]
输出：4
```

**示例 2：**

```
输入：[[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个正向上。
```

**示例 3：**

```
输入：[[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。
```

 

**提示：**

1. `1 <= grid.length <= 10`
2. `1 <= grid[0].length <= 10`
3. `grid[i][j]` 仅为 `0`、`1` 或 `2`

暴力法：

```python
class Solution:
    def orangesRotting(self, grid: [[int]]) -> int:
        if not grid: return -1
        lx = len(grid[0])
        ly = len(grid)
        count = 0
        while True:
            rot = []
            freshes = False
            for y in range(ly):
                for x in range(lx):
                    if not freshes:
                        if grid[y][x]==1:
                            freshes = True
                    if grid[y][x] == 2:
                        for dx, dy in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
                            tx, ty = x + dx, y + dy
                            if tx >= 0 and tx < lx and ty >= 0 and ty < ly and \
                                    grid[ty][tx]==1: rot.append([tx,ty])
            if not rot:
                if freshes:return -1
                else:return count

            for x,y in rot:
                grid[y][x]=2

            # printGrid(grid)
            count+=1
```

> 执行用时 :116 ms, 在所有 Python3 提交中击败了5.90% 的用户
>
> 内存消耗 :13.5 MB, 在所有 Python3 提交中击败了5.09%的用户

看了一下，其实这个思路算是BFS。

## [999. 车的可用捕获量](https://leetcode-cn.com/problems/available-captures-for-rook/)

在一个 8 x 8 的棋盘上，有一个白色车（rook）。也可能有空方块，白色的象（bishop）和黑色的卒（pawn）。它们分别以字符 “R”，“.”，“B” 和 “p” 给出。大写字符表示白棋，小写字符表示黑棋。

车按国际象棋中的规则移动：它选择四个基本方向中的一个（北，东，西和南），然后朝那个方向移动，直到它选择停止、到达棋盘的边缘或移动到同一方格来捕获该方格上颜色相反的卒。另外，车不能与其他友方（白色）象进入同一个方格。

返回车能够在一次移动中捕获到的卒的数量。


**示例 1：**

![img](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/1253_example_1_improved.PNG)

```
输入：[[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","R",".",".",".","p"],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]
输出：3
解释：
在本例中，车能够捕获所有的卒。
```

**示例 2：**

![img](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/1253_example_2_improved.PNG)

```
输入：[[".",".",".",".",".",".",".","."],[".","p","p","p","p","p",".","."],[".","p","p","B","p","p",".","."],[".","p","B","R","B","p",".","."],[".","p","p","B","p","p",".","."],[".","p","p","p","p","p",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]
输出：0
解释：
象阻止了车捕获任何卒。
```

**示例 3：**

![img](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/1253_example_3_improved.PNG)

```
输入：[[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","p",".",".",".","."],["p","p",".","R",".","p","B","."],[".",".",".",".",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."]]
输出：3
解释： 
车可以捕获位置 b5，d6 和 f5 的卒。
```

 

**提示：**

1. `board.length == board[i].length == 8`
2. `board[i][j]` 可以是 `'R'`，`'.'`，`'B'` 或 `'p'`
3. 只有一个格子上存在 `board[i][j] == 'R'`

思路：

就先找R，然后上下左右找就欧克了。

```python
class Solution:
    def numRookCaptures(self, board: [[str]]) -> int:
        for i in range(8):
            for j in range(8):
                if board[i][j] == 'R':
                    rx, ry = i, j
                    break
        rst = 0
        for a, b in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
            x, y = rx, ry
            while x >= 0 and x <= 7 and y >= 0 and y <= 7:
                if board[x][y] == 'B': break
                if board[x][y] == 'p':
                    rst += 1
                    break
                x += a
                y += b
        return rst
```

> 执行用时 :36 ms, 在所有 Python3 提交中击败了60.20% 的用户
>
> 内存消耗 :13.7 MB, 在所有 Python3 提交中击败了5.26%的用户

## [1111. 有效括号的嵌套深度](https://leetcode-cn.com/problems/maximum-nesting-depth-of-two-valid-parentheses-strings/)

**有效括号字符串** 仅由 `"("` 和 `")"` 构成，并符合下述几个条件之一：

- 空字符串
- 连接，可以记作 `AB`（`A` 与 `B` 连接），其中 `A` 和 `B` 都是有效括号字符串
- 嵌套，可以记作 `(A)`，其中 `A` 是有效括号字符串

类似地，我们可以定义任意有效括号字符串 `s` 的 **嵌套深度** `depth(S)`：

- `s` 为空时，`depth("") = 0`
- `s` 为 `A` 与 `B` 连接时，`depth(A + B) = max(depth(A), depth(B))`，其中 `A` 和 `B` 都是有效括号字符串
- `s` 为嵌套情况，`depth("(" + A + ")") = 1 + depth(A)`，其中 A 是有效括号字符串

例如：`""`，`"()()"`，和 `"()(()())"` 都是有效括号字符串，嵌套深度分别为 0，1，2，而 `")("` 和 `"(()"` 都不是有效括号字符串。

 

给你一个有效括号字符串 `seq`，将其分成两个不相交的子序列 `A` 和 `B`，且 `A` 和 `B` 满足有效括号字符串的定义（注意：`A.length + B.length = seq.length`）。

现在，你需要从中选出 **任意** 一组有效括号字符串 `A` 和 `B`，使 `max(depth(A), depth(B))` 的可能取值最小。

返回长度为 `seq.length` 答案数组 `answer` ，选择 `A` 还是 `B` 的编码规则是：如果 `seq[i]` 是 `A` 的一部分，那么 `answer[i] = 0`。否则，`answer[i] = 1`。即便有多个满足要求的答案存在，你也只需返回 **一个**。

 

**示例 1：**

```
输入：seq = "(()())"
输出：[0,1,1,1,1,0]
```

**示例 2：**

```
输入：seq = "()(())()"
输出：[0,0,0,1,1,0,1,1]
```

 

**提示：**

- `1 <= text.size <= 10000`

思路：

遍历一遍统计每个括号所在深度，然后二分，深度低于最深深度一半的成一组，其余成一组，搞定；时间O(2n)空间O(n)

```python
class Solution:
    def maxDepthAfterSplit(self, seq: str) -> [int]:
        # print(seq)
        l = len(seq)
        level, max = 0, 0
        record = []
        for i in range(l):
            if seq[i] == '(':
                level += 1
                if level > max: max = level
                record.append(level)
            if seq[i] == ')':
                record.append(level)
                level -= 1
        max = max >> 1
        for i in range(l):
            if record[i] > max:
                record[i] = 1
            else:
                record[i] = 0

        return record
```

> 执行用时 :52 ms, 在所有 Python3 提交中击败了72.64% 的用户
>
> 内存消耗 :14 MB, 在所有 Python3 提交中击败了16.67%的用户

官方题解这奇偶性神了。

只要嵌套深度为奇数就放到A里，偶数就放到B里

然后不难证明，

```python
括号序列   ( ( ) ( ( ) ) ( ) )
下标编号   0 1 2 3 4 5 6 7 8 9
嵌套深度   1 2 - 2 3 - - 2 - -
嵌套深度   - - 2 - - 3 2 - 2 1 
'('下深度与编号奇偶性相反
')'下深度与编号奇偶性相同
# 小问号你是否有很多朋友
```

```python
class Solution:
    def maxDepthAfterSplit(self, seq: str) -> [int]:
        rst = []
        for i,j in enumerate(seq):
            rst.append((j==")")^(i&1))
        return rst
```

### **这个`^`异或很灵性**。


## [1013. 将数组分成和相等的三个部分](https://leetcode-cn.com/problems/partition-array-into-three-parts-with-equal-sum/)

给你一个整数数组 `A`，只有可以将其划分为三个和相等的非空部分时才返回 `true`，否则返回 `false`。

形式上，如果可以找出索引 `i+1 < j` 且满足 `(A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1])` 就可以将数组三等分。

 

**示例 1：**

```
输出：[0,2,1,-6,6,-7,9,1,2,0,1]
输出：true
解释：0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1
```

**示例 2：**

```
输入：[0,2,1,-6,6,7,9,-1,2,0,1]
输出：false
```

**示例 3：**

```
输入：[3,3,6,5,-2,2,5,1,-9,4]
输出：true
解释：3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4
```

 

**提示：**

1. `3 <= A.length <= 50000`
2. `-10^4 <= A[i] <= 10^4`

思路：

第一眼，双指针

利用sum//3来判断，注意每次分割前后中间不能是空数组。

```python
class Solution:
    def canThreePartsEqualSum(self, A: [int]) -> bool:
        asum = sum(A)
        if asum % 3 > 0: return False
        part = asum // 3
        n = len(A)
        L, R = 0, n - 1
        pl, pr = A[0], A[n - 1]
        while L < R - 1:
            if pr == part and pl == part:
                return True
            if pl != part:
                L += 1
                pl += A[L]
            if pr != part:
                R -= 1
                pr += A[R]
        return False
```

> 执行用时 :60 ms, 在所有 Python3 提交中击败了98.63% 的用户
>
> 内存消耗 :18.4 MB, 在所有 Python3 提交中击败了98.29%的用户

就舒服就完了

## [1071. 字符串的最大公因子](https://leetcode-cn.com/problems/greatest-common-divisor-of-strings/)

对于字符串 `S` 和 `T`，只有在 `S = T + ... + T`（`T` 与自身连接 1 次或多次）时，我们才认定 “`T` 能除尽 `S`”。

返回最长字符串 `X`，要求满足 `X` 能除尽 `str1` 且 `X` 能除尽 `str2`。

 

**示例 1：**

```
输入：str1 = "ABCABC", str2 = "ABC"
输出："ABC"
```

**示例 2：**

```
输入：str1 = "ABABAB", str2 = "ABAB"
输出："AB"
```

**示例 3：**

```
输入：str1 = "LEET", str2 = "CODE"
输出：""
```

 

**提示：**

1. `1 <= str1.length <= 1000`
2. `1 <= str2.length <= 1000`
3. `str1[i]` 和 `str2[i]` 为大写英文字母

思路：

减少遍历次数，先把长度的公约数算出来，依此遍历。

```python
def Toss_Divide(a, b):
    if a < b: a, b = b, a
    while b > 0:
        temp = b
        b = a % b
        a = temp
    return a


def Rounding(n):
    s = int(pow(n, 1 / 2))
    rst = []
    if s * s == n:
        rst.append(s)
        s -= 1
    while s > 0:
        if not n % s:
            rst = [n // s] + rst + [s]
        s -= 1
    return rst


class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        l1, l2 = len(str1), len(str2)
        if l1 < l2:
            l1, l2 = l2, l1
            str1, str2 = str2, str1
        gcd = Toss_Divide(l1, l2)
        cds = Rounding(gcd)
        lcd = len(cds)
        n1 = l1 // gcd
        n2 = l2 // gcd
        for i in range(lcd):
            temp = str2[:cds[i]]
            s1 = temp * (n1 * cds[lcd - i - 1])
            s2 = temp * (n2 * cds[lcd - i - 1])
            if s1==str1 and s2 == str2:
                return temp
        return ''
```

> 执行用时 :28 ms, 在所有 Python3 提交中击败了94.07% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了7.48%的用户

舒服，看题解可以直接用字符去辗转相除，可能用c会快很多吧。

## [1103. 分糖果 II](https://leetcode-cn.com/problems/distribute-candies-to-people/)

排排坐，分糖果。

我们买了一些糖果 `candies`，打算把它们分给排好队的 **`n = num_people`** 个小朋友。

给第一个小朋友 1 颗糖果，第二个小朋友 2 颗，依此类推，直到给最后一个小朋友 `n` 颗糖果。

然后，我们再回到队伍的起点，给第一个小朋友 `n + 1` 颗糖果，第二个小朋友 `n + 2` 颗，依此类推，直到给最后一个小朋友 `2 * n` 颗糖果。

重复上述过程（每次都比上一次多给出一颗糖果，当到达队伍终点后再次从队伍起点开始），直到我们分完所有的糖果。注意，就算我们手中的剩下糖果数不够（不比前一次发出的糖果多），这些糖果也会全部发给当前的小朋友。

返回一个长度为 `num_people`、元素之和为 `candies` 的数组，以表示糖果的最终分发情况（即 `ans[i]` 表示第 `i` 个小朋友分到的糖果数）。

 

**示例 1：**

```
输入：candies = 7, num_people = 4
输出：[1,2,3,1]
解释：
第一次，ans[0] += 1，数组变为 [1,0,0,0]。
第二次，ans[1] += 2，数组变为 [1,2,0,0]。
第三次，ans[2] += 3，数组变为 [1,2,3,0]。
第四次，ans[3] += 1（因为此时只剩下 1 颗糖果），最终数组变为 [1,2,3,1]。
```

**示例 2：**

```
输入：candies = 10, num_people = 3
输出：[5,2,3]
解释：
第一次，ans[0] += 1，数组变为 [1,0,0]。
第二次，ans[1] += 2，数组变为 [1,2,0]。
第三次，ans[2] += 3，数组变为 [1,2,3]。
第四次，ans[0] += 4，最终数组变为 [5,2,3]。
```

 

**提示：**

- `1 <= candies <= 10^9`
- `1 <= num_people <= 1000`

```python
def mySum(n: int):
    return ((1 + n) * n) >> 1


class Solution:
    def distributeCandies(self, candies: int, num_people: int) -> [int]:
        k1 = 0
        while candies >= mySum(k1 * num_people):
            k1 += 1
        k1 -= 1
        rest = candies - mySum(k1 * num_people)
        rst = [0] * num_people
        for i in range(num_people):
            rst[i] += ((2 * (i + 1) + (k1 - 1) * num_people) * k1) >> 1
        k2 = 0
        end = k1 * num_people
        while rest >= end * k2 + mySum(k2):
            k2 += 1
        k2 -= 1
        rest -= end * k2 + mySum(k2)
        for i in range(k2):
            rst[i] += end + i + 1
        rst[k2] += rest
        return rst
```

> 执行用时 :32 ms, 在所有 Python3 提交中击败了95.96% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了22.76%的用户

题太简单，看答案为了看有没有写的很短的，发现没有，就这么地吧。

## [1160. 拼写单词](https://leetcode-cn.com/problems/find-words-that-can-be-formed-by-characters/)

给你一份『词汇表』（字符串数组） `words` 和一张『字母表』（字符串） `chars`。

假如你可以用 `chars` 中的『字母』（字符）拼写出 `words` 中的某个『单词』（字符串），那么我们就认为你掌握了这个单词。

注意：每次拼写时，`chars` 中的每个字母都只能用一次。

返回词汇表 `words` 中你掌握的所有单词的 **长度之和**。

 

**示例 1：**

```
输入：words = ["cat","bt","hat","tree"], chars = "atach"
输出：6
解释： 
可以形成字符串 "cat" 和 "hat"，所以答案是 3 + 3 = 6。
```

**示例 2：**

```
输入：words = ["hello","world","leetcode"], chars = "welldonehoneyr"
输出：10
解释：
可以形成字符串 "hello" 和 "world"，所以答案是 5 + 5 = 10。
```

 

**提示：**

1. `1 <= words.length <= 1000`
2. `1 <= words[i].length, chars.length <= 100`
3. 所有字符串中都仅包含小写英文字母

方法1：匹配字典

```python
def word2dict(word):
    dict = {}
    for i in word:
        if i in dict:
            dict[i] += 1
        else:
            dict[i] = 1
    return dict


def wordInDict(word: str, dict: {}) -> int:
    dw = word2dict(word)
    for i in dw:
        if i in dict:
            if dw[i] > dict[i]: return 0
        else:
            return 0
    return len(word)


class Solution:
    def countCharacters(self, words: [str], chars: str) -> int:
        rst = 0
        dict = word2dict(chars)
        for i in words:
            rst += wordInDict(i, dict)
        return rst
```

>执行用时 :344 ms, 在所有 Python3 提交中击败了22.29% 的用户
>
>内存消耗 :13.9 MB, 在所有 Python3 提交中击败了5.11%的用户

法2：直接遍历，chars还是建立字典

```python
def word2dict(word):
    dict = {}
    for i in word:
        if i in dict:
            dict[i] += 1
        else:
            dict[i] = 1
    return dict

class Solution:
    def countCharacters(self, words: [str], chars: str) -> int:
        rst = 0
        dict = word2dict(chars)
        for word in words:
            flag = True
            dict2 = dict.copy()
            for i in word:
                if i in dict2:
                    if dict2[i]:
                        dict2[i]-=1
                    else:
                        flag = False
                        break
                else:
                    flag = False
                    break
            if flag:rst+=len(word)
        return rst
```

> 执行用时 :108 ms, 在所有 Python3 提交中击败了87.63% 的用户
>
> 内存消耗 :13.7 MB, 在所有 Python3 提交中击败了5.11%的用户

看题解，官方这个for else亮瞎我狗眼

```python
class Solution:
    def countCharacters(self, words: List[str], chars: str) -> int:
        chars_cnt = collections.Counter(chars)
        ans = 0
        for word in words:
            word_cnt = collections.Counter(word)
            for c in word_cnt:
                if chars_cnt[c] < word_cnt[c]:
                    break
            else:
                ans += len(word)
        return ans
```

## [1162. 地图分析](https://leetcode-cn.com/problems/as-far-from-land-as-possible/)

你现在手里有一份大小为 N x N 的『地图』（网格） `grid`，上面的每个『区域』（单元格）都用 `0` 和 `1` 标记好了。其中 `0` 代表海洋，`1` 代表陆地，你知道距离陆地区域最远的海洋区域是是哪一个吗？请返回该海洋区域到离它最近的陆地区域的距离。

我们这里说的距离是『曼哈顿距离』（ Manhattan Distance）：`(x0, y0)` 和 `(x1, y1)` 这两个区域之间的距离是 `|x0 - x1| + |y0 - y1|` 。

如果我们的地图上只有陆地或者海洋，请返回 `-1`。

 

**示例 1：**

**![img](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/1336_ex1.jpeg)

```
输入：[[1,0,1],[0,0,0],[1,0,1]]
输出：2
解释： 
海洋区域 (1, 1) 和所有陆地区域之间的距离都达到最大，最大距离为 2。
```

**示例 2：**

**![img](https://raw.githubusercontent.com/hahahaMing/MyImage/master/leetcode.assets/1336_ex2.jpeg)

```
输入：[[1,0,0],[0,0,0],[0,0,0]]
输出：4
解释： 
海洋区域 (2, 2) 和所有陆地区域之间的距离都达到最大，最大距离为 4。
```

 

**提示：**

1. `1 <= grid.length == grid[0].length <= 100`
2. `grid[i][j]` 不是 `0` 就是 `1`

思路：

利用已经算出的结论减小遍历：

```python
def downLandIn(grid, x, y, d) -> bool:
    if not d and grid[y][x]: return True
    N = len(grid)
    for i in range(d + 1):
        xx = x + i
        yy = y + (d - i)
        if  xx < N and yy < N:
            if grid[yy][xx]:
                return True
    return False


class Solution:
    def maxDistance(self, grid: [[int]]) -> int:
        N = len(grid)
        record = []
        for i in range(N): record.append([-1] * N)
        # print(record)
        # record[0][1]=2
        # printGrid(record)
        rst = 0
        for y in range(N):
            for x in range(N):
                if y:
                    rg = record[y - 1][x]
                    record[y][x] = rg + 1
                else:
                    rg = max(x - 1, N - x) + N - 1
                if x:
                    rg = min(record[y][x - 1], rg)
                    record[y][x] = rg + 1
                for i in range(rg+1):
                    if downLandIn(grid, x, y, i):
                        record[y][x] = i
                        break
                if record[y][x] == -1:
                    return -1
                else:
                    rst = max(rst, record[y][x])
        # printGrid(record)
        if not rst: return -1
        return rst
```

> 执行用时 :952 ms, 在所有 Python3 提交中击败了28.92% 的用户
>
> 内存消耗 :13.9 MB, 在所有 Python3 提交中击败了17.65%的用户

利用不充分，存在n次遍历。

想到一个时间O(5n2)的方法：按4个象限方向遍历出4个表，然后叠加。

```python
class Solution:
    def maxDistance(self, grid: [[int]]) -> int:
        N = len(grid)

        def form(N):
            record = []
            for i in range(N): record.append([-1] * N)
            return record

        gg = []
        rec = form(N)
        for i in range(4): gg.append(form(N))
        atb = [[1, 1], [1, -1], [-1, 1], [-1, -1]]
        stp = [[0, 0], [0, N - 1], [N - 1, 0], [N - 1, N - 1]]
        end = [[N - 1, N - 1], [N - 1, 0], [0, N - 1], [0, 0]]
        rst = 0
        for i in range(4):
            y, x = stp[i]
            ey, ex = atb[i]
            while 0 <= y < N:
                while 0 <= x < N:
                    if grid[y][x]:
                        gg[i][y][x] = 0
                        x += ex
                        continue
                    if y == stp[i][0]:
                        if x != stp[i][1] and gg[i][y][x - ex] != -1:
                            gg[i][y][x] = gg[i][y][x - ex] + 1
                    else:
                        if x == stp[i][1]:
                            if gg[i][y - ey][x] != -1:
                                gg[i][y][x] = gg[i][y - ey][x] + 1
                        else:
                            if gg[i][y][x - ex] != -1:
                                if gg[i][y - ey][x] != -1:
                                    gg[i][y][x] = min(gg[i][y][x - ex], gg[i][y - ey][x]) + 1
                                else:
                                    gg[i][y][x] = gg[i][y][x - ex] + 1
                            elif gg[i][y - ey][x] != -1:
                                gg[i][y][x] = gg[i][y - ey][x] + 1
                    x += ex
                x = stp[i][1]
                y += ey
        for y in range(N):
            for x in range(N):
                temp = []
                for i in range(4):
                    if gg[i][y][x] != -1: temp.append(gg[i][y][x])
                if temp:
                    rst = max(rst, min(temp))
        if not rst: return -1
        return rst
```

> 执行用时 :1104 ms, 在所有 Python3 提交中击败了19.12% 的用户
>
> 内存消耗 :14.1 MB, 在所有 Python3 提交中击败了17.65%的用户

实际运行明明快了一个数量级。。

官方的解法的动态规划比我的好

从左上到右下一次，然后从右下到左上更新一次（利用上一次的数值，因为从右下开始，右下在上次里是完全正确的数值）最后只要取最大值就OK了。





## [面试题 01.06. 字符串压缩](https://leetcode-cn.com/problems/compress-string-lcci/)

字符串压缩。利用字符重复出现的次数，编写一种方法，实现基本的字符串压缩功能。比如，字符串`aabcccccaaa`会变为`a2b1c5a3`。若“压缩”后的字符串没有变短，则返回原先的字符串。你可以假设字符串中只包含大小写英文字母（a至z）。

 **示例1:**

```
 输入："aabcccccaaa"
 输出："a2b1c5a3"
```

 **示例2:**

```
 输入："abbccd"
 输出："abbccd"
 解释："abbccd"压缩后为"a1b2c2d1"，比原字符串长度更长。
```

**提示：**

1. 字符串长度在[0, 50000]范围内。

```python
class Solution:
    def compressString(self, S: str) -> str:
        l = len(S)
        cs = ''
        temp = ''
        lcs = 0
        if not S:return ''
        for i in S:
            if i != temp:
                if temp:
                    cs += temp + str(count)
                    lcs+=2
                count = 1
                temp = i
            else:
                count+=1
        cs += temp + str(count)
        lcs += 2
        if lcs<l:return cs
        else:return S
```

> 执行用时 :56 ms, 在所有 Python3 提交中击败了76.34% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了100.00%的用户

有用双指针的，感觉没必要

## [面试题 01.07. 旋转矩阵](https://leetcode-cn.com/problems/rotate-matrix-lcci/)

给你一幅由 `N × N` 矩阵表示的图像，其中每个像素的大小为 4 字节。请你设计一种算法，将图像旋转 90 度。

不占用额外内存空间能否做到？

 

**示例 1:**

```
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**示例 2:**

```
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

思路：原地旋转，每次操作4个做3次对调就ok

```python
class Solution:
    def rotate(self, matrix: [[int]]) -> None:
        def swith_m(y1,x1,y2,x2):
            matrix[y1][x1],matrix[y2][x2]=matrix[y2][x2],matrix[y1][x1]
        n = len(matrix)
        for i in range(n>>1):
            for j in range(i,n-i-1):
                swith_m(i,j,j,-i-1)
                swith_m(-i-1,-j-1,-j-1,i)
                swith_m(i,j,-i-1,-j-1)
```

> 执行用时 :52 ms, 在所有 Python3 提交中击败了21.33% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了100.00%的用户

题解也就差不多，我这内存100%就圆满了。

## [面试题 10.01. 合并排序的数组](https://leetcode-cn.com/problems/sorted-merge-lcci/)

给定两个排序后的数组 A 和 B，其中 A 的末端有足够的缓冲空间容纳 B。 编写一个方法，将 B 合并入 A 并排序。

初始化 A 和 B 的元素数量分别为 *m* 和 *n*。

**示例:**

```
输入:
A = [1,2,3,0,0,0], m = 3
B = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]
```

**说明:**

- `A.length == n + m`

法1：暴力

```python
class Solution:
    def merge(self, A: [int], m: int, B: [int], n: int) -> None:
        """
        Do not return anything, modify A in-place instead.
        """
        for i in range(m,m+n):
            A[i]+=B[i-m]
        A.sort()
```

> 执行用时 :40 ms, 在所有 Python3 提交中击败了64.90% 的用户
>
> 内存消耗 :13.3 MB, 在所有 Python3 提交中击败了100.00%的用户

法2：双指针

```python
class Solution:
    def merge(self, A: [int], m: int, B: [int], n: int) -> None:
        """
        Do not return anything, modify A in-place instead.
        """
        m -= 1
        n -= 1
        while 1:
            if m == -1:
                A[:n+1] = B[:n+1]
                return
            if n == -1:
                return
            if A[m] > B[n]:
                A[m + n + 1] = A[m]
                m -= 1
            else:
                A[m + n + 1] = B[n]
                n -= 1
```

> 执行用时 :44 ms, 在所有 Python3 提交中击败了45.08% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了100.00%的用户

不懂为啥变慢了。。看了一个哥们好像m+n那块应该重附一个变量，但是没道理呀。。



## [面试题13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

地上有一个m行n列的方格，从坐标 `[0,0]` 到坐标 `[m-1,n-1]` 。一个机器人从坐标 `[0, 0] `的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

**示例 1：**

```
输入：m = 2, n = 3, k = 1
输出：3
```

**示例 1：**

```
输入：m = 3, n = 1, k = 0
输出：1
```

**提示：**

- `1 <= n,m <= 100`
- `0 <= k <= 20`

思路：

把整个区域按10*10分成小块，每一块的操作相同。

block函数返回长宽为m,n的块里面，和最大值为k的可到达区域（m,n<=9）。

主函数将所有区域划分为`10*10`的小块，每个小块的左下角坐标为`(a*10,b*10)`,输入到block中的k为`k-a-b`。

其中需要判断机器人能否从原点走到这个小块的左下角，这里证明只要满足`k>=a+b-1+9`即可，证明如下：

若可到达`(a*10,b*10)`需先到达`(a*10-1,b*10)`或`(a*10,b*10-1)`,此两点数位之和为a+b-1+9；

从原点出发，先沿着y轴走到`(0,b*10)`，路中数位之和最大值为b-1+9<=a+b-1+9;
再向x轴方向走到`(a*10,b*10)`，路中数位之和最大值为a+b-1+9;
故如果`k>=a+b-1+9`，则可以从原点走到`(a*10,b*10)`，注意a=b=0需要单独判断。
那么下面只要遍历就好了，都是数学计算。

```python
def block(m, n, k):
    if m > n: m, n = n, m
    if k <= m:
        return ((k + 1) * (k + 2)) >> 1
    elif k <= n:
        return (((m + 1) * (m + 2)) >> 1) + (k - m) * (m + 1)
    elif k <= m + n:
        return (m + 1) * (n + 1) - (((m + n - k) * (m + n - k + 1)) >> 1)
    else:
        return (m + 1) * (n + 1)


class Solution:
    def movingCount(self, m: int, n: int, k: int) -> int:
        a, b = 0, 0
        rst = 0
        while a * 10 < m:
            b = 0
            while b * 10 < n:
                if (a == 0 and b == 0) or k > a + b + 7:
                    mm = 9 if m - 1 - a * 10 > 9 else m - 1 - a * 10
                    nn = 9 if n - 1 - b * 10 > 9 else n - 1 - b * 10
                    rst += block(mm, nn, k - a - b)
                b += 1
            a += 1
        return rst
```

> 执行用时 :36 ms, 在所有 Python3 提交中击败了99.91% 的用户
>
> 内存消耗 :13.4 MB, 在所有 Python3 提交中击败了100.00%的用户




## [面试题 17.16. 按摩师](https://leetcode-cn.com/problems/the-masseuse-lcci/)

一个有名的按摩师会收到源源不断的预约请求，每个预约都可以选择接或不接。在每次预约服务之间要有休息时间，因此她不能接受相邻的预约。给定一个预约请求序列，替按摩师找到最优的预约集合（总预约时间最长），返回总的分钟数。

**注意：**本题相对原题稍作改动

 

**示例 1：**

```
输入： [1,2,3,1]
输出： 4
解释： 选择 1 号预约和 3 号预约，总时长 = 1 + 3 = 4。
```

**示例 2：**

```
输入： [2,7,9,3,1]
输出： 12
解释： 选择 1 号预约、 3 号预约和 5 号预约，总时长 = 2 + 9 + 1 = 12。
```

**示例 3：**

```
输入： [2,1,4,5,3,1,1,3]
输出： 12
解释： 选择 1 号预约、 3 号预约、 5 号预约和 8 号预约，总时长 = 2 + 4 + 3 + 3 = 12。
```

思路：

动态规划，每次记录前n个数组中

1. 计算尾数的最大值$m1_n$
2. 不计算尾数的最大值$m2_n$

这样每增加一个数，更新这两个值：

1. $m1_{n+1}=m2_n+x_{n+1}$
2. $m2_{n+1}=MAX(m1_n,m2_n)$

最后比较这两个值

```python
class Solution:
    def massage(self, nums: [int]) -> int:
        m1,m2 = 0,0
        for i in nums:
            temp = m1
            m1 = m2+i
            m2 = max(temp,m2)
        return max(m1,m2)
```

> 执行用时 :40 ms, 在所有 Python3 提交中击败了60.83% 的用户
>
> 内存消耗 :13.6 MB, 在所有 Python3 提交中击败了100.00%的用户

看题解，一样。

## [面试题40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 

**示例 1：**

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

**示例 2：**

```
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

 

**限制：**

- `0 <= k <= arr.length <= 10000`
- `0 <= arr[i] <= 10000`

```python
class Solution:
    def getLeastNumbers(self, arr: List[int], k: int) -> List[int]:
        arr.sort()
        return arr[:k]
```

> 执行用时 :64 ms, 在所有 Python3 提交中击败了82.62% 的用户
>
> 内存消耗 :14.4 MB, 在所有 Python3 提交中击败了100.00%的用户

```python
from math import log


class Solution:
    def getLeastNumbers(self, arr: [int], k: int) -> [int]:
        if not k :return []
        n = len(arr)
        c1 = k * (log(k, 2) + n - k)
        c2 = n * log(n, 2)
        if c1 < c2:
            rst = arr[:k]
            rst.sort()
            rst = rst[::-1]
            for i in range(k, n):
                if arr[i] < rst[0]:
                    rst[0] = arr[i]
                    for j in range(1, k):
                        if rst[j - 1] < rst[j]:
                            rst[j - 1], rst[j] = rst[j], rst[j - 1]
                        else:
                            break
            return rst
        else:
            arr.sort()
            return arr[:k]
```

> 执行用时 :52 ms, 在所有 Python3 提交中击败了96.82% 的用户
>
> 内存消耗 :14.7 MB, 在所有 Python3 提交中击败了100.00%的用户

### **BFPRT算法**

就是优化快排利用那个值的选择方式，百度很多代码，里面坑很多，用c++的时候可以来复习一下。

```python
def InsertSort(arr: [int], left: int, right: int) -> int:
    """
    对数组中[left...right]插入排序并返回[left...right]的中位数
    """
    for i in range(left + 1, right + 1):
        temp = arr[i]
        j = i - 1
        while j >= left and arr[j] > temp:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = temp
    return ((right - left) >> 1) + left



def Partition(arr, left, right, pivot_index):
    """
    利用主元下标 pivot_index 进行对数组 array[left, right] 划分，并返回
    划分后的分界线下标。
    :param arr:
    :param left:
    :param right:
    :param pivot_index:
    :return:
    """
    arr[pivot_index], arr[right] = arr[right], arr[pivot_index]  # 把主元放置于末尾
    partition_index = left  # 跟踪划分的分界线
    for i in range(left, right):
        if arr[i] < arr[right]:
            # 比主元小的都放在左侧
            arr[partition_index], arr[i] = arr[i], arr[partition_index]
            partition_index += 1
    arr[partition_index], arr[right] = arr[right], arr[partition_index]  # 最后把主元换回来
    return partition_index


def BFPRT(arr, left, right, k):
    """
    求数组arr下标 left到 right中的第 k个数
    """
    if right - left < 5:
        InsertSort(arr, left, right)
        return arr[left + k - 1]
    sub_right = left - 1
    for i in range(left, right - 3, 5):
        index = InsertSort(arr, i, i + 4)
        sub_right += 1
        arr[sub_right], arr[index] = arr[index], arr[sub_right]
    # 以上代码将整个数组分为以5为单位的组，分别求中位数逐个放到数组最前面
    # print(arr)
    # print("---"*20)
    mid = (left + sub_right) >> 1
    # BFPRT(arr, left, sub_right, mid - left + 1)
    m = Partition(arr, left, right, mid)
    cur = m - left + 1
    if k == cur:
        return arr[m]
    elif k < cur:
        return BFPRT(arr, left, m - 1, k)
    else:
        return BFPRT(arr, m + 1, right, k - cur)

class Solution:
    def getLeastNumbers(self, arr: [int], k: int) -> [int]:
        if not k: return []
        BFPRT(arr,0,len(arr)-1,k)
        return arr[:k]
```



## [面试题57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

输入一个正整数 `target` ，输出所有和为 `target` 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

 

**示例 1：**

```
输入：target = 9
输出：[[2,3,4],[4,5]]
```

**示例 2：**

```
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```

 

**限制：**

- `1 <= target <= 10^5`

```python
class Solution:
    def findContinuousSequence(self, target: int) -> [[int]]:
        rst = []
        for i in range(1, (target >> 1) + 1):
            n = (pow((1 + 4 * i * i + 8 * target - 4 * i), 1 / 2) - 1) / 2
            intn = int(n)
            if n - intn == 0: rst.append([j for j in range(i, intn + 1)])
        return rst
```

> 执行用时 :216 ms, 在所有 Python3 提交中击败了54.55% 的用户
>
> 内存消耗 :13.5 MB, 在所有 Python3 提交中击败了100.00%的用户

## [面试题59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

请定义一个队列并实现函数 `max_value` 得到队列里的最大值，要求函数`max_value`、`push_back` 和 `pop_front` 的**均摊**时间复杂度都是O(1)。

若队列为空，`pop_front` 和 `max_value` 需要返回 -1

**示例 1：**

```
输入: 
["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
[[],[1],[2],[],[],[]]
输出: [null,null,null,2,1,2]
```

**示例 2：**

```
输入: 
["MaxQueue","pop_front","max_value"]
[[],[],[]]
输出: [null,-1,-1]
```

 

**限制：**

- `1 <= push_back,pop_front,max_value的总操作数 <= 10000`
- `1 <= value <= 10^5`

思路：多建立一个数组记录从第i个到最后的最大值，实时更新。

```python
class MaxQueue:

    def __init__(self):
        self.nums = []
        self.maxs = []
        self.l = 0
    
    def max_value(self) -> int:
        if self.maxs:
            return self.maxs[0]
        else:
            return -1
    
    def push_back(self, value: int) -> None:
        self.nums = self.nums + [value]
        # refreshing maxs[]
        for i in range(self.l):
            if value <= self.maxs[self.l - 1 - i]:
                break
            else:
                self.maxs[self.l - 1 - i] = value
        self.maxs = self.maxs + [value]
        #
        self.l += 1
    
    def pop_front(self) -> int:
        if not self.l: return -1  # if nums is empty, return null
        old_top = self.nums[0]
        self.nums = self.nums[1:]
        self.maxs = self.maxs[1:]  # refreshing maxs[]
        self.l -= 1
        return old_top
```

>执行用时 :356 ms, 在所有 Python3 提交中击败了30.11% 的用户

> 内存消耗 :17 MB, 在所有 Python3 提交中击败了100.00%的用户

不懂为啥这么慢，感觉和官方思路一样。妈的官方题解进去比我还慢好吧。

暴力法288ms，python牛逼。

## [面试题62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

 

**示例 1：**

```
输入: n = 5, m = 3
输出: 3
```

**示例 2：**

```
输入: n = 10, m = 17
输出: 2
```

 

**限制：**

- `1 <= n <= 10^5`
- `1 <= m <= 10^6`

只看答案的运动，其实就是一直在朝一个方向转，每次转m%当前数组长度，最后转到0的位置，反推回去，就得到原来的位置，就是答案。

```python
class Solution:
    def lastRemaining(self, n: int, m: int) -> int:
        rst = 0
        for i in range(1,n+1):
            rst+=m % i
            if rst>=i:rst-=i
        return rst
```

> 执行用时 :140 ms, 在所有 Python3 提交中击败了49.74% 的用户
>
> 内存消耗 :13.7 MB, 在所有 Python3 提交中击败了100.00%的用户

我这有一句傻了，看官方代码

```python
class Solution:
    def lastRemaining(self, n: int, m: int) -> int:
        rst = 0
        for i in range(2,n+1):
            rst = (rst+m)%i
        return rst
```

> 执行用时 :76 ms, 在所有 Python3 提交中击败了95.50% 的用户
>
> 内存消耗 :13.7 MB, 在所有 Python3 提交中击败了100.00%的用户

