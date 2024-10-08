## 96_Java开发数据部分笔记

### 遍历数组(数据结构)

```
    int[] arr = {1,2,3,4,5};
    for(int i=0;i<arr.length;i++){
        System.out.print(arr[i] + " ");
    }
```
### 遍历链表(数据结构)
```
public void Ergodic() {
		ListNode indexNode = head;
		while (indexNode.getNext() != null) {
			System.out.print(indexNode.getVal()+" ");
			indexNode = indexNode.getNext();
		}
	}
```

### 遍历数组(Java类)
foreach和iteator迭代器是一样的,因为foreach的底层用的是iteator的迭代器

arraylist用:

推荐使用普通for循环

### 遍历链表(Java实体类)
Java中LinkedList底层是链表结构

linkedList用:
推荐使用迭代器遍历
```
for (Iterator it2 = arrL.iterator(); it2.hasNext(); ) {
    arrLTmp2.add(it2.next());
}
```

首先使用IntelliJ IDEA软件的反编译功能对上述的案例的class文件进行反编译，反编译后的案例代码如下：

这里需要注意的一个是,对数组进行增强for遍历其实底层实现就是运用了普通数组遍历是采用的带索引下标的迭代（遍历）：

集合使用增强for遍历时其底层实现方式: 其原理就是获取迭代器（Iterator）对集合元素进行迭代操作：

## 遍历hashMap
```
推荐使用entrySet的迭代器遍历  
//第三种 entrySet的迭代器遍历
        Iterator<Map.Entry<Integer, String>> iterator1 = map2.entrySet().iterator();
        while (iterator1.hasNext()){
            Map.Entry<Integer, String> entry = iterator1.next();
            entry.getValue();
        }
```


### 新建list,Map等
```aidl
# 创建一个list
List<String> list = new ArrayList<>(); // 普通方式
Arrays.asList(32.5,78.3,45.6); //这种方式创建出来的数组是固定长度的.不推荐使用
Lists.newArrayList(20304, 30304, 40304, 50106);
Stream.of(56.3, 3.64, 4.65).collect(toList());
```

### 判断null

```java
# String判空
StringUtils.isNotEmpty(str);
StringUtils.isBlank();  
StringUtils.isBlank 比isEmpty多的是,假如有空格,isBlank依旧可以判断出来

# 集合判空-list
CollectionUtils.isEmpty();

# 使用ObjectUtils工具类（org.springframework.util）可以自动根据类型判断空，请看源码

public static boolean isEmpty(Object object) {
        if (object == null) {
        return true;
        } else if (object instanceof CharSequence) {
        return ((CharSequence)object).length() == 0;
        } else if (object.getClass().isArray()) {
        return Array.getLength(object) == 0;
        } else if (object instanceof Collection) {
        return ((Collection)object).isEmpty();
        } else {
        return object instanceof Map ? ((Map)object).isEmpty() : false;
        }
}

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



        //应用  把list变成一个复杂的map集合
Map<String, ManagedEntityStatus> alarmStateMap = 
        alarmStateList.stream().collect(Collectors.toMap(x -> x.getAlarm().getValue(), x -> x.getOverallStatus()));
Map<Integer, Student> map = 
        list.stream().collect(Collectors.toMap(Student::getId, student -> student));

        // 对数据进行处理 通过groupingBy 把其中一项提出来做 map 的key

        List<TSsfwClzyLbInfoCl> cllistBatch = materialSaveAllRepository.getCllistBatch(batchList);
        Map<String, List<TSsfwClzyLbInfoCl>> tSsfwClzyLbInfoClList =
        cllistBatch.stream().collect(Collectors.groupingBy(TSsfwClzyLbInfoCl::getCAjbh));
        
        //map和flatmap的区别
        //Stream.flatMap，正如它的名称所猜测的，是map和一个flat行动。这意味着您首先对元素应用一个函数，然后将其扁平化。Stream.map只对流应用函数，而不对流进行平坦处理。

        //为了理解什么是扁平化，考虑一个像[[1,2,3]，[4,5,6]，[7,8,9]]这样的具有“两个层次”的结构。 扁平化意味着将其转化为“一个一级”结构：[1,2,3,4,5,6,7,8,9]。


        


        // 把List里面的list封装成map  ==(  )
Map<String, AjbhDsrListDTO> collect = ajBhDsrDTO.stream()
      .flatMap(e -> e.getDsrList().stream())
      .collect(Collectors.toMap(AjbhDsrListDTO::getZjhm, x -> x));


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