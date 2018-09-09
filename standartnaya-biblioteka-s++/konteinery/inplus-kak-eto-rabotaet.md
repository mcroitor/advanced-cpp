# Inplus: Как это работает

Приведенные таблицы показывают, каким требованиям должен удовлетворять интерфейс контейнера, чтобы его можно было использовать вместе с алгоритмами из стандартной библиотеки С++.

## Некоторые рекомендации по использованию контейнеров

| Критерий | Рекомендация |
| :--- | :--- |
| Возможность вставки нового элемента на произвольную позицию контейнера | Если нужна, выбирайте последовательный контейнер; ассоциативные контейнеры не подходят. |
| быстрая вставка | list |
| Структура памяти контейнера должна соответствовать правилам языка C | Только vector |
| критична скорость поиска | Рассмотрите сортированные векторы и ассоциативные контейнеры |

## Общие рекомендации по использованию STL

* Не пытайтесь писать контейнерно-независимый код, так вы себя ограничиваете в возможностях
* Реализуйте быстрое и корректное копирование объектов в контейнерах
* Старайтесь не использовать итераторы повторно \(особенно после удаления элементов из контейнера\)
* Используйте алгоритмы вместо циклов
* Всегда включайте только нужные заголовки - это позволит сократить время компиляции
* **Научитесь читать сообщения компилятора**
* Обязательно прочтите следующие книги:
  * Герб Саттер, Андрей Александреску, _**Стандарты программирования на С++**_
  * Герб Саттер, _**Решение сложных задач на С++**_

## Реализация класса array, совместимого со стандартной библиотекой С++

Подобная реализация существует в С++, начиная со стандарта С++11.

{% code-tabs %}
{% code-tabs-item title="array.cpp" %}
```cpp
template <typename TYPE, size_t SIZE>
class array{
    TYPE el[SIZE];
public:
    typedef TYPE value_type;
    typedef TYPE& reference;
    typedef const TYPE& const_reference;
    typedef TYPE* pointer;
    typedef const TYPE* const_pointer;
    typedef TYPE* iterator;
    typedef const TYPE* const_iterator;

    array(){}
    array(TYPE* p, size_t s){
        size_t i;
        for(i = 0; i != s && i != SIZE; ++i){
            el[i] = p[i];
        }
    }
    array(const array<TYPE, SIZE>& a){
        size_t i;
        for(i = 0; i != SIZE; ++i){
            el[i] = a.el[i];
        }
    }
    ~array(){}

    array<TYPE, SIZE> operator = (const array<TYPE, SIZE>& a){
        size_t i;
        for(i = 0; i != SIZE; ++i){
            el[i] = a.el[i];
        }
        return *this;
    }
    TYPE& operator [](const size_t& i){
        return el[i];
    }


    iterator begin() { return el; }
    const_iterator begin() const { return el; }
    iterator end() { return el + SIZE; }
    const_iterator end() const { return el + SIZE; }

    bool operator == (const array<TYPE, SIZE>& a){
        size_t i;
        for(i = 0; i != SIZE; ++i){
            if( el[i] != a.el[i]) return false;
        }
        return true;
    }

    size_t size() const { return SIZE; }
    size_t max_size() const { return SIZE; }
    bool empty() const { return false; }

    void swap(array<TYPE, SIZE>& a){
        array<TYPE, SIZE> tmp = a;
        a = *this;
        *this = tmp;
    }
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

