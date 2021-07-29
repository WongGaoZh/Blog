## 遍历数组(数据结构)



```
    int[] arr = {1,2,3,4,5};
    for(int i=0;i<arr.length;i++){
        System.out.print(arr[i] + " ");
    }
```



## 遍历链表(数据结构)
```
public void Ergodic() {
		ListNode indexNode = head;
		while (indexNode.getNext() != null) {
			System.out.print(indexNode.getVal()+" ");
			indexNode = indexNode.getNext();
		}
	}
```

## 遍历数组(Java类)
foreach和iteator迭代器是一样的,因为foreach的底层用的是iteator的迭代器

arraylist用:

推荐使用普通for循环

## 遍历链表(Java实体类)
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
