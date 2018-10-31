# Шаблоны программирования

## Понятие шаблона функций

Шаблоном функций называют общее описание семейства функций \(обобщенного алгоритма \). Об этом уже было сказано, но рассмотрим их поподробней.

{% code-tabs %}
{% code-tabs-item title="find\_min\_int.cpp" %}
```cpp
int min(int a, int b){
  int result = a;
  if(b < a) {
    result = b;
  }
  return result;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Если нам понадобится поиск минимального целого числа, то можно воспользоваться функцией, представленной в листинге _**find\_min\_int.cpp**_. Однако, для поиска минимума среди действительных чисел придется в программу добавить функцию из листинга _**find\_min\_float.cpp**_:

{% code-tabs %}
{% code-tabs-item title="find\_min\_float.cpp" %}
```cpp
float min(float a, float b){
  float result = a;
  if(b < a) {
    result = b;
  }
  return result;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Таким образом, для каждого типа приходится переопределять функцию.

{% code-tabs %}
{% code-tabs-item title="find\_min\_tpl.cpp" %}
```cpp
template<typename TYPE>
TYPE min(TYPE a, TYPE b){
  TYPE result = a;
  if(b < a) {
    result = b;
  }
  return result;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Шаблоны классов

Шаблоном классов называется описание множества типов данных с одинаковым поведением. Стандартным примером шаблона классов являются контейнеры. Пример описания шаблона классов:

{% code-tabs %}
{% code-tabs-item title="class\_template.cpp" %}
```cpp
template<class TYPE>
class AClass{
    // implementation
};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

