# J

J — функциональный векторный язык программирования, созданный Кеннетом Айверсоном и Роджером Хуэем в начале 1990-х. Является диалектом языка APL. 

Основные черты — ориентированность на MIMD инструкции, функциональный подход (переменные исключены вообще, вместо них действует механизм переименований и комбинаторы).

Рассмотрим возможности данного языка на основе нескольких примеров

 - 2 + 2
 
> 4

Для начала неплохо.

В нотации J + называется глаголом, а 2 страва и слева от него называются существительным

J легко стравляется как со скалярными значениями так и с векторами и даже многомерными матрицами

 - 1 2 3 4 + 5 6 7 8

>6 8 10 12
 
Для демострации работы с матрицами сначала создадим одну из них
 
 - 2 3 $ 1

>1 1 1

>1 1 1

Глагол $ означает - придать размерность. В данном примере он создает матрицу 2 на 3 элемента и заполняет ее единицами. Если существетильное справа будет представляет из себя вектор, то матрица заполнится с помощью его циклического повторения

 - 3 5 $ 1 2 3 4
 
>1 2 3 4 1

>2 3 4 1 2

>3 4 1 2 3

Убедимся, что плюс работает и с матрицами

 - (3 5 $ 1 2 3 4) + (3 5 $ 4 3 2 1)

>5 5 5 5 5

>5 5 5 5 5

>5 5 5 5 5

Глаголы в J могут изпользоваться как в диадном (принимающим 2 существительных) так и монадном варианте

 - 3%2
 
>1.5

 - %2
 
>0.5

% в диадном варианте означет - разделить, а в монадном - обратить

Так же в J присутствуют наречия, которые добавляются к глаголам, и изменяют их функциональность

/ - наречие между

 - f/ (a a1 a2 a3) эквивалентно (((a f a1) f a2) f a3)

Пример
 
 - +/ 1 2 3 4
 
> 10


J умеет комбинировать глаголы для создания новых глаголов

Рассмотри классический пример

 - (+/ % #) 1 2 3 4 

> 2.5

Данное социтание глаголов называется филкой и работает по такому принципу

 - (f g h) a эквивалентно (f a) g (h a)

Разберем пример

> (+/ % #) 1 2 3 4 
> это
> (+/ 1 2 3 4) % (# 1 2 3 4)

- +/ находит сумму элементов векторами
- #  находит длину вектора
- %  делит сумму на колличество

то есть (+/ % #) - это среднее арифметическое

В J присутствует возможномсть именования глаголов 

 - avg =: +/ % #
 - avg 1 2 3 4 
 
> 2.5

Кроме вилки в j так же существует крюк

- (g h) a эквивалентно a g (h a)

Цепочки глаголов, состоящие более чем из трех разбираются на филки справа налево

 - (f g h t k r) a эквивалентно (f (g h (t k r))) a

Так же в J присутствует глагол-связка, который позволяет выполнять глаголы последовательно

 - g@h a эквивалентно g(h a)
 
Пример

 - -:@(+/) 1 2 3 4
 
> 5

Сложить и разделить попалам

В J отсутствует приоритет операций, и они всегда выполняются права налево

 - 2 * 2 + 2
 
> 8


#### Разберем какой-нибудь небольшой пример

 - geron =: %:@(*/)@(-:@(+/)-,&0)
 
Данный код реализует формулу Герона, которая вычисляет площадь треугольника по его сторонам.

 - geron 8 6 10

> 24

данный глагод состоит из 3 последовательных глаголов (в порядке выполнения)

 - 1. -:@(+/)-,&0
 - 2. */
 - 3. %:
 
2 и 3 являются простыми глаголами в то время как 1 является вилкой

 - -:@(+/) - ,&0 = f g h 
 - где 
 - f = -:@(+/)
 - g = -
 - h = ,&0
 
Рассмотрим их в порядке выполнения

 - ,&0

Состоит из глагола , и наречия &

, означает - скрепить вектора 

 - 2 3 4 , 1 2 3

>2 3 4 1 2 3

Наречие &(число) означает, что мы должны выполнить даидную версию глагола, и использовать в качестве второго существительного заданное после знака &

 - f&a b эквивалентно b f a
 - a&f b эквивалентно a f b
 - где f - глагол, а a и b - существительные
 
выходит данный глагол добавляет 0 к вектору 

 -  (,&0) 8 6 10
 
> 8 6 10 0

Следующим выполнится глагол 

 - -:@(+/)

Который состаит из 2 глаголов, первый из каторых сумма элементов, а второй - поделить попалам

Как вы уже догадались, этот глагол находит полупериметр

 - -:@(+/) 8 6 10

> 12

Затем выполняется глагол - и вычитает из полумериметры все стороны

 - (-:@(+/)-,&0) 8 6 10

> 4 6 2 12

В первом элементе выходного вектора находтся полупериметр без первой стороны, и так далее. Хочу заметить, что в последнем элементе находтся сам полупериметр. собственно, для этого мы и добавляли 0 к начальному вектору

Далее - дело за малым - выполнить последовательно 2 глагола

> */ - который находит произведение всех элементов вектора
> %: - который извлекает квадратный корень

В итоге получаем

 - %:@(*/)@(-:@(+/)-,&0) 8 6 10
> 24

Быстро и просто

*J идельно подходит в качестве замены калькулятору и позволяет быстро производить вычисления по сложным формулам.*  
