# Выгрузка старых заказов из интернет-магазина в RetailCRM

## Описание

Для чего нужно.  Если вы подключаете RetailCRM к уже действующему магазину - наверняка там уже есть созданные ранее заказы.  Выгружая эти данные мы позволяем RetailCRM просчитывать более точную аналитику, и как минимум, показывать сколько учтенных покупок у того или иного пользователя.

## Пошаговая инструкция

1. К этому моменту у Вас уже должен быть куплен, подключен к магазину и настроен компонент modRetailCRM. Под настройкой я подразумеваю заполнение ключа АПИ, символьного кода сайта и адреса вашей CRM, так как здесь мы уже используем компонент. Если это не сделано - вернитесь к предварительной настройке компонента.

2. Если заказов у вас в базе немного (понятие относительное, скажем меньше сотни) - мы можем запустить в админке MODX компонент console и выполнить в нем следующий код

```php
<?php
if (!$modRetailCrm = $modx->getService(
  'modretailcrm',
  'modRetailCrm',
  MODX_CORE_PATH . 'components/modretailcrm/model/modretailcrm/',
  array($modx)
)) {
  $modx->log(modX::LOG_LEVEL_ERROR, '[modRetailCrm] - Not found class modRetailCrm');
  return;
}

$q = $modx->newQuery('msOrder');
//Сколько пользователей за раз передаем
$limit = 50;
//Сколько пропускаем от начала
$offset = 0;
$q->limit($limit, $offset);
$orders = $modx->getIterator('msOrder', $q);
//Выгружаем по одному
foreach($orders as $msOrder) {
  $modRetailCrm->msOnCreateOrder($msOrder);
}


```

По сути на этом все. Могу только добавить, что при большой базе эффективнее будет вынести код в отдельный php файл и запустить его через консоль сервера. Только не забудьте в этом случае в начале файла подключить MODX. Если не знаете как это сделать - читаем [это][1]

[1]: https://modx.pro/development/3163

## Возможные ошибки

Если что-то пошло не так, и выгрузка заказов не удалась, в первую очередь проверяем что вы заполнили системные настройки modRetailCRM.
Затем можем попробовать выгрузить поменьше заказов. Для начала и один сойдет.  Просто меняем ```$limit```
Если и сейчас что-то идет на так - включаем логгирование ошибок в системной настройке **modretailcrm_log** пробуем еще раз и смотрим журнал ошибок MODX

Как правило, RetailCRM хорошо описывает все ошибки - и этого шага будет достаточно, чтобы понять, чего не хватает или что пошло не так.
