# yandex

## Сборка мусора и планировщик - секреты

TODO

## Ссылочные типы данных

Ссылочные типы данных в GoLang - это типы данных, которые хранятся в куче (heap) и передаются по ссылке, а не по значению. Это означает, что при передаче ссылочного типа данных в функцию, функция работает с оригинальным объектом, а не с его копией. Некоторые из ссылочных типов данных в GoLang:

Срезы (slices) - это динамические массивы, которые представляют собой ссылку на последовательность элементов определенного типа.

Карты (maps) - это ассоциативные массивы, которые представляют собой ссылку на набор пар ключ-значение.

Каналы (channels) - это механизм для обмена данными между горутинами (goroutines) в многопоточной программе.

Указатели (pointers) - это переменные, которые хранят адрес в памяти другой переменной.

Структуры (structs) - это пользовательские типы данных, которые могут содержать поля разных типов.

Интерфейсы (interfaces) - это типы данных, которые определяют набор методов, которые должны быть реализованы для типа данных, чтобы он удовлетворял интерфейсу.

Функции (functions) - это типы данных, которые могут быть переданы в качестве аргументов другим функциям или возвращены из функций.

Все эти типы данных являются ссылочными в GoLang и передаются по ссылке, а не по значению.

## Передача слайса как аргумента функции

Длина и вместимость передаются по значению, но массив значений передается по ссылке. Вследствие этого получается неявное поведение: добавленные элементы не сохранятся в исходный слайс, но изменение существующих останется.

```go
package main

import "fmt"

func main() {
	cap := 4 // если 3, то ответы разные; если 4 - одинаковые
	var a = make([]int, 0, cap)
	a = append(a, 111, 222, 333)

	fmt.Printf("%#v\n", getArray(a))
	fmt.Printf("%#v\n", a)
}

func remove(slice []int, s int) []int {
	return append(slice[:s], slice[s+1:]...)
}

func getArray(a []int) []int {
	a = append(a, 444)
	a = remove(a, 0)
	return a
}
```

## [/article](./article/) - алгоритмические задачи

- [Как проходят алгоритмические секции на собеседованиях в Яндекс](https://habr.com/ru/companies/yandex/articles/449890/)

### Задача A. Камни и украшения

> Даны две строки строчных латинских символов: строка J и строка S. Символы, входящие в строку J, — «драгоценности», входящие в строку S — «камни». Нужно определить, какое количество символов из S одновременно являются «драгоценностями». Проще говоря, нужно проверить, какое количество символов из S входит в J.

Это очень простая разминочная задача, к которой прилагаются решения на нескольких языках программирования, чтобы участники могли освоиться с проверяющей системой.

Алгоритм достаточно простой: из строки с «драгоценностями» необходимо построить множество, затем пройтись по строке с «камнями» и каждый символ проверить на вхождение в это множество. Используйте такую реализацию множества, чтобы гарантировать линейную сложность полученного решения, несмотря на то, что входные строки очень короткие и поэтому возможно сдать даже квадратичный по сложности алгоритм.

### Задача B. Последовательно идущие единицы

> Требуется найти в бинарном векторе самую длинную последовательность единиц и вывести её длину.

Алгоритм решения следующий: пройтись по всем элементам массива; встретив единицу, нужно увеличить счётчик длины текущей последовательности, а, встретив ноль, нужно обнулить этот счётчик. В конце нужно вывести максимальное из значений, которые принимал счётчик.

Проверьте, что правильно обрабатываете ситуацию, когда массив заканчивается на искомую последовательность единиц. При аккуратной реализации такая ситуация не потребует специальной обработки.

Постарайтесь использовать лишь константный объём дополнительной памяти.

### Задача C. Удаление дубликатов

> Дан упорядоченный по неубыванию массив целых 32-разрядных чисел. Требуется удалить из него все повторения.

Правильный алгоритм последовательно обрабатывает элементы массива, сравнивая их с последним выведенным. Нужно не забыть обновлять переменную, содержащую последний выведенный элемент и, кроме того, не ошибиться при обработке последнего элемента.

При решении этой задачи также не нужно использовать дополнительную память.

### Задача D. Генерация скобочных последовательностей

> Дано целое число n. Требуется вывести все правильные скобочные последовательности длины 2 \* n, упорядоченные лексикографически (см. https://ru.wikipedia.org/wiki/Лексикографический_порядок). В задаче используются только круглые скобки.

Это пример относительно сложной алгоритмической задачи. Будем генерировать последовательность по одному символу; в каждый момент мы можем к текущей последовательности приписать либо открывающую скобку, либо закрывающую. Открывающую скобку можно дописать, если до этого было добавлено менее n открывающих скобок, а закрывающую — если в текущей последовательности количество открывающих скобок превосходит количество закрывающих. Такой алгоритм при аккуратной реализации автоматически гарантирует лексикографический порядок в ответе; работает за время, пропорциональное произведению количества элементов в ответе на n; при этом требует линейное количество дополнительной памяти.

Примером неэффективного алгоритма был бы следующий: сгенерируем все возможные скобочные последовательности, а затем выведем лишь те из них, что окажутся правильными. При этом объём ответа не позволит решить задачу быстрее, чем тот алгоритм, что приведёт выше.

### Задача E. Анаграммы

Эта достаточно простая задача — типичный пример задачи, для решения которой необходимо использовать ассоциативные массивы. При решении нужно учитывать, что символы могут повторяться, поэтому необходимо использовать не множества, а словари. Поэтому решение будет следующим: составим из каждой строки по словарю, который для каждого символа будет хранить количество его повторений; затем сравним получившиеся словари. Если они совпадают, необходимо вывести единицу, в противном случае — ноль.

Альтернативное решение: отсортируем входные строки, а затем сравним их. Это решение хуже в том, что оно работает медленнее, а также меняет входные данные. Зато такое решение не использует дополнительной памяти.

Если в процессе собеседования у вас возникло несколько вариантов решения, отличающихся своими по своим характеристикам, расскажите об этом. Всегда здорово, когда разработчик знает несколько вариантов решения задачи и может рассказать об их сильных и слабых сторонах.

### Задача F. Слияние k сортированных списков

> Даны k отсортированных в порядке неубывания массивов неотрицательных целых чисел, каждое из которых не превосходит 100. Требуется построить результат их слияния: отсортированный в порядке неубывания массив, содержащий все элементы исходных k массивов. Длина каждого массива не превосходит 10 \* k.

Для каждого массива создадим по указателю; изначально каждый указатель расположен в начале соответствующего массива. Элементы, соответствующие позициям указателей, поместим в любую структуру данных, которая поддерживает извлечение минимума — это может быть мультимножество или, например, куча. Далее будем извлекать из этой структуры минимальный элемент, помещать его в ответ, сдвигать позицию указателя в соответствующем массиве и помещать в структуру данных очередной элемент из этого массива.

В этой задаче многие испытывают сложности с форматом ввода. Обратите внимание на то, что первые элементы строк не описывают элементы массивов, они описывают длины массивов!

#### Answer #1:

> В данном коде мы создаем структуру Item, которая содержит значение элемента, номер массива, из которого был взят элемент, и индекс элемента в массиве. Затем мы создаем приоритетную очередь PriorityQueue на основе этой структуры, которая будет использоваться для извлечения минимального элемента.
>
> Далее мы создаем указатели на начало каждого массива и добавляем первые элементы каждого массива в приоритетную очередь. Затем мы начинаем извлекать минимальные элементы из очереди, добавляя их в результирующий массив и сдвигая указатель на соответствующий массив. Если указатель не достиг конца массива, мы добавляем следующий элемент из этого массива в приоритетную очередь.
>
> В итоге мы получаем отсортированный в порядке неубывания массив, содержащий все элементы исходных k массивов. В примере выше мы передаем в функцию mergeArrays три отсортированных массива и получаем отсортированный массив [0 0 3 5 6 6 7 28].

#### Answer #2:

> В этом коде мы используем те же самые структуры данных и алгоритм, что и в предыдущем примере, но более лаконично записываем код. Мы создаем массив pointers для хранения указателей на текущий элемент в каждом массиве, инициализируем его нулями и используем его для проверки достижения конца каждого массива.
>
> Также мы используем оператор append для добавления элементов в результирующий массив, вместо явного указания индекса. Это делает код более читаемым и лаконичным.
>
> В итоге мы получаем тот же отсортированный в порядке неубывания массив, содержащий все элементы исходных k массивов.

#### Answer #3:

> Этот код считывает количество массивов k, затем считывает каждый массив и его элементы, сливает массивы в один отсортированный массив и выводит его элементы. Для слияния массивов используется куча, которая поддерживает извлечение минимума и добавление элементов. Код также содержит функции для построения кучи и поддержания ее свойств.

## [/leetcode](./leetcode/)

### linked lists:

- [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
- [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
- [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
- [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

### binary search:

- [704. Binary Search](https://leetcode.com/problems/binary-search/)
- [374. Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/)
- [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
- [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
- [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
- [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

### hash table:

- [136. Single Number](https://leetcode.com/problems/single-number/) - решить за O(1) по памяти
- [1. Two Sum](https://leetcode.com/problems/two-sum/)
- [18. 4Sum](https://leetcode.com/problems/4sum/)
- [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)
- [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)
- [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

### queue/stack:

- [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

### dfs/bfs:

- [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)
- [301. Remove Invalid Parentheses](https://leetcode.com/problems/remove-invalid-parentheses/)

### sort:

- [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

### heap/hash:

- [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)
- [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

### two pointers:

- [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
- [763. Partition Labels](https://leetcode.com/problems/partition-labels/)

### sliding window:

- [480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)
- [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
- [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

### tree:

- [100. Same Tree](https://leetcode.com/problems/same-tree/)
- [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
- [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
- [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/)

### greedy problems:

- [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
- [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
- [714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)
- [309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)
