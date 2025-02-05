# Товар

Товар miniShop2 является расширением класса обычного ресурса MODX.
Он отличается собственным интерфейсом в системе управления и расширенным набором свойств.

![Товар](https://file.modx.pro/files/5/4/6/546f08580a43a9a34fca13625666055d.png)

## Основное меню

При создании товара вы можете только сохранить его, или отменить это действие.

А при изменении добавляются еще кнопки просмотра на сайте, копирования и перехода по соседним товарам (если они есть).

## Панель товара

Товар наследует и расширяет стандартную панель ресурса MODX.
Так как разных свойств у него много, все они удобно расположены в отдельном наборе вкладок.

Первой идёт стандартные свойства ресурса:

![Панель товара - 1](https://file.modx.pro/files/f/a/0/fa0a70de3b0b4ade9dc823c61ef4bf69.png)

Затем настройки ресурса:

![Панель товара - 2](https://file.modx.pro/files/5/b/7/5b7c0a87bc2f115ae6321b403c43173d.png)

Обратите внимание, что чекбокс "Контейнер" заменяется на "Показывать в меню". Товары не могут быть контейнерами, для этого нужно использовать категории.
Все товары по умолчанию скрываются из меню, чтобы дерево работало быстрее, но вы можете выборочно их показывать с помощью этого переключателя.

Поведение этого переключателя при создании нового товара управляется системной настройкой **ms2_product_show_in_tree_default**.

### Свойства товара

Это специальная вкладка, на которой собраны дополнительные свойства товара, такие как цена, артикул, вес, производитель и т.д.
Свойства товара обязательны и едины для всех.

![Свойства товара](https://file.modx.pro/files/0/7/b/07bc6a3d032b1df6d562f45eca710a1a.png)

Набор и порядок вывода полей на этой вкладке управляется системной настройкой **ms2_product_extra_fields**. Доступны по умолчанию:

- **price** - стоимость товара, число до 2х знаков после запятой
- **old_price** - стоимость товара, число до 2х знаков после запятой
- **article** - артикул, можно редактировать как текст
- **weight** - вес товара, число до 3х знаков после запятой
- **color** - массив цветов товара, автосписок
- **size** - массив размеров товара, автосписок
- **made_in** - страна производства товара, обычный текст, с подсказками
- **vendor** - выбор производителя из выпадающего списка
- **tags** - массив тегов товара, автосписок
- **new** - отметка о том, что товар новинка: да \ нет
- **pupular** - отметка о том, что товар популярный: да \ нет
- **favorite** - отметка о том, что товар особенный: да \ нет

Изменить набор доступных свойств можно только через [систему плагинов][1].
Саму вкладку можно скрыть через настройку **ms2_product_tab_extra**.

### Опции товара

Неограниченный список опций, которые наследуются от категории и могут отличаться у разных товаров.

![Опции товара](https://file.modx.pro/files/3/4/3/343138dcb7ea9d2ce801d3d6772ad96d.png)

Сами опции создаются в соответствующем разделе настроек магазина и добавляются в настройках категории. Если у категории нет опций, то эта вкладка не выводится.
Также её можно скрыть принудительно, используя настройку **ms2_product_tab_options**.

Подробнее про опции товаров можно прочитать в [соответствующем разделе][2].

### Связи товара

Эта вкладка появляется только при редактировании товара, потому что для её работы должен существовать id товара, который отсутствует на момент его создания.

![Связи товара](https://file.modx.pro/files/2/4/1/2417516e02ff308cb1f53f8f883226a0.png)

Доступные связи создаются в настройках магазина. Выключить эту вкладку можно системной настройкой **ms2_product_tab_links**.

### Категории

Каждый товар магазина может находится в нескольких категориях.
У него должна быть одна обязательная категория, прописанная в свойстве **parent**, и могут быть дополнительные - указанные на этой вкладке.

![Категории](https://file.modx.pro/files/8/2/f/82ffb0c829e766b631bfb056f9f6052c.png)

Не забудьте сохранить товар при изменении набора категорий! Родную категорию товара из дерева выключить нельзя.

### Комментарии

Эта вкладка выводится только если на сайте установлен компонент **Tickets** и включена системная настройка **ms2_product_show_comments**.

![Комментарии](https://file.modx.pro/files/5/c/4/5c43f7a822acfe411cfe1eef88f16d92.png)

Для того, чтобы посетители могли комментировать ваши товары, нужно вызвать на их страницах сниппет **TicketComments**.

### Дубликаты

Товары можно копировать, при этом копируются:

- Все свойства документа
- Все настройки документа
- Свойства товара
- Опции товара
- Связи товара
- Категории товара

**Файлы галереи не копируются** просто потому, что это длительная операция, особенно если используется удалённый источник файлов типа Amazon S3, и процесс может отвалиться по таймауту.

[1]: /components/minishop2/development/product-plugins
[2]: /components/minishop2/interface/settings
