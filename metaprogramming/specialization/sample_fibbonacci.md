# Пример: числа Фиббоначчи

> **Задача:** Вычислить N-й член последовательности Фиббоначчи во время компилляции.

Последовательность Фиббоначчи определяется рекурентной формулой `F(n) = F(n - 1) + F(n - 2), F(0) = 0, F(1) = 1`

Данное определение можно описать в шаблонах следующим образом:

{% code-tabs %}
{% code-tabs-item title="tpl\_fibbonacci.cpp" %}
```cpp
#include <iostream>

template<size_t Value>
struct fibbonacci {
    enum {
        RESULT = fibbonacci<Value - 1>::RESULT + fibbonacci<Value - 2>::RESULT
    };
};

template<>
struct fibbonacci<0> { enum { RESULT = 0 }; };

template<>
struct fibbonacci<1> { enum { RESULT = 1 }; };

int main(){
    std::cout << fibbonacci<20>::RESULT << std::endl;
    return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

