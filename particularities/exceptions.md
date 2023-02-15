# Обработка исключительных ситуаций

Обработка исключений это мощный инструмент написания безопасного кода. 

В настоящее время является популярным программирование с использований обработки исключений. В большинстве случаев подобный подход не оправдан, потому что написанный код не должен иметь исключительных ситуаций вообще. Однако есть ряд ситуаций, которые требуют использование данного механизма, а именно, когда предполагаются альтернативные потоки выполнения программы, связанные с ошибками. Поэтому обработка исключений используется в случаях, когда код может иметь множество ветвей выполнения, а также в написании тестов.

Рассмотрим код, приведенный листинге **exception\_example.cpp**.

{% code-tabs %}
{% code-tabs-item title="exception\_example.cpp" %}
```cpp
#include <exception>
#include <iostream>

int main(){
    int a, b, c;
    try{
        a = b / c;
    }
    catch(std::exception e){
        std::cerr << e.what();
    }
    return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Он представляет собой безопасное деление целых чисел. В связи с тем, что переменные не инициализированы, делитель может равняться 0, что вызовет аварийное завершение программы. 

Код, который может вызывать аварийное завершение приложения, помещается в блок **try**. В случае возникновения ошибки выполняется код в блоке **catch.** Переменная в круглых скобках блока catch содержит информацию об ошибке. Для каждого типа ошибки может быть объявлен свой блок catch. Стандартная библиотека С++ предлагает ряд типов исключений, которые объявлены в заголовочном файле **exception.** В частности, базовым классом для все исключений является **std::exception**.

Проверку деления на ноль в данном случае можно выполнить менее кардинальным способом \(**division\_zero.cpp**\).

{% code-tabs %}
{% code-tabs-item title="divizion\_zero.cpp" %}
```cpp
#include <iostream>

int main(){
    int a, b, c;
    if(c != 0){
        a = b / c;
    }
    else{
        std::cerr << "division by zero";
    }
    return 0;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Однако, в случае более сложного кода подобным образом обрабатывать исключительную ситуацию уже невозможно, например, когда деление представлено в отдельной функции.
