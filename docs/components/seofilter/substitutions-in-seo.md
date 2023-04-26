# Замены в SEO текстах

По умолчанию, в текстах SEO правил для подстановки слов из поля нужно использовать в качестве плейсхолдера его синоним (alias). Например, добавляя поле цвет (синоним color) в правило, в его текстах нужно писать `{$color}`, чтобы вместо этого слова было написано: зелёный или красный.

Но если правило состоит из одного поля, то можно использовать как alias, так и плейсхолдер `{$value}`. Если включены [склонения, подчсёты, выборки][1], то будет доступно множество плейсхолдеров. Если вы не хотите использовать возможности Fenom, то можете просто писать в синтаксисе MODX: `[[+value]]`, `[[+color]]`и т.д.

## Таблица доступных тегов, для использования в текстах

| Плейсхолдер                    | Описание                                                                                                                            | Пример                                            |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| `{$value}`,`{$[alias]}`        | Вместо плейсхолдеры будет подставлено значение слова для поля                                                                       | `{$color}` -  **Красный**                         |
| `{$input}`, `{$[alias]_input}` | Запрос слова для поля, может совпадать со значением, но не всегда. Просто {$input} доступен при использовании одного поля в правиле | `{$metro_input}` - **6** (id метро)               |
| `{$alias}`, `{$[alias]_alias}` | Синоним слова для поля. В примере для значения цвета "Красный"                                                                      | `{$color_alias}` - **krasnyij**                   |
| `{$total}`, `{$count}`         | Количество результатов, если включены подсчёты или подсчитаны                                                                       | `{$count | decl : 'товар\товара\товаров' : true}` |
| `{$page}`,`{$page_number}`     | Номер текущей страницы результатов. Чтобы стал доступен такой плейсхолдер - заполните системную настройку **seofilter_page_key**    | 1                                                 |
| `{$id}`,`{$page_id}`           | ID ресурса, к которому привязано правило. Можно получить его поля                                                                   | `{$id \ resource : 'pagetitle'}`                  |
| `{$rule_id}`                   | ID правила                                                                                                                          | 19                                                |
| `{$seo_id}`                    | ID ссылки из таблицы URL                                                                                                            | 123                                               |
| `{$url}`                       | Адрес SEO страницы (без окончаний и приставок)                                                                                      | pleeryi/cvet-sinij                                |
| `{$link}`                      | Название ссылки(SEO страницы)                                                                                                       | Синие плееры                                      |
| `{$createdon}`                 | Дата создания ссылки                                                                                                                | 2018-03-15 10:31:39                               |
| `{$editedon}`                  | Дата редактирования ссылки (может отсутствовать)                                                                                    |                                                   |

::: tip
В таблице вместо `[alias]` используйте синоним своего поля
:::

 Если вы включили склонения по падежам, то будут досутпны такие теги:

| Падеж                    | Ед. число                | Множ. число                 | Примеры               |
| ------------------------ | ------------------------ | --------------------------- | --------------------- |
| **Именительный**         | `{$value_i}`, `{$value}` | `{$m_value_i}`,`{$m_value}` | красный / красные     |
| **Родительный**          | `{$value_r}`             | `{$m_value_r}`              | красного / красных    |
| **Дательный**            | `{$value_d}`             | `{$m_value_d}`              | красному / красным    |
| **Винительный**          | `{$value_v}`             | `{$m_value_v}`              | красного / красных    |
| **Творительный**         | `{$value_t}`             | `{$m_value_t}`              | красным / красными    |
| **Предложный**           | `{$value_p}`             | `{$m_value_p}`              | красном / красных     |
| **Предлож. с предлогом** | `{$value_o}`             | `{$m_value_o}`              | о красном / о красных |
| Вопрос **где?**          | `{$value_in}`            | -                           | в красном             |
| Вопрос **куда?**         | `{$value_to}`            | -                           | в красного            |
| Вопрос **откуда?**       | `{$value_from}`          | -                           | из красного           |

Если вы активировали подсчёты и прописали выборку нескольких полей по цене:

| Тег                     | Описание                                    | Пример             |
| ----------------------- | ------------------------------------------- | ------------------ |
| **count**               | Выведет количество найденных результатов    | 137                |
| **min_price_id**        | id самого дешевого товара                   | 55                 |
| **min_price_price**     | Цена самого дешевого товара                 | 4000               |
| **min_price_pagetitle** | Название (pagetitle) самого дешевого товара | Nokia 3310         |
| **max_price_id**        | id самого дорогого товара                   | 88                 |
| **max_price_price**     | Цена самого дорогого товара                 | 80000              |
| **max_price_pagetitle** | Название (pagetitle) самого дорогого товара | Samsung Galaxy S8+ |

Таблица некоторых полезных модификаторов Fenom

| Модификатор                | Описание                                                                                         | Пример использования                                   | Результат до     | После           |
| -------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------ | ---------------- | --------------- |
| **lower (low)**            | конвертирование строки в нижний регистр                                                          | `{$color \| lower}`                                    | Красный          | красный         |
| **upper (up)**             | конвертирование строки в верхний регистр                                                         | `{$color \| upper}`                                    | Красный          | КРАСНЫЙ         |
| **ucfirst**                | преобразует в верхний регистр первый символ первого слова в строке, а остальные символы в нижний | `{$color \| ucfirst}`                                  | красный          | Красный         |
| **declension  (decl)**     | склоняет слово, следующее за числом по правилам русского языка                                   | `{$count \| declension:'товар\|товара\|товаров':true}` | 5                | 5 товаров       |
| **number (number_format)** | форматирование числа(цены) php функцией number_format()                                          | `{$max_price_price \| number : 0 : '.' : ' '}`         | 4000             | 4 000           |
| **replace**                | заменяет все вхождения строки поиска на строку замены                                            | `{$raion \| replace : "район" : ""}`                   | Пушкинский район | Пушкинский      |
| **option**                 | получает значения из системных настроек MODX                                                     | `{'site_name' \| option}`                              |                  | MODX Revolution |

Можно использовать несколько модификаторов указывая их через `|`. Пример: `{'site_name' | option | upper}`.
Более полный список в Модификаторах [Fenom][0]

[0]: /components/pdotools/parser#шаблонизатор-fenom
[1]: /components/seofilter/additional-features