## 静态代码块 {#blogTitle2}

```
static
 {
//
静态代码块    

}
```

关于静态代码块，要注意的是：

1. 它是
   **随着类的加载而执行，只执行一次，并优先于主函数**
   。具体说，
   **静态代码块是由类调用**
   的。类调用时，先执行静态代码块，然后才执行主函数的。
2. **静态代码块其实就是给类初始化的，而构造代码块是给对象初始化的**
   。
3. 静态代码块中的变量是局部变量，与普通函数中的局部变量性质没有区别。
4. 一个类中可以有多个静态代码块

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

```
public class Test{
staitc int cnt=6;
static{
      cnt+=9;
}
public static void main(String[] args) {
      System.out.println(cnt);
}
static{
      cnt/=3;
}
}
运行结果：5


public class HelloA {
    public HelloA(){//构造函数
        System.out.println("A的构造函数");    
    }
    {//构造代码块
        System.out.println("A的构造代码块");    
    }
    static {//静态代码块
        System.out.println("A的静态代码块");        
    }
    public static void main(String[] args) {
    }
}
运行结果：
A的静态代码块
```



