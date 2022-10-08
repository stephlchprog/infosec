---
# Front matter
title: "Лабораторная работа № 5. Дискреционное разграничение прав в Linux. Исследование
влияния дополнительных атрибутов"
author: "Лёшьен Стефани, НФИбд-02-19"

# Generic otions
lang: ru-RU
toc-title: "Содержание"

# Pdf output format
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
### Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Misc options
indent: true
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Изучение механизмов изменения идентификаторов, применения SetUID- и Sticky-битов. Получение практических навыков работы в консоли с дополнительными атрибутами. Рассмотрение работы механизма смены идентификатора процессов пользователей, а также влияние бита
Sticky на запись и удаление файлов.

# Последовательность выполнения работы

1. От имени пользователя root установили компилятор gcc c помощью команды yum install gcc.

2. Cоздали файл simpleid.c

 ![Установка gcc и создание файла simpleid.c](images/img2.png){#fig:1 width=100%}

 ![Код программы в simpleid.c](images/img1.png){ #fig:002 width=70% }

4. Скомплилировали программу и убедились, что файл программы создан.

5. Выполните программу simpleid и системную программу id.

 ![Скомплилировали и выполните программу simpleid](images/img4.png){ #fig:003 width=70% }

6. Создали файл simpleid2.c. Усложнили программу, добавив вывод действительных идентификаторов.

 ![Код программы в simpleid2.c](images/img3.png){ #fig:004 width=70% }

7. Скомпилируйте и запустите simpleid2.c.

8. От имени суперпользователя выполните команды:
chown root:guest /home/guest/simpleid2
Эта команда изменяет права владения файла. Мы установили владения для root и группы guest.
chmod u+s /home/guest/simpleid2
Эта команда добавляет выполнение от имени пользователя для юзера.(рис. -@fig:005)

9. Выполните проверку правильности установки новых атрибутов и смены
владельца файла simpleid2 с помощью ls.

![Выполнили команды chown и chmod](images/img5.png){ #fig:005 width=70% }

10. Создайте программу readfile.c и откомпилировали её.

![программу readfile.c](images/img6.png){ #fig:006 width=70% }

11. Смените владельца у файла readfile.c и изменили права так, чтобы только суперпользователь
(root) мог прочитать его, a guest не мог.Проверили что пользователь guest не может прочитать файл readfile.c. В реузльтате чего было отказано в чтение файла. (рис. -@fig:007)

![Смените владельца у файла readfile.c](images/img8.png){ #fig:007 width=70% }

![Изменили права](images/img7.png){ #fig:008 width=70% }

12. Выяснили, что атрибут Sticky установлен на директории /tmp, для чего
выполните команду ls -l / | grep tmp. 

![атрибут Sticky на директории /tmp](images/img9.png){ #fig:009 width=70% }

13. От имени пользователя guest создали файл file01.txt в директории /tmp
со словом test. Просмотрели атрибуты у только что созданного файла и разрешили чтение и запись для категории пользователей «все остальные».

14. От пользователя guest2 (не являющегося владельцем) попробовали прочитать файл. От пользователя guest2 попробуйте дозаписать в файл /tmp/file01.txt слово test2 командой echo "test2" > /tmp/file01.txt. Проверили содержимое файла. Попровобовали удалить файл.

![Разрешили чтение и запись для категории пользователей все остальные](images/img10.png){ #fig:010 width=70% }

15. Повысили свои права до суперпользователя следующей командой su -
и выполните после этого команду, снимающую атрибут t (Sticky-бит) с
директории /tmp. Повторили предыдущие шаги. Нам теперь удалось удалить файл.

![Сняли атрибут Sticky и смогли удалить файл.](images/img11.png){ #fig:011 width=70% }

# Выводы

Изучили механизмов изменения идентификаторов, применения SetUID- и Sticky-битов. 

# Выводы

Получили практических навыков работы в консоли с дополнительными атрибутами. Рассмотрели работы механизма смены идентификатора процессов пользователей, а также влияние бита
Sticky на запись и удаление файлов.

# Библиография

1. Методические материалы курса