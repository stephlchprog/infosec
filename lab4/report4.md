---
# Front matter
title: Отчёт по лабораторной работе №4
subtitle: "Дискреционное
разграничение прав в Linux. Расширенные
атрибуты"
author: "Стефани Лёшьен"

# Generic otions
lang: ru-RU
toc-title: "Содержание"

# Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

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
  name: el
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

Получить практические навыки работы в консоли с расширенными
атрибутами файлов.

# Ход работы 

1. От имени пользователя guestt установила командной chmod 600 test на файл test права, разрешающие чтение и запись для владельца файла.

2. От имени администратора установила расширенный атрибут а.     

3. Пробовала выполнить дозапись, чтение, удаление, переименование файла и установила права с помощью командной chmod 000 file. 

![Проверка с расш. атрибутом a.](image5.png){ #fig:001 width=70% }

4. Сняла расширенный атрибут а и повторила предыдущие операции.

![Проверка без расширенных атрибутов.](image3.png){ #fig:001 width=70% }

5. От имени администратора установила расширенный атрибут i и повторила действий по шагам.

 ![Проверка с расш. атрибутом i.](image4.png){ #fig:001 width=70% }

12. Заполнила таблицу «Разрешённые действия».
Если операция разрешена, занесла в таблицу знак «+», если не разрешена, знак «-».


| Операция               | Без расш. атрибутов                | С расш. атрибутов а       | С расш. атрибутов i     | 
| -----------------------| -----------------------------------| --------------------------|-------------------------|
| Дозапись в файл        | -                                  | -                         | -                       |
| Чтение файла           | +                                  | +                         | +                       |
| Перезапись файла       | +                                  | -                         | -                       |           
| Удаление файла         | +                                  | -                         | -                       | 
| Переименование файла   | +                                  | -                         | -                       |
| Установка прав         | +                                  | -                         | -                       |    

# Итоги выполнения лабораторной работы

Получила практических навыков работы в консоли с расширенными
атрибутами файлов.

# Библиография{.unnumbered}

1. Зегжда Д. П., Ивашко А. М. Основы безопасности информационных систем. — М.: Горячая линия — Телеком, 2016. — 452 с.
2. Мэйволд Э. Безопасность сетей. Эком, 2016 г., 528 с. — http://www.intuit.ru/department/security/netsec/


::: {#refs}
:::



