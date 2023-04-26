# Синтаксис тегов

Теги - это основные рабочие элементы MODX для пользователя сайта.

Указывая теги на странице, вы можете вызвать какой-то кусок HTML или PHP кода, текст из словаря, или переменные документа.

Для упрощения логики работы парсера, увеличения производительности и избежания путаницы с составными тегами синтаксис тегов разных элементов сведен к одному принципу: все теги объявляются в квадратных скобках, а тип тега определяется символом перед наименованием.

## Для элементов и полей

::: v-pre
| Элемент             | В Evolution (Старый) | В Revolution (Новый) | Пример (для Revolution) |
|---------------------|----------------------|----------------------|-------------------------|
| Шаблоны             | Нет                  | Нет                  |                         |
| Поля ресурсов       | `[*field*]`          | `[[*field]]`         | `[[*pagetitle]]`        |
| Дополнительные поля | `[*templatevar*]`    | `[[*templatevar]]`   | `[[*tags]]`             |
| Чанки               | `{{ chunk }}`        | `[[$chunk]]`         | `[[$header]]`           |
| Сниппеты            | `[[snippet]]`        | `[[snippet]]`        | `[[getResources]]`      |
| Плагины             | Нет                  | Нет                  |                         |
| Модули              | Нет                  | В Revo нет модулей   |                         |
:::

## Для вывода контента

| Элемент             | В Evolution (Старый) | В Revolution (Новый)       | Пример (для Revolution)        |
|---------------------|----------------------|----------------------------|--------------------------------|
| Плейсхолдеры        | `[+placeholder+]`    | `[[+placeholder]]`         | `[[+modx.user.id]]`            |
| Ссылки              | `[~link~]`           | `[[~link]]`                | `[[~[[*id]]? &scheme=`full`]]` |
| Системные настройки | `[(system_setting)]` | `[[++system_setting]]`     | `[[++site_start]]`             |
| Языковые теги       | Нет                  | `[[%language_string_key]]` |                                |
| Комментарии         | Нет                  | `[[-this is a comment]]`   |                                |

## Для системных значений парсера MODX

| Описание                                              | Тэг      |
|-------------------------------------------------------|----------|
| Выводит время потраченное на запросы к базе данных    | `[^qt^]` |
| Выводит количество запросов к базе данных             | `[^q^]`  |
| Выводит время потраченное на работу PHP скриптов      | `[^p^]`  |
| Выводит общее время потраченное на генерацию страницы | `[^t^]`  |
| Выводит источник содержимого (база данных или кэш)    | `[^s^]`  |

Принятие такого упрощенного формата позволяет новому парсеру быть полностью рекурсивным и не зависеть от регулярных выражений.

Раньше каждый набор тегов анализировался самостоятельно в определенном порядке, по уровням вложенности, и обработка внутреннего тега откладывалось до следующего прохода. Сейчас теги обрабатываются по мере их появления, независимо от типов элементов, которые они представляют, и встроенные теги обрабатываться перед внешним тегом (то есть обработка сложных тегов идет изнутри), что позволяет прописывать гораздо более сложные составные теги.

## Комментарии в тегах

Обсуждения на форумах показывают, что некоторые люди чувствуют необходимость в комментариях. По умолчанию при обнаружении тега, который представляет собой элемент, которого не существует, парсер отбрасывает весь тег. Используя это поведение, вы можете добавить комментарии в шаблоны, чанки и поля ресурсов, и этот комментарий не будет отображен во фронтенде. Однако, если тег составной, то все внутренние теги обработаются прежде чем парсер отбросит значение.

В MODX Revolution 2.2 любой тег, начинающейся с дефиса (-) игнорируются парсером, и любые теги внутри такого комментария будут отбрасываться. Это позволяет вставлять в комментарии любые составные теги, не влияя на нагрузку.

```modx
[[- Это комментарий. Он будет удален из вывода страницы. ]]
```

## Структура тегов

Каждый тег состоит из нескольких частей. Давайте разберем тег на составляющие:

```modx
[[ // открываем тег
! // указание, что тег НЕкешируемый (необязательно)
elementToke // тип элемента $ - чанк, * - поле элемента или ТВ, + - плейсхолдер, и т. д.
elementName // имя элемента
@propertyset // указать набор параметров для этого элемента (необязательно)
f =`modifier` // один или несколько фильтров вывода (необязательно)
? // указание того, ч   дальше идут параметры элемента (необязательно если параметры отсутствуют)
&propertyName=`propertyValue`  //любой параметр элемента, начинающийся с &
&me2=`propertyValue2` // параметров может быть сколько угодно, и все начинаются с &
]] //закрываем тег
```

Эти теги могут прописываться в одну линию или располагаться на нескольких строках для удобства. Оба варианта однозначны:

```modx
[[!getResources? &parents=`123` &limit=`5`]]

[[!getResources?
  &parents=`123`
  &limit=`5`
]]
```

## Параметры

Все теги (а не только сниппеты) могут иметь параметры. Например, у нас есть чанк `Hello`:

```modx
Привет, [[+name]]!
```

В чанке есть плейсхолдер. Мы хотим задать значение для этого плейсхолдера. Раньше нужно было использовать сниппет, который установит это значение. Но не теперь. Просто укажите нужное значение в параметрах чанка:

```modx
[[$Hello? &name=`Сергей`]]
```

На выходе мы получим:

```
Привет, Сергей!
```

## Кэширование

Мы можем указать, что тег нужно обрабатывать после каждого запроса страницы. Для этого нужно просто поставить восклицательный знак сразу после открывающихся квадратных скобок:

```modx
[[!snippet]]
[[!$chunk]]
[[!+placeholder]]
[[!*template_var]]
```

и т. д.

## Плейсхолдеры

Если вам необходимо, чтобы сниппет каждый раз при запросе страницы устанавливал какой-то плейсхолдер, то плейсхолдер тоже нужно вызывать некэшируемым:

```modx
[[!Profile]]
Привет, [[!+username]].
```

## Проверка синтаксиса

Для новичков синтаксис тегов кажется сложноватым, поэтому вы можете использовать плагин SyntaxChecker для проверки правильности написания тегов.

Также можно использовать замечательный редактор **Ace**, который хорошо подсвечивает синтаксив тегов MODX.