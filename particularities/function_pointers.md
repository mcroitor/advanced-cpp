# Указатель на функцию

Функция располагается в памяти по определенному адресу, который можно присвоить указателю в качестве его значения. Адресом функции является ее точка входа. Именно этот адрес используется при вызове функции. Так как указатель хранит адрес функции, то она может быть вызвана с помощью этого указателя. Указатель на функцию можно передавать другим функциям в качестве аргумента. 

Объявление указателя на функцию идентично объявлению функции:

```cpp
type func(params);
type (*pfunc)(params);
```



```cpp
void sort(double* v, size_t length); // объявление функции
void (*pfunc)(double*, size_t); // объявление указателя на функцию
```



```cpp
typedef void (*pfunc_t)(double*, size_t);
pfunc_t pSort = sort;
```



```cpp
​#include <stdio.h>

typedef void (*event_t)(void*, void*);

struct Button{
    size_t x, y, width, height;
    char text[255];
    event_t OnClick;
};

void ButtonPressed(void* /*sender*/, void* /*arg*/){
    puts("button pressed!");
}

int main(){
    Button btn = {5, 5, 75, 30, "Press Me!", ButtonPressed};
    btn.OnClick();
    return 0;
}
```


