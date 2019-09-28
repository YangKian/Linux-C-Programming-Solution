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

### 习题1：用循环解决第 3 节“递归”的所有习题，体会递归和循环这两种不同的思路。

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



## Chapter 8.1

### 习题1：编写一个程序，定义两个类型和长度都相同的数组，将其中一个数组的所有元素拷贝给另一个。既然数组不能直接赋值，想想应该怎么实现。

```c
#include <stdio.h>
#define N 10

void myCopy(int a[], int b[]) {
    for(int i = 0; i < N; ++i) {
		b[i] = a[i];
    }
}
```



## Chapter 8.2

### 习题1：用`rand`函数生成[10, 20]之间的随机整数，表达式应该怎么写？

A：`int x = rand() % 11 + 10;`



## Chapter 8.3

### 习题1：补完本节直方图程序的`main`函数，以可视化的形式打印直方图。

```c
int main(void) {
    int i, histogram[10] = {0}, count = N;

    gen_random(10);
    for(i = 0; i < N; i++) {
		histogram[a[i]]++;
    }

    for(int i = 0; i < 10; ++i) {
		printf("%d\t", i);
    }
    
    printf("\n");

    while(count > 0) {
		for(int i = 0; i < 10; ++i) {
	   		 if(histogram[i] != 0) {
				printf("*\t");
				--histogram[i];
				--count;
				if(count <= 0) {
		   			 return 0;
				}
	    	} else {
				printf(" \t");
	  	  }
		}
		printf("\n");
    }
    printf("\n");
     return 0;
}
```



### 习题2：定义一个数组，编程打印它的全排列。比如定义：

```
#define N 3
int a[N] = { 1, 2, 3 };
```

### 则运行结果是：

```
$ ./a.out
1 2 3 
1 3 2 
2 1 3 
2 3 1 
3 2 1 
3 1 2 
1 2 3
```

### 程序的主要思路是：

1. 把第1个数换到最前面来（本来就在最前面），准备打印1xx，再对后两个数2和3做全排列。
2. 把第2个数换到最前面来，准备打印2xx，再对后两个数1和3做全排列。
3. 把第3个数换到最前面来，准备打印3xx，再对后两个数1和2做全排列。

### 可见这是一个递归的过程，把对整个序列做全排列的问题归结为对它的子序列做全排列的问题，注意我没有描述Base Case怎么处理，你需要自己想。你的程序要具有通用性，如果改变了`N`和数组`a`的定义（比如改成4个数的数组），其它代码不需要修改就可以做4个数的全排列（共24种排列）。

### 完成了上述要求之后再考虑第二个问题：如果再定义一个常量`M`表示从`N`个数中取几个数做排列（`N == M`时表示全排列），原来的程序应该怎么改？

### 最后再考虑第三个问题：如果要求从`N`个数中取`M`个数做组合而不是做排列，就不能用原来的递归过程了，想想组合的递归过程应该怎么描述，编程实现它。



## Chapter 10.2

### 习题1：看下面的程序：

```
#include <stdio.h>

int main(void)
{
	int i;
	char str[6] = "hello";
	char reverse_str[6] = "";

	printf("%s\n", str);
	for (i = 0; i < 5; i++)
		reverse_str[5-i] = str[i];
	printf("%s\n", reverse_str);
	return 0;
}
```

### 首先用字符串`"hello"`初始化一个字符数组`str`（算上`'\0'`共6个字符）。然后用空字符串`""`初始化一个同样长的字符数组`reverse_str`，相当于所有元素用`'\0'`初始化。然后打印`str`，把`str`倒序存入`reverse_str`，再打印`reverse_str`。然而结果并不正确：

```
$ ./main 
hello
```

### 我们本来希望`reverse_str`打印出来是`olleh`，结果什么都没有。重点怀疑对象肯定是循环，那么简单验算一下，`i=0`时，`reverse_str[5]=str[0]`，也就是`'h'`，`i=1`时，`reverse_str[4]=str[1]`，也就是`'e'`，依此类推，i=0,1,2,3,4，共5次循环，正好把h,e,l,l,o五个字母给倒过来了，哪里不对了？用`gdb`跟踪循环，找出错误原因并改正。

A：该程序会将`\0`置换到开头；修正：`reverse_str[4 - i] = str[i]`



## Chapter 11.4

### 习题1：快速排序是另外一种采用分而治之策略的排序算法，在平均情况下的时间复杂度也是Θ(nlgn)，但比归并排序有更小的时间常数。它的基本思想是这样的：

```
int partition(int start, int end)
{
	从a[start..end]中选取一个pivot元素（比如选a[start]为pivot）;
	在一个循环中移动a[start..end]的数据，将a[start..end]分成两半，
	使a[start..mid-1]比pivot元素小，a[mid+1..end]比pivot元素大，而a[mid]就是pivot元素;
	return mid;
}

void quicksort(int start, int end)
{
	int mid;
	if (end > start) {
		mid = partition(start, end);
		quicksort(start, mid-1);
		quicksort(mid+1, end);
	}
}
```

### 请补完`partition`函数，这个函数有多种写法，请选择时间常数尽可能小的实现方法。想想快速排序在最好和最坏情况下的时间复杂度是多少？

```c
void swap(int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

int partition(int start, int end) {
    int pivot = arr[end];
    int i = start, j;
    for(j = start; j < end; ++j) {
		if(arr[j] < pivot) {
	    	swap(i, j);
	   		 ++i;
		}
    }
    swap(i, end);
    return i;
}
```

时间复杂度：

- 最好情况：每次选择分割点都恰好能将数组分为均等的两部分，此时时间复杂度为：$O(nlogn)$；
- 最坏情况：数组自身已经有序，即每次划分得到的两部分都极其不均等，此时时间复杂度为：$O(n^2)$；



## Chapter 11.5

### 习题1：实现一个算法，在一组随机排列的数中找出最小的一个。你能想到的最直观的算法一定是Θ(n)的，想想有没有比Θ(n)更快的算法？

A：没有比O(n)更快的算法了，因为数组无序，要找到最小值，在没有遍历完整个数组的情况下，是没办法确定没有遍历到的元素中是否还存在比当前最小值还小的元素的。

### 习题2：在一组随机排列的数中找出第二小的，这个问题比上一个稍复杂，你能不能想出Θ(n)的算法？

A：能，先对数组进行一次快排，将数组分成三个部分：[start, pivot - 1]，[pivot]，[pivot + 1, end]，然后比较目标值与pivot的大小，若小于pivot，则仅需在前一半中查找，同理若大于，则在后一半中查找，时间复杂度为，每次递归查找的数量都是上一次的一半，即：$n + \frac{n}{2} + \frac{n}{4}  + ... 1 = 2n - 1$ ，即时间复杂度为$O(n)$



### 习题3：进一步泛化，在一组随机排列的数中找出第k小的，这个元素称为k-th Order Statistic。能想到的最直观的算法肯定是先把这些数排序然后取第k个，时间复杂度和排序算法相同，可以是Θ(nlgn)。这个问题虽然比前两个问题复杂，但它也有平均情况下时间复杂度是Θ(n)的算法，将上一节习题1的快速排序算法稍加修改就可以解决这个问题：

```
/* 从start到end之间找出第k小的元素 */
int order_statistic(int start, int end, int k)
{
	用partition函数把序列分成两半，中间的pivot元素是序列中的第i个;
	if (k == i)
		返回找到的元素;
	else if (k > i)
		从后半部分找出第k-i小的元素并返回;
	else
		从前半部分找出第k小的元素并返回;
}
```

### 请编程实现这个算法。

```c
void swap(int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

int partition(int start, int end) {
    int pivot = arr[end];
    int i = start, j;
    for(j = start; j < end; ++j) {
		if(arr[j] < pivot) {
	    	swap(i, j);
	    	++i;
		}
    }
    swap(i, end);
    return i;
}

int order_statistic(int start, int end, int k) {
    int i;
    if(end >= start) {
		i = partition(start, end);
		if(k == i) {
	    	printf("%d\n", i);
	    	return arr[i];
		} else if(k > i) {
	    	return order_statistic(i + 1, end, k);
		} else {
	    	return order_statistic(start, i - 1, k);
		}
    }
}
```



## Chapter 11.6

### 习题1：本节的折半查找算法有一个特点：如果待查找的元素在数组中有多个则返回其中任意一个，以本节定义的数组`int a[8] = { 1, 2, 2, 2, 5, 6, 8, 9 };`为例，如果调用`binarysearch(2)`则返回3，即`a[3]`，而有些场合下要求这样的查找返回`a[1]`，也就是说，如果待查找的元素在数组中有多个则返回第一个。请修改折半查找算法实现这一特性。

```c
int binarysearch(int target, int low, int high) {
    while(low <= high){
		int mid = low + (high - low >> 1);
		if(arr[mid] < target) {
	    	low = mid + 1;
		} else if (arr[mid] > target) {
	    	high = mid - 1;
		} else {
	    	while(arr[mid - 1] == target) {
			--mid;
	    	}
	    	return mid;
		}
    }
    return -1;
}
```



### 习题2：编写一个函数`double mysqrt(double y);`求`y`的正平方根，参数`y`是正实数。我们用折半查找来找这个平方根，在从0到`y`之间必定有一个取值是`y`的平方根，如果我们查找的数`x`比`y`的平方根小，则x2<y，如果我们查找的数`x`比`y`的平方根大，则x2>y，我们可以据此缩小查找范围，当我们查找的数足够准确时（比如满足|x2-y|<0.001），就可以认为找到了`y`的平方根。思考一下这个算法需要迭代多少次？迭代次数的多少由什么因素决定？

```c
#include <stdio.h>
#include <math.h>

double mysqrt(double y) {
    double low = 0, high = y;
    while(low < high) {
		double mid = low + (high - low) / 2;
		double square_mid = mid * mid;
        if (fabs(square_mid - y) < 0.001) {
	    	return mid;
		} else if(square_mid > y) {
	    	high = mid - 0.001; //注意精度，不要错写成mid - 1
		} else if (square_mid < y) {
	    	low = mid + 0.001;
		}
    }
    return -1.0;
}
```

迭代次数为：log(y)次，迭代次数的多少是由精度来决定的。



### 习题3：编写一个函数`double mypow(double x, int n);`求`x`的`n`次方，参数`n`是正整数。最简单的算法是：

```
double product = 1;
for (i = 0; i < n; i++)
	product *= x;
```

### 这个算法的时间复杂度是Θ(n)。其实有更好的办法，比如`mypow(x, 8)`，第一次循环算出x·x=x2，第二次循环算出x2·x2=x4，第三次循环算出4·x4=x8。这样只需要三次循环，时间复杂度是Θ(lgn)。思考一下如果`n`不是2的整数次幂应该怎么处理。请分别用递归和循环实现这个算法。

```c
#include <stdio.h>
#include <math.h>
double mypow(double x, int n) {
    double result = 1;
    while(n != 0) {
		if((n & 1) == 1) {
	    	result *= x;
		}
		x *= x;
		n >>= 1;
    }
    return result;
}
```



## Chapter 14.2

### 习题1：二进制小数可以这样定义：

### (0.A1A2A3...)2=A1×2-1+A2×2-2+A3×2-3+...

### 这个定义同时也是从二进制小数到十进制小数的换算公式。从本节讲的十进制转二进制的推导过程出发类比一下，十进制小数换算成二进制小数应该怎么算？

A：将十进制的小数部分乘2取整，直到小数部分变成0为止，然后将得到的结果按顺序排列；



### 习题2：再类比一下，八进制（或十六进制）与十进制之间如何相互换算？

A：将底数分别换成8或16



## Chapter 16.1

### 习题1.1.1：下面两行`printf`打印的结果有何不同？请读者比较分析一下。

```
int i = 0xcffffff3;
printf("%x\n", 0xcffffff3>>2);
printf("%x\n", i>>2);
```

A：

- 第一个的结果为：0x33fffffc，因为此时原数 0xcffffff3是被当成无符号数来处理的；
- 第二个的结果为：0xf3fffffc，因为此时i是int类型，是有符号数，原数 0xcffffff3展开成二进制的值的最高8位是：11001111，最高为是1，即为负数，右移时在高位补的值为1，即前8位变为:11110011，转为16进制就是f3，所以结果为：0xf3fffffc



### 习题1.3.1：统计一个无符号整数的二进制表示中1的个数，函数原型是`int countbit(unsigned int x);`。

```c
//方法一
int countbit(unsigned int x) {
    uint mask = 1;
    int count = 0;
    while(mask != 0) {
        if((mask & x) != 0) {
            ++count;
        }
        mask <<= 1;
    }
    return count;
}
//方法二
int countbit(unsigned int x) {
    int count = 0;
    while(x != 0) {
        ++count;
        x &= (x - 1);
    }
    return count;
}
```



### 习题1.3.2：用位操作实现无符号整数的乘法运算，函数原型是`unsigned int multiply(unsigned int x, unsigned int y);`。例如：(11011)2×(10010)2=((11011)2<<1)+((11011)2<<4)。



### 习题1.3.3：对一个32位无符号整数做循环右移，函数原型是`unsigned int rotate_right(unsigned int x);`。所谓循环右移就是把低位移出去的部分再补到高位上去，例如`rotate_right(0xdeadbeef, 16)`的值应该是0xefdeadbe。

