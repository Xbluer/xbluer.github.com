---
layout: post
title:  "Python dict 学习笔记"
tagline: ""
category : note
tags: [Python]
---

#### 定义方法:
1. `dictname = {key1: val1, key2: val2}`
2. 使用dict函数  
<code>
items = [(key1, val1), (key2， val2)]  
d = dict(items)  
</code>

#### 基本操作:
1. `len(d)`，返回d中key-value数目
2. `d[k]`，返回关联在k上的值
3. `d[k] = v`，将v关联到健k上
4. `del d[k]`，删除键k的项
5. `k in d`，检查d中是否含有键为k的项,查找的是key不是value

#### 字典方法:
1. **clear**：清楚dict中所有的项，__原地操作__，所以无返回值  
<code>
d1 = {k1: v1, k2, v2}  
d2 = d1  
d1.clear()  # d1和d2都==none  
d3 = {k1: v1, k2, v2}  
d4 = d3
d3 = {}   # d4 == {k1: v1, k2: v2}
</code>
2. **copy** 浅拷贝，不同的dict共享value，对value的原地操作将会影响到其他共享value的的dict。  
 **deepcopy** 深拷贝，复制所有的value
3. **fromkeys** 对给定的key建立新的dict，value默认为none，也可以提供默认值  
<code>
\>>>dict.fromkeys(['name', 'age'], '(unknown)'])  
{'age': '(unknown)', 'name': '(unknown)'}
</code>
4. **get**，一种更宽松的访问dict中item的方法，当key不在dict中时不会出错  
<code>
\>>>d = {}  
\>>>print d['name']  
error  
\>>>print d.get('name')  
None
</code>
5. **has_key**，检查dict中是否含有给定的key。`d.has_key('name')` 相当于 `d.has_key('name')`
6. **items**，将dict中所有项以list方式返回，list中的每一项都是key-value对，返回时没有固定的顺序。  
**iteritems**，返回迭代器对象，而非list
7. **keys**，将dict中的key以list的形式返回  
**iterkeys**，返回针对key的迭代器  
8. **pop**，获得对应key的value，且将次key-value从dict中删除。
9. **popitem**，随机弹出一个key-value对。
10. **setdefault**，获得给定的key关联的value。如果key不再dict中，则返回设定的默认值，且更新dict；如果key存在，则只返回value不更新dict  
一定程度上类似于get  
<code>
\>>>d = {}  
\>>>d.setdefault('name', 'N/A')  
'N/A'
</code>
11. **update**，利用一个dict更新另一个dict，提供的dict中的新key被添加到原dict中，若有相同的则覆盖。  
<code>
d = {k1: v1, k3: v31}  
x = {k2: v2, k3: v32}  
d.update(x)
\>>>print d  
{k1: v1, k2: v2, __k3: v32__} 
</code>

12. **values**，返回dict中所有value值的list，可以重复  
**itervalues**，返回迭代器

----------------------------------------------
