# Создание библиотек программирования

Создание библиотек программирования в общем напоминает создание обычных приложений, однако есть определённые тонкости.

В заголовочном файле библиотеки описываются ресурсы, предназначенные для экспорта \(то есть для использования программами\). Эти ресурсы называются **интерфейсом библиотеки **\(или **программным интерфейсом библиотеки** – **API** от Application Programming Interface\). Пример заголовочного файла статической библиотеки, описывающей работу с комплексными числами, представлен на листинге 1. Как видно, это обычное объявление класса.

**Листинг 1. Заголовочный файл библиотеки комплексных чисел.**

```cpp
#ifndef COMPLEX_H
#define COMPLEX_H

#include <iostream>
#include <string>

class complex{
    double _real, _imaginary;
public:
    complex(const double& = 0, const double& = 0);
    complex(const complex&);
    ~complex();
    
    void operator +=(const complex&);
    void operator -=(const complex&);
    void operator *=(const complex&);
    void operator /=(const complex&);
    
    complex& operator = (const complex&);
    complex& operator = (const double&);
    
    const double& real() const;
    const double& imaginary() const;
    void swap(complex&);
    std::string toString() const;
    double module() const;
    complex conjugate() const;
};

complex operator + (const complex&, const complex&);
complex operator - (const complex&, const complex&);
complex operator * (const complex&, const complex&);
complex operator / (const complex&, const complex&);

std::ostream& operator << (std::ostream&, const complex&);
bool operator  == (const complex&, const complex&);

complex pow(const complex&, const complex&);
double abs(const complex&);

#endif	/* COMPLEX_H */
```

Заголовочные файлы могут включаться в проект несколько раз. Это может вызывать избыточное определение типов данных и переменных, что будет приводить к ошибкам компиляции. Именно поэтому описание ресурсов библиотек в заголовочных файлах заключается между директивами препроцессора

`#ifndef CONSTANT_NAME  
#define CONSTANT_NAME  
  
//…  
  
#endif`

Эти директивы на этапе препроцессирования программного кода позволяют включать определения ресурсов только один раз. Компиляторы фирмы Microsoft \(и Intel\) позволяют использовать вместо конструкции ветвления препроцессора конструкцию

`#pragma once`

Что является предписанием компилятору, что данный файл подключается только раз.

Реализация объявленных в заголовочном файле ресурсов происходит, как и обычно, в файле cpp.

**Листинг 2. Реализация класса комплексных чисел.**

```cpp
#include "complex.h"
#include <sstream>

complex::complex(const double& r, const double& im): _real(r), _imaginary(im){}
complex::complex(const complex& c): _real(c._real), _imaginary(c._imaginary){}
complex::~complex(){}

void complex::operator +=(const complex& c){
    _real += c._real;
    _imaginary += c._imaginary;
}
void complex::operator -=(const complex& c){
    _real -= c._real;
    _imaginary -= c._imaginary;
}
void complex::operator *=(const complex& c){
    complex tmp;
    tmp._real = _real*c._real - _imaginary*c._imaginary;
    tmp._imaginary = _real*c._imaginary + _imaginary*c._real;
    swap(tmp);
}
void complex::operator /=(const complex& c){
    complex tmp;
    tmp = (*this) * c.conjugate();
    _real = tmp._real / c.module();
    _imaginary = tmp._imaginary / c.module();
}

complex& complex::operator =(const complex& c){
    _real = c._real;
    _imaginary = c._imaginary;
    return *this;
}
complex& complex::operator =(const double& d){
    _real = d;
    _imaginary = 0;
    return *this;
}

const double& complex::real() const { return _real; }
const double& complex::imaginary() const { return _imaginary; }
void complex::swap(complex& c){
    complex tmp(c);
    c._imaginary = _imaginary;
    c._real = _real;
    _imaginary = tmp._imaginary;
    _real = tmp._real;
}
std::string complex::toString() const{
    std::ostringstream strcout;
    strcout << _real << " + i*( " << _imaginary << " )";
    return strcout.str();
}
double complex::module() const{
    return _real*_real + _imaginary*_imaginary;
}
complex complex::conjugate() const{
    return complex(_real, -_imaginary);
}

// extern class interface
complex operator + (const complex& c1, const complex& c2){
    complex tmp(c1);
    tmp += c2;
    return tmp;
}
complex operator - (const complex& c1, const complex& c2){
    complex tmp(c1);
    tmp += c2;
    return tmp;
}
complex operator * (const complex& c1, const complex& c2){
    complex tmp(c1);
    tmp += c2;
    return tmp;
}
complex operator / (const complex& c1, const complex& c2){
    complex tmp(c1);
    tmp += c2;
    return tmp;
}

std::ostream& operator << (std::ostream& os, const complex& c){
    os << c.toString();
    return os;
}
bool operator  == (const complex& c1, const complex& c2){
    return ((c1.real() == c2.real()) && (c1.imaginary() == c2.imaginary()));
}

complex pow(const complex&, const complex&){
    complex result;
    return result;
}
double abs(const complex& c){
    return c.module();
}
```

## Особенности создания динамических библиотек

В случае разработки динамических библиотек, их создание может происходить аналогично созданию статических библиотек. Особенностью, в данном случае, будет только выбор типа проекта. В этом случае библиотека может использоваться неявно \(то есть, через подключение заголовочного файла и указание использования файла импорта библиотеки\). Для использования функций библиотеки необходимо явно указать, к каким функциям может получить доступ зависимая от данной библиотеки программа. Для этого используется специальная конструкция языка _**\_\_declspec\(dllexport\)**_, указывающая, что функция \(класс\) доступна для использования. В листинге 3 представлен пример заголовочного файла, в котором описан класс комплексного числа. Этот класс предназначен на экспорт.

**Листинг 3. Экспорт классов и функций.**

```cpp
#pragma once
#include <iostream>
#include <string>

#define DLLEXPORT __declspec(dllexport)

class DLLEXPORT complex
{
    double _real, _imaginary;
public:
    complex(const double = 0, const double = 0);
    complex(const complex&);
    ~complex(void);

    complex& operator = (const complex&);
    const double& real() const;
    const double& imaginary() const;
    const std::string toString() const;

    void operator += (const complex&);
};

DLLEXPORT bool operator == (const complex&, const complex&);
DLLEXPORT complex operator + (const complex&, const complex&);
DLLEXPORT std::ostream& operator << (std::ostream&, const complex&);
DLLEXPORT int min(int, int);
```

Также функции на экспорт можно определить при помощи def-файла \(файл определения модуля\), что позволяет не использовать конструкцию \_\_declspec\(dllexport\). Файл определения модуля описывается по следующим правилам:

1. Первая строка файла есть указание имени библиотеки, при помощи инструкции _LIBRARY_.
2. Вторая секция открывается инструкцией _EXPORTS_, она указывает, какие части библиотеки идут на экспорт. Каждая следующая строка представляет имя функции и порядковый номер в библиотеке, выделенный символом @.

{% code-tabs %}
{% code-tabs-item title="extmath.h" %}
```cpp
#ifndef EXTMATH_H
#define EXTMATH_H

size_t gcd (size_t, size_t);
bool is_prime(size_t);
bool is_odd(size_t);

#endif
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="extmath.cpp" %}
```cpp
#include "extmath.cpp"

size_t gcd (size_t a, size_t b){
    if(a == 0){
        return b;
    }
    while(b != 0){
        size_t tmp = a % b;
        a = b;
        b = tmp;
    }
    return a;
}
bool is_prime(size_t p){
    size_t i = 2;
    size_t top = p / 2;
    while(i <= top){
        if(p / i == 0){
            return false;
        }
        ++ i;
    }
    return true;
}
bool is_odd(size_t p){
    return (p % 2 == 0);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**Листинг 4. Пример def-файла**

```text
LIBRARY  EXTMATH
EXPORTS
            gcd          @1
            is_prime     @2
            is_odd       @3
```

Описанный в листингах 2 и 3, класс можно использовать только в С++ проектах. Для создания библиотек, совместимых с языком С, необходимо заголовочные файлы писать в стиле С \(без использования классов, шаблонов и др. специфичные для С++ конструкции\). Также желательно в макросе DLLEXPORT приписать модификатор **extern ”C” **– это позволяет, при компиляции библиотеки, сохранить оригинальные имена экспортируемых ресурсов \(некоторые компиляторы «кодируют» имена функций и классов\).

Использование динамической библиотеки, созданной таким образом, не будет возможно в других, отличных от С/С++, языках программирования. Это связанно с тем, что в языках Pascal, Basic и др. функции читают входные параметры слева направо, тогда как в С/С++ входные параметры читаются справа налево. Для создания совместимой с этой группой языков библиотеки используется модификатор _**\_\_stdcall**_ \(или эквивалент _**\_\_pascal**_\), который меняет порядок чтения входных параметров функции.

> В OS Windows динамические библиотеки могут иметь также точку входа - функцию DllMain:
>
> ```cpp
> #include <windows.h>
>
> BOOL APIENTRY DllMain( HMODULE hModule,
>                        DWORD  ul_reason_for_call,
>                        LPVOID lpReserved
>  ){
>     switch (ul_reason_for_call){
>         case DLL_PROCESS_ATTACH:
>         case DLL_THREAD_ATTACH:
>         case DLL_THREAD_DETACH:
>         case DLL_PROCESS_DETACH:
>             break;
>     }
>     return TRUE;
> }
> ```

## Сборка библиотек

Самый простой способ создания библиотек - выбор соответстувующего проекта в среде программирования. Все современные среды программирования предлагают проекты как статических, так и динамических библиотек.



