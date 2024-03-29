# Преобразование типов

В связи с тем, что С++ является строго типизированным языком, лучшая практика в преобразовании типов - избегать преобразований. Часто, если появляется необходимость в преобразовании типов \(не задуманная программистом\), значит, в программе есть какие-то проблемы.

Так как язык С++ является наследником языка С, то в программах С++ можно использовать преобразование типов в стиле С. Однако, данное преобразование типов является небезопасным, так как позволяет использовать заведомо неправильные конструкции, например:

{% code title="badboy.cpp" %}
```cpp
struct array{
  double* value;
  size_t size;
}* p = new array;

float* a = (float*)p;
```
{% endcode %}

Компилятор языка С++ в данном случае не выдаст ошибку, не смотря на то, что представлено заведомо неправильное преобразование указателей.

Для написания безопасного кода в язык С++ были введены 4 оператора преобразования типов, такие как:

* **static\_cast** - используется для преобразования ~~статических~~ переменных базового типа или сравнимых указателей \(например, указатель на объект родительского класса в указатель на объект дочернего класса\). Пример использования:

  ```cpp
  int a = 10, b = 3;
  float result = static_cast<float>(a) / b;
  ```

* **const\_cast** - используется при преобразовании неконстантных выражений в константые и наоборот.  _**Не рекомендуется к использованию!**_ Данный оператор позволяет , в частности, изменять выражения, определённые как константные.
* **dynamic\_cast** - используется для преобразования указателей. Чаще всего можно обойтись без данного преобразования. Пример использования:

  ```cpp
  struct A {int value; };
  struct B: A { void Print(){}; };
  A* a = new B();
  dynamic_cast<B*>(a)->Print();
  ```

* **reinterpret\_cast** - по поведению совпадает с преобразованием типов в стиле С. _**Не рекомендуется к использованию!**_



