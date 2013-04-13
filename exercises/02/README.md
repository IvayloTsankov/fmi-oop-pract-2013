# Упражнение 2 — файлове, входно-изходни операции

Отворете [http://www.cplusplus.com/reference/iolibrary/](http://www.cplusplus.com/reference/iolibrary/) или [http://en.cppreference.com/w/cpp/io](http://en.cppreference.com/w/cpp/io) за справки за входно-изходната библиотека на C++.

## Задача 1

Да се напише програма, която чете от файл input.txt. Файлът се състои от редове с числа, като в края на всеки ред стои символът #. Например:

    1 10 2 12 13 #
    4 8 10 12 13 #
    13 10 -1 5 6 #

Нека програмата да намира и извежда на екрана минималното и максималното число на всеки ред:

    Min/Max: 1/13
    Min/Max: 4/13
    Min/Max: -1/13

## Задача 2

Реализирайте същата задача, но за редове, които не завършват с #.

## Задача 3

Да се напише програма, която извежда статистическа оценка (в проценти) за честотата на срещане на всеки символ в даден текстов файл. Изведете информация само за символите, съдържащи се поне веднъж. Пътят до входния файл да може да бъде подаван като аргумент от командния ред, като ако няма подаден такъв се използва файла `stats.txt`.

## Задача 4

TODO: Задачка за форматиран вход/изход.

## Задача 5

Да се напише програма, която намира и извежда поредния номер на най-дългия ред на даден текстов файл. Номерацията започва от 1. В случай, че има повече от 1 ред с дължина, равна на дължината на най-дългия ред, програмата да извежда информация само за първия.