
### Java易混淆知识点

#### Java多态

概念: 事物在运行过程中存在不同的状态

原理: 

前提: 多态的存在有三个条件,1. 要有继承关系,2 子类重写父类的方法,3.父类引用指向子类

应用:

成员变量:  会拿到父类的,

成员方法: 会拿到子类的

静态方法:  会拿到父类的

---

#### Java怎么获取list长度

使用length时，你正在处理一个数组(int[])。

使用length()时，你正在处理一个字符串或类似字符串的序列。

使用size()时，你正在处理一个集合（如列表、集合、映射等）。

---

#### 创建数组的几种方式
```
// 静态初始化
    int[] intArray2  = new int []{20,21,22};
// 静态初始化简化方式
    int[] intArray3  = {30,31,32};
// 动态初始化
    int[] intArray4  = new int [3]; 
-----------------
// 错误写法：静态初始化不能指定元素个数
    int[] intErrorArray5 = new int[3]{50,51,52};
// 错误写法：动态初始化必须指定元素个数
    int[] intErrorArray6 = new int[];

```
#### Java的恒等判断 ==与equals()的区别
 + ==:比较引用类型比较的是地址值是否相同
 + equals:比较引用类型默认也是比较地址值是否相同，注意:String类重写了equals()方法，比较的是内容是否相同。


#### 
第一个问题: 静态代码块,静态方法,普通代码块, 构造方法,普通方法

静态代码块:
public class CodeBlock {
    static{
        System.out.println("静态代码块");
    }
    {
        System.out.println("构造代码块");
    }
    
    public CodeBlock (){
        System.out.println("构造方法");
    }
    
    public void test(){
        System.out.println("普通方法");
    }
    
    public  static void test(){
        System.out.println("静态方法");
    }
}



重点是: 静态方法和普通方法是只有被调用的时候才会加载

然后其他三个加载顺序是 静态代码块要最早执行,
其他两个是按顺序执行的



