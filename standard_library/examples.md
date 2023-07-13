# Примеры использования стандартной библиотеки С++

В данной главе приведены ряд примеров, представляющих собой постепенное усложнение задания и его решение.

## Пример 1: 

_Дан целочисленный массив. Отсортировать элементы массива в убывающем порядке._

Решение:

{% code title="stdcpp01.cpp" %}
```cpp
#include <algorithm>
#include <functional>
#include <iostream>


int main(){
    int a[5] = {5, 2, 1, 8, 7};
    std::sort(a, a+5, std::less<int>());
    int i;
    for(i = 0; i != 5; ++i){
        std::cout << a[i] << " ";
    }
    return 0;
}
```
{% endcode %}

## Пример 2: 

_Считать из файла input.txt массив целых чисел, разделенных пробельными символами. Отсортировать их и записать в файл output.txt._

Решение:

{% code title="stdcpp02.cpp" %}
```cpp
#include <vector>
#include <algorithm>
#include <fstream>

int main(){
    std::ifstream fin("input.txt");
    std::ofstream fout("output.txt");

    std::vector<int> v;

    std::copy(std::istream_iterator<int>(fin), 
        std::istream_iterator<int>(),
        std::inserter(v, v.end()));
    std::sort(v.begin(), v.end());
    std::copy(v.begin(), 
        v.end(), 
        std::ostream_iterator<int>(fout, " "));
    return 0;
}
```
{% endcode %}

## Пример 3: 

_В файле input.txt хранится список, содержащий информацию о людях: фамилия, имя, возраст. Считать эти данные в массив, отсортировать их по возрасту и записать в файл output.txt. Вывести на экран информацию о человеке, чей возраст более 20, но менее 25 лет._

Решение:

{% code title="stdcpp03.cpp" %}
```cpp
#include <string>
#include <vector>
#include <fstream>
#include <algorithm>

struct Man{
    std::string firstname, secondname;
    size_t age;
};

std::ostream& operator << (std::ostream& out, const Man& p){
    out << p.firstname << " " << p.secondname << " " << p.age;
    return out;
}

std::istream& operator >> (std::istream& in, Man& p){
    in >> p.firstname >> p.secondname >> p.age;
    return in;
}

struct comparator{
    comparator(){}
    bool operator ()(const Man& p1, const Man& p2){
        return p1.age < p2.age;
    }
};

struct Predicat{
    size_t begin, end;
    Predicat(int p1, int p2): begin(p1), end(p2) {}
    bool operator ()(const Man& p){
        return (p.age > begin) && (p.age < end);
    }
};

int main(){
    std::ifstream fin("input.txt");
    std::ofstream fout("output.txt");

    std::vector<Man> v;
    std::vector<Man>::iterator i;

    std::copy(std::istream_iterator<Man>(fin), 
        std::istream_iterator<Man>(),
        std::inserter(v, v.end()));
    std::sort(v.begin(), v.end(), comparator());

    i = std::find_if(v.begin(), v.end(), Predicat(20, 25));
    std::cout << (*i) << std::endl;

    std::copy(v.begin(), 
        v.end(), 
        std::ostream_iterator<Man>(fout, "\n"));
    return 0;
}
```
{% endcode %}

##  Пример 4:

Сложение двух трехмерных векторов

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <functional>
#include <iterator>

typedef std::array<int, 3> v3d;

v3d operator + (const v3d& a, const v3d& b){
    v3d tmp;
    std::transform(
        a.begin(), a.end(), 
        b.begin(), 
        tmp.begin(), 
        std::plus<v3d::value_type>());
    return tmp;
}

std::ostream& operator << (std::ostream& out, const v3d p){
    std::copy(p.begin(), p.end(), std::ostream_iterator<v3d::value_type>(out, " "));
    return out;
}

int main(int argc, char** argv) {
    v3d a = {1, 2, 3},
            b = {3, 2, 1},
            c;
    c = a+b;
    std::cout << c;
    return 0;
}
```

