---
title: Function Pointers in C
categories:
- C
---

Function pointers are like having a pet dragon; dangerous, unpredictable, and often unnecessary. So, if you find yourself dealing with function pointers, embrace the chaos and remember to keep a fire extinguisher handy! For your difficult journey ahead, here is a brief reference guide.

A very basic example of how you can assign a pointer to a function.
~~~C
void (*FunctionPtr)()= &Function;
~~~
Here `FunctionPtr` points to the adress of `Function` of type `void` with no arguments. It can be invoked by simply calling `(*FunctionPtr)()`.

```C
void (*FunctionPtr)()=Function;
```
This is also a valid statement, behaves exactly the same as previous one. In fact, all of the statements below behave the same.

```C
void (*FunctionPtr1)() = Function;
void (*FunctionPtr2)() = &Function;
void (*FunctionPtr1)() = *Function;
void (*FunctionPtr2)() = *******Function;
```
or alternatively and interestingly with `typedef`
```C
typedef void (FunctionType) ();
typefef void (*FunctionPtrType)();

FunctionType FunctionPtr = Function; // <-- This is an error
FunctionType* FubctionPtr = Function;
FunctionPtrType FunctionPtr = *Function;

```

Hence, can be invoked as follows

```C
(*FunctionPtr)();
FunctionPtr();
(**FunctionPtr)();
```

In short, dereferencing a function or address of a function evaluates to the pointer to that function.

On an unrelated note, `typedef void (FunctionType)()` can be used to declare a function of the same type, so `FunctionType Function` evaluates to `void Function()`.

You can also do a bit of hack to force a pointer to take a value of different type than it's own. It's useful invoking symbols from dynamic libraries.

```C
int (*funcPtr)();
int value;
void (*funcPtr2)() = function;

*(void**)(&funcPtr) = funcPtr2;

value = functionPtr2();
```

As you can see `funcPtr2` is of type `void` but can be assigned to a function pointer of type `int`. It is useful to silence the compiler warnings for mismatch of pointer types i-e `void *` to `int(*functionPtr)`.

You can also return a function pointer of any type, preferred way to do it is with `typedef`, consider this:

```C
typedef int(*functPtrType)(int,int)

int add(int a, int b)
{
    return a+b;
}

funcPtrType returnfunction(functPtrType funcPtr)
{
    return funcPtr;
}

/* In another function you can call
funcPtr2=returnfunction(add);
*/
```

or another way to go about this is without using `typedef`

```C
int add(int a, int b)
{
    return a+b;
}

int(*returnfunction(int(*funcPtr)(int,int))(int,int)
{
    return funcPtr;
}
```
This code is equivalent to what is written above.
