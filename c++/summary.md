Summary
======================================
1. stdafx.h、stdafx.cpp的作用

  这两个文件用于建立一个预编译的头文件".PCH"和一个预定义的类型文件"STDAFX.OBJ"。由于MFC体系结构非常大，各个源文件中都包含许多头文件，如果每次都编译的话比较费时。那么,如果我们把常用的MFC头文件都放在stdafx.h中，如afxwin.h、afxext.h、afxdisp.h、afxcmn.h等，然后再让stdafx.cpp包含这个stdafx.h文件。这样，由于编译器可以识别哪些文件已经编译过，所以stdafx.cpp就只编译一次(这里一定要有CPP文件,如果只有.h是不行的,因为头文件是不能被编译的)，并生成所谓的预编译头文件(因为它存放的是头文件编译后的信息，故而得名)。

   [如果读者以后在编程时不想让自己源文件中引用的MFC头文件每次都被编译，也可以将它加入到stdafx.h中。采用预编译头文件方式,从而可以加速编译过程。

   Windows和MFC的include文件都非常大，即使有一个快速的处理程序，编译程序也要花费相当长的时间来完成工作。由于每个.CPP文件都包含相同的include文件，为每个.CPP文件都重复处理这些文件就显得很傻了。]

2. ``#include /<stdio.h/>``

 （注：在TC2.0中，允许不引用此头文件而直接调用其中的函数，但这种做法是不标准的。也不建议这样做。以避免出现在其他IDE中无法编译或执行的问题。）

 int getchar()//从标准输入设备读入一个字符<br/>
int putchar()//向标准输出设备写出一个字符<br/>
int scanf(char*format[,argument…])//从标准输入设备读入格式化后的数据<br/>
int printf(char*format[,argument…])//向标准输出设备输出格式化字符串<br/>
char gets(char*string)//从标准输入设备读入一个字符串<br/>
int puts(char*string)//向标准输出设备输出一个字符串<br/>
int sprintf(char*string,char*format[,…])//把格式化的数据写入某个字符串缓冲区<br/>

3. 迭代和递归的区别：

        //1.迭代方法：

        public class Fab_iterate {
        public static void main(String[] args) {
        System.out.println("结果是："+Fab(8));   //求第八个位置的数
        }
        public static long Fab(int index){    //斐波那契数列
        if(index == 1 || index == 2){
        return 1;
        }else{
        long f1 = 1L;
        long f2 = 1L;
        long f3 = 0;
        for(int i = 0;i < index-2;i ++){   //迭代求值
         f3 = f1 + f2;
         f1 = f2;
         f2 = f3;
        }
        return f3;
        }
        }
        }

        //2.递归方法：

        public class Fab_recursion {
        public static void main(String[] args){
        System.out.println("结果是："+fab(8));    //求第八个位置的数
        }
        public static long fab(int index){    //斐波那契数列
        if(index == 1 || index == 2){
        return 1;
        }else{
         return fab(index-1)+fab(index-2);    //递归求值
        }
        }
        }

    递归的基本概念:程序调用自身的编程技巧称为递归,是函数自己调用自己.

    一个函数在其定义中直接或间接调用自身的一种方法,它通常把一个大型的复杂的问题转化为一个与原问题相似的规模较小的问题来解决,可以极大的减少代码量.递归的能力在于用有限的语句来定义对象的无限集合.

    使用递归要注意的有两点:

    1)递归就是在过程或函数里面调用自身;

    2)在使用递归时,必须有一个明确的递归结束条件,称为递归出口.

    递归分为两个阶段:

    1)递推:把复杂的问题的求解推到比原问题简单一些的问题的求解;

    2)回归:当获得最简单的情况后,逐步返回,依次得到复杂的解.

    迭代:利用变量的原值推算出变量的一个新值.如果递归是自己调用自己的话,迭代就是A不停的调用B.

    递归中一定有迭代,但是迭代中不一定有递归,大部分可以相互转换.能用迭代的不用递归,递归调用函数,浪费空间,并且递归太深容易造成堆栈的溢出.

4. 注意c++字符串和c字符串的区别
	
    c++的字符串就是string，c风格的字符串就是char *c=“abcabc”
    string是没有null或者’\0’的结束符的，C如果没有的话，遍历会识别不了结束，就会出错
5. 以下错误代码
	
    	template <class T>
		void foo(const T& t)
		{
    		// 声明一个指向某个类型为T::bar的对象的指针
    		T::bar * p;
		}
		struct StructWithBarAsType
		{
    		typedef int bar;
		};
		int main()
		{
    		StructWithBarAsType x;
    		foo(x);
		}
 这段代码看起来能通过编译，但是事实上这段代码并不正确。因为编译器并不知道T::bar究竟是一个类型的名字还是一个某个变量的名字。究其根本，造成这种歧义的原因在于，编译器不明白T::bar到底是不是“模板参数的非独立名字”，简称“非独立名字”。注意，任何含有名为“bar”的项的类T，都可以被当作模板参数传入foo()函数，包括typedef类型、枚举类型或者变量等。
 
 为了消除歧义，C++语言标准规定：

		A name used in a template declaration or definition and that is dependent on a template-parameter is assumed not to name a type unless the applicable name lookup finds a type name or the name is qualified by the keyword typename.
        
 意即出现上述歧义时，编译器将自动默认bar为一个变量名，而不是类型名。所以上面例子中的代码 T::bar * p 会被解释为乘法，而不是声明p为指向T::bar类型的对象的指针。

 解决问题的最终办法，就是显式地告诉编译器，T::bar是一个类型名。这就必须用typename关键字，例如：
		template <typename T>
		void foo(const T& t)
		{   
    		// 声明一个指向某个类型为T::bar的对象的指针
    	typename T::bar * p;
		}
 这样，编译器就确定了T::bar是一个类型名，p也就自然地被解释为指向T::bar类型的对象的指针了。
 
 通用的规则很简单：在你涉及到一个在 template（模板）中的 nested dependent type name（嵌套依赖类型名）的任何时候，你必须把单词 typename 放在紧挨着它的前面。
6. LIB和DLL的区别与使用

 共有两种库：
 一种是LIB包含了函数所在的DLL文件和文件中函数位置的信息（入口），代码由运行时加载在进程空间中的DLL提供，称为动态链接库dynamic link library。
 一种是LIB包含函数代码本身，在编译时直接将代码加入程序当中，称为静态链接库static link library。
 共有两种链接方式：
 动态链接使用动态链接库，允许可执行模块（.dll文件或.exe文件）仅包含在运行时定位DLL函数的可执行代码所需的信息。
 静态链接使用静态链接库，链接器从静态链接库LIB获取所有被引用函数，并将库同代码一起放到可执行文件中。

 关于lib和dll的区别如下：
（1）lib是编译时用到的，dll是运行时用到的。如果要完成源代码的编译，只需要lib；如果要使动态链接的程序运行起来，只需要dll。
（2）如果有dll文件，那么lib一般是一些索引信息，记录了dll中函数的入口和位置，dll中是函数的具体内容；如果只有lib文件，那么这个lib文件是静态编译出来的，索引和实现都在其中。使用静态编译的lib文件，在运行程序时不需要再挂动态库，缺点是导致应用程序比较大，而且失去了动态库的灵活性，发布新版本时要发布新的应用程序才行。
（3）动态链接的情况下，有两个文件：一个是LIB文件，一个是DLL文件。LIB包含被DLL导出的函数名称和位置，DLL包含实际的函数和数据，应用程序使用LIB文件链接到DLL文件。在应用程序的可执行文件中，存放的不是被调用的函数代码，而是DLL中相应函数代码的地址，从而节省了内存资源。DLL和LIB文件必须随应用程序一起发行，否则应用程序会产生错误。如果不想用lib文件或者没有lib文件，可以用WIN32 API函数LoadLibrary、GetProcAddress装载。

7. 理解复杂声明可用的“右左法则”：
　　从变量名看起，先往右，再往左，碰到一个圆括号就调转阅读的方向；括号内分析完就跳出括号，还是按先右后左的顺序，如此循环，直到整个声明分析完。举例：
　　int (\*func)(int \*p);
　　首 先找到变量名func，外面有一对圆括号，而且左边是一个\*号，这说明func是一个指针；然后跳出这个圆括号，先看右边，又遇到圆括号，这说明 (\*func)是一个函数，所以func是一个指向这类函数的指针，即函数指针，这类函数具有int\*类型的形参，返回值类型是int。
　　int (\*func[5])(int \*);
　　func 右边是一个[]运算符，说明func是具有5个元素的数组；func的左边有一个\*，说明func的元素是指针（注意这里的\*不是修饰func，而是修饰 func[5]的，原因是[]运算符优先级比\*高，func先跟[]结合）。跳出这个括号，看右边，又遇到圆括号，说明func数组的元素是函数类型的指 针，它指向的函数具有int\*类型的形参，返回值类型为int。
也可以记住2个模式：
type (\*)(....)函数指针
type (\*)[]数组指针

8. 写出float  x 与“零值”比较的if语句

 请写出 float  x 与“零值”比较的 if 语句： 
 const float EPSINON = 0.00001; 
if ((x >= - EPSINON) && (x <= EPSINON) 
不可将浮点变量用“==”或“！=”与数字比较，应该设法转化成“>=”或“<=”此类形式。 

 EPSINON应该是一个很小的值吧   因为计算机在处理浮点数的时候是有误差的，所以判断两个浮点数是不是相同，是要判断是不是落在同一个区间的，这个区间就是   [-EPSINON,EPSINON]   EPSINON一般很小，10的-6次方以下吧
 
9. char类型的加减运算，比如
		char m='m';
        cout<<m+""<<endl;//这种加法转string
   就会出现奇怪的表示
   		ss of p:
   我这里表示成了这个
   
   原因是：
   char zhao = 50;
   char nian = 10;
   在GCC编译器中
    char类型的字符占1个字节，可以当做整型进行加减，本身这是没错的
    问题出在：
    scanf("%d",&zhao);
    scanf("%d",&nian);
    变量zhao找在输入时用的格式是%d，是整型，占四个字节，存入内存时也会向连着的四个内存写输入的值（最低字节写10，高字节都是0），这样就会占用大于1个字节的内存。
    同理，变量nian也是的，输入时会把变量zhao的值给覆盖掉，输入10时，变量zhao=0,(你可printf看看)
    所以最后就是0+10=10，也就是说打印是会出错的
    
10. vector除了默认的几种构造函数的初始化

 还可以用数组初始化，如用int数组或者string数组
 		 //数组初始化vector
         int iarray[]={1,2,3,4,5,6,7,8,9,0};
         //count： iarray数组个数
         size_t count=sizeof(iarray)/sizeof(int);
         //int数组初始化 ivec3 
         vector<int> ivec3(iarray,iarray+count);
         for(int_ite=ivec3.begin ();int_ite!=ivec3.end ();int_ite++)
          cout<<"ivec3: "<<*int_ite<<endl;

         //string数组初始化 svec1
         string word[]={"ab","bc","cd","de","ef","fe"};
         //s_count： word数组个数
         size_t s_count=sizeof(word)/sizeof(string);
         //string数组初始化 svec1 
         vector<string> svec1(word,word+s_count);
         for(string_ite=svec1.begin ();string_ite!=svec1.end ();string_ite++)
          cout<<"svec1: "<<*string_ite<<endl;
    所以关键还是sizeOf的运用，这里iarray直接是数组名字，所以sizeof是数组大小，如果声明为指针int *iaaray={...}，那sizeof（iaaray）就是一个4字节（32位）/8字节（64位）大小了
    
11. sizeOf用法

 http://blog.csdn.net/candyliuxj/article/details/6307814
 http://www.cnblogs.com/lidabo/archive/2012/08/27/2658519.html
 
12. 

补充
==============================================
*
