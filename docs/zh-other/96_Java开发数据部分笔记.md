## 96_Java开发数据部分笔记

### 新建list,Map等
```aidl
# 创建一个list
List<String> list = new ArrayList<>(); // 普通方式
Arrays.asList(32.5,78.3,45.6); //这种方式创建出来的数组是固定长度的.不推荐使用
Lists.newArrayList(20304, 30304, 40304, 50106);
Stream.of(56.3, 3.64, 4.65).collect(toList());


# 创建一个map
HashMap<String, String> map = new HashMap<String, String>();

```

### 判断null

```java
# String判空
StringUtils.isNotEmpty(str);
StringUtils.isBlank();  
StringUtils.isBlank 比isEmpty多的是,假如有空格,isBlank依旧可以判断出来

# 集合判空
CollectionUtils.isEmpty();

```

### Map遍历取值

```java
Optional<TracerReceiveInnerRequest> opTriReq = Optional.ofNullable(request);
        byte[] imgBytes = opTriReq.map(TracerReceiveInnerRequest::getProcessorRequestApiBody)
                .map(ProcessorRequestApiBody::getImageMattingList)
                .map(item -> item.get(0))
                .map(ImageMatting::getImage)
                .orElse(new byte[0]);
```

### 1.8版本Stream的常见操作
```java
//使用stream的写法
        /*
         * 1.获取集合的stream对象
         * 2.使用filter方法完成过滤
         * 3.使用sort方法完成排序
         * 4.使用collect方法将处理好的stream对象转换为集合对象
         *  补充 : groupingBy是分组 
         */
        result1 = stuList.stream()
                .filter(s -> s.getScore()>=90)
                //.sorted((s1,s2) -> Integer.compare(s2.getScore(), s1.getScore()))
                //使用Comparator中的comparing方法
                .sorted(Comparator.comparing(Student :: getScore).reversed())
                .collect(Collectors.toList());
        System.out.println(result1);

//stream的map和reduce 
  map用来归类，结果一般是一组数据，比如可以将list中的学生分数映射到一个新的stream中。
  reduce用来计算值，结果是一个值，比如计算最高分。
  //使用map方法获取list数据中的name
        List<String> names = list.stream().map(Student::getName).collect(Collectors.toList());
        System.out.println(names)
  //计算学生总分
        Integer totalScore1 = list.stream().map(Student::getScore).reduce(0,(a,b) -> a + b);
        System.out.println(totalScore1);

//应用  把list变成一个复杂的map集合
Map<String, ManagedEntityStatus> alarmStateMap = alarmStateList.stream().collect(Collectors.toMap(x -> x.getAlarm().getValue(), x -> x.getOverallStatus()));



map和flatmap的区别
Stream.flatMap，正如它的名称所猜测的，是map和一个flat行动。这意味着您首先对元素应用一个函数，然后将其扁平化。Stream.map只对流应用函数，而不对流进行平坦处理。

为了理解什么是扁平化，考虑一个像[[1,2,3]，[4,5,6]，[7,8,9]]这样的具有“两个层次”的结构。 扁平化意味着将其转化为“一个一级”结构：[1,2,3,4,5,6,7,8,9]。


对数据进行处理 通过groupingBy 把其中一项提出来做 map 的key

 List<TSsfwClzyLbInfoCl> cllistBatch = materialSaveAllRepository.getCllistBatch(batchList);
 Map<String, List<TSsfwClzyLbInfoCl>> tSsfwClzyLbInfoClList = cllistBatch.stream().collect(Collectors.groupingBy(TSsfwClzyLbInfoCl::getCAjbh));




把list变成map
Map<Integer, Student> map = list.stream().collect(Collectors.toMap(Student::getId, student -> student));


把List里面的list封装成map  ==(  )
Map<String, AjbhDsrListDTO> collect = ajBhDsrDTO.stream()
      .flatMap(e -> e.getDsrList().stream())
      .collect(Collectors.toMap(AjbhDsrListDTO::getZjhm, x -> x));


```


### 判断字符串中是否包括某个字符
| 正则的规则
| .代表任意字符,d代表数字,w代表汉子,标点符号
| *代表匹配{0,} --- ? 代表匹配{0,1} ---- +代表匹配{1,}
```java
第一种使用 indexOf（）方法
Str.indexOf("abc")；


第二种是:使用正则
String str ="hello world";
String reg = ".*ll.*";
sout(Pattern.matchers(reg,str))); 或者 sout(str.matches(reg)))
```





### 断言
```
Assert.notNull(attributes, () -> {
            return "No auto-configuration attributes found. Is " + metadata.getClassName() + " annotated with " + ClassUtils.getShortName(name) + "?";
        });
```

### Spring计时器StopWatch使用

```
public static void main(String[] args) {
        StopWatch sw = new StopWatch("test");
        try {
            //启动任务一
            sw.start("任务一");
            TimeUnit.SECONDS.sleep(1);
            sw.stop();
            
            //启动任务二
            sw.start("任务二");
            TimeUnit.SECONDS.sleep(3);
            sw.stop();
            System.out.println(sw.prettyPrint());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
```