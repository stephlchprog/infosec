---
# Front matter
title: "Лабораторная работа № 6. Мандаторное разграничение прав в Linux."
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

Развить навыки администрирования ОС Linux. Получить первое практическое знакомство с технологией SELinux1. Проверить работу SELinx на практике совместно с веб-сервером
Apache.

# Последовательность выполнения работы

1. От имени пользователя root установили веб-сервер Apache c помощью команды yum install httpd.

 ![Установка веб-сервер Apache](images/img2.png){#fig:1 width=100%}

2. В конфигурационном файле /etc/httpd/httpd.conf задали параметр ServerName: ServerName test.ru
   Для чтобы при запуске веб-сервера не выдавались лишние сообщения об ошибках, не относящихся к лабораторной работе.
   Также отключили пакетный фильтр командами :
   iptables -F
   iptables -P INPUT ACCEPT iptables -P OUTPUT ACCEPT
   
   Убедились, что SELinux работает в режиме enforcing политики targeted с помощью команд getenforce и sestatus.

 ![Отключили пакетный фильтр](images/img3.png){ #fig:002 width=70% }

3. Обратились с помощью браузера к веб-серверу, запущенному на компьютере, и убедились, что последний    работает:
   service httpd start
   
 ![service httpd start](images/img8.png){ #fig:003 width=70% }

4. Нашли веб-сервер Apache в списке процессов : ps auxZ | grep httpd
   или ps -eZ | grep httpd. Посмотрели  текущее состояние переключателей SELinux для Apache с помощью команды sestatus -bigrep httpd
   Многие переключателей находятся в положении «off»

 ![Нашли веб-сервер Apache в списке процессов](images/img9.png){ #fig:004 width=70% }
 
 ![Многие переключателей находятся в положении «off»](images/img6.png){ #fig:005 width=70% }

5. Определили тип файлов и поддиректорий, находящихся в директории /var/www, с помощью команды ls -lZ    /var/www и определили тип файлов, находящихся в директории /var/www/html: ls -lZ /var/www/html.
   Создали от имени суперпользователя html-файл /var/www/html/test.html следующего содержания:
   <html>
   <body>test</body>
   </html>
   
 ![Создали от имени суперпользователя html-файл](images/img7.png){ #fig:006 width=70% }
 
 ![html-файл test.html](images/img1.png){ #fig:007 width=70% }
 
6. Обратились к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1/test.html. Файл  успешно отображён.
 
 ![html-файл в веб-сервер](images/img12.png){ #fig:008 width=70% }

7. Проверили контекст файла можно командой ls -Z. Изменили контекст файла /var/www/html/test.html с
httpd_sys_content_t на любой другой, к которому процесс httpd не должен иметь доступа, например, на samba_share_t.

![Изменили контекст файла на samba_share_t](images/img11.png){ #fig:009 width=70% }

8. Попробовали ещё раз получить доступ к файлу через веб-сервер, введя в
браузере адрес http://127.0.0.1/test.html. Мы получили сообщение об ошибке.

9. Просмотрели системный лог-файл tail /var/log/messages и выполнили команду :
semanage port -a -t http_port_t -р tcp 81
После этого проверили список портов командой:
semanage port -l | grep http_port_t
Порт 81 появился в списке.

![Проверили список портов ](images/img13.png){ #fig:010 width=70% }

10. Вернули контекст httpd_sys_cоntent__t к файлу /var/www/html/ test.html. После этого мы получили доступ к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1:81/test.html.
Мы увидели содержимое файла — слово «test».

11. Удалили привязку http_port_t к 81 порту:
semanage port -d -t http_port_t -p tcp 81
и проверили, что порт 81 удалён.
Удалили файл /var/www/html/test.html

![Удалили файл /var/www/html/test.html](images/img15.png){ #fig:012 width=70% }

# Выводы

Развили навыки администрирования ОС Linux. Получили первое практическое знакомство с технологией SELinux1. Проверили работу SELinx на практике совместно с веб-сервером Apache. 

# Библиография

1. Методические материалы курса