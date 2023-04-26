# HybridAuth

Сниппет выводит форму для авторизации на сайте.

## Параметры

| Название               | По умолчанию                     | Описание                                                                                                                                                                                                        |
|------------------------|----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **&activeProviderTpl** | `tpl.HybridAuth.provider.active` | Чанк для вывода иконки привязанного сервиса.                                                                                                                                                                    |
| **&addContexts**       |                                  | Дополнительные контексты, через запятую. Например, ``&addContexts=`web,ru,en` ``                                                                                                                                |
| **&groups**            |                                  | Список групп для регистрации пользователя, через запятую. Можно указывать роль юзера в группе через двоеточие. Например, ``&groups=`Users:1` `` добавит юзера в группу "Users" с ролью "member".                |
| **&loginContext**      | `current context`                | Основной контекст для авторизации. По умолчанию - текущий.                                                                                                                                                      |
| **&loginResourceId**   | `0`                              | Идентификатор ресурса, на который отправлять юзера после авторизации. По умолчанию, это 0 - обновляет текущую страницу.                                                                                         |
| **&loginTpl**          | `tpl.HybridAuth.login`           | Этот чанк будет показан анонимному пользователю, то есть любому гостю.                                                                                                                                          |
| **&logoutResourceId**  | `0`                              | Идентификатор ресурса, на который отправлять юзера после завершения сессии. По умолчанию, это 0 - обновляет текущую страницу.                                                                                   |
| **&logoutTpl**         | `tpl.HybridAuth.logout`          | Этот чанк будет показан авторизованному пользователю.                                                                                                                                                           |
| **&providerTpl**       | `tpl.HybridAuth.provider`        | Чанк для вывода ссылки на авторизацию или привязку сервиса к учетной записи.                                                                                                                                    |
| **&providers**         | `all available`                  | Список провайдеров авторизации, через запятую. Все доступные провайдеры находятся тут `{core_path}components/hybridauth/model/hybridauth/lib/Providers/`. Например, ```&providers=`Google,Twitter,Facebook````. |
| **&rememberme**        | `1`                              | Запоминает пользователя на долгое время. По умолчанию - включено.                                                                                                                                               |

## Примеры

Сниппет нужно вызывать некэшированным, так как в зависимости от авторизации пользователя он выводит разные чанки.

При обычном вызове сниппет выведет всех провайдеров, зарегистрированных в системе

```modx
[[!HybridAuth]]
```

Их можно ограничить, указав списком, через запятую:

```modx
[[!HybridAuth?
  &providers=`Yandex,Google,Twitter,Facebook,Vkontakte`
]]
```

Авторизация в 2 контекста сразу:

```modx
[[!HybridAuth?
  &providers=`Yandex,Google`
  &loginContext=`web`
  &addContexts=`en`
]]
```

## Настройки провайдеров

Для каждого провайдера авторизации указывается отдельная системная настройка с префиксом **ha.keys.**:

![Настройки провайдеров](https://file.modx.pro/files/0/6/3/063adfe9b80ed7c6053b97e3818e0e0b.png)

Значение настройки - JSON массив, содержимое которого зависит от самого провайдера.

Например, для Google нужно указать **id** и secret, а для Twitter - **key** и secret. Так что, смотрите внимательно, какие настройки вам выдаят сервис при регистрации.

### Ссылки на регистрацию у провайдеров

Для работы компонента нужно получить ключи от провайдера. Поэтому, вот несколько основных ссылок:

- [Яндекс][1]
- [Вконтакте][2]
- [Twitter][3]
- [Google][4]
- [Facebook][5]
- [Остальные провайдеры][6]

### Контексты

Если у вас несколько независимых контекстов на сайте, вы можете организовать для них авторизацию через одних и тех же провайдеров, но для разных доменов.

Для этого нужно указывать ключи непосредственно в настройках контекста, а не в общих системных.

*Для правильной работы сниппета желательно включить дружественные url.*

[1]: /components/hybridauth/providers/yandex
[2]: /components/hybridauth/providers/vkontakte
[3]: /components/hybridauth/providers/twitter
[4]: /components/hybridauth/providers/google
[5]: /components/hybridauth/providers/facebook
[6]: /components/hybridauth/providers/