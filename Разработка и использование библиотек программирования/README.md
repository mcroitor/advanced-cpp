# Разработка и использование библиотек программирования

Под **библиотеками программирования** понимают **архивы ресурсов программирования**, таких как функции, классы, объекты, константы и различные переменные. В частности, константами могут быть заданы графические или мультимедийные данные. Библиотеки делятся по принципу связывания с приложением на **статические** и **динамические.** Статические библиотеки представляются двумя файлами:

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

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#x421;&#x442;&#x430;&#x442;&#x438;&#x447;&#x435;&#x441;&#x43A;&#x438;&#x435; &#x431;&#x438;&#x431;&#x43B;&#x438;&#x43E;&#x442;&#x435;&#x43A;&#x438;</b>
      </th>
      <th style="text-align:left"><b>&#x414;&#x438;&#x43D;&#x430;&#x43C;&#x438;&#x447;&#x435;&#x441;&#x43A;&#x438;&#x435; &#x431;&#x438;&#x431;&#x43B;&#x438;&#x43E;&#x442;&#x435;&#x43A;&#x438;</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>&#xB7; &#x412; &#x43F;&#x440;&#x43E;&#x433;&#x440;&#x430;&#x43C;&#x43C;&#x443;
          &#x432;&#x43A;&#x43B;&#x44E;&#x447;&#x430;&#x435;&#x442;&#x441;&#x44F;
          &#x43A;&#x43E;&#x434; &#x442;&#x43E;&#x43B;&#x44C;&#x43A;&#x43E; &#x438;&#x441;&#x43F;&#x43E;&#x43B;&#x44C;&#x437;&#x443;&#x435;&#x43C;&#x44B;&#x445;
          &#x444;&#x443;&#x43D;&#x43A;&#x446;&#x438;&#x439;.</p>
        <p>&#xB7; &#x41F;&#x440;&#x43E;&#x433;&#x440;&#x430;&#x43C;&#x43C;&#x430;
          &#x43D;&#x435; &#x437;&#x430;&#x432;&#x438;&#x441;&#x438;&#x442; &#x43E;&#x442;
          &#x43D;&#x430;&#x43B;&#x438;&#x447;&#x438;&#x44F; &#x431;&#x438;&#x431;&#x43B;&#x438;&#x43E;&#x442;&#x435;&#x43A;
          &#x432; &#x41E;&#x421;.</p>
        <p>&#xB7; &#x417;&#x430;&#x43D;&#x438;&#x43C;&#x430;&#x44E;&#x442; &#x43E;&#x442;&#x43D;&#x43E;&#x441;&#x438;&#x442;&#x435;&#x43B;&#x44C;&#x43D;&#x43E;
          &#x43C;&#x430;&#x43B;&#x43E; &#x43C;&#x435;&#x441;&#x442;&#x430; &#x432;
          &#x41E;.&#x41F;.</p>
        <p>&#xB7; &#x411;&#x43E;&#x43B;&#x44C;&#x448;&#x43E;&#x439; &#x440;&#x430;&#x437;&#x43C;&#x435;&#x440;
          &#x43F;&#x440;&#x43E;&#x433;&#x440;&#x430;&#x43C;&#x43C;.</p>
        <p>&#xB7; &#x412; &#x441;&#x43B;&#x443;&#x447;&#x430;&#x435;&#x442; &#x43E;&#x431;&#x43D;&#x43E;&#x432;&#x43B;&#x435;&#x43D;&#x438;&#x44F;
          &#x431;&#x438;&#x431;&#x43B;&#x438;&#x43E;&#x442;&#x435;&#x43A;&#x438;
          &#x442;&#x440;&#x435;&#x431;&#x443;&#x435;&#x442;&#x441;&#x44F; &#x43F;&#x435;&#x440;&#x435;&#x43A;&#x43E;&#x43C;&#x43F;&#x438;&#x43B;&#x44F;&#x446;&#x438;&#x44F;
          &#x43F;&#x440;&#x43E;&#x433;&#x440;&#x430;&#x43C;&#x43C;&#x44B;.</p>
      </td>
      <td style="text-align:left">
        <p>&#xB7; &#x41F;&#x440;&#x43E;&#x433;&#x440;&#x430;&#x43C;&#x43C;&#x430;
          &#x437;&#x430;&#x433;&#x440;&#x443;&#x436;&#x430;&#x435;&#x442; &#x432;
          &#x43E;&#x43F;&#x435;&#x440;&#x430;&#x442;&#x438;&#x432;&#x43D;&#x443;&#x44E;
          &#x43F;&#x430;&#x43C;&#x44F;&#x442;&#x44C; &#x432;&#x441;&#x44E; &#x438;&#x441;&#x43F;&#x43E;&#x43B;&#x44C;&#x437;&#x443;&#x435;&#x43C;&#x443;&#x44E;
          dll.</p>
        <p>&#xB7; &#x41D;&#x435;&#x441;&#x43A;&#x43E;&#x43B;&#x44C;&#x43A;&#x43E;
          &#x43F;&#x440;&#x43E;&#x433;&#x440;&#x430;&#x43C;&#x43C; &#x440;&#x430;&#x431;&#x43E;&#x442;&#x430;&#x435;&#x442;
          &#x441; &#x43E;&#x434;&#x43D;&#x43E;&#x439; dll &#x43E;&#x434;&#x43D;&#x43E;&#x432;&#x440;&#x435;&#x43C;&#x435;&#x43D;&#x43D;&#x43E;.</p>
        <p>&#xB7; &#x41C;&#x430;&#x43B;&#x435;&#x43D;&#x44C;&#x43A;&#x438;&#x439;
          &#x440;&#x430;&#x437;&#x43C;&#x435;&#x440; &#x43F;&#x440;&#x43E;&#x433;&#x440;&#x430;&#x43C;&#x43C;</p>
        <p>&#xB7; &#x422;&#x440;&#x435;&#x431;&#x443;&#x435;&#x442; &#x43D;&#x430;&#x43B;&#x438;&#x447;&#x438;&#x435;
          dll &#x432; &#x41E;&#x421;.</p>
        <p>&#xB7; &#x412;&#x43E;&#x437;&#x43C;&#x43E;&#x436;&#x43D;&#x43E; &#x43E;&#x431;&#x43D;&#x43E;&#x432;&#x43B;&#x435;&#x43D;&#x438;&#x435;
          &#x432;&#x435;&#x440;&#x441;&#x438;&#x438; dll &#x431;&#x435;&#x437; &#x43F;&#x435;&#x440;&#x435;&#x43A;&#x43E;&#x43C;&#x43F;&#x438;&#x43B;&#x44F;&#x446;&#x438;&#x438;
          &#x43F;&#x440;&#x43E;&#x433;&#x440;&#x430;&#x43C;&#x43C;&#x44B;.</p>
      </td>
    </tr>
  </tbody>
</table>Можно предложить следующие рекомендации:

1. Если библиотека редкая или нестандартная, лучше использовать статическую компоновку программы. 
2. Если библиотека широко распространена или стандартная, то рекомендуется использовать динамическую компоновку программы. 
3. При разработке библиотеки программирования желательно создавать обе версии библиотек: и динамическую, и статическую, чтобы покрыть все требования пользователя.

