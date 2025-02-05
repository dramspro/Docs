# Очередь писем

## Формирование очереди

Для оформления рассылки писем пользователям их необходимо добавить в очередь.

- Перейдите в **Компоненты** -> **Sendex**

![Формирование очереди - 1](https://file.modx.pro/files/3/f/0/3f0e673a7ed51e205d2e683d35914390.png)

- На вкладке **Очередь писем** в выпадающем списке выбрать рассылку, для которой нужно сгенерировать письма.

![Формирование очереди - 2](https://file.modx.pro/files/5/0/9/5099cea4f7eb982ef5ca4ee59faca458.png)

![Формирование очереди - 3](https://file.modx.pro/files/4/1/a/41ae797ee96de03bf8c634e72e722bc9.png)

- Затем нужно отправить сгенерированное.

## Рассылка писем

Для рассылки писем есть несколько способов:

1. Вручную. Нужно зайти в **Компоненты** -> **Sendex**, вкладка **Очередь писем**. Выбрать пиьсмо и отправить, через контекстное меню.
    ![Рассылка писем](https://file.modx.pro/files/4/1/a/41ae797ee96de03bf8c634e72e722bc9.png)

2. Автоматически, через **cron**. В комплекте с дополнением идёт файл `core/components/sendex/cron/send.php`, который нужно добавить в cron. Частота запусков зависит от количества ваших подписчиков и ресурсов хостинга - за раз скрипт отправляет до 100 писем. После отправки письмо удаляется из очереди.

3. Через API.

    ```php
    $modx->addPackage('sendex', MODX_CORE_PATH . 'components/sendex/model/');

    $q = $modx->newQuery('sxQueue');
    $queue = $modx->getCollection('sxQueue');
    /** @var sxQueue $email */
    foreach ($queue as $email) {
      $email->send();
    }
    ```
