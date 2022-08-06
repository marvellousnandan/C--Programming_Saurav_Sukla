## Lec 44 - Virtual Function in C++ (Part 1)

Base class Pointer:

- Base class pointer can point to the object of any of its descendant class

- But its converse is not true

Note:

Agar pointer parent class ka banate hai to uske child class ka object ka address contain kar sakta hai...

but child class pointer can't points parent's object...

```cpp
#include<iostream>
using namespace std;
class A
{
    public:
        void f1() {    }    //function Overriding
};
class B: public A
{
    public:
        void f1() {    }    //function Overriding
        void f2() {    }
};
int main()
{
    A *p,o1;
    B o2;
    p=&o2;
    // o2.f1(); //B(child) class f1() will work...
    p->f1();   //A(Parent) class f1() will work...
}
//See next program
```

p->f1( )    //=>  Here,  p will not able to find out o1 ka address hai ya o2 ka address hai...

pointer ka type declaration ke time pata lag gaya tha (    A *p,o1;    )... i.e. pointer ka type hai "A". Early binding karte waqt compiler... pointer ke type ko dekhega... na ki pointer ke content ko...

Conclusion:

Function ko call agar object ke dwara kiya ja raha hai to object ke type ko dekhte hai...

Pointer ke dawara agar function ko call kar rahe hai to pointer ke type ko dekhte hai...

parent ke pointer ke through calling ho raha hai to... child ka object banane ke baad bhi child wala f1( ) nahi chal raha hai... aaur ye problem hai...

(The purpose of Overriding is that for child class object... latest child definition will works...)

To solve this problem the solution is to not proceed Early Binding  

We need LATE BINDING in the run time... because in run time we can know the content of the pointer... and which address pointer contains...

```cpp
// Solution (same as previous program)

#include<iostream>
using namespace std;
class A
{
    public:
    virtual void f1() {    }    //SEE
};
class B: public A
{
    public:
        void f1() {    }    //function Overriding
        void f2() {    }
};
int main()
{
    A *p,o1;
    B o2;
    p=&o2;
    // o2.f1(); //B(child) class f1() will work...
   // p->f1();  //A(Parent) class f1() will work... // WRONG //see below
    p->f1();    // f1() of child(B) class will work...
}
```

virtual void f1() { }    =>    Now compiler understand that f1() function will have LATE BINDING...

In late binding, call ke waqt pointer ke type ko aadhar nahi mana jaayega... pointer ke content of aadhar mana jaayega... aaur pointer B(child) ke object ko point kar raha hai... issi liye child ke object wala f1( ) function call ho raha hai...

virtual function is recommended in the case of Function Overriding...

f1() is also a virtual function in child class which is no need to declare (Since, f1() is declared virtual in parent class...)

----------

## Lec 45 - Virtual Function in C++ (Part 2)

In any class if even atleast one virtual function exists... for that class compiler will declare a pointer / <mark>variable</mark> in the class as a member from its side...

![](images/1.png)

*_vptr    =>   pointer / <mark>variable</mark> made by compiler and this varible is an Instance member variable...

*_vptr exists for both class A and class B

vptr is a pointer... and it contains Address of Array...

(Jitne objects banyenge sabhi ke liye alag alag vptr banega...)



vtable is a Static Array of function pointers... and it doesn't depends on Object...



EB:    Early Binding    (In Early Binding, Compiler sees the pointer type... )

LB:    Late Binding

![](images/2.png)

Conclusion:

In Early Binding, Compiler sees the pointer type... and thats the issue of Overriding...

object f1() to humne child ka banaya hai... aur child wala f1 nahi chal raha...

ye dekhte hi nahi ki pointer mai kis object ka Address hai...

So, we use Virtual Function for Early Binding....

(AND)

Aagr Late Binding hoti hai to run time pe pointer ka type nahi... pointer kisko point kar raha hai usko dekha jata hai... Aur pointer jisko point kar raha hota hai uske ander <mark>vptr</mark> hota hai jo <mark>vtable</mark> ko point karta hai... Aur vtable mai virtual function ke addresses hote hai...

----------

## Lec 46 - Abstract Class in C++ (Part 1)

**<u>Pure Virtual function</u>**:

A do nothing function is called pure virtual function.

(Ek aisa function jiski koi definition na ho usee pure virtual function bol sakte hai...)



void fun( )=0; 

(i.e. function should be assign to 0... So compiler will know that function is of "Do Nothing" nature...)



here, Overriding should be required in the child class....

NOTE:

do nothing person cann't have Objects...

```cpp
#include<iostream>
using namespace std;
class Person
{
    public:
        void fun()=0;    //do nothing function
};
class Student:public Person
{
    public:
        void fun()
        {
        
        }
}
```


