---
title: java理论小记
tags: java
notebook: 极客指南
---

- [相关数据类型区别](#相关数据类型区别)
  - [数组(Array)](#数组array)
  - [列表(List)  如果使用List,那么集合内的数据类型有且只能是一种,不允许多种](#列表list--如果使用list那么集合内的数据类型有且只能是一种不允许多种)
  - [数组列表(ArrayList)](#数组列表arraylist)

### 相关数据类型区别

#### 数组(Array)

```java
int[] primes = {2, 3, 5, 7, 11, 13};  
Arrays.asList(arrstr)   // 数组转List  
Arrays.toString(arrstr)  // 数组转String  
List<String> list = new ArrayList(Arrays.asList(table))  //数组转换为列表  
```

#### 列表(List)  如果使用List,那么集合内的数据类型有且只能是一种,不允许多种  

```java
List<int> list = new List<int>();   or   List<int> list = new ArrayList<>();  
//新增数据  
 list.Add(123);  
//修改数据  
list[0] = 345;  
//移除数据  
list.RemoveAt(0);  
```

#### 数组列表(ArrayList)

```java
ArrayList中插入不同类型的数据是允许的。因为ArrayList会把所有插入其中的数据都当作为object类型来处理  
ArrayList list = new ArrayList();  
//新增数据  
 list.Add("acrs");  
 list.Add(123);  
//修改数据  
 list[2] = 345;  //修改索引位置为2的值,将值修改为345;  
//移除数据  
 list.RemoveAt(2);  //移除索引为2的数据  
//插入数据   
list.Insert(0, "hello world");  //插入索引位置为0,数值为"hello world"  
}  

List list;     //正确   list=null;   
List list=new List();    //   是错误的用法  
List<int> list = new List<int>();  //正确  
List<int> list = new ArrayList<>();   //正确,需要注意的是;这句创建了一个ArrayList的对象后把赋值给了List。此时它是一个List对象了，所以会出现有些  ArrayList有但是List没有的属性和方法，那么这个List对象就不能再用了  
ListArray listArray = new ListArray();   //正确,这里创建的对象则保留了ArrayList的所有属性  
```
