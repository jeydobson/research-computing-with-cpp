---
title: Function Templates
---

## Function Templates

### Function Templates Example

Credit to [www.cplusplus.com][OverloadedFunctions]:

{{cppfrag('02','sumFunctionExample/sumFunctionExample.cc')}}

Produces this output when run:

{{execute('02','sumFunctionExample/sumFunctionExample')}}


### Why Use Function Templates?

```c++
int Add(int a, int b);
double Add(double a, double b);
```

* Instead of function overloading
    * Reduce your code duplication
    * Reduce your maintenance
    * Reduce your effort
    * Also see this [Additional tutorial][TemplatesTutorial].


### Language Definition 1

From the [language reference](http://en.cppreference.com/w/cpp/language/function_template):


```
template < parameter-list > function-declaration
```

so:

```
template < class T >  // note 'class'
void MyFunction(T a, T b) 
{
  // do something
}
```

or:

```
template < typename T1, typename T2 >  // note 'typename'
T1 MyFunctionTwoArgs(T1 a, T2 b) 
{
  // do something
}
```

### Language Definition 2

* Also
    * Can use ```class``` or ```typename```.
    * I prefer ```typename```.
    * Template parameter can apply to references, pointers, return types, arrays etc.


### Default Argument Resolution

Given:

```
double GetAverage<typename T>(const std::vector<T>& someNumbers);
```

then:

```
std::vector<double> myNumbers;
double result = GetAverage(myNumbers);
```

will call:

```
double GetAverage<double>(const std::vector<double>& someNumbers);
```

So, if function parameters can inform the compiler uniquely as to which function to instantiate, its automatically compiled. 


### Explicit Argument Resolution

However, given:

```
double GetAverage<typename T>(const std::vector<T>& someNumbers);
```

and:

```
std::vector<int> myIntegers;
double result = GetAverage(myIntegers);
```

But you don't want the int version called, you can:

```
double result = GetAverage<double>(myIntegers);
```

* i.e. name the template parameter explicitly.
* Cases for Explicit Template Argument Specification
    * Call a specific version (eg. int as above)
    * Also if method parameters do not allow compiler to deduce anything eg. ```PrintSize()``` method.


### Beware of Code Bloat

Given:

```
double GetMax<typename T1, typename T2>(const &T1, const &T2);
```

and:

```
double r1 = GetMax(1,2);
double r2 = GetMax(1,2.0);
double r3 = GetMax(1.0,2.0);
```

* The compiler will generate 3 different max functions.
* Be Careful
    * Executables/libraries get larger
    * Compilation time will increase
    * Error messages get more verbose
    
    
### Two Stage Compilation

* Basic syntax checking (eg. brackets, semi-colon, etc)
* Further checks when function is instantiated (eg. existence of + operator).


### Instantiation

* Template functions can be
    * .h file only
    * .h file that includes .cxx/.txx file (e.g. ITK)
    * .h file and separate .cxx/.txx file (by convention a .hpp file)
* Object Code is only really generated if code is used
* In general
    * Most libraries/people prefer header only implementations

    
### Explicit Instantiation - 1

Language Reference [here][FunctionTemplate]

[Microsoft Example][ExplicitInstantiationMicrosoft]

Given (library) header:
{{cppfrag('02','explicitInstantiation/explicitInstantiation.h')}}

Given (library) implementation:
{{cppfrag('02','explicitInstantiation/explicitInstantiation.cc')}}


### Explicit Instantiation - 2

Given client code:
{{cppfrag('02','explicitInstantiation/explicitInstantiationMain.cc')}}

We get:
{{execute('02','explicitInstantiation/explicitInstantiationMain')}}


### Explicit Instantiation - 3

* Mainly used by compiled library providers
* Forces instantiation of the function
* Must appear after the definition
* Must appear only once for given argument list
* Stops implicit instantiation, errors like

```
Linking CXX executable explicitInstantiationMain.x
Undefined symbols for architecture x86_64:
  "void f<float>(float)", referenced from:
```


### Implicit Instantiation - 1

* Instantiated as they are used
* Normally via ```#include``` header files. 

Given (library) header, that containts implementation:
{{cppfrag('02','implicitInstantiation/implicitInstantiation.h')}}


### Implicit Instantiation - 2

Given client code:
{{cppfrag('02','implicitInstantiation/implicitInstantiation.cc')}}

We get:
{{execute('02','implicitInstantiation/implicitInstantiation')}}

[OverloadedFunctions]: http://www.cplusplus.com/doc/tutorial/functions2 'Overloaded Functions and Template Functions'
[FunctionTemplate]: http://en.cppreference.com/w/cpp/language/function_template 'Function Template Reference'
[TemplatesTutorial]: http://www.codeproject.com/Articles/257589/An-Idiots-Guide-to-Cplusplus-Templates-Part 'Templates Tutorial'
[ExplicitInstantiationDisc]: http://stackoverflow.com/questions/2351148/explicit-instantiation-when-is-it-used 'Explicit Instantiation Discussion'
[ExplicitInstantiationMicrosoft]: http://msdn.microsoft.com/en-us/library/by56e477%28VS.80%29.aspx 'Microsoft Explicit Instantiation Example'