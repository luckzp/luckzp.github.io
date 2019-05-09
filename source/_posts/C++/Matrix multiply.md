---
layout: post
title: "Matrix multiply"
date: 2019-4-29 16:14:55
comments: true
tags: 
	- C++
---
如果直接按矩阵乘法定义来计算的话需要MNP次乘法

```c
struct matrix
{
    int a[2][2];
};
matrix mul(matrix x,matrix y)
{
    matrix temp;
    memset(temp.a,0,sizeof(temp.a));
    for(int i=0;i<2;i++)
        for(int j=0;j<2;j++)
        for(int k=0;k<2;k++)
        temp.a[i][j]=(temp.a[i][j]+x.a[i][k]*y.a[k][j])%M;
    return temp;
}
matrix mpow(matrix A,int n)
{
    matrix B;
    memset(B.a,0,sizeof(B.a));
    for(int i=0;i<2;i++)
        B.a[i][i]=1;
    while(n>0)
    {
        if(n&1)
            B=mul(B,A);
        A=mul(A,A);
        n>>=1;
    }
    return B;
}
```

<!--more--> 

## 有一个产生1~5的随机数，怎么产生1~7的随机数

### 特殊：

已知有个rand7()的函数，返回1到7随机自然数，怎样利用这个rand7()构造rand10()，随机1~10。产生随机数的主要原则是每个数出现的概率是相等的，如果可以得到一组等概率出现的数字，那么就可以从中找到映射为1~10的方法。rand7()返回1~7的自然数，构造新的函数 (rand7()-1)*7 + rand7()，这个函数会随机产生1~49的自然数。原因是1~49中的每个数只有唯一的第一个rand7()的值和第二个rand7()的值表示，于是它们出现的概率是相等。但是这里的数字太多，可以丢弃41~49的数字，把1~40的数字分成10组，每组映射成1~10中的一个，于是可以得到随机的结果。具体方法是，利用(rand7()-1)*7 + rand7()产生随机数x，如果大于40则继续随机直到小于等于40为止，如果小于等于40，则产生的随机数为(x-1)/4+1。

### 一般：

已知有个randM()的函数，返回1到M随机自然数，怎样利用这个randM()构造randN()，随机1~N。上题的扩展。当N<=M时可以直接得到。当N>M时，类似构造(randM()-1)*M + randM()，可以产生1~M^2（即randM^2），可以在M^2中选出N个构造1~N的映射。如果M^2还是没有N大，则可以对于randM^2继续构造，直到成功为止。