# C++/CLI学习总结                

1. 基本类型与CLI值类型          

|基本类型|字节|CLI值类类型|
|:-:|:-:|:-:|
|bool|1|System::Boolean|
|char|1|System::SByte|
|singed char|1|System::SByte|
|unsigned char|1|System::Byte|
|short|2|System::Int16|
|unsigned short|2|System::UInt16|
|int|4|System::Int32|
|unsigned int|4|System::UInt32|
|long|4|System::Int32|
|unsigned long|4|System::UInt32|
|long long|8|System::Int64|
|unsigned long long|8|System::UInt64|
|float|4|System::Single|
|double|8|System::Double|
|long double|8|System::Double|
|wchar_t|2|System::Char|      


2. 在CLI环境中，safe_cast用于显示强制类型转换，不成功时抛出异常。其用法与static_cast一样      

3. CLI枚举     
```
enum class Suit {Clubs, Diamonds, Hearts, Spades};
Suit suit = Suit::Diamonds;
enum class Face:char { Ace,Two,Three,Four,Five,Six,Seven,Eight,Nine,Ten,Jack,Queen,King};
```
**由于C++/CLI枚举定义中的变量都是类对象，因此不能在函数内部定义(类在函数外定义)**         

4. 跟踪句柄^     

跟踪句柄^类似于本地C++指针，但当CLR压缩堆过程中会改变该对象的地址，垃圾回收器自动更新句柄所包含的地址。因此不能像本地指针那样用跟踪句柄执行地址的运算，也不允许对跟踪句柄进行强制类型转换。      
**在CLR堆中创建的对象必须被跟踪句柄引用**。所有属于引用类型的对象都存储在堆中，因此为引用这些对象所创建的对象都必须是跟踪句柄。此外，在堆上分配的变量都不能在全局作用域内声明。      

5. CLR数组分配在可回收垃圾的堆上，必须用array<typename>指出要创建的数组，需要用句柄来访问         

6. C++/CLI字符串String     

C++/CLI字符串String是Unicode字符组成的字符串，是由System:Char类型的字符序列组成的字符串。        
String对象是固定不变的，一旦创建完毕后就不能再被修改了。因此所有的字符串操作都是做创建新的字符串

7. 跟踪引用%          

跟踪引用%类似于本地C++引用，表示某对象的别名。可以给栈上的值对象、CLR堆上的跟踪句柄创建跟踪引用。跟踪引用本身总是在栈上创建，如果垃圾回收移动了被引用的对象，则跟踪引用将被自动更新。        

8. interior_ptr定义的内部指针         

C++/CLI还提供了一种用关键字interior_ptr定义的内部指针，它允许进行地址的运算符操作。必要时，该指针内存储的地址会由CLR垃圾回收自动更新。内部指针总是函数的局部自动变量。           
内部指针在指定类型时应注意：可以包含堆栈上值类型对象的地址，也可以包含指向CLR堆上某对象句柄的地址，还可以是本地类对象或本地指针，但不能是**CLR堆上整个对象的地址**。            
```
interior_ptr<String^> pstr1;	// OK -- pointer to a handle
interior_ptr<String>  pstr2;	// ERROR -- pointer to a String object
```

9. C++/CLI中函数         

C++/CLI中函数的工作方式与ISO/ANSI C++完全相同，但由于在C++/CLI中用跟踪句柄和跟踪引用替代了本地指针和引用，因此也带来了一些变化，主要包括：    
* CLR程序中函数的形参与返回值可以是数值类型、跟踪句柄、跟踪引用和内部指针    
* 如果某个形参是CLR数组，程序不需要另外的参数指定其大小，因为数组大小在属性Length中        
* 在C++/CLI程序中，不能像C++一样进行地址的算术运算，而应该使用数组索引      
* 可以方便地返回CLR堆上的句柄，因为CLR有垃圾回收机制自动清理无用的内存     
* C++/CLI函数的接受可变长度参数的机制与本地C++不同      
* C++/CLI中main()函数访问命令行实参的机制与本地C++不同        

10. 类函数              

类函数是C++/CLI中引入的新概念，其功能类似于函数模板，但原理上却迥然不同。使用函数模板时，编译器根据模板生成函数源代码，然后将其与代码一起编译。这种方法容易导致“代码膨胀”。类函数与之不同，类函数本身将被编译，在函数调用时，实际类型在运行时取代了类函数的类型形参，这不会导致代码新增的问题。             
```
generic<typename T> where T:IComparable
T MaxElement(array<T>^ x)
{
	T max = x[0];
	for(int i=1; i<x->Lenght; i++)
		if(max->CompareTo(x[i])<0)
			max = x[i];
	return max;
}
```


11. C++/CLI中可以定义两种类型的struct和class :       

 value class、value struct、ref class、ref struct.      
 stuct和class的区别仅仅在于默认访问级别。下面以class来介绍，同样适用于struct：                   

* value class 和 ref class是双关键字，并不能单独适用vlaue或ref
* 值类型的对象包含自己的数据，引用类的对象只能用句柄来访问
* 在C++/CLI中函数成员不能声明为const类型，取而代之的是字面值类型**literal**。利用literal的一个缺点是，必须在定义常量的同时指定它的值。另外一种定义常量的方法是使用**initonly**，使用该修饰符的常量只能在构造函数的* 初始化列表或构造函数体内进行一次初始化，之后不能被修改
* 在非静态函数成员中，this指针类型与本地C++不同：值类型的this指针为内部指针类型（interior_ptr<T>），而引用类的this指针为句柄类型（T^）
* C++/CLI类的数据成员不能包含本地C++数组或本地C++类对象
* C++/CLI类无友元函数
* C++/CLI类的数据成员不能包含位类型的数据成员
* C++/CLI类的数据成员不能有默认的形参
* C++/CLI中，不能重写默认构造函数。默认构造函数将所有的值类型初始化为0，引用类型初始化为nullptr。同样，也不能重载复制构造函数和赋值构造函数
* 静态构造函数没有形参，且没有初始化表，总是被声明为private。它不能被直接调用，而是由普通构造函数在调用之前自动调用，可用来在运行期指定static initonly的值
* 引用类更加类似于本地C++类，但引用类没有默认的复制构造函数和赋值运算符，如果需要，必须显式添加      

12. 属性      

属性是C++/CLI的类成员，拥有get和set函数。类可以有两种属性：标量属性和索引属性。在定义了属性之后，对应的get_属性名和set_属性名自动成为系统保留名称，不能为其它目的而使用它们  

13. [参考](http://www.cppblog.com/golq/archive/2009/06/27/88644.html)

