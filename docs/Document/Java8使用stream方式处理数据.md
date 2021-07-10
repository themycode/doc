---
title: Java 8 中使用 Stream 方式处理数据
tags: java, stream
notebook: wiki
---

![Java 8 中使用 Stream 方式处理数据](https://mydlq-club.oss-cn-beijing.aliyuncs.com/images/java-java8-stream-1001.png?x-oss-process=style/shuiyin)

## 文章目录

- [一、什么是 Stream](http://www.mydlq.club/article/90/#%E4%B8%80%E4%BB%80%E4%B9%88%E6%98%AF-stream)
- [二、Stream 操作分类](http://www.mydlq.club/article/90/#%E4%BA%8Cstream-%E6%93%8D%E4%BD%9C%E5%88%86%E7%B1%BB)
- [三、Stream 特性](http://www.mydlq.club/article/90/#%E4%B8%89stream-%E7%89%B9%E6%80%A7)
- [四、数据转换为 Stream](http://www.mydlq.club/article/90/#%E5%9B%9B%E6%95%B0%E6%8D%AE%E8%BD%AC%E6%8D%A2%E4%B8%BA-stream)
- [五、Stream 转换得到指定类型数据](http://www.mydlq.club/article/90/#%E4%BA%94stream-%E8%BD%AC%E6%8D%A2%E5%BE%97%E5%88%B0%E6%8C%87%E5%AE%9A%E7%B1%BB%E5%9E%8B%E6%95%B0%E6%8D%AE)
- [六、Stream 对于基本类型的封装](http://www.mydlq.club/article/90/#%E5%85%ADstream-%E5%AF%B9%E4%BA%8E%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%B0%81%E8%A3%85)
- [七、Stream 的串行与并行](http://www.mydlq.club/article/90/#%E4%B8%83stream-%E7%9A%84%E4%B8%B2%E8%A1%8C%E4%B8%8E%E5%B9%B6%E8%A1%8C)
  - [1、Stream 的并行介绍](http://www.mydlq.club/article/90/#1stream-%E7%9A%84%E5%B9%B6%E8%A1%8C%E4%BB%8B%E7%BB%8D)
  - [2、Stream 并行示例](http://www.mydlq.club/article/90/#2stream-%E5%B9%B6%E8%A1%8C%E7%A4%BA%E4%BE%8B)
- [八、Stream 中间操作（有状态）常用 API](http://www.mydlq.club/article/90/#%E5%85%ABstream-%E4%B8%AD%E9%97%B4%E6%93%8D%E4%BD%9C%E6%9C%89%E7%8A%B6%E6%80%81%E5%B8%B8%E7%94%A8-api)
  - [1、distinct](http://www.mydlq.club/article/90/#1distinct)
  - [2、sorted](http://www.mydlq.club/article/90/#2sorted)
  - [3、skip](http://www.mydlq.club/article/90/#3skip)
  - [4、limit](http://www.mydlq.club/article/90/#4limit)
- [九、Stream 中间操作（无状态）常用 API](http://www.mydlq.club/article/90/#%E4%B9%9Dstream-%E4%B8%AD%E9%97%B4%E6%93%8D%E4%BD%9C%E6%97%A0%E7%8A%B6%E6%80%81%E5%B8%B8%E7%94%A8-api)
  - [1、map](http://www.mydlq.club/article/90/#1map)
  - [2、peek](http://www.mydlq.club/article/90/#2peek)
  - [3、filter](http://www.mydlq.club/article/90/#3filter)
  - [4、mapToInt](http://www.mydlq.club/article/90/#4maptoint)
  - [5、mapToDouble](http://www.mydlq.club/article/90/#5maptodouble)
- [十、Stream 终端操作（短路操作）常用 API](http://www.mydlq.club/article/90/#%E5%8D%81stream-%E7%BB%88%E7%AB%AF%E6%93%8D%E4%BD%9C%E7%9F%AD%E8%B7%AF%E6%93%8D%E4%BD%9C%E5%B8%B8%E7%94%A8-api)
  - [1、anyMatch](http://www.mydlq.club/article/90/#1anymatch)
  - [2、allMatch](http://www.mydlq.club/article/90/#2allmatch)
  - [3、noneMatch](http://www.mydlq.club/article/90/#3nonematch)
  - [4、findFirst](http://www.mydlq.club/article/90/#4findfirst)
  - [5、findAny](http://www.mydlq.club/article/90/#5findany)
- [十一、Stream 终端操作（非短路操作）常用 API](http://www.mydlq.club/article/90/#%E5%8D%81%E4%B8%80stream-%E7%BB%88%E7%AB%AF%E6%93%8D%E4%BD%9C%E9%9D%9E%E7%9F%AD%E8%B7%AF%E6%93%8D%E4%BD%9C%E5%B8%B8%E7%94%A8-api)
  - [1、max](http://www.mydlq.club/article/90/#1max)
  - [2、min](http://www.mydlq.club/article/90/#2min)
  - [3、count](http://www.mydlq.club/article/90/#3count)
  - [4、reduce](http://www.mydlq.club/article/90/#4reduce)
  - [5、forEach](http://www.mydlq.club/article/90/#5foreach)
  - [6、forEachOrdered](http://www.mydlq.club/article/90/#6foreachordered)
  - [7、toArray](http://www.mydlq.club/article/90/#7toarray)
  - [8、collect](http://www.mydlq.club/article/90/#8collect)

  !版权声明：本博客内容均为原创,每篇博文作为知识积累,写博不易,转载请注明出处。

___

**参考地址：**

- [Java Stream 详解](https://colobu.com/2016/03/02/Java-Stream/)
- [菜鸟教程 Java 8 Stream](https://www.runoob.com/java/java8-streams.html)
- [使用 JDK 8 Stream 简化集合操作](https://www.cnblogs.com/hhhshct/p/11275516.html)

## 一、什么是 Stream[](http://www.mydlq.club/article/90/#%E4%B8%80%E4%BB%80%E4%B9%88%E6%98%AF-stream)

       在 `Java 8` 中增加了一个新的抽象接口 `Stream API`，它支持声明式的处理数据。使用 `Stream` 操作集合似于使用 SQL 语句数据库查找数据类似，提供直观的方法进行操作。 同时 Stream API 让开发者能够快速写出干净、简洁的代码，提高开发者的开发效率。

Stream 将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道传输过程中对流进行处理， 比如筛选、排序、聚合等操作。在经过一系列中间操作后形成最终的管道，得到处理的结果。

## 二、Stream 操作分类[](http://www.mydlq.club/article/90/#%E4%BA%8Cstream-%E6%93%8D%E4%BD%9C%E5%88%86%E7%B1%BB)

Stream 操作分为 `中间操作`（Intermediate operations）、`终端操作`（Terminal operations），信息如下：

___

___

- **中间操作：** 中间操作其实就是进行逻辑处理。这个操作可以有一个或者多个连续操作，将一个流转换成另一个流，这些操作不会消耗流，其目的是建立一个流水线，直到终端操作发生后，才会做数据的最终执行。
  - **无状态：** 指数据处理时，不受之前"中间操作"的影响；
  - **有状态：** 指数据处理时，受之前"中间操作"的影响，有状态的方法往往需要更大的性能开销；
- **终端操作：** 一个 Stream 对象只能有一个终端操作（Terminal operations），这个操作一旦发生，就会真实处理数据，生成对应的处理结果。
  - **非短路操作：** 指必须处理所有元素才能得到最终结果；
  - **短路操作：** 指遇到某些符合条件的元素就可以得到最终结果；

## 三、Stream 特性[](http://www.mydlq.club/article/90/#%E4%B8%89stream-%E7%89%B9%E6%80%A7)

- **不存储数据：** Stream 不对数据进行存储，而是按照特定的规则对数据进行处理；
- **不改变数据源：** Stream 通常不会改变原有数据源，操作时一般是对原有的数据源创建副本，然后对副本进行处理，生成新的 Stream。
- **具有惰性化：** Stream 很多操作是有向后延迟的，需要一直等到它弄清楚了最后需要多少数据才会开始。而中间操作（Intermediate operations）永远是惰性化的。
- **可以是无限的：** 集合有固定大小，Stream 则不一定。limit(n) 和 findFirst() 这类的短路操作，可以对无限的 Stream 快速完成运算处理。
- **支持并行能力：** Stream 是支持并行能力的，可以轻松执行并行化对数据进行处理。
- **可被消耗的：** Stream 的生命周期中，Stream 的元素只被访问一次。与迭代器一样，必须生成新的 Stream 后， 才能重新访问与开始源中相同的元素。

## 四、数据转换为 Stream[](http://www.mydlq.club/article/90/#%E5%9B%9B%E6%95%B0%E6%8D%AE%E8%BD%AC%E6%8D%A2%E4%B8%BA-stream)

一般我们需要获取 `Stream` 对象后才能对其进行操作，下面列出了一些数据转换为 `Stream` 的常用方法：

```java
 1public class StreamExample {
 2    
 3    public static void main(String[] args){
 4        // 多个数据直接转换为 Stream
 5        Stream stream1 = Stream.of("a","b","c");
 6        
 7        // 数组转换为 Stream
 8        String[] strArrays = new String[] {"a","b"}
 9        Stream stream2 = Arrays.stream(strArrays);
10        
11        // list 列表转换为 Stream
12        List<String> strList = new ArrayList<>();
13        strList.add("a");
14        strList.add("b");
15        Stream stream3 = list.stream();
16        
17        // Set 集合转换为 Stream
18        Set<String> strSet = new HashSet<>();
19        strSet.add("a");
20        strSet.add("b");
21        Stream stream4 = strSet.stream();
22        
23        // Map 集合转换为 Stream
24        Map<String,Integer> map = new HashMap<>();
25        map.put("a", 100);
26        map.put("b", 200);
27        Stream stream5 = map.entrySet().stream();
28    }
29    
30}
```

## 五、Stream 转换得到指定类型数据[](http://www.mydlq.club/article/90/#%E4%BA%94stream-%E8%BD%AC%E6%8D%A2%E5%BE%97%E5%88%B0%E6%8C%87%E5%AE%9A%E7%B1%BB%E5%9E%8B%E6%95%B0%E6%8D%AE)

       既然可以把集合或者数组转换成流，那么也就可以把流转换回去，使用 `collect()` 方法就能实现，不过一般还需要配合 `Collectors` 工具类一起使用，`Collectors` 类中内置了一系列收集器实现，如下：

- **toList()：** 将元素收集到一个新的 List 集合中；
- **toSet()：** 将元素收集到一个新的 Set 集合中；
- **toCollection()：** 将元素收集到一个新的 ArrayList 集合中；
- **joining()：** 将元素收集到一个可以用分隔符指定的字符串中；

下面再介绍下 Stream 转换指定类型的常用示例：

**(1)、Stream 转换为数组**

```java
1 public class StreamExample {
2
3    public static void main(String[] args){
4        // 创建 Stream，将 Stream 转换为数组
5        Stream stream = Stream.of("a","b","c");
6        Object[] objectArray = stream.toArray(String::new);
7    }
8
9}
```

java

**(2)、Stream 转换位字符串**

```java
 1 public class StreamExample {
 2
 3    public static void main(String[] args){
 4        // 创建 Stream，将 Stream 转换为字符串
 5        Stream stream = Stream.of("a","b","c");
 6        String str = stream.collect(Collectors.joining).toString;
 7        System.out.println(str);
 8    }
 9
10}
```

**(3)、Stream 转换为 List 列表**

```java
1public class StreamExample {
2
3    public static void main(String[] args){
4        // 创建 Stream，将 Stream 转换为 List 列表
5        Stream stream = Stream.of("a","b","c");
6        List<String> strList = (List<String>) stream.collect(Collectors.toList());
7    }
8
9}
```

**(4)、Stream 转换为 Set 集合**

```java
1public class StreamExample {
2
3    public static void main(String[] args){
4        // 创建 Stream，将 Stream 转换为 Set 集合
5        Stream stream = Stream.of("a","b","c");
6        Set<String> strSet = (Set<String>) stream.collect(Collectors.toSet());
7    }
8
9}
```

**(5)、Stream 转换为 Map 集合**

```java
1public class StreamExample {
2
3    public static void main(String[] args){
4        // 创建 Stream，将 Stream 转换为 Map 集合
5        Stream stream = Stream.of("a","b","c");
6        Map<String, String> strMap = (Map<String, String>) stream.collect(Collectors.toMap(x -> key, y -> x + "的value值"));
7    }
8
9}
```

## 六、Stream 对于基本类型的封装[](http://www.mydlq.club/article/90/#%E5%85%ADstream-%E5%AF%B9%E4%BA%8E%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%B0%81%E8%A3%85)

       因为 `Java` 的范型不支持基本类型，所以我们无法用 `Stream<int>` 这样的类型，如果写了类似代码在编译器中会提示编译错误。为了存储 `int`，只能使用 `Stream<Integer>`，但这样会产生频繁的装箱、拆箱操作。为了提高效率，对基本类型数据 `int`、`long`、`double` 进行了封装，分别为 `IntSteam`、`LongStream`、`DoubleSteam`，使用方法和 `Stream` 类似，示例代码如下：

```java
 1public class StreamExample {
 2    
 3    public static void main(String[] args){
 4        // IntSteam
 5        IntStream.of(new int[]{10, 20, 30}).forEach(System.out::println);
 6        IntStream.range(1, 5).forEach(System.out::println);
 7        IntStream.rangeClosed(1, 5).forEach(System.out::println);
 8
 9        // LongStream
10        LongStream.of(new long[]{1L, 2L, 3L}).forEach(System.out::println);
11        LongStream.range(1, 5).forEach(System.out::println);
12        LongStream.rangeClosed(1, 5).forEach(System.out::println);
13
14        // DoubleSteam
15        DoubleStream.of(1.11, 2.23, 3.14).forEach(System.out::println);
16    }
17    
18}
```

## 七、Stream 的串行与并行[](http://www.mydlq.club/article/90/#%E4%B8%83stream-%E7%9A%84%E4%B8%B2%E8%A1%8C%E4%B8%8E%E5%B9%B6%E8%A1%8C)

### 1、Stream 的并行介绍[](http://www.mydlq.club/article/90/#1stream-%E7%9A%84%E5%B9%B6%E8%A1%8C%E4%BB%8B%E7%BB%8D)

       在 `Stream` 中，最明显的特点就是存在并行操作，不过如果使用默认方式执行中间与终端操作，那么整个执行过程其实是个串行操作。如果想让 `Stream` 并行处理数据，那么需要 `Stream` 中调用 `parallel()` 或者集合中调用 `parallelStream()` 方法来开启并行执行。

```java
1List<String> list = Arrays.asList("One", "Two", "Three", "Four", "Five");
2Stream<String> parallelStream = list.stream().parallel();
```

或者：

```java
1List<String> list = Arrays.asList("One", "Two", "Three", "Four", "Five");
2Stream<String> parallelStream = data.parallelStream();
```

其中 `Stream` 底层使用的是 `ForkJoinTask` 实现 `Stream` 的并行处理，充分利用 `CPU` 的多核能力，`Stream` 的 `API` 将底层复杂实现完全屏蔽了，开发者仅需调用一个方法即可实现并行计算。

### 2、Stream 并行示例[](http://www.mydlq.club/article/90/#2stream-%E5%B9%B6%E8%A1%8C%E7%A4%BA%E4%BE%8B)

```java
 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 并行计算
 5        long startTime = System.currentTimeMillis();
 6        long sumResult1 = LongStream.rangeClosed(1, 100000000000L).parallel().sum();
 7        System.out.println("并行执行耗时：" + (System.currentTimeMillis() - startTime));
 8        // 串行计算
 9        startTime = System.currentTimeMillis();
10        long sumResult2 = LongStream.rangeClosed(1, 100000000000L).sum();
11        System.out.println("串行执行耗时：" + (System.currentTimeMillis() - startTime));
12        // 输出汇总结果
13        System.out.println("汇总结果1 = " + sumResult1 + "，汇总结果2 = " + sumResult2);
14    }
15
16}
```

执行结果：

```
1并行执行耗时：8317
2串行执行耗时： 36651
3
4汇总结果1 = 932356074711512064 ，汇总结果2 = 932356074711512064
```

可以看到使用并行执行所花费的时间远低于串行所花费的时间，不过在使用并行执行时一定要先考虑好使用情况，考虑执行数据是否需要顺序执行，是否涉及线程安全，是否涉及使用网络等，并不是全部情况都能使用并行执行完成处理逻辑的，所以在使用之前一定要慎重，使用不好很可能会带来性能的不升反降。

一般情况下，如机器学习和数据处理等比较适合使用并行处理数据任务，其它方便需要使用者自己衡量进行测试，是否该使用并行执行任务。

## 八、Stream 中间操作（有状态）常用 API[](http://www.mydlq.club/article/90/#%E5%85%ABstream-%E4%B8%AD%E9%97%B4%E6%93%8D%E4%BD%9C%E6%9C%89%E7%8A%B6%E6%80%81%E5%B8%B8%E7%94%A8-api)

### 1、distinct[](http://www.mydlq.club/article/90/#1distinct)

保证输出的流中包含唯一的元素，通常用于数据去重。

```java
- **接口定义：**
  - Stream<T> distinct();
- **方法描述：**
  - 在 `distinct` 接口定义中不接收任何参数，该方法的作用是根据 `hashCode()` 和 `equals()` 方法来获取不同的元素，因此我们的元素必须实现这两个方法。如果 `distinct()` 正在处理有序流，那么 对于重复元素将保留处理元素时的顺序。而在处理无序流的情况下，则不一定保证元素的顺序。在有序流的并行执行情况下，保持 `distinct()` 的顺序性是需要高昂的缓冲开销。如果我们在处理元素时，不需要保证元素的顺序性，那么我们可以使用 `unordered()` 方法实现无序流。
- **使用示例：**


 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 创建 List 集合
 5        List<Integer> strList = new ArrayList<>();
 6        strList.add(1);
 7        strList.add(2);
 8        strList.add(2);
 9        strList.add(1);
10        strList.add(3);
11
12        // 执行 distinct 操作
13        strList.stream().distinct().forEach(System.out::println);
14    }
15
16}
```

### 2、sorted[](http://www.mydlq.club/article/90/#2sorted)

对数据进行排序，不过对于有序流，排序是稳定的，而对于非有序流，不保证排序稳定。

```java
- **接口定义：**
  - Stream<T> sorted();
  - Stream<T> sorted(Comparator<? super T> comparator);
- **方法描述：**
  - 使用 `Stream<T> sorted();` 方法时，它会将 `Stream` 中的元素按照 `自然排序` 方式对元素进行排序。等到将全部元素处理完成后，将元素组成新的 `Stream` 返回。
  - 使用 `Stream<T> sorted(Comparator<? super T> comparator);` 方法时，它接收的是一个 `Comparator` 类型参数，在 `Lambda` 表达式中 `Comparator<T>` 一般是用于比较两个参数，设置一个算法逻辑，执行完后返回一个整数，可以是负数、零、正整数，它的的不同的值表示两个值比较的不同，一般排序会按这个比较结果进行排序。等到将全部元素处理完成后，将元素组成新的 `Stream` 返回。
- **使用示例：**


 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 自然排序
 5        List<String> strList1 = new ArrayList<>();
 6        strList1.add("a");
 7        strList1.add("b");
 8        strList1.add("c");
 9        strList1.stream().sorted().forEach(System.out::println);
10        
11        // 指定规则排序
12        List<Integer> strList2 = new ArrayList<>();
13        strList2.add(30);
14        strList2.add(10);
15        strList2.add(20);
16        strList2.stream().sorted((x,y) -> x-y).forEach(System.out::println);
17    }
18
19}
```

java

### 3、skip[](http://www.mydlq.club/article/90/#3skip)

根据指定数值，从指定位置跳过流中某些元素。

```java
- **接口定义：**
  - Stream<T> skip(long n);
- **方法描述：**
  - 在 `skip` 接口定义中是接收 `long` 类型参数，指定要跳过前 `n` 个元素，将第 `n` 个后的元素组成新的流返回。
- **使用示例：**


 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 创建 List 集合
 5        List<Integer> strList = new ArrayList<>();
 6        strList.add(1);
 7        strList.add(2);
 8        strList.add(3);
 9        strList.add(4);
10        strList.add(5);
11
12        // 执行 peek 操作
13        strList.stream().skip(1).forEach(System.out::println);
14    }
15
16}
```

### 4、limit[](http://www.mydlq.club/article/90/#4limit)

根据指定数值，限制只能访问流中最大访问的个数。这是一个有状态的、短路方法。

```java
- **接口定义：**
  - Stream<T> limit(long maxSize);
- **方法描述：**
  - 在 `limit` 接口定义中是接收 `long` 类型参数，指定限制 `maxSize` 个元素，即将 `maxSize` 和它之前的元素组成新的流返回。
- **使用示例：**


 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 创建 List 集合
 5        List<Integer> strList = new ArrayList<>();
 6        strList.add(1);
 7        strList.add(2);
 8        strList.add(3);
 9        strList.add(4);
10        strList.add(5);
11
12        // 执行 limit 操作
13        strList.stream().limit(1).forEach(System.out::println);
14    }
15
16}
```

## 九、Stream 中间操作（无状态）常用 API[](http://www.mydlq.club/article/90/#%E4%B9%9Dstream-%E4%B8%AD%E9%97%B4%E6%93%8D%E4%BD%9C%E6%97%A0%E7%8A%B6%E6%80%81%E5%B8%B8%E7%94%A8-api)

### 1、map[](http://www.mydlq.club/article/90/#1map)

对流中的元素进行处理，然后返回，返回值的类型可以和原来的元素的类型不同。

```java
- **接口定义：**
  - Stream<R> map(Function<? super T, ? extends R> mapper);
- **方法描述：**
  - 在 `map` 接口定义中是接收 `Function` 类型参数，了解 `Lambda` 表达式就可以知道 `Function<T,R>` 是接收一个 `T` 返回处理后的值 `R`。所以，这里 `map` 方法就是对流中的元素进行处理，然后返回一个新的元素。等到将全部元素处理完成后将元素组成新的流返回。
- **使用示例：**


 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 创建 List 集合
 5        List<String> strList = new ArrayList<>();
 6        strList.add("zhangsan");
 7        strList.add("lisi");
 8        strList.add("wangwu");
 9        strList.add("zhaoliu");
10        strList.add("sunqi");
11
12        // 执行 map 操作
13        strList = strList.stream().map(x -> "测试：" + x).collect(Collectors.toList());
14        strList.forEach(System.out::println);
15    }
16
17}
```

### 2、peek[](http://www.mydlq.club/article/90/#2peek)

对流中的每个元素进行操作处理，返回的流和原来的流保存一样的元素。

```java
- **接口定义：**
  - Stream<T> peek(Consumer<? super T> action);
- **方法描述：**
  - 在 `peek` 接口定义中是接收 `Consumer` 类型参数，了解 `Lambda` 表达式就可以知道 `Consumer<T>` 是接收一个 `T` 返回处理后不返回值。所以，这里 `peek` 方法就是对流中的元素进行处理而不返回值。等到将全部元素处理完成后将元素组成新的流返回。
- **使用示例：**


 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        List<String> strList = new ArrayList<>();
 5        strList.add("zhangsan");
 6        strList.add("lisi");
 7        strList.add("wangwu");
 8        strList.add("zhaoliu");
 9        strList.add("sunqi");
10
11        // 执行 peek 操作
12        strList.stream().peek(x -> System.out.println("forEach 1：" + x))
13                .peek(x -> System.out.println("forEach 2：" + x))
14                .forEach(System.out::println);
15    }
16
17}
```

### 3、filter[](http://www.mydlq.club/article/90/#3filter)

主要用于数据过滤，过滤不符合 predicate 条件的元素，保留过滤后的元素。

```java
- **接口定义：**
  - Stream<T> filter(Predicate<? super T> predicate);
- **方法描述：**
  - 在 `filter` 接口定义中是接收 `Predicate` 类型参数，了解 `Lambda` 表达式就可以知道 `Predicate<T>` 是接收一个 `T` 进行验证，返回布尔值。所以，这里 `filter` 方法就是设置验证条件，对流中的元素进行验证，返回符合条件的全部元素组成新的流。
- **使用示例：**


 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        List<String> strList = new ArrayList<>();
 5        strList.add("zhangsan");
 6        strList.add("lisi");
 7        strList.add("wangwu");
 8        strList.add("zhaoliu");
 9        strList.add("sunqi");
10
11        // 执行 filter 操作
12        strList = strList.stream().filter(x -> x.length() > 5).collect(Collectors.toList());
13        strList.forEach(System.out::println);
14    }
15
16}
```

### 4、mapToInt[](http://www.mydlq.club/article/90/#4maptoint)

主要用于对流中元素进行一对一转换为 int 整数，然后可以进行一些求和、平均值、最大最小值等处理。

```java
- **接口定义：**
  - IntStream mapToInt(ToIntFunction<? super T> mapper);
- **方法描述：**
  - 在 `mapToInt` 接口定义中可知，它接收 `ToIntFunctio` 类型参数，在 `Lambda` 中 `ToIntFunction<T>` 函数的作用为接受一个输入参数，返回一个 `int` 类型结果，根据这点很容易了解到 `mapToInt` 方法就是将 `Stream` 中原有的元素类型转换为 `int` 类型到新的 `Stream` 中。
- **使用示例：**


 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 获取 Stream 对象
 5        Stream<Integer> stream1 = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
 6        // 获取元素最大值
 7        int max = stream1.mapToInt(x -> x).summaryStatistics().getMax();
 8        // 输出
 9        System.out.println(max);
10    }
11
12}
```

### 5、mapToDouble[](http://www.mydlq.club/article/90/#5maptodouble)

主要用于对流中元素进行一对一转换为 double 双精度浮点型，然后可以进行一些求和、平均值、最大最小值等处理。

```java
- **接口定义：**
  - IntStream mapToInt(ToDoubleFunction<? super T> mapper);
- **方法描述：**
  - 在 `mapToDouble` 接口定义中可知，它接收 `ToDoubleFunction` 类型参数，在 `Lambda` 中 `ToDoubleFunction<T>` 函数的作用为接受一个输入参数，返回一个 `int` 类型结果，根据这点很容易了解到 `ToDoubleFunction` 方法就是将 `Stream` 中原有的元素类型转换为 `double` 类型到新的 `Stream` 中。
- **使用示例：**

 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 获取 Stream 对象
 5        Stream<Integer> stream1 = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
 6        // 获取元素最大值
 7        double max = stream1.mapToDouble(x -> x).summaryStatistics().getMax();
 8        // 输出
 9        System.out.println(max);
10    }
11
12}
```

java

## 十、Stream 终端操作（短路操作）常用 API[](http://www.mydlq.club/article/90/#%E5%8D%81stream-%E7%BB%88%E7%AB%AF%E6%93%8D%E4%BD%9C%E7%9F%AD%E8%B7%AF%E6%93%8D%E4%BD%9C%E5%B8%B8%E7%94%A8-api)

### 1、anyMatch[](http://www.mydlq.club/article/90/#1anymatch)

判断数据列表中是否存在任意一个元素符合设置的 predicate 条件，如果是就返回 true，否则返回 false。

```java
- **接口定义：**
  - boolean anyMatch(Predicate<? super T> predicate);
- **方法描述：**
  - 在 `anyMatch` 接口定义中是接收 `Predicate` 类型参数，在 `Lambda` 表达式中 `Predicate<T>` 是接收一个 `T` 类型参数，然后经过逻辑验证返回布尔值结果。这里 `anyMatch` 表示，判断的条件里，任意一个元素符合条件，就返回 `true` 值。
- **使用示例：**

 1iimport java.util.stream.Stream;
 2
 3public class StreamExample {
 4
 5    public static void main(String[] args) {
 6        // 获取 Stream 对象
 7        Stream<String> stream = Stream.of("ab","bc","cd");
 8        // 判断 stream 中元素是否包含 b 内容
 9        boolean isMatch = stream.anyMatch(str->str.contains("b"));
10        // 输出
11        System.out.println(isMatch);
12    }
13
14}
```

java

### 2、allMatch[](http://www.mydlq.club/article/90/#2allmatch)

只有数据列表中全部元素都符合设置的 predicate 条件时，才返回 true，否则 flase，流为空时总是返回 true。

```java
- **接口定义：**
  - boolean allMatch(Predicate<? super T> predicate);
- **方法描述：**
  - 在 `allMatch` 接口定义中是接收 `Predicate` 类型参数，在 Lambda 表达式中 `Predicate<T>` 是接收一个 T 类型参数，然后经过逻辑验证返回布尔值结果。这里 `allMatch` 表示，判断的条件里，全部元素符合条件，就返回 `true` 值。
- **使用示例：**

 1import java.util.stream.Stream;
 2
 3public class StreamExample {
 4
 5    public static void main(String[] args) {
 6        // 获取 Stream 对象
 7        Stream<String> stream = Stream.of("ab","abc","abcd");
 8        // 判断 stream 中元素是否都是以 a 开头
 9        boolean isMatch = stream.allMatch(str->str.startsWith("a"));
10        // 输出
11        System.out.println(isMatch);
12    }
13
14}
```

java

### 3、noneMatch[](http://www.mydlq.club/article/90/#3nonematch)

只有数据列表中全部元素都不符合设置的 predicate 条件时，才返回 true，否则 flase，流为空时总是返回 true。

```java
- **接口定义：**
  - boolean noneMatch(Predicate<? super T> predicate);
- **方法描述：**
  - 在 `noneMatch` 接口定义中是接收 `Predicate` 类型参数，在 `Lambda` 表达式中 `Predicate<T>` 是接收一个 `T` 类型参数，然后经过逻辑验证返回布尔值结果。这里 `noneMatch` 表示与 `allMatch` 相反，判断条件里的元素，所有的元素都不符合，返回 `true` 值。
- **使用示例：**


 1import java.util.stream.Stream;
 2
 3public class StreamExample {
 4
 5    public static void main(String[] args) {
 6        // 获取 Stream 对象
 7        Stream<String> stream = Stream.of("ab","abc","abcd");
 8        // 判断 stream 中元素是否都不是以 a 开头
 9        boolean isMatch = stream.noneMatch(str->!str.startsWith("a"));
10        // 输出
11        System.out.println(isMatch);
12    }
13
14}
```

java

### 4、findFirst[](http://www.mydlq.club/article/90/#4findfirst)

返回第一个元素，如果流为空，返回空的 Optional 对象。

```java
- **接口定义：**
  - Optional<T> findFirst();
- **方法描述：**
  - 在 `findFirst` 方法的作用是返回集合中的第一个对象。
- **使用示例：**


 1import java.util.Optional;
 2import java.util.stream.Stream;
 3
 4public class StreamExample {
 5
 6    public static void main(String[] args) {
 7        // 获取 Stream 对象
 8        Stream<Integer> stream = Stream.of(1, 3, 5, 8, 10, 13, 15);
 9        // 通过 filter 过滤，然后获取其中第一个元素
10        Optional<Integer> first = stream.filter(x -> x > 3).findFirst();
11        // 输出
12        if (first.isPresent()){
13            System.out.println(first.get());
14        }
15    }
16
17}
```

java

### 5、findAny[](http://www.mydlq.club/article/90/#5findany)

返回任意一个元素，如果流为空，返回空的 Optional 对象。

```java
- **接口定义：**
  - Optional<T> findAny();
- **方法描述：**
  - 在 `findAny` 方法的作用是返回集合中的任何一个对象。
- **使用示例：**

 1import java.util.Optional;
 2import java.util.stream.Stream;
 3
 4public class StreamExample {
 5
 6    public static void main(String[] args) {
 7        // 获取 Stream 对象
 8        Stream<Integer> stream = Stream.of(1, 3, 5, 8, 10, 13, 15);
 9        // 通过 filter 过滤，然后获取其中第一个元素
10        Optional<Integer> first = stream.parallel().filter(x -> x > 3).findAny();
11        // 输出
12        if (first.isPresent()) {
13            System.out.println(first.get());
14        }
15    }
16
17}
```

java

## 十一、Stream 终端操作（非短路操作）常用 API[](http://www.mydlq.club/article/90/#%E5%8D%81%E4%B8%80stream-%E7%BB%88%E7%AB%AF%E6%93%8D%E4%BD%9C%E9%9D%9E%E7%9F%AD%E8%B7%AF%E6%93%8D%E4%BD%9C%E5%B8%B8%E7%94%A8-api)

### 1、max[](http://www.mydlq.club/article/90/#1max)

返回流中所有元素的最大值。

```java
- **接口定义：**
  - Optional<T> max(Comparator<? super T> comparator);
- **方法描述：**
  - 在 `max` 接口定义中是接收 Comparator 类型参数，`Lambda` 常用函数的 `Consumer<T>` 一般是用于比较两个参数，设置一个算法逻辑，执行完后返回一个整数，可以是负数、零、正整数，根据返回的值结果进行排序筛选出最大值。
- **使用示例：**


 1import java.util.Optional;
 2import java.util.stream.Stream;
 3
 4public class StreamExample {
 5
 6    public static void main(String[] args) {
 7        // 获取 Stream 对象
 8        Stream<Integer> stream = Stream.of(1, 3, 5, 8);
 9        // 求 Stream 元素中最大值
10        Optional optional = stream.max((x, y) -> x - y);
11        // 判断 Optional 中值是否为空，不为空就输出
12        if (optional.isPresent()) {
13            System.out.println(optional.get());
14        }
15    }
16
17}
```

java

### 2、min[](http://www.mydlq.club/article/90/#2min)

返回流中所有元素的最小值。

```java
- **接口定义：**
  - Optional<T> min(Comparator<? super T> comparator);
- **方法描述：**
  - 在 `max` 接口定义中是接收 `Comparator` 类型参数，`Lambda` 常用函数的 `Consumer<T>` 一般是用于比较两个参数，设置一个算法逻辑，执行完后返回一个整数，可以是负数、零、正整数，根据返回的值结果进行排序筛选出最小值。
- **使用示例：**


 1import java.util.Optional;
 2import java.util.stream.Stream;
 3
 4public class StreamExample {
 5
 6    public static void main(String[] args) {
 7        // 获取 Stream 对象
 8        Stream<Integer> stream = Stream.of(1, 3, 5, 8);
 9        // 求 Stream 元素中最小值
10        Optional optional = stream.min((x, y) -> x - y);
11        // 判断 Optional 中值是否为空，不为空就输出
12        if (optional.isPresent()) {
13            System.out.println(optional.get());
14        }
15    }
16
17}
```

java

### 3、count[](http://www.mydlq.club/article/90/#3count)

统计流中所有元素的数目。

```java
- **接口定义：**
  - long count();
- **方法描述：**
  - 该方法用于统计 `Stream` 中元素个数，返回 `long` 类型结果。
- **使用示例：**


 1import java.util.Optional;
 2import java.util.stream.Stream;
 3
 4public class StreamExample {
 5
 6    public static void main(String[] args) {
 7        // 获取 Stream 对象
 8        Stream<Integer> stream = Stream.of(1, 3, 5, 8);
 9        // 求 Stream 元素中最小值
10        Optional optional = stream.min((x, y) -> x - y);
11        // 判断 Optional 中值是否为空，不为空就输出
12        if (optional.isPresent()) {
13            System.out.println(optional.get());
14        }
15    }
16
17}
```

### 4、reduce[](http://www.mydlq.club/article/90/#4reduce)

主要用于对数据进行合并处理。

```java
- **接口定义：**
  - T reduce(T identity, BinaryOperator<T> accumulator);
  - Optional<T> reduce(BinaryOperator<T> accumulator);
  - <U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);
- **方法描述：**
  - 当使用 `Optional<T> reduce(BinaryOperator<T> accumulator);` 方法时操作 `Stream` 中的数据时，通过累加器 `accumulator` 迭代计算，最终得到一个 `T` 类型的 Optional 对象。
  - 当使用 `T reduce(T identity, BinaryOperator<T> accumulator);` 方法时，给定一个初始值 `identity`，通过累加器 `accumulator` 迭代计算，得到一个同 `Stream` 中数据同类型的结果。
  - 当使用 `<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);` 方法时，给定一个初始值 `identity`，通过累加器 `accumulator` 迭代计算，得到一个 `identity` 类型的结果，第三个参数用于使用并行流时，进行合并结果。
- **使用示例：**

 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        /* 不结合 Optional，需要设置初始值的 reduce 方法 */
 5        // 使用 reduce 进行字符串连接
 6        Stream<String> stream1 = Stream.of("a", "b", "c", "d");
 7        String concatStr = stream1.reduce("",String::concat);
 8        System.out.println(concatStr);
 9        // 使用 reduce 求最小值
10        Stream<Integer> stream2 = Stream.of(-5, 0, 3, 7, 11);
11        double minValue = stream2.reduce(Integer.MAX_VALUE, Integer::min);
12        System.out.println(minValue);
13        // 使用 reduce 求和
14        Stream<Integer> stream3 = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
15        int sumValue = stream3.reduce(0, Integer::sum);
16        System.out.println(sumValue);
17
18        /* 结合 Optional，不需要设置初始值的 reduce 方法 */
19        // 使用 reduce 拼接字符串
20        Stream<String> stream4 = Stream.of("a","b","c","d");
21        Optional<String> optional = stream4.reduce(String::concat);
22        System.out.println(optional.orElse(""));
23    }
24
25}
```

### 5、forEach[](http://www.mydlq.club/article/90/#5foreach)

主要用于对数据遍历整个流中元素，执行指定逻辑，在并发执行时不保证顺序。

```java
- **接口定义：**
  - void forEach(Consumer<? super T> action);
- **方法描述：**
  - 对 `Stream` 中的每个元素都执一段逻辑代码，例如，循环打印输出元素的值，但是需要注意的是，在并发执行时无法保证执行顺序。
- **使用示例：**


 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 获取 Stream 对象
 5        Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
 6        // 使用 forEach 输出
 7        stream.forEach(System.out::println);
 8    }
 9
10}
```

### 6、forEachOrdered[](http://www.mydlq.club/article/90/#6foreachordered)

主要用于对数据遍历整个流中元素，执行指定逻辑，在并发执行时保证顺序。

```java
- **接口定义：**
  - void forEachOrdered(Consumer<? super T> action);
- **方法描述：**
  - 作用和 `forEach` 作用类似，但是其在并发执行时以保证执行顺序。
- **使用示例：**

 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 获取 Stream 对象
 5        Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
 6        // 并发执行，使用 forEachOrdered 输出，保证输出顺序
 7        stream.parallel().forEachOrdered(System.out::println);
 8    }
 9
10}
```

### 7、toArray[](http://www.mydlq.club/article/90/#7toarray)

将 Stream 流中的数据存储到数组中。

```java
- **接口定义：**
  - Object\[\] toArray();
  - <A> A\[\] toArray(IntFunction<A\[\]> generator);
- **方法描述：**
  - 将 `Stream` 中的元素，存储到 `Object` 数组。
- **使用示例：**

 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        // 获取 Stream 对象
 5        Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
 6        // 将元素转换为 Object 数组
 7        Object[] objectArray = stream.toArray();
 8        // 输出
 9        Arrays.stream(objectArray).forEach(System.out::println);
10    }
11
12}
```

### 8、collect[](http://www.mydlq.club/article/90/#8collect)

常用的聚合方法，能将数据进行聚合操作。不仅如此，它还能与 Collector 配合使用，对聚合后的数据进行处理。

```java
- **接口定义：**
  - <R> R collect(Supplier<R> supplier,BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner);
  - <R, A> R collect(Collector<? super T, A, R> collector);
- **方法描述：** 。
  - 当使用 `<R> R collect(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner);` 方法时，它第一个参数 `supplier` 为结果存放容器，第二个参数 `accumulator` 为结果如何添加到容器的操作，第三个参数 `combiner` 则为多个容器的聚合策略。
  - 当使用 `<R, A> R collect(Collector<? super T, A, R> collector);` 方法时，这种是接收 `Collector` 类型参数，它使 `collect` 操作更加强大，对于绝大部分操作可以分解为以下主要步骤：`提供初始容器`\->`加入元素到容器`\->`并发下多容器聚合`\->`对聚合后结果进行操作`。同时 `Collector` 接口又提供了 `of` 静态方法帮助你最大化的定制自己的操作，官方也提供了 `Collectors` 这个类封装了大部分的常用收集操作。
- **使用示例：**

 1public class StreamExample {
 2
 3    public static void main(String[] args) {
 4        List<String> strList = new ArrayList<>();
 5        strList.add("a");
 6        strList.add("b");
 7        strList.add("c");
 8        List<String> asList = strList.stream().collect(ArrayList::new, ArrayList::add, ArrayList::addAll);
 9        System.out.println(asList);
10    }
11}
```

---END---

___
!版权声明：本博客内容均为原创,每篇博文作为知识积累,写博不易,转载请注明出处。
