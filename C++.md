# C++
## 函数重载（函数名相同）
### 1.形参数量不同
### 2.形参类型不同
### 3.返回值类型不同，有歧义，所以不能形参为空的返回值类型不同
### 4.形参有隐式转换的不行
```c
int main()
{
    display(long i);
    display(double i);
    display(10);
    //这样就会转换错误
}
```

## 函数声明
### 既有声明又有实现，将默认函数写在声明里

## void test();
### void(*P)() = test();
### 等价的调用的时候用p()&test()都可以

## exturn "c"
### 按照C语言的方式去编译
```c
exturn "c"{
    void ...()
}

exturn "c" void ...()

exturn "c"{
    #include<math.h>
    
//或者在头文件中把函数声明放在exturn "c"中，这样就可以直接调用include用了
}

```
### 当声明和实现都存在时，在声明里写就可以了
### 用法
#### 1.调用第三方框架
#### 2.C/C++混合开发时候
#### 3.C++里面后台不会
### 如果想把头文件里面的方法用在C和C++中须知
#### CPP后缀名的文件中，编译器在开头会给一个隐形的定义，告诉编译器它是个C++的文件
```c
define __cplusplus
```
#### 这时候只要在头文件中写(条件编译)
```c
#ifdef __cplusplus
extern "c" {
#endif
    fun1()
    fun2()
     ...
#ifdef __cplusplus
}
#endif
```
### 一些比较严谨的宏定义（防止漏掉或者重复占用内存）
```c
#ifndef ABC
#define ABC
.....方法
#endif
```
```c
#pragma once
```
## 内联函数(inline)(使得编译体积变大，但是速度变快，用内存空间换取运行速度)
### 编译器将函数调用转换为函数体代码
```c
//开辟栈空间
void func()
{
    cout<<"5555";
    cout<<"52125";
}
//回收栈空间
inline void func()
{
    cout<<"5555";
    cout<<"52125";
}

int main()
{
    func()----->cout<<"5555";
                cout<<"52125";
}
```
### 什么时候用内联函数
#### 1.函数代码体积不大
#### 2.经常使用的函数
#### 3.递归函数不会自动成为和无法手动使用内联函数
### 和宏的调用的区别
#### 内联函数多了语法检测和函数特性
##### 翻译一下：就是少用宏

## const 
### 不想让别人修改这个变量的时候用
```c

    struct Date{  
        int year;
        int month;
        int day;
    };
    
    const Date d = {2011,5,8};
    //这样就不能修改其中的值了
    //但是使用指针还是可以修改
    Date *p = &d;
    p->year =  2012;
    //酱紫改

    //想要都不能改的话，酱紫改
    const Date*p = &d;
    p -> year = 2012;
    (*P).month = 6;
    *p = d_1;
    //都不可以了
```

## 引用
### 引用从始至终都指向一个变量，中间不能修改，相当于一个外号只能代表一个人 但是指针可以修改，指向别人
### 引用的本质就是指针，只是编译器削弱了它的功能，所以引用是弱化了的指针 （不能瞎指）

### 常引用
#### 可以指向临时数据
```cpp
int main()
{
    const int & p = 30;//合法的
}
```
#### 常引用作为参数，可以接受const和非const实参、并且可以和非 const引用构成重载函数
```cpp
int main()
{  
    int a = 10;
    int b = 20;
    int sum(const int &a, const int &b)
    {
        return a+b;
    }
    sum(10,20);
    sum(a,b);
    //如果参数不是常引用的话，则不能传入（10，20）
}
```

## 面向对象
### struct class区别
#### struct 的默认权限是 公开
####  class 的默认权限是 私有
```cpp
int main()
{  
    void test(){}
    struct Person
    {
        void (*run)() = test;
    }//利用C语言模拟的面向对象
}
```
### 用类创建的普通对象和指针对象，都在栈空间里，自动分配和回收。
### 类中的函数其实不在类中，而是在外部代码段里
### 类中如有多个变量，则按类中的定义顺序来连续地址

### this
#### 类中的函数在代码区，而变量在栈区，编译器给我加了一个指针this ,  this = &你创建的对象

### 汇编中利用指针间接访问所指向对象的成员变量？
#### 1.从指针中取出对象的地址
#### 2.利用对象的地址+成员变量的偏移量计算出成员变量的地址

## 内存分四段
### 全局区
### 代码块
### 堆区 //需要自己释放
### 栈区 //

## 堆区开辟空间
### malloc
```cpp
int main()  
{
    char *p = (char *)malloc (4);
    //这样实际上只指向了第一个字节，因为char是一个字节，但是给他开了4个字节
    *p = 10;
    *(p + 1) = 11;
    ...
    //这样可以给这四个字节赋值
    
    free(p);
    //酱紫就可以把四个字节全部回收了
}
```

### new
```cpp
int main()
{
    int * p = new int()
    * p = 10;
    delete p ;
}
```

### 注意，上面两种申请动态内存的方式必须动态分配
```cpp
    char *p = (char *)malloc (4);
    ===========================
    char *p = new char[4];
    
    free(p);
    ===========================
    delete[] p;
```

### 开辟动态内存的初始化
```cpp
int main()
{  
    int *p = (int *) malloc(4);
    *p = 0;
    
    int size = sizeof(int) * 10;
    int * p = (int *) malloc(size);
    // memory set
    memset(p, 1, 40); //从P的首个地址到往后数40个单词，全部初始化成1
    
    int * p = new int (5);//初始化为五
    int * p = new int ();//初始化为0
    int * p = new int [3](); //都初始化为0
    int * p = new int [3]{}; //都初始化为0
    int * p = new int [3]{5}; //首元素被初始化为5 ，后面的为0。
    
}

```

### 对象的内存
```cpp
int main()
{  
    //栈空间 因为是局部变量
    Person person;
    //堆空间 动态申请出来的空间
    Person * p = new Person;
    return 0;//注意：指针变量P的地址其实还是存在栈空间，只不过它指向的值在堆空间里面
}
```

## 构造函数(Constructor)
### 构造函数可以重载
### 当有形参的构造函数时，必须要自己搞一个没有形参的构造函数
```cpp
class Person
{  
    person()
    {
        cout<<"This is Constructor!!!!";
    }
};

int main()
{  
    Person * p = new Person;
    //酱紫写也可以自动调用构造函数
    
    Person * p = (Person *) malloc(sizeof(Person))
    //不会调用构造函数
}
```

### 初始化问题
```cpp
Person g_person;
//全局区，成员变量初始化为0

int main()
{
    //堆空间：没有初始化成员变量
    Person * p0 = new Person ;
    //堆空间： 成员变量初始化为0
    Person * p1 = new Person() ;
}
```

## 析构函数
### ~类名()  
### 只有在栈空间中定义的局部变量才会调用，也适用于构造函数
### 构造函数和析构函数必须要在public 中

## 内存泄露
### 该释放的内存没有被释放
### 用来清理内部new出来的空间

## 命名空间
```cpp
int main()
{  
    namespace XWZ
    {
        class Person
        {
            int m_age;
            int m_money;
        }    
    }
}
```
### 命名空间可以嵌套
```cpp
int main()
{  
    namespace XWZ
    {
        namespace nihao{
        class Person
        {
            int m_age;
            int m_money;
        }    
      }
    }
}

```

### 当命名空间和不在命名空间的函数名相同时，要调用不在命名空间的函数时，在函数名前加::(原理是我们写的每一个语句都在一个隐形的大命名空间里)

## 继承  
```cpp
int main()
{
    struct Person{
        int m_age;
    }  
    struct Student : Person
    {
        ......
    }
}
```

##  成员访问权限
### public : 公共的，任何地方都可以访问（struct 默认)
### protected : 子类内部，当前类内部可以访问
### private : 私有的，只有当前类内部可以访问(class 默认)

## 初始化列表
```cpp
    int main()
    {
        class Person
        {
            public:
                int m_age;
                int m_height;
                Person():Person(10,20)//必须写在初始化列表里
                {
                    Person(0,0);//不可以这样调用
                }
                Person(int age = 0 , int height = 0) : m_age(age) , m_height(height)
                {
                    // 初始化列表的顺序应该与定义变量的顺序相同，因为是赋值给地址中的值
                }
        }
    }
    //如果函数的声明和实现是分离的，那么初始化列表只能写在实现中
```

## 多态(同一操作作用于不同的对象，有不同的操作结果)
### 父类指针可以指向子类对象，但是反过来不行
```cpp
int main()
{
    struct Person{
        int m_age;
    }  
    struct Student : Person{
        int m_score;
    }
    
    Person * p = new Student();
    //父类指针指向子类对象
    //访问的范围只有Person中的变量，访问的字节<指向的，不会超出边界，所以安全。

}
```

### 多态默认只看指针类型从而调用该类型类中的函数
### 所以C++中的多态通过虚函数来实现
#### 只需要在父类中写 virtual 变为虚函数，则子类里自动变为虚函数
### 多态的三个要素
#### 1.子类重写父类的成员函数(override)
#### 2.父类指针指向子类对象
#### 3.利用父类指针调用重写的成员函数

### 纯虚函数
#### 父类中没有实质意义的没有函数实现的叫纯虚函数
```cpp
int mian()
{  
    class Animal{
        virtual void speak() = 0;//定义的规范，具体的要看具体的子类
    }
}
```

### 多继承(C++允许一个类有多个父类)   **  *不建议使用*  **
```cpp
int main()
{
    struct Student{
        
    }
    
    struct People{
        
    }
    
    struct Worker{
        
    }
    
    struct Undergraduate : private People , public Worker{
        
    }
}
```

### 虚继承
#### 解决菱形继承
#### 最父类被称为虚继承
```cpp
struct Student : virtual Person {}
//此时的Person中的元素就被当成公共元素，不会出现重复现象
```

## 静态(static)
### 静态成员变量
#### 1.静态成员变量会存放在全局区 //只有一份变量
#### 2.静态成员变量要放在全局区进行初始化
#### 3.不依赖于对象存在 直接可以用类名访问 类名::静态成员变量
#### 4.释放类指针的时候，不会释放静态成员变量
#### 5.静态成员变量必须初始化
### 静态成员函数
#### 访问的三种手段
```cpp
class Person{
    static void talk(){}  
}

int main()
{
    Person person;
    person.talk();
    
    Person * p = new Person;
    p->talk();
    
    Person::talk();
}
```
### this->只能用在非静态成员函数的内部
### virtual 不能用在静态成员函数上，因为virtual是多态的东西，但是静态成员函数只有一份地址，两者冲突，不可能连用

### 单例模式 *****重点(利用静态创建只能生成一个窗口之类的具体应用)
```cpp
class Rocket
{
    private:
        Rocker(){} //将构造函数私有化，确保外边不能自己创建对象
        ~Rocker(){} //确保在外边不能进行delete
        Rocket( const Rocket &rocket){}//防止外界利用拷贝构造创建不知名堆空间
        void operator= (const Rocket &rocket){}
        //防止利用等于号进行简单复制这样的无意义操作
        static Rocket *ms_rocket; //设置一个静态指针变量，确保只有一个地址
    public:
        static Rocket *sharedRocket()
        {
            //这里要考虑多线程安全，如果同时多线程，会造成多线程-1的new出来的空间浪费
            if (ms_rocket == NULL)
            {
                ms_rocket == new Rocket();
            }
            return ms_rocket;
        } //设置成静态函数的原因:非静态函数只能用外界创造的对象调用，而外界不能创造对象
          //使用判断语句，确保外界调用多次函数时，只会new出来一个空间，防止内存浪费 
        static void deleteRocket()
        {
            if (ms_rocket != NULL)
            {
                delete ms_rocket;
                ms_rocket = NULL;
                //防止产生野指针
            }
        }
          int main()
          {  
            Rocket * p = Rocket :: sharedRocker();
            //核心思想： 只给外界提供一个用类名访问的函数，用来接受指针地址，其他的都在类内部进行，确保只有单例。
          }
}



Rocker *Rocket::ms_rocket = NULL;//静态成员变量必须在全局区初始化
```

## const(非静态)
### const 成员变量，必须要初始化
### 如果是const 函数 声明和实现中都要写const
#### 内部不能改非静态的成员变量
#### 内部只能调用const成员函数，static成员函数
#### 非const成员函数可以调用const成员函数
#### 非const成员函数和const成员函数构成重载
#### 非const对象(指针)优先调用非const成员函数
#### const对象（指针）只能调用const 成员函数，static成员函数
```cpp
class Person
{
    public:
        const int m_price = 0; //每个对象中都有一份
        static const int m_price = 0; //所有只有一份
        void talk() const{}
}

```

## 拷贝构造函数 ***** *重点*
```cpp
    class Person
    {
        public:
            int m_talk;
            int m_leagth;
            Person(){}
            Person(const Person & person){
                this -> m_talk = person.m_talk;
                this -> m_leagth = person.m_leagth;
            }//拷贝构造函数
    }
    int main()
    {
    Person person_1(person_0); 
    }
```
###  与父类有联系的拷贝构造
```cpp
class Person {
 public:
    int m_age;
    Person(int age = 0) : m_age(age){}
    Person(const Person &person) : m_age(Person.m_age){}
}

class Student : public Person{
  public :
    int m_score;
    Student(int age = 0 , int Score = 0) : Person(age), m_score(score){}
    Student(const Student & student) : Person(student), m_mcore(student.m_score){}
}
```

## 深拷贝.浅拷贝
### 字符串后面有个隐形的\o算一个字节
```cpp
class Car
{
    int m_price;
    char *m_name;
  public:
    Car(int price = 0 , char *name = NULL)
    {
        if(name == NULL)return;
        
        //申请新的堆空间
        m_name = new char[strlen(name) + 1]{};
        //拷贝字符串数据到新的堆空间
        strcpy(m_name,name);
    }
    ~Car()
    {
        if(m_name == NULL)return;
        delete[] m_name;
        m_name = NULL;
    }
}
//深拷贝
```
### 深拷贝
#### 产生了新的存储空间（堆空间）
### 浅拷贝
#### 把人家的指针地址也拷贝了，容易 double free

## 隐式构造
```cpp
class Person {
 public:
    int m_age;
    Person(int age = 0) : m_age(age){}
    Person(const Person &person) : m_age(Person.m_age){}
}

int main()
{  
    Person person = 10;//隐式类型转换为将10传给构造函数，构造出一个对象
    //要想禁用掉，则在构造函数开头添加explicit关键词
    explicit Person(int age = 0) : m_age(age){}
}
```

## 友元 //可以访问对方private中的函数，不需要调用get方法获取了，可以提高执行效率。
```cpp
#include <iostream>
 
using namespace std;
 
class Box
{
   double width;
public:
   friend void printWidth( Box box );
   void setWidth( double wid );
};
 
// 成员函数定义
void Box::setWidth( double wid )
{
    width = wid;
}
 
// 请注意：printWidth() 不是任何类的成员函数
void printWidth( Box box )
{
   /* 因为 printWidth() 是 Box 的友元，它可以直接访问该类的任何成员 */
   cout << "Width of box : " << box.width <<endl;
}
 
// 程序的主函数
int main( )
{
   Box box;
 
   // 使用成员函数设置宽度
   box.setWidth(10.0);
   
   // 使用友元函数输出宽度
   printWidth( box );
 
   return 0;
}
```

## 内部类（嵌套类）
```cpp
class Person{
    int m_age;
    public:
        class Car{
            int m_price;
        };
};

int main()
{
    Person::Car car;//酱紫创建
}
```
### 成员函数可以访问其外部类里面的东西(反过来不行)

## 运算符重载
### 加减乘除
```cpp
//数据类型设为Point
const Point operator+(const Point &p1 , const Point &p2) const
{  
    return Point(p1.m_x + p2.m_x,p1.m_y + p2.m_y);
}
```
### 不让p1+p2这样的当左值
#### 在运算符重载函数前面加上const
#### 在最后再加一个const是为了返回值还可以再次调用operator函数，实现连续加等类似操作

### 重载单目运算符
```cpp
// -号
const Point operator- const(){
    return Point(x,y);
}
//里面不需要传参

// 这是前置++号
const Point operator++ const(){}
// 这是后置++号
const Point operator++ const(int){}
```

### 重现 <<\ >>运算符
```cpp
ostream  &operator<<(ostream &cout , const Point &point ){
    return cout; }

istream &operator >> (istream &cin , Point &point){
    cin >> point.m_x;
    cin >> point.m_Y;
    return cin;
}
```

## 模板(template)*** *重点*
```cpp
template <typename/class T> T add(T a, T b){
    return a + b;
    //不同类型的话可以加一个<typename T,typename A>
}

add(10,20);
add<int>(10,20);
```
### 模板函数实现和声明必须放在一个文件中，放在头文件中，后缀名规范为 hpp，用h也可，具体原因编译是每个cpp文件单独编译，最后再链接。

## 动态数组*** *重点*
```cpp
class Array
{
// 用于指向首元素
    int * m_data;
// 元素个数
    int m_size;
// 容量
    int m_capacity;
    
pubilc:
    Array(int capacity)
    {
        if(capacity<=0)
        {
            m_capacity = 10;//默认容量为零
        }
        else {
            m_capacity = capacity;
        }
        m_data = new int[m_capacity]
    }
    
    ~Array()
    {  
        if(m_data != NULL)
        {
            delete [] m_data;
        }    
    }
    
    void add(int value)
    {
        if(m_size == m_capacity)
        {
            cout<<"容量不够"；
            return ;  
        }
        m_data[m_size++] = value;
    }
    
    int get(int index)
    {
        if(index<0 || index >= m_size)
        {
            throw "数组下表越界";
        }
    }
};
```
### 可以用类模板将其成为模板函数，泛型编程

## 类型转换
### 1.static_cast
#### 对比dynamic_cast , 缺乏运行时安全检测
#### 不能交叉转换(不是同一继承体系的，无法转换)
#### 常用于基本数据类型的转换，非const转成const
### 2.dynamic_cast
#### 一般用于多态类型转换，一般运行时具有安全检测，如果危险，给你转成空指针
### 3.reinterpret_cast
#### 底层的强转，没有任何类型检查和格式转换，仅仅是简单的二进制数据拷贝
##### 可以交叉转换
### 4.const_cast
#### 将const变量转换为非const语句
### 使用格式：：xx_cast<type>(expression)

## C11
```cpp
auto a = 10;
//自己给我推测出来是什么类型，在初始化定义的时候用
decltype(a) b = 10;
//就是和a 一样的类型
int *p = nullptr;
//空指针这样写可以解决二义性问题，只能用在指针上，不能当成0来用
int array[] = { 11, 22, 33, 44, 55};
for (int item : array)
{
    cout << item << endl;
}
//快速遍历，不需要自己++了
int array[]{ 11, 22, 33, 44, 55};
//更加简洁的创建数组
```

## Lambda表达式//只能在当前函数中使用
### 具体形式
```cpp
([]{cout << "hello" <<endl;})();
//右边那个()就相当于函数的识别符，没有的话就不能调用
//左边括号里面的语句可以把它当成一个函数名
//两者结合形成 lambda表达式

void (* p)() = []{cout << "hello" <<endl;} //因为是个无名函数，所以才能够左值拿一个函数名指针去接住
auto p = []{
    cout << "hello" <<endl;
}

//创建一个lambda表达式
auto p = [](int a, int b) -> int {
    return a+b;
};
```
### 变量捕获
```cpp
int main()
{
    int a = 10;
    int b = 20;
    
    auto func = [a, b]() mutable
    //加上mutable 里面值可改，不加不能改    
    // 也可以写成[=] 意思为捕获全部里面需要的东西
    //默认都是值捕获，值捕获不能改变量，因为就相当于把a直接替换成了10.
    //值捕获不能修改外边的值，因为只是简单的拷贝
    //
    {
        cout << a << endl;
        cout << b << endl;
    }
    
    //地址捕获/引用捕获
    auto func = [&a, &b]
    {
        cout << a <<endl;
    } //酱紫就可以捕获地址中的值了，也就是最新的值，而不是捕获变量在上方的值。
    
    a = 20; 
}
```

## C14
### 泛型Lambda表达式 (参数也可以用auto了)
```cpp
auto func = [](auto v1, auto v2){return v1 + v2;};
//之前参数不能写auto
```
### 对捕获的变量可以进行出初始化

## C17
### 可以进行初始化的if-else 以及switch 语句
```cpp
if (int a = 10; a>10){}
```

## 异常
### 有可能出一场的地方 用try{}包着
###  try下面紧接着可以扔一个数字出去
### catch(数据类型) 接到throw扔出来的数据类型，产生相应的提示信息

## 智能指针(Smart Pointer)
### 传统指针存在的问题
#### 1.需要手动管理内存
#### 2.容易发生内存泄漏(忘记释放，出现异常)
#### 3.释放之后产生野指针
### 智能指针就解决了这些问题
#### 1.auto_ptr:属于C98标准，在C11中不推荐使用(不能用于数组)
#### 2.shared_ptr:属于C11
#### 3.unique_ptr:属于C11
```cpp
class Person{
    
}

int main()
{
    auto_ptr<Person> p(new Person());
    //用完自动调用析构，和delete
    //智能指针销毁，它指向的对象也销毁   
}
```
### 自己实现
```cpp
class SmartPointer
{
    pubilc:
        SmartPoint(T *obj) : m_obj(obj){}
        ~SmartPoint(){
            if(m_obj == nullptr) return;
            
            delete m_obj;
        }
     
    T *operator ->()
    {
        return m_obj;
    }
}

int main()
{
   {
     SmartPointer<Person> p(new Person(20));
     p -> run();
   }
}
```
### shared_ptr
####  强引用，两个强引用指针不能互相指，不然堆空间的不能释放
```cpp
shared_ptr<Person> p(new Person(10));
```
### weak_ptr
#### 弱引用 ，一强一弱可以指
### unique_ptr
#### 也会对一个对象产生强引用，它可以确保同一时间只有1个指针指向对象
```cpp
unique_ptr<Person> p1(new Person());
unique_ptr<Person> p2(p1); // 酱紫不可以，所以不常用
/**********************************
unique_ptr<Person> p0;
{
    unique_ptr<Person> p1(new Person());
    p0 = std::mov(p1)
}
**********************************/
//酱紫就可以
```