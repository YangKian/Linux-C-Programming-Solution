# Linux C编程一站式学习

## Chapter 1.1

### 习题 1.解释执行的语言相比编译执行的语言有什么优缺点？

- 优点：
  - 少了编译的过程，调试速度更快；
  - 具有平台无关性。
- 缺点：
  - 缺少编译生成的中间文件，执行效率上不如编译执行的语言高。



## Chapter 2.2

### 习题1. 总结前面介绍的转义序列的规律，想想在printf的格式化字符串中怎么表示一个%字符？写个小程序实验一下。

```c
#include <stdio.h>
int main(void) {
    printf("this is a %%");
    return 0;
}
```



## Chapter 2.5

### 习题1. 假设变量x和n是两个正整数，我们知道x/n这个表达式的结果是取Floor，例如x是17， n是4。如果希望结果去Ceiling应该怎么写表达式呢？例如x是17， n是4， 则结果是5，而x是16，n是4，则结果是4。

```c
#include <stdio.h>
int ceiling(int a, int b) {
    return a % b == 0 ? a / b : a / b + 1;
}

int main(void) {
    int res = ceiling(17, 4);
    printf("17/4 = %d\n", res);
    res = ceiling(16, 4);
    printf("16/4 = %d\n", res);
   return 0;
}
```



## Chapter 3.3

### 习题1. 定义一个函数increment，它的作用是将传进来的参数加1，然后在main函数中用increment函数来增加变量的值，这个increment函数能奏效吗？为什么？

>```c
>void increment(int x)
>{
>	x = x + 1;
>}
>
>int main(void)
>{
>	int i = 1, j = 2;
>	increment(i); /* i now becomes 2 */
>	increment(j); /* j now becomes 3 */
>	return 0;
>}
>```

A：不能奏效。传入的参数是int类型，即传入的是值的拷贝，不是传引用，increment函数没有返回值，故在increment函数执行完毕后，其内存被释放，increment函数内计算的结果作为increment函数的局部变量也会被销毁，无法传到main函数中。



### 习题2.如果在一个程序中调用了printf函数却不包含头文件，例如`int main(void)  { printf("\n"); }`，编译时会报警告：`warning: incompatible implicit declaration of built-in function 'printf'`。请分析错误原因。

A：根据警告提示的内容：隐式声明与内置的`printf`函数不一致。查询`printf`的函数可知，函数签名为：`int printf( const char* format , [argument] ... );`，而隐式声明的格式为：`int func(arg)`，即两个签名的参数个数不相同，所以报错。



## Chapter 4.1

### 习题1. 以下是程序段编译能通过，执行也不出错，但是执行结果不正确，请分析一下哪里错了。还有，既然错了为什么编译能通过呢？

>int x = -1;
>
>if(x >0);
>
>​	printf("x is positive.\n")

A：编译检查的是语法错误，而该段代码中语法完全正确，所以编译能通过。执行结果不正确是因为代码逻辑不正确，if语句后面直接跟了`;`号表明该语句已经结束，所以不管判断结果如何print语句一定会执行。代码逻辑不正确编译器是无法检查发现的。



## Chapter 4.2

### 习题1.写两个表达式，分别取分别取整型变量`x`的个位和十位。

```c
int unit = x % 10; //取个位
int decade = x / 10 % 10; //取十位
```



### 习题2.写一个函数，参数是整形变量x，功能是打印x的个位和十位。

```c
#include <stdio.h>
void printFunc (int x) {
    printf("数字%d的个位数是：%d,十位数是：%d",  x,  x%10, x/10%10);
}
```



## Chapter 4.3

### 习题1.把代码段

```
if (x > 0 && x < 10);
else
	printf("x is out of range.\n");
```

### 改写成下面这种形式：

```
if (____ || ____)
	printf("x is out of range.\n");
```

### ____应该怎么填？

A：$x\le0$，$x\ge10$；



### 习题2.把代码段：

```
if (x > 0)
	printf("Test OK!\n");
else if (x <= 0 && y > 0)
	printf("Test OK!\n");
else
	printf("Test failed!\n");
```

### 改写成下面这种形式：

```
if (____ && ____)
	printf("Test failed!\n");
else
	printf("Test OK!\n");
```

### ____应该怎么填？

A：$x\le0$，$y\le0$



### 习题3.有这样一段代码：

```
if (x > 1 && y != 1) {
	...
} else if (x < 1 && y != 1) {
	...
} else {
	...
}
```

### 要进入最后一个`else`，x和y需要满足条件____ || ____。这里应该怎么填？

A：$x == 1$, $y == 1$



### 习题4.以下哪一个if判断条件是多余的可以去掉？这里所谓的“多余”是指，某种情况下如果本来应该打印`Test OK!`，去掉这个多余条件后仍然打印`Test OK!`，如果本来应该打印`Test failed!`，去掉这个多余条件后仍然打印`Test failed!`。

```
if (x<3 && y>3)
	printf("Test OK!\n");
else if (x>=3 && y>=3)
	printf("Test OK!\n");
else if (z>3 && x>=3)
	printf("Test OK!\n");
else if (z<=3 && y>=3)
	printf("Test OK!\n");
else
	printf("Test failed!\n");
```

A：去掉的条件是：`else if (z<=3 && y>=3)`



## Chapter 5.1

### 习题1. 编写一个布尔函数`int is_leap_year(int year)`，判断参数 year 是不是闰年。如果某年份能被4整除，但不能被100整除，那么这一年就是闰年，此外，能被400整除的年份也是闰年。

```c
#include <stdio.h>
void is_leap_year (int year) {
    if((year % 4 == 0 && year % 100 != 0) || year % 400 == 0) {
		printf("%d年是闰年", year);
    } else {
		printf("%d年不是闰年", year);
    }
}
```



### 习题2.编写一个函数`double myround(double x)`，输入一个小数，将它四舍五入。例如`myround(-3.51)`的值是-4.0，`myround(4.49)`的值是4.0。可以调用`math.h`中的库函数`ceil`和`floor`实现这个函数。

```c
#include <stdio.h>
#include <math.h>
double myround (double x) {
    if(x>=0) {
	if((int)x * 10 % 10 >= 5) {
	    return ceil(x);
	} else {
	    return floor(x);
	} 
    } else {
	if(((int)(-x * 10)) % 10 >= 5) {
	    printf("-3.51来到了floor函数中\n");
	    return floor(x);
	} else {
	    return ceil(x);
    }
  } 
}
```



## Chapter 5.3

### 习题1：编写递归函数求两个正整数`a`和`b`的最大公约数（GCD，Greatest Common Divisor），使用Euclid算法：

1. 如果`a`除以`b`能整除，则最大公约数是`b`。
2. 否则，最大公约数等于`b`和`a%b`的最大公约数。

### Euclid算法是很容易证明的，请读者自己证明一下为什么这么算就能算出最大公约数。最后，修改你的程序使之适用于所有整数，而不仅仅是正整数。

```c
#include <stdio.h>
int GCD(int a, int b) {
    if(a % b == 0) {
	return b;
    } else {
	return GCD(b, a % b);
    }
}
```



### 习题2：编写递归函数求Fibonacci数列的第`n`项，这个数列是这样定义的：

fib(0)=1
fib(1)=1
fib(n)=fib(n-1)+fib(n-2)

```c
#include <stdio.h>
int fib(int n) {
    if(n < 2) return 1;
    return fib(n - 1) + fib(n - 2);
}
```



## Chapter 6.1

### 习题1：用循环解决[第 3 节 “递归”](https://akaedu.github.io/book/ch05s03.html#func2.recursion)的所有习题，体会递归和循环这两种不同的思路。

```c
#include <stdio.h>
int fib(int n) {
    int a = 1, b = 1;
    if(n < 2) return 1;
    for(int i = 2; i <= n; ++i) {
	int temp = a;
	a += b;
	b = temp;
    }
    return a + b;
}
```

```c
int GCD(int a, int b){
    while(a % b != 0) {
	int temp = a;
	a = b;
	b = temp % b;
    }
    return b;
}
```



### 习题2：

```c
#include <stdio.h>
int count(){
    int sum = 0;
    for(int i = 1; i <= 100; ++i) {
	int a = i;
       while(a != 0) {
	   if(a % 10 == 9) {
	       ++sum;
	   }
	   a /= 10;
       }
    }
    return sum;
}
```



## Chapter 6.4

### 习题1：求素数这个程序只是为了说明`break`和`continue`的用法才这么写的，其实完全可以不用`break`和`continue`，请读者修改一下控制流程，去掉`break`和`continue`而保持功能不变。

```c
#include <stdio.h>
int is_prime(int n){
    for(int i = 2; i < n; ++i){
	if(n % i == 0){
	    return 0;
	}
    }
    return 1;
}
```



## Chapter 6.5

### 习题1：上面打印的小九九有一半数据是重复的，因为8*9和9*8的结果一样。请修改程序打印这样的小九九：

```
1	
2	4	
3	6	9	
4	8	12	16	
5	10	15	20	25	
6	12	18	24	30	36	
7	14	21	28	35	42	49	
8	16	24	32	40	48	56	64	
9	18	27	36	45	54	63	72	81
```

```c
#include <stdio.h>
void printFunc(int n) {
    for(int i = 1; i <= n; ++i) {
	for(int j = 1; j <= i; ++j) {
	    printf("%d\t", i*j);
	}
	printf("\n");
    }
}
```



### 习题2：编写函数`diamond`打印一个菱形。如果调用`diamond(3, '*')`则打印：

```
	*
*	*	*
	*
```

### 如果调用`diamond(5, '+')`则打印：

```
		+
	+	+	+
+	+	+	+	+
	+	+	+
		+
```

### 如果用偶数做参数则打印错误提示。

```c
#include <stdio.h>
void printFunc(int n, char b) {
    if(n % 2 == 0) {
	printf("不能使用偶数做参数"); 
	return;
    }

    int count = -1;  // count用来记录每行打印字符的个数
    for(int i = 1; i <= n; i ++) {
	if(i <= (n >> 1) + 1){
	   count += 2;
	} else {
	   count -= 2;
	}
	
	int print_start = (n - count) >> 1;  // print_start用来记录开始打印的位置
	for(int j = 1; j <= n; ++j) {
	    if(j <= print_start || j > (n - print_start)) {
		printf("\t");
	    } else {
		printf("%c\t", b);
	     }
	}
	printf("\n");
    }
}
```



## Chapter 7.2

### 习题1.在本节的基础上实现一个打印复数的函数，打印的格式是x+yi，如果实部或虚部为0则省略，例如：1.0、-2.0i、-1.0+2.0i、1.0-2.0i。最后编写一个`main`函数测试本节的所有代码。想一想这个打印函数应该属于上图中的哪一层？

```c
void printFunc(struct complex_struct z) {
    if(real_part(z) == 0 && img_part(z) == 0) {
	printf("0\n");
    } else if(real_part(z) == 0) {
	printf("%fi\n", img_part(z));
    } else if(img_part(z) == 0) {
	printf("%f\n", real_part(z));
    } else {
	if(img_part(z) > 0) {
	    printf("%f + %fi\n", real_part(z), img_part(z));
	} else {
	    printf("%f - %fi\n", real_part(z), -img_part(z));
	}
    }
}
```

属于存储表示层。

### 习题2：实现一个用分子分母的格式来表示有理数的结构体`rational`以及相关的函数，`rational`结构体之间可以做加减乘除运算，运算的结果仍然是`rational`。

```c
#include <stdio.h>
struct Rational {
    int member, denominator;
};

struct Rational make_Rational(int member, int denominator) {
    struct Rational r;
    r.member = member;
    r.denominator = denominator;
    return r;
}

struct Rational add_Rational(struct Rational a, struct Rational b) {
    struct Rational r;
    int gcd = 0, lcm = 0;
    gcd = maxvalue(a.denominator, b.denominator); //求最大公约数
    lcm = a.denominator * b.denominator / gcd; //求最小公倍数
    r.member = a.member * lcm / a.denominator + b.member * lcm / b.denominator;
    r.denominator = lcm;
    return r;
}

struct Rational sub_Rational(struct Rational a, struct Rational b) {
    struct Rational r;
    int gcd = 0, lcm = 0;
    gcd = maxvalue(a.denominator, b.denominator); //求最大公约数
    lcm = a.denominator * b.denominator / gcd; //求最小公倍数
    r.member = a.member * lcm / a.denominator - b.member * lcm / b.denominator;
    r.denominator = lcm;
    return r;
}

struct Rational mul_Rational(struct Rational a, struct Rational b) {
    struct Rational r;
    int member = a.member * b.member;
    int denominator = a.denominator * b.denominator;
    r.member = member;
    r.denominator = denominator;
    return r;
}

struct Rational div_Rational(struct Rational a, struct Rational b) {
    struct Rational r;
    int member = a.member * b.denominator;
    int denominator = a.denominator * b.member;
    r.member = member;
    r.denominator = denominator;
    return r;
}

void print_Rational(struct Rational z){
    if(z.member == 0) {
	printf("0\n");
	return;
    }
    int gcd = maxvalue(z.member, z.denominator);
    if(z.denominator / gcd == 1) {
	printf("%d\n", z.member / gcd);
    } else {
	printf("%d/%d\n", z.member / gcd, z.denominator / gcd);
    }
}

int maxvalue(int a, int b) {
    if(a % b == 0) {
	return b;
    } else {
	return maxvalue(b, a % b);
    }
}
```



## Chapter 7.3

### 习题1：本节只给出了`make_from_real_img`和`make_from_mag_ang`函数的实现，请读者自己实现`real_part`、`img_part`、`magnitude`、`angle`这些函数。

```c
#include <math.h>
#include <stdio.h>
double real_part(struct complex_struct z)
{
    if(z.t == RECTANGULAR) {
	return z.a;
    } else {
	return z.a * cos(z.b);
    }
}

double img_part(struct complex_struct z)
{
    if(z.t == RECTANGULAR) {
	return z.b;
    } else {
	return z.a * sin(z.b);
    }
}

double magnitude(struct complex_struct z) {
    if(z.t == RECTANGULAR) {
	return sqrt(z.a * z.a + z.b * z.b);
    } else {
	return z.a;
    }
}

double angle(struct complex_struct z) {
    if(z.t == RECTANGULAR) {
	double ang = acos(-1.0);
	if(z.a > 0) {
	    return atan(z.b / z.a);
	} else {
	    return atan(z.b / z.a) + ang;
	}
    } else {
	return z.b;
    }
}
```



### 习题2：编译运行下面这段程序：

```
#include <stdio.h>

enum coordinate_type { RECTANGULAR = 1, POLAR };

int main(void)
{
    int RECTANGULAR;
    printf("%d %d\n", RECTANGULAR, POLAR);
    return 0;
}
```

### 结果是什么？并解释一下为什么是这样的结果。

A：结果是：0 2，原因是：定义了一个局部变量RECTANGULAR，但是没有给它赋值，导致访问到了未知的内存空间内存储的值。同时，该值还覆盖了全局枚举变量RECTANGULAR中的正确值。
