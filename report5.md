---
# Front matter
title: "Лаб.4 Дискреционное разграничение прав в Linux. Расширенные атрибуты"
author: "Поляков Иван Андреевич"

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
  - \usepackage[T2A]{fontenc}
---

# Цель работы

Изучение механизмов изменения идентификаторов, применения
SetUID- и Sticky-битов. Получение практических навыков работы в консоли с дополнительными атрибутами. Рассмотрение работы механизма
смены идентификатора процессов пользователей, а также влияние бита
Sticky на запись и удаление файлов

# Последовательность выполнения работы
## Создание программы
1. Войдите в систему от имени пользователя guest.

2. Создайте программу simpleid.c:

![Рис. 1](img5/1.png){ #fig:001 width=100% }

3. Скомплилируйте программу и убедитесь, что файл программы создан:

![Рис. 2](img5/2.png){ #fig:002 width=100% }

4. Выполните программу simpleid:

![Рис. 3](img5/3.png){ #fig:003 width=100% }

5. Выполните системную программу id:

![Рис. 4](img5/4.png){ #fig:004 width=100% }

6. Усложните программу, добавив вывод действительных идентификаторов:

![Рис. 5](img5/5.png){ #fig:005 width=100% 

7. Скомпилируйте и запустите simpleid2.c:

![Рис. 6](img5/6.png){ #fig:006 width=100% }

8. От имени суперпользователя выполните команды:

![Рис. 7](img5/7.png){ #fig:007 width=100% }

9. Используйте sudo или повысьте временно свои права с помощью su.

10. Выполните проверку правильности установки новых атрибутов и смены
владельца файла simpleid2:

![Рис. 8](img5/8.png){ #fig:008 width=100% }

11. Запустите simpleid2 и id:

![Рис. 9](img5/9.png){ #fig:009 width=100% }

12. Проделайте тоже самое относительно SetGID-бита.

![Рис. 10](img5/10.png){ #fig:010 width=100% }

13. Создайте программу readfile.c:

![Рис. 11](img5/11.png){ #fig:011 width=100% }

14. Откомпилируйте её

![Рис. 12](img5/12.png){ #fig:012 width=100% }

15. Смените владельца у файла readfile.c (или любого другого текстового
файла в системе) и измените права так, чтобы только суперпользователь
(root) мог прочитать его, a guest не мог.

![Рис. 13](img5/13.png){ #fig:013 width=100% }

16. Проверьте, что пользователь guest не может прочитать файл readfile.c.

![Рис. 14](img5/14.png){ #fig:014 width=100% }

17. Смените у программы readfile владельца и установите SetU’D-бит.

![Рис. 15](img5/15.png){ #fig:015 width=100% }

18. Проверьте, может ли программа readfile прочитать файл readfile.c?

![Рис. 16](img5/16.png){ #fig:016 width=100% }

19. Проверьте, может ли программа readfile прочитать файл /etc/shadow?
Отразите полученный результат и ваши объяснения в отчёте.

![Рис. 17](img5/17.png){ #fig:017 width=100% }

## Исследование Sticky-бита

1. Выясните, установлен ли атрибут Sticky на директории /tmp, для чего
выполните команду

![Рис. 18](img5/18.png){ #fig:018 width=100% }

2. От имени пользователя guest создайте файл file01.txt в директории /tmp
со словом test:

![Рис. 19](img5/19.png){ #fig:019 width=100% }

3. Просмотрите атрибуты у только что созданного файла и разрешите чтение и запись для категории пользователей «все остальные»:

4. От пользователя guest2 (не являющегося владельцем) попробуйте прочитать файл /tmp/file01.txt:

![Рис. 20](img5/20.png){ #fig:020 width=100% }

5. От пользователя guest2 попробуйте дозаписать в файл
/tmp/file01.txt слово test2 командой

![Рис. 21](img5/21.png){ #fig:021 width=100% }

6. Проверьте содержимое файла командой

![Рис. 22](img5/22.png){ #fig:022 width=100% }

7. От пользователя guest2 попробуйте записать в файл /tmp/file01.txt
слово test3, стерев при этом всю имеющуюся в файле информацию командой

![Рис. 23](img5/23.png){ #fig:023 width=100% }

8. Проверьте содержимое файла командой

![Рис. 24](img5/24.png){ #fig:024 width=100% }

9. От пользователя guest2 попробуйте удалить файл /tmp/file01.txt командой

![Рис. 25](img5/25.png){ #fig:025 width=100% }

10. Повысьте свои права до суперпользователя следующей командой и выполните после этого команду, снимающую атрибут t (Sticky-бит) с
директории /tmp:

![Рис. 26](img5/26.png){ #fig:026 width=100% }

11. Покиньте режим суперпользователя командой exit

12. От пользователя guest2 проверьте, что атрибута t у директории /tmp
нет:

![Рис. 27](img5/27.png){ #fig:027 width=100% }

13. Повторите предыдущие шаги. Какие наблюдаются изменения?

![Рис. 28](img5/28.png){ #fig:028 width=100% }

14. Удалось ли вам удалить файл от имени пользователя, не являющегося
его владельцем? 

15. Повысьте свои права до суперпользователя и верните атрибут t на директорию /tmp:

![Рис. 29](img5/29.png){ #fig:029 width=100% }

# Выводы

Изученил механизмы изменения идентификаторов, применения
SetUID- и Sticky-битов. Получил практические навыки работы в консоли с дополнительными атрибутами. Рассмотрение работы механизма
смены идентификатора процессов пользователей, а также влияние бита
Sticky на запись и удаление файлов
