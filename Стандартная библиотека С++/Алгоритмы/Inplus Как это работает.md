# inplus: Как это работает

Реализации алгоритмов стандартной библиотеки С++ очень просты и эффективны. Поэтому было бы полезным уделить некоторое время на чтение исходников.

Как уже было отмечено, требования к контейнерам и итераторам определяют, какие возможности \(типы, методы и операции\) у нас есть для написания алгоритмов. В частности, из определения алгоритма find:

```cpp
template <class InputIterator, class T>
InputIterator find(InputIterator first, InputIterator last, const T& value);
```

можно понять, что итераторы обладают минимальными требованиями, а именно, оператор инкремента, оператор сравнения, оператор разыменования. Реализация данного алгоритма имеет следующий вид:

{% code-tabs %}
{% code-tabs-item title="ex\_find.cpp" %}
```cpp
template <class InputIterator, class T>
InputIterator find(InputIterator first, InputIterator last, const T& value){
    while(first != last){
        if(*first == value){
            return first;
        }
        ++ first;
    }
    return first;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Предикатная форма не сильно отличается от прямой.

{% code-tabs %}
{% code-tabs-item title="ex\_find\_if.cpp" %}
```cpp
template <class InputIterator, class Predicate>
InputIterator find_if(InputIterator first, InputIterator last, Predicate pred) {
    while (first != last) {
        if (pred(*first)) {
            return first;
        }
        ++first;
    }
    return first;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Также просто выглядят и другие алгоритмы, например, сравнение двух полуинтервалов.

{% code-tabs %}
{% code-tabs-item title="ex\_equal.cpp" %}
```cpp
template <class InputIterator1, class InputIterator2>
bool equal(InputIterator1 first1, InputIterator1 last1, InputIterator2 first2){
    while(first1 != last1){
        if(*first1 != *first2){
            return false;
        }
        ++ first1;
        ++ first2;
    }
    return true;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Наконец, один из алгоритмов, меняющих последовательности, прямой алгоритм копирования и его предикатная форма. Кстати, предикатная форма была введена только в стандарте С++11, однако алгоритм очень полезен в случае, когда нужно выбрать все элементы, удовлетворяющие условию.

{% code-tabs %}
{% code-tabs-item title="ex\_copy.cpp" %}
```cpp
template <class InputIterator, class OutputIterator>
OutputIterator copy(
        InputIterator first, 
        InputIterator last, 
        OutputIterator result) {
    while (first != last) {
        *result = *first;
        ++first;
        ++result;
    }
    return result;
}

template <class InputIterator, class OutputIterator, class Predicate>
OutputIterator copy_if(
        InputIterator first, 
        InputIterator last, 
        OutputIterator result, 
        Predicate pred) {
    while (first != last) {
        if (pred(first)) {
            *result = *first;
        }
        ++first;
        ++result;
    }
    return result;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

