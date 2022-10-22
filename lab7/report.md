---
# Front matter
title: "Лабораторная работа № 7. Элементы криптографии. Однократное гаммирование"
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

Освоить на практике применение режима однократного гаммирования.

# Задача

Нужно подобрать ключ, чтобы получить сообщение «С Новым Годом,
друзья!». Требуется разработать приложение, позволяющее шифровать и
дешифровать данные в режиме однократного гаммирования. Приложение
должно:
 1.  Определить вид шифротекста при известном ключе и известном открытом тексте.
 2.  Определить ключ, с помощью которого шифротекст может быть преобразован в некоторый фрагмент текста, представляющий собой один из
возможных вариантов прочтения открытого текста.


# Последовательность выполнения работы

1. В новом блокноте в Google Colaboratory я импортировала нужные мне библиотеки.

 ![Импорт библиотек](images/img1.png){#fig:1 width=100%}

2. Далее, я написала фунцию для определения вида шифротекста при известном ключе и известном открытом тексте. Сначала я перевела наш открытый текст в шестнадцатеричную систему и сгенерировала рандомный ключ. При помощи ключа я получаю зашифрованный текст в шестнадцатеричном формате и шифротекст.

 ![Функция определения шифротекста](images/img2.png){ #fig:002 width=70% }

3. В результате выполнения этой функции мы получаем на выход открытый текст, ключ в шестнадцатеричной системе счисления.
   
 ![результат](images/img3.png){ #fig:003 width=70% }

4. вторая фунция 

 ![Вторая функция](images/img4.png){ #fig:004 width=70% }
 
 ![Результат](images/img5.png){ #fig:005 width=70% }

5. Проверяем если полученный ключ совпадает с тем, который мы получили на предыдущем шаге.
 ![Проверяем если полученный ключ совпадает с тем, который мы получили на предыдущем шаге.](images/img6.png){ #fig:006 width=70% }
 
 ![html-файл test.html](images/img1.png){ #fig:007 width=70% }
 

# Выводы

Я освоила на практике применение режима однократного гаммирования.


# Библиография

1. Методические материалы курса
