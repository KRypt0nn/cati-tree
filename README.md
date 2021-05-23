<h1 align="center">🚀 CATI Tree</h1>

**CATI Tree** *(Category Identification Tree)* - библиотека для реализации идентификации наборов данных на PHP 7.4+

Данная структура данных и реализованный в ней алгоритм придуманы лично мной и наверняка будут работать как куски кусков. Более подробную информацию можно прочесть [здесь](https://twitter.com/krypt0nn/status/1394701165238046724?s=20)

## Установка

```
composer require krypt0nn/cati-tree
```

## Пример работы

```php
<?php

use CATI\Tree;

$tree = (new Tree)
    # Добавляем примеры приветствий
    ->addSample ('greeting', [
        ['hello'],
        ['hi']
    ])
    # Добавляем примеры прощаний
    ->addSample ('farewell', [
        ['bye'],
        ['goodbye']
    ])
    # Подгатавливаем (обучаем) дерево к предсказаниям
    ->prepare ();

echo $tree->predict (['hello', 'world']) ?: 'unknown'; // greeting

echo $tree->predict (['bye']) ?: 'unknown'; // farewell

echo $tree->predict (['something', 'else']) ?: 'unknown'; // unknown
```

В данном примере в качестве `samples` я указал просто слова, хотя на практике туда, конечно, пихать нужно обычные предложения. Структура при вызове метода `prepare` найдёт уникальные черты для каждого предложения и при нахождении их в переданных в метод `predict` списках будет выдавать название какой-то категории. Просто как насрать в подъезде, но ведь работает. А для моих целей ничего сложнее и не надо было

```php
<?php

use CATI\Tree;

$tree = (new Tree)
    # Добавляем примеры приветствий
    ->addSample ('greeting', [
        ['hello'],
        ['hi']
    ])
    # Добавляем примеры прощаний
    ->addSample ('farewell', [
        ['bye'],
        ['goodbye']
    ])
    # Подгатавливаем (обучаем) дерево к предсказаниям
    ->prepare ();

# Сохраняем паттерны идентификации
$patterns = $tree->getPatterns ();

# Создаём новое дерево и сразу пихаем в него данные после обучения,
# чтобы не делать это второй раз. Обратите внимание, что
# dataset и labels (то есть training samples) будут пусты и их
# нужно подгружать вручную
$tree = new Tree ($patterns);
```

Автор: [Подвирный Никита](https://vk.com/technomindlp)
