---
# Front matter
title: "Отчёт по лабораторной работе №8"
subtitle: "Элементы криптографии. Шифрование (кодирование) различных исходных текстов одним ключом"
author: "Лёшьен Стефани"

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

Освоить на практике применение режима однократного гаммирования на примере кодирования различных исходных текстов одним ключом.

# Ход работы

Я выполняла лабораторную работу на языке python. Сначала я подключила библиотеки:

```
import numpy as np
import operator as op
import sys
```

По условию лабораторной работы, я создала две функции. Также я задала 2 переменные строкового типа , "я учусь в РУДН", "на оржиникдзе!" и подсчитала длину строк.

```
p1="я учусь в РУДН"
print(len(p1))
p2="на оржиникдзе!"
print(len(p2))

```

Первая функция осуществляет перевод в шестнадцатеричную систему, генерирует рандомный ключ с помощью которого будет получаться сообщение в шестнадцатиричной системе и его перевод его в строку. 

```
def encrypt(text1, text2):
    print("text1: ", text1)
    newtext1=[]
    for i in text1:
        newtext1.append(i.encode("cp1251").hex())
    print("text1 in 16: ", newtext1)
    print("text2: ", text2)
    newtext2=[]
    for i in text2:
        newtext2.append(i.encode("cp1251").hex())
    print("text2 in 16: ", newtext2)
    r=np.random.randint(0,255, len(text1))
    key=[hex(i)[2:] for i in r]
    newkey=[]
    for i in newkey:
        key.append(i.encode("cp1251").hex().upper())
    print("key in 16: ", key)
    xortext1=[]
    for i in range(len(newtext1)):
        xortext1.append("{:02x}".format(int(key[i], 16) ^ int(newtext1[i],16)))
    print("cypher text1 in 16: ", xortext1)
    en_text1=bytearray.fromhex("".join(xortext1)).decode("cp1251")
    print("cypher text1: ", en_text1)
    xortext2=[]
    for i in range(len(newtext2)):
        xortext2.append("{:02x}".format(int(key[i],16)^ int(newtext2[i],16)))
    print("cypher text2 in 16: ", xortext2)
    en_text2=bytearray.fromhex("".join(xortext2)).decode("cp1251")
    print("cypher text2: ", en_text2)
    return key, xortext1, en_text1, xortext2, en_text2

```
Выполнила вызов этой функции:

```
k, t1, et1, t2, et2 = encrypt(p1,p2)
```
![Результат выполнения функции crypt.](images/1.png){ #fig:001 width=70% }

Вторая функция определяет ключ, который будет брать открытый текст и шифровать его в шестнадцатеричную систему.


```
def decrypt(c1, c2, p1):
    print("cypher text1: ", c1)
    newc1=[]
    for i in c1:
        newc1.append(i.encode("cp1251").hex())
    print("cypher text1 in 16: ", newc1)
    print("cypher text2: ", c2)
    newc2=[]
    for i in c2:
        newc2.append(i.encode("cp1251").hex())
    print("cypher text2 in 16: ", newc2)
    print("open text1: ", p1)
    newp1=[]
    for i in p1:
        newp1.append(i.encode("cp1251").hex())
    print("open text1 in 16: ", newp1)
    xortmp=[]
    sp2=[]
    for i in range(len(p1)):
        xortmp.append("{:02x}".format(int(newc1[i],16) ^ int(newc2[i], 16)))
    for i in range(len(p1)):
        sp2.append("{:02x}".format(int(xortmp[i],16) ^ int(newp1[i], 16)))
    print("open text2 in 16: ", sp2)
    p2=bytearray.fromhex("".join(sp2)).decode("cp1251")
    print("open text2: ", p2)
    return p1,p2
    
```

Выполнила вызов этой функции:

```
decrypt(et1, et2, p1)
```

![Результат выполнения функции decrypt.](images/2.png){ #fig:002 width=70% }


# Выводы

Я освоила на практике на практике применение режима однократного гаммирования на примере кодирования различных исходных текстов одним ключом.


# Библиография

1. Методические материалы курса
