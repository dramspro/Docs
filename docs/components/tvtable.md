---
title: TVTable
description: Дополнительное поле в виде таблицы
logo: https://modstore.pro/assets/extras/tvtable/logo-lg.png
author: wax100
modstore: https://modstore.pro/packages/utilities/tvtable
repository: https://github.com/wax100/TVTable
outline: [2,3]
---

# TVTable

TVTable — это ТВ-параметр в виде таблицы, в которой можно менять количество не только строк, но и столбцов.

![Интерфейс компонента](https://file.modx.pro/files/d/7/9/d79ba778f513994ae68c6cea75bc7528.png)

## Использование

Для того, чтобы использовать компонент после его установки вам необходимо создать TV-параметр и во вкладке **Параметры ввода** в поле **Тип ввода** выбрать `Table`. У данного типа ввода есть следующие параметры:

### Количество столбцов {#columns}

Параметр отвечает за строгое количество столбцов. Если вы хотите чтобы ваша таблица имела неограниченое количество, то оставьте его пустым.

::: details Примечание
Имеет приоритет над свойством [Максимальное количество столбцов](#max-columns). Существующие значения не будут изменены и в том случае, если количество столбцов в существующих данных будет больше указанного лимита, то у пользователей будет возможность удалять столбцы до указанного лимита, а если количество столбцов будет меньше указанного лимита, то при следующем редактировании будет добавлено недостающее количество столбцов.
:::

### Количество строк {#rows}

Аналогично с предыдущим параметром, только он отвечает за количество строк.

::: details Примечание
Имеет приоритет над свойством [Максимальное количество строк](#max-rows). Существующие значения не будут изменены и в том случае, если количество строк в существующих данных будет больше указанного лимита, то у пользователей будет возможность удалять строки до указанного лимита, а если количество строк будет меньше указанного лимита, то при следующем редактировании будет добавлено недостающее количество строк.
:::

### Максимальное количество столбцов {#max-columns}

Данный параметр отвечает за максимальное количество строк и если вы хотите чтобы ваша таблица имела неограниченое количество, то оставьте его пустым.

::: details Примечание
Существующие значения не будут изменены и в том случае, если количество столбцов в существующих данных будет больше указанного лимита, то у пользователей будет возможность удалять столбцы до указанного лимита.
:::

### Максимальное количество строк {#max-rows}

Аналогично с предыдущим параметром, только он отвечает за максимальное количество строк.

::: details Примечание
Существующие значения не будут изменены и в том случае, если количество столбцов в существующих данных будет больше указанного лимита, то у пользователей будет возможность удалять столбцы до указанного лимита.
:::

### Заголовки столбцов {#headers}

Список заголовков столбцов разделенный двойной вертикальной чертой `||`. За их вывод в сниппете [TVTable](#snippet-tvtable) отвечает параметр **displayHeaders**.

![Демонстрация работы параметра Заголовки столбцов](https://file.modx.pro/files/a/d/1/ad19a87942c0b6c96bb3d1db974ebdaf.gif)

### Заголовок по умолчанию <Badge type="info" text="Необъязательный" />

Данное значение будет выведено для пустых заголовков.

### Ширина полей в пикселях

Минимальное значение: `20`.

![Демонстрация параметра Ширина полей в пикселях](https://file.modx.pro/files/1/6/6/166e794e1d8b1df95aea15b9d1ce80e3.png)

### Сортировка строк

Включить возможность сортировать строки с помощью перетаскивания.

## Сниппет TVTable

Ниже представлен список параметров сниппета.

| Название           | По умолчанию                                               | Описание                                                             |
|--------------------|------------------------------------------------------------|----------------------------------------------------------------------|
| **id**             | Текущий ресурс                                             | ID ресурса                                                           |
| **tv**             |                                                            | ID или название TV-параметра для вывода                              |
| **input**          |                                                            | Данный параметр предназначен для прямой передачи значения            |
| **tableClass**     |                                                            | CSS класс `<table>`                                                  |
| **headClass**      |                                                            | CSS класс `<thead>`                                                  |
| **bodyClass**      |                                                            | CSS класс `<body>`                                                   |
| **head**           | `1`                                                        | Данный параметр отвечает за вывод `<thead>`                          |
| **displayHeaders** | `0`                                                        | Вывод заголовков столбцов из параметров ввода TV                     |
| **wrapperTpl**     | `@INLINE <table class="[[+classname]]">[[+table]]</table>` | Чанк оформления всей таблицы                                         |
| **thTpl**          | `@INLINE <th>[[+val]]</th>`                                | Чанк оформления ячейки заголовка таблицы                             |
| **trTpl**          | `@INLINE <tr>[[+cells]]</tr>`                              | Чанк оформления строки таблицы                                       |
| **tdTpl**          | `@INLINE <td>[[+val]]</td>`                                | Чанк оформления ячейки строк таблицы                                 |
| **getX**           |                                                            | Получение строки по индексу. Также можно указывать: `first`, `last`  |
| **getY**           |                                                            | Получение столбца по индексу. Также можно указывать: `first`, `last` |

::: tip
Если указать оба параметра `getX` и `getY`, то вы получите только значение, а не таблицу.
:::

## Системные настройки

### `tvtable_clear_button`

- Тип: `boolean`
- По умолчанию: `Нет`

Данная настройка отвечает за отображение кнопки очистки таблицы. Если выбрать `Да`, то рядом с каждой таблицей будет отображаться кнопка для очистки таблицы.

### `tvtable_remove_confirm`

- Тип: `boolean`
- По умолчанию: `Да`

Если выбрать `Да`, то перед удалением строк, столбцов и очисткой таблицы нужно будет подтвердить.