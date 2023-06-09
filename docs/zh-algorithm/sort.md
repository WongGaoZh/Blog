# 排序算法

## 排序算法有哪些,时间复杂度如何
| 排序算法 | 时间复杂度     | 额外空间复杂度 | 稳定性 |
|------|-----------|---------|-----|
| 选择排序 | O(N^2)    | O(1)    | 无   |
| 冒泡排序 | O(N^2)    | O(1)    | 有   |
| 插入排序 | O(N^2)    | O(1)    | 有   |
| 归并排序 | O(N*logN) | O(N)    | 有   |
|      |           |         |     |


## 三大基本排序方式
> 选择排序,冒泡排序,插入排序是最基本的三个排序方式, 其他排序都是用到了
> 快速排序就是冒泡排序的一种改进
> 希尔排序是插入排序的一种改进
> 归并排序用的是分而治之的思想,不属于三大基本排序

因为网上有很多描述三大基本排序的, 所以我这里就不具体重复了. 后来我习惯用先确定那个来区分这三个排序方式, 区分如下:
```java
// 冒泡排序
// 0 ~ N
// 0 ~ N-1
// 0 ~ N-2

// 选择排序
// 0 ~ N
// 1 ~ N
// 2 ~ N
// 第一次遍历找到最小的值的下标


//插入排序
// 0~N
// 0~j |i~N 其中0~j是有序的 第一层循环
// j~N 第二层循环,是比较i位置和0-j的里面的数

```

## 归并排序
归并排序的时间复杂度 可以通过递归的master公式来算出来 , 也就是

归并排序的时间复杂度分为 :O(N*logN)
====(我跟着算了一遍,从头把我大学的高数知识捡起来了,哈哈)

具体公式不看了 , 我换个思路去分析为嘛时间复杂度是O(N*logN)

```java

原因是像 选择排序, 冒泡排序, 插入排序是在大量浪费比较行为
在每次排序额时候只会拿出来一个, 对于其他排序没有影响
而 归并排序 是左排序和右排序, 减少了排序
怎么证明:

用非递归(迭代)来实现归并排序
例子: [2,1,0,4,9,7,8,6]
也就是用步长来实现, 好比第一次步长为1的时候,
[1,2,0,4,7,9,6,8] 也就是 2和1换, 0和4换
然后在这个基础上 ,变成步长为2
[1,2,0,4,7,9,6,8] 也就是 1,2 和0,4换
变成了 [0,1,2,4,6,7,8,9]

所以时间复杂度为步长 1,2,4,
每次排序为N
也就是N * logN
```


> 核心思想:

先把数组分成两等分.
然后把两等分分别排序
排序好了以后用双指针挨个遍历两个数组, 组成新的数组也就变成 归并排序

>具体代码实现 : 

用递归来实现把数据两等分
在merge排序里面, 定义一个新的数组,
// 定义一个新的数组, help,
// [L...mid] [mid+1...R] 都是排好序的
// [help.....]
// 从两个数组里面挨个从头拿. 都放到help里面就好了


## 快速排序

> 核心思想:

先把数组分成三部分.
<X 的在左边, ==x的在中间, 小于X的在右边
x == 数组的最后一位


>具体代码实现思想 : 

{左区域}[L....R]{右区域}
[less指针为左区域最后,默认是-1] [more指针是右区域也就是R] index从L开始
第一种情况 : 如果当前指针值小于X的话, 当前值和less指针的下一位交换, less右扩, index右移
第二种情况 : 如果当前值==X的话, 那么当前值直接跳下一个
第三种情况 : 如果当前值>X的话,当前值和more指针,进行交换. 当前index不变

最后把more的第一个指针和arr[R]进行交换就完事了. 

> 时间复杂度如何变成N * logN

**关于在于每次递归之前,先随机拿到一个数和arr[R]进行交换**
证明 : 
上面情况的最好情况是 [1,3,2,7,6,5,4] 每次最后一个数都是正中间的数
上面情况的最坏情况是 [1,2,3,4,5,6,7] 每次最后一个数都是最边上的数
因为用了随机数替换最后一位,那么说明了
最好情况和最坏情况中间的每个情况,都有N分之一的概率
求概率叠加,最终能证明收敛到O(N*logn)



## 堆排序

### 一些概念
```java
1_堆结构就是用数组实现的完全二叉树结构
2_完全二叉树中如果每颗子树的最大值都在顶部就是大根堆
3_完全二叉树中如果每颗子树的最小值都在顶部就是小跟堆
4_堆结构中heapInsert 与 heapify操作
5_堆结构的增大和减少
6_优先级队列结构就是堆排序
```

> 核心思想 :

arr[0,1,2,3,4,5,6,7,8,9]
来表示堆的话, 
左子树 : 2*i+1
右子树 : 2*i+2
父节点 : (i-1)/2
数组的size可以来判断完全二叉树的深度

> 具体代码实现 :

heapInsert流程 :
// 新加进来的数，现在停在了index位置，请依次往上移动，
// 移动到0位置，或者干不掉自己的父亲了，停！

```java
public static void heapInsert(int[] arr, int index) {
		while (arr[index] > arr[(index - 1) / 2]) {
			swap(arr, index, (index - 1) / 2);
			index = (index - 1) / 2;
		}
	}
```
heapify流程 : 
// 从index位置，往下看，不断的下沉
// 停：较大的孩子都不再比index位置的数大；已经没孩子了

```java
public static void heapify(int[] arr, int index, int heapSize) {
		int left = index * 2 + 1; // 左孩子的下标
		while (left < heapSize) { // 下方还有孩子的时候
			// 两个孩子中，谁的值大，把下标给largest
			// 1）只有左孩子，left -> largest
			// 2) 同时有左孩子和右孩子，右孩子的值<= 左孩子的值，left -> largest
			// 3) 同时有左孩子和右孩子并且右孩子的值> 左孩子的值， right -> largest
			int largest = left + 1 < heapSize && arr[left + 1] > arr[left] ? left + 1 : left;
			// 父和较大的孩子之间，谁的值大，把下标给largest
			largest = arr[largest] > arr[index] ? largest : index;
			if (largest == index) {
				break;
			}
			swap(arr, largest, index);
			index = largest;
			left = index * 2 + 1;
		}
	}
```








