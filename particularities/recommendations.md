# Рекоммендации по написанию кода

Следует помнить, что стандартная библиотека С++ является очень мощным средством, которым также надо учиться пользоваться. Поэтому, чтобы написанный вами код и стандартная библиотека (или другая библиотека, а может даже ваш коллега) дружили, надо придерживаться ряда правил по написанию кода. И это касается как стиля написания, так и желательных реализаций.

А поработаем-ка на конкретном примере, на обыкновенной дроби. Обыкновенная дробь представляется двумя целыми числами, причем второе число не может быть равно 0.

```cpp
struct Fraction {  
    int numerator, denominator;  
};
```

Итак, что же нужно сделать, чтобы ваш класс без проблем заработал со стандартной библиотекой?

## 1. __Конструктор по умолчанию, конструктор копирования, оператор копирования.__ 
   
> Ремарка: оператор копирования часто также называют конструктором копирования. 

Не постесняйтесь определить все эти конструкторы. Стандартные алгоритмы могут работать с формальными параметрами, передаваемыми как по ссылке, так и по значению. Также алгоритмы (которые любят изменять ваши контейнеры) ну очень хотят копировать элементы. Конечно, компилятор создаст сам все эти функции по умолчанию, однако есть два немаловажных "НО":

 * если определен хоть один конструктор, остальные конструкторы компилятор не создаст
 * если вы работаете с выделением памяти, то конструкторы компилятора вам точно не подходят.

## 2. __Операторы сравнения и упорядочения.__
   
Также полезная штука, которую можно определить автоматически. Это помогает без проблем работать с алгоритмами поиска и сортировки. Конечно, иногда полезнее определить соответствующие функторы, например, в случае, когда существует несколько способов упорядочивания. Например, людей мы можем упорядочивать как по имени в алфавитном порядке, так и по возрасту или по росту. Достаточно определить только операторы `==` и `<` и подключить заголовочный файл `utility`, остальные операторы сравнения определены в пространстве `std::rel_ops`.

## 3. __Арифметические операторы.__ 
 
Возможно, они вам могут понадобиться. Здесь можно дать несколько советов.

Напоминаю, что определение дружественных функций нарушает принцип инкапсуляции. Поэтому старайтесь обходиться без них. По той же причине нежелательны различные функции доступа. Я вообще крайне пессимистично настроен по отношению к функциям доступа и дружественным функциям. Функции доступа можно вводить лишь в том случае, если в реальной жизни объект позволяет такое поведение (например, человек может поменять фамилию, а дробь свой числитель - не может). Как же решить получающуюся проблему? Оказывается, оператор сложения легко выразить через оператор `+=`, и т.д.

```cpp
Fraction operator + (const Fraction& p1, const Fraction& p2){  
    Fraction tmp = p1;  
    tmp += p2;   
    return tmp;  
}  
```

## 4. __Ввод / вывод.__ 

Полезная вещь, которую также можно определить без использования функций доступа или же дружественных функций. Например, оператор ввода хорошо определяется при помощи конструкторов.

```cpp
std::istream& operator >> (std::istream& in, Fraction& p){  
    int n, d;  
    in >> n >> d;  
    p = Fraction(n, d);  
    return in;  
}
```

Исходя из указанных рекомендаций класс обыкновенной дроби определяется следующим образом (`fraction.h`):

```cpp
#ifndef FRACTION_H  
#define FRACTION_H  
  
#include <utility>  
#include <iostream>  
#include <string>  
  
using namespace std::rel_ops;  
  
class Fraction {  
    int numerator, denominator;  
      
    void normalize();  
public:  
    Fraction();  
    Fraction(const Fraction&);  
    Fraction(const int, const int);  
    Fraction operator =(const Fraction&);  
    virtual ~Fraction();  
      
    std::string to_string() const;  
    double value() const;  
      
    void operator +=(const Fraction&);  
    void operator -=(const Fraction&);  
    void operator *=(const Fraction&);  
    void operator /=(const Fraction&);      
};  
  
bool operator == (const Fraction&, const Fraction&);  
bool operator < (const Fraction&, const Fraction&);  
  
Fraction operator + (const Fraction&, const Fraction&);  
Fraction operator - (const Fraction&, const Fraction&);  
Fraction operator * (const Fraction&, const Fraction&);  
Fraction operator / (const Fraction&, const Fraction&);  
  
std::istream& operator >> (std::istream&, Fraction&);  
std::ostream& operator << (std::ostream&, const Fraction&);  
  
#endif /* FRACTION_H */  
```

Соответствующий CPP файл будет следующим (`fraction.cpp`):

```cpp
#include "Fraction.h"  
  
//additional  
#include <sstream>  
  
int gcd(int a, int b) {  
    return (b == 0) ? a : gcd(b, a % b);  
}  
  
int _abs(int p) {  
    return (p > 0) ? p : -p;  
}  
  
int _sign(int p){  
    return (p < 0) ? -1 : 1;  
}  
  
// realization  
Fraction::Fraction()  
: numerator(0), denominator(1) {  
}  
  
Fraction::Fraction(const Fraction& orig)  
: numerator(orig.numerator), denominator(orig.denominator) {  
}  
  
Fraction::Fraction(const int n, const int d)  
: numerator(n), denominator(d) {  
    normalize();  
}  
  
Fraction Fraction::operator=(const Fraction& orig) {  
    numerator = orig.numerator;  
    denominator = orig.denominator;  
    return * this;  
}  
  
Fraction::~Fraction() {  
}  
  
void Fraction::normalize() {  
    int tmp = gcd(_abs(numerator), _abs(denominator));  
    numerator /= _sign(denominator) * tmp;  
    denominator /= _sign(denominator) * tmp;  
}  
  
std::string Fraction::to_string() const {  
    std::ostringstream strout;  
    strout << numerator << " / " << denominator;  
    return strout.str();  
}  
  
double Fraction::value() const {  
    return double(numerator) / denominator;  
}  
//operators  
  
void Fraction::operator*=(const Fraction& p) {  
    numerator *= p.numerator;  
    denominator *= p.denominator;  
    normalize();  
}  
  
void Fraction::operator/=(const Fraction& p) {  
    numerator *= p.denominator;  
    denominator *= p.numerator;  
    normalize();  
}  
  
void Fraction::operator+=(const Fraction& p) {  
    numerator = numerator * p.denominator + p.numerator*denominator;  
    denominator *= p.denominator;  
    normalize();  
}  
  
void Fraction::operator-=(const Fraction& p) {  
    numerator = numerator * p.denominator - p.numerator*denominator;  
    denominator *= p.denominator;  
    normalize();  
}  
  
Fraction operator+(const Fraction& p1, const Fraction& p2) {  
    Fraction tmp = p1;  
    tmp += p2;  
    return tmp;  
}  
  
Fraction operator-(const Fraction& p1, const Fraction& p2) {  
    Fraction tmp = p1;  
    tmp -= p2;  
    return tmp;  
}  
  
Fraction operator*(const Fraction& p1, const Fraction& p2) {  
    Fraction tmp = p1;  
    tmp *= p2;  
    return tmp;  
}  
  
Fraction operator/(const Fraction& p1, const Fraction& p2) {  
    Fraction tmp = p1;  
    tmp /= p2;  
    return tmp;  
}  
  
bool operator == (const Fraction& p1, const Fraction& p2){  
    return p1.to_string() == p2.to_string();  
}  
  
bool operator < (const Fraction& p1, const Fraction& p2){  
    return p1.value() < p2.value();  
}  
  
std::istream& operator>>(std::istream& in, Fraction& p) {  
    int n, d;  
    char fix; // is a slash sign  
    in >> n >> fix >> d;  
    p = Fraction(n, d);  
    return in;  
}  
  
std::ostream& operator<<(std::ostream& out, const Fraction& p) {  
    out << p.to_string();  
    return out;  
}
```

Хочу отметить некоторые хитрости, использованные в коде:

Дробь всегда нормализуется после изменений, то есть сокращается - за это отвечает закрытый метод `normalize()`.

Определен метод `to_string`, который удобно использовать при сравнении двух объектов (то есть если строковые представления объектов равны, то равны и сами объекты), а также для вывода в поток.
в операторе чтения из потока есть переменная, которая используется для чтения знака дроби. Это сделано для совместимости с оператором записи в поток. В принципе, реализация сравнения через `to_string` удобна, но не эффективна. Однако, данная идея подсказывает универсальный способ сравнения объектов через сериализацию.

В данной реализации не хватает важной функциональности - проверки деления на ноль.

## константы

В приведенном примере кода можно увидеть использование модификатора __const__. Настоятельно рекоммендуется его использование, так как он 

 - подскажет компилятору, как можно оптимизировать код;
 - защитит от ошибок при кодировании.

Например, приписанный `const` к параметру функции гарантирует, что внутри функции данный параметр не будет изменён. Указание `const` после имени метода гарантирует, что объект, у которого данный метод вызывается, не будет изменён данным методом.

## наименования

Программист, на самом деле, больше читает код чем пишет его. Поэтому, если вы пишете код, позаботьтесь о его читателях. Это будете вы, через некоторое время или сторонний человек, поэтому, естественно, код должен быть легко читаемым и понимаемым.

Рекоммендую ознакомиться, например, с [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html). Это поможет вам писать привычный и понятный код. Ну, и читать его, конечно же.