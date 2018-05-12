# Разработка и использование библиотек программирования

Под **библиотеками программирования** понимают **архивы ресурсов программирования**, таких как функции, классы, объекты, константы и различные переменные. В частности, константами могут быть заданы графические или мультимедийные данные. Библиотеки делятся по принципу связывания с приложением на **статические** и **динамические. **Статические библиотеки представляются двумя файлами:

1. Заголовочный файл с расширением h \(hxx, hpp\), описывающий интерфейс библиотеки;
2. Бинарный архив с расширением lib \(в Windows\) или а \(\*nix совместимых системах\), хранящий все ресурсы программирования в виде объектного кода.

При использовании функций \(классов или других ресурсов\) статических библиотек, на этапе компиляции программы из библиотеки компилятор берёт только объектные коды используемых ресурсов. Поэтому, при размере статической библиотеки в несколько мегабайт, программа вырастает в объёме на несколько десятков килобайт \(в зависимости от размера используемых функций\).

Динамические библиотеки представляются тремя файлами:

1. Заголовочный файл с расширением h \(hxx, hpp\), описывающий интерфейс библиотеки;
2. Бинарный архив с расширением dll \(в Windows\) или so \(\*nix совместимых системах\), называемый динамической \(в \*nix системах - разделяемой\) библиотекой, и хранящий все ресурсы программирования в виде бинарного кода.
3. файл с расширением lib \(в Windows\) или а \(\*nix совместимых системах\), называемый библиотекой импорта и хранящий места расположения ресурсов программирования в динамической библиотеке.

При компиляции программы, использующей динамическую библиотеку, исполняемый код программы вырастает в размере на несколько байт, так как вместо бинарного кода функции из динамической библиотеки в программу включается вызов этой функции из библиотеки, объявленный в библиотеке импорта.

Динамические библиотеки могут использоваться двумя различными способами:

* Неявное связывание библиотеки с приложением происходит при подключении заголовочного файла, а также при указании в свойствах проекта использования библиотеки. В этом случае библиотека будет загружаться в оперативную память перед запуском приложения. В случае запуска приложения операционная система проверяет наличие динамических библиотек, связанных с программой, по системным путям, в текущей директории или в оперативной памяти. Только после этого программа выполняется. Если же хотя бы одна библиотека не была найдена, то приложение завершается с соответствующей ошибкой.
* Явное связывание библиотеки осуществляется при помощи функции LoadLibrary, которая загружает библиотеку в момент вызова функции. В этом случае приложение может работать без библиотеки до того момента, пока не будет она загружена функцией LoadLibrary.

Существует также определённый тип библиотек, которые не могут быть представимы иначе как в виде исходного кода – это библиотеки, содержащие шаблоны классов и функций. Эти библиотеки поставляются в заголовочных файлах.

Заголовочные файлы обычно содержатся в папке INCLUDE компилятора, статические библиотеки и библиотеки импорта – в папке LIB, динамические библиотеки содержаться в системных папках \( c:\\Windows\System32 \). Эти пути можно переопределить в программе либо на этапе компиляции.

Каждый тип библиотеки программирования имеет свои преимущества и недостатки. Например, использование статических библиотек повышает переносимость программ за счет того, что они не зависят от каких-либо модулей \(динамических библиотек\), однако код программы вырастает. Использование динамических библиотек позволяет создавать маленькие по объёму программы, однако в процессе выполнения они будут занимать в оперативной памяти место динамической библиотекой.


