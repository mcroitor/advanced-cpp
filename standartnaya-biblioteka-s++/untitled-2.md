# Примитивы, определённые в библиотеке STL

В STL определены некоторые вспомогательные структуры и функции, облегчающие разработку приложений.

Например, чтобы избежать избыточности определений операторов сравнения, библиотека содержит следующее \(заголовочный файл _utility_, пространство имен rel\_ops\):

```cpp
template <class Tl, class T2>
inline bool operator !=(const T1& x, const T2& y) { 
     return !(x == y);
}

template <class Tl, class T2>
inline bool operator >(const T1& x, const T2& y) {
    return y < x;
}

template <class Tl, class T2>
inline bool operator <=(const T1& x, const T2& y) { 
     return !(y < x);
}

template <class Tl, class T2>
    inline bool operator >=(const T1& x, const T2& y) { 
 return !(x < y);
}
```

Как видно, для некоторого класса достаточно определить операторы &lt; и ==, остальные операторы будут построены автоматически на их основе.

Другим полезным определением в стандартной библиотеке является упорядоченная пара некоторых переменных различного типа. Выглядит это определение следующим образом:

```cpp
template <class T1, class T2> struct pair {
    typedef T1 first_type;
    typedef T2 second_type;
    T1 first;
    T2 second;
    pair() : first(T1()), second(T2()) {}
    pair(const T1& x, const T2& y) : first(x), second(y) {}
    template <class U, class V> 
    pair (const pair<U,V> &p) : first(p.first), second(p.second) { }
 };
```

Для упрощения создания упорядоченной пары введена функция make\_pair, которая выглядит следующим образом:

```cpp
template <class T1,class T2> 
pair<T1,T2> make_pair (T1 x, T2 y) {
    return ( pair<T1,T2>(x,y) );
 }
```

В стандартной библиотеке большинство алгоритмов оперируют функциональными объектами. Базовыми классами для этих объектов являются классы unary\_function и binary\_function \(объявлены в заголовочном файле _functional_\):

```cpp
template <class Arg, class Result>
 struct unary_function {
     typedef Arg argument_type;
     typedef Result result_type;
 };
```

```cpp
template <class Arg1, class Arg2, class Result>
 struct binary_function {
     typedef Arg1 first_argument_type;
     typedef Arg2 second_argument_type;
     typedef Result result_type;
 };
```

Рекомендуется и пользовательские функциональные объекты наследовать от этих классов. На основе данных структур в STL объявлены некоторые полезные функторы:

| Арифметические операции: | Описание |
| :--- | :--- |
| **plus** | шаблонный функтор сложения |
| **minus** | шаблонный функтор вычитания |
| **multiplies** | шаблонный функтор умножения |
| **divides** | шаблонный функтор деления |
| **modulus** | шаблонный функтор вычисления модуля |

Принципиальная реализация данных функторов подобна реализации функтора plus \(с \)

{% code-tabs %}
{% code-tabs-item title="plus.cpp" %}
```cpp
template<class TYPE>
struct plus: public binary_function<TYPE, TYPE, TYPE> {
    TYPE operator()(const TYPE& arg1, const TYPE& arg2)const {
        return arg1 + arg2;
    }
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

| Операции сравнения: | Описание |
| :--- | :--- |
| **equal\_to** | шаблонный функтор сравнения |
| **not\_equal\_to** | шаблонный функтор неравенства |
| **greater** | шаблонный функтор, оператор &gt; |
| **less** | шаблонный функтор, оператор &lt; |
| **greater\_equal** | шаблонный функтор, оператор &gt;= |
| **less\_equal** | шаблонный функтор, оператор &lt;= |

| Логические операции: |  |
| :--- | :--- |
| **logical\_and** | шаблонный функтор, логическое и |
| **logical\_or** | шаблонный функтор, логическое или |
| **logical\_not** | шаблонный функтор, логическое отрицание |

Эти операции могут использоваться совместно с алгоритмами, изменяющими последовательности.

### Адапторы

Адапторы являются специальными функторами, которые "изменяют" \(на самом деле создают новый тип\) существующие функторы. Например, подшивки позволяют преобразовать бинарный функтор в унарный, фиксируя в качестве одного из параметров некоторое значение.

| Отрицатели |  |
| :--- | :--- |
| **not1** | возвращает отрицание унарного функтора |
| **not2** | возвращает отрицание бинарного функтора |

| Подшивки |  |
| :--- | :--- |
| **bind1st** | "подшивает" к функтору некоторое значение первым параметром |
| **bind2nd** | "подшивает" к функтору некоторое значение вторым параметром |

