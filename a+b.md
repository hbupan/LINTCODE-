#### 题目表述
给出两个整数 aa 和 bb , 求他们的和

#### 题解
##### 常规版本
使用变成语言自带的数学运算符号进行计算
```python
def sum(a, b):
    return a + b
```

##### 进阶版本
不使用运算符号进行计算，使用位运算进行计算，分析加法中的规律可以发现：

| a | b | sum | carry |
| --- | --- | --- | --- |
| 0 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 1 | 0 | 1 |

上表中发现，sum就是两个数字的异或，进位则是按位与的结果。求和分成两部分一部分是不考虑进位的结果，另一部分是进位的结果，两者最终再求和就得到最终的结果。

```java
public class Solution {
    /**
     * @param a: An integer
     * @param b: An integer
     * @return: The sum of a and b 
     */
    
    public int aplusb(int a, int b) {
        // write your code here
        int sum = 0;
        while(b!=0){
            sum = a^b;
            b = (a&b) << 1;
            a = sum;
        }
        return a;
    }
}
```

#### 扩展 使用位运算实现四则运算
##### 1. 减法的实现
加法的逆运算就是减法的实现，因为只需要考虑负数的二进制表示 补码的表示，负数则是对应的正数按位去反加1得到最终的补码。按位取反符号 ～
```java
public class Solution {
    /**
     * @param a: An integer
     * @param b: An integer
     * @return: The sum of a and b 
     */
    
    public int aplusb(int a, int b) {
        // write your code here
        int sum = 0;
        while(b!=0){
            sum = a^b;
            b = (a&b) << 1;
            a = sum;
        }
        return a;
    }
    
    public int aminisb(int a, int b) {
        return aplus(a, aplusb(~b,1))
    }
}
```

##### 2.乘法的实现
> 简单的版本：乘法的实现可以参照加法，毕竟乘法就是n个相同的数字加起来的结果，因此可以反复调用n次两个数相加的结果，得倒最终的结果。同时需要考虑正负号的问题，加上符号后得到最终的结果。

> 手动模拟乘法的运算法则，菪乘数第n位对应的位置是1时则被乘数向左移动n位，对应位置位0则不移动，最终把移位的结果相加得到最终的结果。
> ![9a885bd7c78f89a6f65b2fc32ce3ac70.png](evernotecid://10F4D728-684E-4D33-B375-28262BEF3D70/appyinxiangcom/15062839/ENResource/p356)@w=200
```java
public class Solution {
    public int multiply(int a, int b){
        int abs_a = a < 0 ? aplusb(~a, 1) : a;
        int abs_b = b < 0 ? aplusb(~b, 1) : b;
        
        int result = 0;
        while(abs_b){
            if(abs_b & 1){
                result = aplusb(result, abs_a);
            }
            abs_b = abs_b >> 1;
            abs_a = abs_a << 1;
        }
        if((a^b) < 0){
            result = aplusb(~result, 1);
        }
        return result;
    }
}
```

##### 3.除法运算
除法可以看作是多次减法。


#### 总结
位运算的基本操作方法有限，分为
* 运算类，& 按位与，^ 按位异或， | 按位或运算, ~ 按位取反
* 移动类，<< 左移运算， >> 右移运算
在使用按位运算时就是模拟最原始的计算机的基础硬件的运算。
更多位运算的骚操作阅读[位运算简介及实用技巧](http://www.matrix67.com/blog/archives/263)
