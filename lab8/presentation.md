---
## Front matter
title: Лабораторная работа №8
subtitle: Элементы криптографии. Шифрование (кодирование) различных исходных текстов одним ключом
author: Лёшьен Стефани
institute: RUDN University, Moscow, Russian Federation
date: 29 October, 2022, Moscow, Russian Federation

## Formatting
toc: false
slide_level: 2
header-includes: 
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
aspectratio: 43
section-titles: true
theme: metropolis
---

# Цель выполнения лабораторной работы

Освоить на практике применение режима однократного гаммирования на примере кодирования различных исходных текстов одним ключом.

## Подключение библиотек

```
import numpy as np
import operator as op
import sys

```
```
p1="я учусь в РУДН"
print(len(p1))
p2="на оржиникдзе!"
print(len(p2))

```


## Функция encrypt


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

## Вывод encrypt

```
k, t1, et1, t2, et2 = encrypt(p1,p2)
```

![Результат выполнения функции encrypt.](images/1.png){#fig:001 width=70% }


## Функция decrypt

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

![Результат выполнения функции decrypt.](images/2.png){#fig:002 width=70% }

## Вывод decrypt

```
decrypt(et1, et2, p1)
```

![Результат выполнения функции decrypt.](images/2.png){#fig:003 width=70% }


# Итоги

- изучили шифрование в режиме гаммирования
- написали код из 2-х функций для решения задачи