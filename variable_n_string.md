# Java2C++



## Java与C++共同点

### 变量
#### 1 内置类型的表示
Java包括**基本数据类型**和**包装数据类型**。C++包括**算数类型**和**空类型**。算数类型分为**整型**和**浮点型**（bool类型也属于整形）。  
Java与C++在基本类型上最大的不同是Java在各种操作系统中的大小是固定的，而C++会随着编译器位数变化而变化。  

|Java|size(bit)|C++|size(bit)|
|:---:|:---:|:---:|:---:|
|byte|1 * 8|
|short|2 * 8|short|2 * 8|
|int|4 * 8|short int</br>int</br>long int</br>_int8_t</br>_int16_t</br>_int32_t</br>_int64_t|2 * 8</br>4 * 8</br>8 * 8</br>1 * 8</br>2 * 8</br>4 * 8</br>8 * 8|
|long|8 * 8|long</br>long long|8 * 8</br>8 * 8|
|float|4 * 8|float|4 * 8|
|double|8 * 8|double</br>long double|8 * 8</br>8 * 8|
|char|2 * 8|char</br>wchar_t</br>char16_t</br>char32_t|1 * 8</br>2 * 8</br>2 * 8</br>4 * 8|
|boolean|1 * 8|bool|1 * 8|

C++除了与Java类似的数据类型外，还提供了整型的无符号类型。除布尔型和扩展的字符类型，其他类型都有signed和unsigned两种。两者的主要区别在与signed类型在底层的存储结构中，头部的1个bit位用于表示当前符号的正负属性，而unsigned则只表现正数。比较特殊的是char类型，同时拥有char，signed char与unsigned char三种类型，而char表现为哪一种类型根据编译器决定。

#### 2 数据类型的转换
Java中的boolean与int类型无法相互转换，而C++中的bool可以与int相互转换（任意非0数字都会转换为true，bool只能转换为0/1）。  

C++中类型转换有几种特殊情况：
- 赋予unsigned类型超出它范围的值：结果为初始值对无符号数表示的数值总数取模后的余数·。
- 赋予signed类型超出它范围的值：结果是未定义的，程序可能崩溃或继续运行。

#### 3 字面值常量
每个字面值常量对应一种数据类型，字面值常量的形式和值决定了它的数据类型。  

**整型和浮点型的字面量**
C++与Java的整型与浮点型默认的表示方式类似。  

*整型*  
|进制|表示方法|示例|
|:---:|:---:|:---:|
|十进制|正常数字|56|
|八进制|0开头数字|056|
|十六进制|0x或0X开头数字|0x56/0X56|

十进制一般为signed int类型，而八进制与十六进制为int，unsigned int, long, unsigned long, long long, unsigned long long中可以容纳数字的最小类型。如果都不能容纳，那么会报错。

*浮点型*  
浮点型指一个科学计数法表示的小数，e后表示10的指数，默认是double类型。

C++指定字符类型的方式与Java有所不同
![C++指定字面量类型](./imgs/1_指定字面量类型的方法.png)

如果没有精准指定字面量的类型，编译器也会自适应的赋予数据类型，比如后缀只有LL，那么可能会被赋予unsigned long long或long long；如果只有U，那么会被转换为unsigned int, unsinged long, unsigned long long中的一种。 

*指针*
C++中的指针字面量为nullptr。 

#### 4 变量初始化
C++相较于Java拥有更多的初始化方式。
|类型|Java|C++|
|:---:|:---:|:---:|
|默认初始化|不支持|int i</br>(受变量定义位置的影响)|
|赋予初始值|int i = 0|int i = 0|
|列表初始化|不支持|int i = {0}</br>int i = (0)</br>int i(0)|

比较特殊的情况是，Java在类中定义的变量类型会进行默认初始化。而C++中定义于函数体外部的变量会执行默认初始化，定义于内部的则不会，直接拷贝或者访问这种变量会造成错误，C++对象的变量值由类决定。

#### 5 声明与定义
C++相较于Java支持分离式编译，为了支持这一特点C++将变量的**声明**与**定义**分开在不同文件中。声明是将名字为程序所知，一个文件想要使用别处定义的名字，那么必须首先包含对名字的生命；定义是创建实体与名字关联起来。  
声明是规定变量的类型和名字，并分配内存空间，同时还可能赋予初始值。（使用extern关键字表明对变量的声明，而不是定义，前提是不进行显示初始化）。  
变量只能被定义一次，但是可以被多次声明。但是尝试在函数体内部对extern声明的变量进行定义会报错。  

#### 6 名字作用域
*作用域大小*
所有花括号外定义的名字拥有全局作用域，而其他位置定义的只有花括号内的块作用域。  
*嵌套作用域*
内层作用域中可以直接使用外层作用域中定义的变量，与Java不同的是，C++内层作用域中可以对变量进行重新定义，形成新的变量。特殊情况下可以使用显式调用方式使用外层作用域变量。
![嵌套作用域](imgs/2_嵌套作用域.png)

#### 7 常量
Java中使用final关键字指定常量，C++使用const指定变量。原则上const常量只在本文件内有效，因为C++在进行编译时会直接将常量名称替换为相应的值。如果想在多个文件中使用同一个const常量，那么无论声明和定义都需要用extern关键字修饰，那么声明和定义的常量名称为同一个

#### 8 string
Java中的String底层是长度不可变的char数组。而C++中的string是长度可变的，类似于Java的String与StringBuilder。  
*标准库类型*
||Java|C++|
|:---:|:---|:---|
|初始化|String s  默认初始化</br>String s = "abc"  存放于字符串常量池</br>string s = new String("abc")  存放于堆|string s 默认初始化</br>string s = "abc"  字符串字面值副本</br>string s("abc")  字符串字面值副本</br>string t = s t为s副本</br>string t(s)  t为s副本</br>string s(10, 'a')  10个a组成的字符串|
|比较|String是一个类</br>String一旦定义不可改变|string是库类型</br>string定义后可变|
|函数|长度：s.length()</br>访问下标：s.charAt(idx)</br>获取子串：s.substring(start, end + 1)</br>字符首次出现位置：s.indexOf(c)</br>子串首次出现位置：s.indexOf(substr)</br>判断相等：s.equalsTo(t)</br>字典顺序：s.compareTo(t)</br>是否为空：s.empty()|s.size()</br>s[idx]</br>s.substr(start, end + 1)</br>s.find_first_of(c)</br>s.find(substr)</br>s == t</br>s >(<) t</br>s.empty()|

string的初始化分为**直接初始化**和**拷贝初始化**。如果不使用等号，采用的就是直接初始化，反之为拷贝初始化，两者的区别在于拷贝初始化把对象右边的初始值拷贝到左边的字符串中。  

C++与Java在字符串拼接方面有着很大的不同：C++不允许两个字面量直接使用‘+’拼接，而Java则允许。这意味着在C++中使用`string s = "ab" + 'c';`是错误的。  


*C风格字符串*


## C++特性
