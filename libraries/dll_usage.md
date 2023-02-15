# Использование динамических библиотек

При разработке приложений программисту \(да и программе\) необходимо знать, какие функции предоставляет ему библиотека. Эту информацию даёт программисту и разрабатываемой программе заголовочный файл.

## Статическая компоновка

В библиотеке программирования хранится байт-код функции (или другого ресурса программирования). При сборке приложения, после компиляции всех исходников в байт-код (объектные файлы), компоновщик собирает все объекты в один исполняемы файл. В этом случае в исполняемый файл попадает только код той библиотечной функции, которая используется. Размер исполняемого файла увеличивается, однако приложение, при запуске, не зависит от других файлов (динамических библиотек).

## Динамическая компоновка

Статическая компоновка \(использование статических библиотек\) по реализации совпадает с динамической компоновкой. Разница наблюдается в принципе работы компоновщика. 

## Явное связывание динамической библиотеки

```cpp
HMODULE WINAPI LoadLibrary(LPCTSTR lpFileName);  
BOOL WINAPI FreeLibrary(HMODULE hModule);  
FARPROC WINAPI GetProcAddress(HMODULE hModule, LPCSTR lpProcName);
```

```cpp
LPTSTR MAKEINTRESOURCE(WORD wInteger);  
HRSRC WINAPI FindResource(HMODULE hModule, LPCTSTR lpName, LPCTSTR pType);  
HGLOBAL WINAPI LoadResource(HMODULE hModule, HRSRC hResInfo);  
HBITMAP LoadBitmap(HINSTANCE hInstance, LPCTSTR lpBitmapName);  
int WINAPI LoadString(HINSTANCE hInstance, UINT uID, LPTSTR lpBuffer, int nBufferMax);
```

