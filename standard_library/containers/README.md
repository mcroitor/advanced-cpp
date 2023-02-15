# Контейнеры

Как уже было отмечено, под контейнером понимают объект, содержащий другие \(однотипные\) объекты, называемые элементами контейнера. Стандартная библиотека С++ предоставляет типичные контейнеры, такие как: список, вектор, очередь, карта, множество и др.Доступ к элементам контейнера осуществляется через итераторы.

К контейнерам выдвигается ряд общих требований. Это осуществляется для того, чтобы использование контейнеров было одинаковым, независимо от его реализации. Соответственно, часто контейнеры бывают взаимозаменяемы.

##  Требования контейнеров

| выражение | возвращаемый тип | утверждение/примечание состояние до/после |
| :--- | :--- | :--- |
| **X::value\_type** | Т |   |
| **X::reference** |  T& |   |
| **X::const\_refe rence** |  const T& |   |
| **X::pointer** | тип указателя, указывающий на X::reference | указатель на T в модели памяти, используемой контейнером |
| **X::iterator** | тип итратора, указывающий на X::reference | итератор любой категории, кроме итератора вывода. |
| **X::const\_iter ator** | тип итератора, указывающий на X:: const\_reference | постоянный итератор любой категории, кроме итератора вывода. |
| **X::difference \_type** | знаковый целочисленный тип | идентичен типу расстояния X::iterator и X::const\_iterator |
| **X::size\_type** | беззнаковый целочисленный тип  | size\_type может представлять любое неотрицательное значение difference\_type |
| **X u;** |   | конструктор по умолчанию. после: u.size\(\) == 0. |
| **X\(\)** |   |  конструктор по умолчанию. X\(\).size\(\) == 0. |
| **X\(a\)** |   |  конструктор копирования. a == X\(a\). |
| **X u\(a\); X u = a;** |   | конструктор копирования. после: u = a. |
| **\(&a\)-&gt;~X\(\)** | результат не используется | после: a.size\(\) == 0.  примечание: деструктор применяется к каждому элементу a, и вся память возвращается. |
| **a.begin\(\)** | iterator; const\_iterator для постоянного a |  итератор, указывающий на первый элемент |
| **a.end\(\)** | iterator; const\_iterator для постоянного a |  итератор, указывающий на конец контейнера |
| **a == b** | обратимый в bool | == - это отношение эквивалентности. примечание: equal определяется в разделе алгоритмов. |
| **a != b** | обратимый в bool |   |
| **r = a** | X& | после: r == a. |
| **a.size\(\)** | size\_type |  количество элементов в контейнере |
| **a.max\_size\(\)** | size\_type | size\(\) самого большого возможного контейнера. |
| **a.empty\(\)** | обратимый в bool |   |
| **a &lt; b** | обратимый в bool | до: &lt; определён для значений T. &lt; - отношение полного упорядочения.  lexicographical \_compare определяется в разделе алгоритмов. |
| **a &gt; b** | обратимый в bool |   |
| **a &lt;= b** | обратимый в bool |   |
| **a &gt;= b** | обратимый в bool |   |
| **a.swap\(b\)** | void |   |

## Последовательные контейнеры

Последовательные контейнеры хранят свои элементы в строго линейном порядке. К последовательным контейнерам относятся хорошо известные структуры данных вектор \(vector\), список \(list\), очередь \(deque\), а также строка символов \(string\). Существуют также некоторые последовательные контейнеры, которые могут отсутствовать в стандартной библиотеке С++: стек \(stack\) и массив \(array\).

В таблице ~~7~~ указаны требования к последовательным контейнерам \(обязательные операции\).

Таблица ~~7~~ Требования последовательностей \(в дополнение к контейнерам\)

| выражение | возвращаемый тип | утверждение/примечание состояние до/после |
| :--- | :--- | :--- |
| **X\(n, t\) X a\(n, t\);** |   | после: size\(\) == n. создаёт последовательность с n копиями t. |
| **X\(i, j\) X a\(i, j\);** |   | после: size\(\) == расстоянию между i и j. создаёт последовательность, равную диапазону \[i, j\). |
| **a.insert\(p, t\)** | iterator | вставляет копию t перед p. возвращаемое значение указывает на вставленную копию. |
| **a.insert\(p, n, t\)** | результат не используется | вставляет n копий t перед p. |
| **a.insert\(p, i, j\)** | результат не используется | вставляет копии элементов из диапазона \[i, j\) перед p. |
| **a.erase\(q\)** | результат не используется | удаляет элемент, указываемый q. |
| **a.erase\(ql, q2\)** | результат не используется | удаляет элементы в диапазоне \[ql, q2\). |

 В таблице ~~8~~ указаны операции, которые выражаются через обязательные операции контейнеров. В различных типах контейнеров эти опреации могут отсутствовать, например, у контейнера типа вектор нет возможности вставлять элементы в начало контейнера.

Таблица ~~8~~ Необязательные операции последовательностей

| выражение | возвращаемый тип | семантика исполнения | контейнер |
| :--- | :--- | :--- | :--- |
| **a.front\(\)** | reference; const\_reference для постоянного a | \*a.begin\(\) | vector, list, deque |
| **a.back\(\)** | reference; const\_reference для постоянного a | \*\(--a. end\(\)\) | vector, list, deque |
| **a.push\_front\(t\)** | void | a.insert\(a.begin\(\), t\) | list, deque |
| **a.push\_back\(t\)** | void | a.insert\(a.end\(\), t\) | vector, list, deque |
| **a.pop\_front \(\)** | void | a.erase\(a.begin\(\)\) | list, deque |
| **a.pop\_back \(\)** | void | a.erase\(-- a.end\(\)\) | vector, list, deque |
| **a\[n\]** | reference; const\_reference для постоянного a | \*\(a.begin\(\) + n\) | vector, deque |

Каждый последовательный контейнер имеет свои преимущества и недостатки, поэтому выбор того или иного контейнера для использования должен диктоваться задачей. Например, список обладает быстрым способом вставки новых элементов в любую позицию \(за константное время\), а у архитектура вектора определяет быстрый доступ к элементам \(за константное время\).

## Ассоциативные контейнеры

Таблица ~~9~~ Требования ассоциативных контейнеров \(в дополнение к контейнерам\)

| выражение | возвращаемый тип | утверждение/примечание состояние до/после |
| :--- | :--- | :--- |
| **X::key\_type** | Key |   |
| **X::key\_compare** | Compare | по умолчанию less&lt;key\_type&gt;. |
| **X::value\_compare** | тип бинарного предиката | то же, что key\_compare для set и multiset;  отношение упорядочения пар, вызванное первым компонентом \(т.е. Key\), для map и multimap. |
| **X\(c\) X a\(c\);** |   | создает пустой контейнер;  использует с как объект сравнения. |
| **X\(\) X a;** |   | создает пустой контейнер;  использует Compare\(\) как объект сравнения. |
| **X\(i,j,c\)  X a\(i,j,c\);** |  | cоздает пустой контейнер и вставляет в него элементы из диапазона \[i, j\); использует с как объект сравнения. |
| **X\(i,j\) X a\(i,j\);** |   | то же, что выше, но использует Compare\(\) как объект сравнения. |
| **a.key\_comp\(\)** | X::key\_compare | возвращает объект сравнения, из которого а был создан. |
| **a.value\_comp\(\)** | X::value\_compare | возвращает объект value\_compare, созданный из объекта сравнения. |
| **a\_uniq.insert\(t\)** | pair&lt;iterator, bool&gt; | вставляет t, если и только если в контейнере нет элемента с ключом, равным ключу t. Компонент boolвозвращенной пары показывает, происходит ли вставка, а компонент пары iterator указывает на элемент с ключом, равным ключу t. |
| **a\_eq.insert\(t\)** | iterator | вставляет t и возвращает итератор, указывающий на вновь вставленный элемент. |
| **a.insert\(p, t\)** | iterator | вставляет t, если и только если в контейнерах с уникальными ключами нет элемента с ключом, равным ключу t; всегда вставляет t в контейнеры с дубликатами.  всегда возвращает итератор, указывающий на элемент с ключом, равным ключу t. итератор p - подсказка, указывающая, где вставка должна начать поиск. |
| **a.insert\(i, j\)** | результат не используется | вставляет в контейнер элементы из диапазона \[i, j\); |
| **a.erase\(k\)** | size\_type | стирает все элементы в контейнере с ключом, равным k. возвращает число уничтоженных элементов. |
| **a.erase\(q\)** | результат не используется | стирает элемент, указанный q. |
| **a.erase\(ql, q2\)** | результат не используется | стирает все элементы в диапазоне \[ql, q2\). |
| **a.find\(k\)** | iterator; const\_iterator для константы a | возвращает итератор, указывающий на элемент с ключом, равным k, или a.end\(\), если такой элемент не найден. |
| **a.count\(k\)** | size\_type | возвращает число элементов с ключом, равным k. |
| **a.lower\_bound\(k\)** | iterator;  const\_iterator для константы a | возвращает итератор, указывающий на первый элемент с ключом не меньше, чем k. |
| **a.upper\_bound\(k\)** | iterator; const\_iterator для константы a | возвращает итератор, указывающий на первый элемент с ключом больше, чем k. |
| **a.equal\_range\(k\)** | pair&lt;iterator, itеrator&gt;; pair&lt;const\_iter ator, const\_iterator&gt;для константы a | эквивалент make\_pair\(lower\_boud\(k\), upper\_bound \(k\)\). |
