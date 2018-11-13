# Использование динамических библиотек

При разработке приложений программисту \(да и программе\) необходимо знать, какие функции предоставляет ему библиотека. Эту информацию даёт программисту и разрабатываемой программе заголовочный файл.

## Статическая компоновка

## Динамическая компоновка

Статическая компоновка \(использование статических библиотек\) по реализации совпадает с динамической компоновкой. Разница наблюдается в принципе работы компоновщика. 

## Явное связывание динамической библиотеки

`HMODULE WINAPI LoadLibrary(LPCTSTR lpFileName);  
BOOL WINAPI FreeLibrary(HMODULE hModule);  
FARPROC WINAPI GetProcAddress(HMODULE hModule, LPCSTR lpProcName);`

`LPTSTR MAKEINTRESOURCE(WORD wInteger);  
HRSRC WINAPI FindResource(HMODULE hModule, LPCTSTR lpName, LPCTSTR pType);  
HGLOBAL WINAPI LoadResource(HMODULE hModule, HRSRC hResInfo);  
HBITMAP LoadBitmap(HINSTANCE hInstance, LPCTSTR lpBitmapName);  
int WINAPI LoadString(HINSTANCE hInstance, UINT uID, LPTSTR lpBuffer, int nBufferMax);`

