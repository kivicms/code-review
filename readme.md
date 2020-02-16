#Методика проведения Code Review
##Цели
1. Снижение вероятности возникновения ошибок
2. Погружение программистов в разрабатываемый функционал
3. Предотвращение роста технического долга
4. Соблюдение стандартов оформления кода (Code Style)
5. Повышение качества кода
6. Требования к программисту, проводящему Сode Review:
7. Программист, проводящий Сode Review, должен быть Frontend-разработчиком, если назначаемый MR соответствует проектам всей группы jetStyle и проектам: st, ui-react, в иных случаях он должен быть Backend-разработчиком.
8. Программист, проводящий Сode Review, должен соблюдать правила написания кода, указанные ниже.
9. Всё, что не указано в правилах написания кода по желанию автора кода может быть интерпретировано в его пользу в случае если изменения не наносят вреда проекту в целом.
10. Программист, проводящий Сode Review, вправе сам залить изменения в master, используя правила принятия изменений в основную ветку репозитория, если учтены все перечисленные факторы: 
    1. есть права на залитие в master-ветку искомого репозитория;
    2. в merge-request не более 10 строк измененного кода (считается как добавление, так и удаление строк);
11. Если программист, проводящий Сode Review, не уверен на 100% соблюдены ли правила написания кода в merge-request , то он вправе перевести merge-request на другого программиста в целях повторного проведения Сode Review.
12. После того как программист, проводящий Сode Review, убедится что все правила написания кода соблюдены, он должен поставить пометку о том что Сode Review пройдено и уведомить об этом сдающего.
13. В случае если назначенный на программиста Merge Request относится к конструктору страниц, то Сode Review по нему проводить не нужно. Необходимо такой Merge Request залить в master, используя правила принятия изменений в основную ветку репозитория при условии есть права на залитие в master-ветку искомого репозитория, в иных случаях переназначить Merge Request на другого программиста, используя требования при маршрутизации Merge Request.

* ## Правила написания кода:
14. Логика работы кода соответствует тому, что должно быть реализовано в задаче
15. Внешний вид в вёрстке соответствует тому, что представлено на тестовом сервере
16. Оформление кода (Code Style ) соответствует стандарту PSR-2
17. Разработанный функционал соответствует утверждённым стандартам PSR
18. Для многострочных массивов обязательна завершающая запятая, так как позволяет легче добавлять новые элементы в конец массива. Для однострочных массивов array(1, 2) предпочтительней array(1, 2,)
19. Строковые константы должны быть обрамлены в одинарные кавычки (в PHP);

####Пример обрамления строк:
```php
$fio = 'Иванов Иван Иванович';
$text = "Меня зовут $fio"; //Нет нарушения регламента. Не является строковой константой, так как значение строки может измениться в ходе выполнения PHP скрипта. Собственно поэтому она приравнивается имени со знаком $ (переменной).
$text = 'Меня зовут $fio'; //Нет нарушения регламента, но работать будет не корректно. В строку не подставится переменная $fio. Также в этом случае строковая константа приравнивается имени со знаком $ (переменной), что противоречит синтаксису PHP языка.
$text = "Меня зовут" . $fio; //Есть нарушение регламента. Строковая константа, без определенного имени, обрамлена в двойные кавычки. Она будет проходить лишнюю обработку в ходе выполнения PHP скрипта.
$text = 'Меня зовут' . $fio; //Нет нарушения регламента.

```
* Отсутствуют ошибки PHP (E_NOTICE и выше), отсутствуют ошибки JS
* В местах, где возможно потенциальное возникновение ошибки, необходимо делать проверки. Ошибки писать в лог с уровнем WARNING.
* Отсутствует дублирование кода
* Вёрстка отвечает требованиям Стандарт верстки html-страниц
* Дублирующие функционал страницы удалены
* На страницах, отображаемых клиенту, работает код Google tag manager
* Ошибки и исключения записываются в лог, каждая запись минимально включает в себя
    * дату и время возникновения;
    * стек вызовов;
    * ссылка, по которой был получен неверный по мнению приложения контент и содержимое запроса (если ошибка при запросе в сторонний сервис);
    * достаточную информацию для идентификации пользователя
* В затрагиваемых файлах отсутствуют куски комментированного кода
* В профилировании минимально требуемое количество операций, например
    * отсутствует старт сессии для анонимов
* Доступ выполнению кода есть у минимального возможного круга лиц
* Используется "тонкий" контроллер, вся бизнес логика вынесена в модель
* Контроллер не содержит SQL-запросы
* Контроллер не содержит html и другую разметку
* Попапы, требующие дополнительного запроса на сервер, необходимо загружать по требованию пользователя
* Используемый код, константы и методы расположены как можно ближе к вызывающему коду
* Поля класса и методы имеют минимальную область видимости, требуемую на момент сдачи кода (по умолчанию private), нет GOD-переменных
* Пользователю не выводятся значения phpinfo(), $_ENV, других переменных
* Нет уязвимостей:
    * присутствует проверка CSRF на формах;
    * все запросы на изменение отправляются методом POST;
    * значения отправляемых пользователем значений очищаются от вредоносного кода (защита от XSS)
* Для страниц авторизованного пользователя наличие запрета на индексацию в метатегах
* Запрос на изменение и комментарий не содержит ошибок языка
* Запрос на изменение не ухудшает проект в целом (не добавляет дубли кода), рефакторинг приветствуется;
* Assignments in conditions (присваивания в условиях) запрещены;
* Операции в условии должны быть заключены в скобки.
* Для внешних операций запрещается использовать текстовые параметры, которые могут быть изменены (например, название домена). Нужно использовать идентификаторы.
* Форма, отправляющая на сервер данные, должна быть оформлена при помощи модели. Логика проверок JS должна дублироваться на стороне сервера (желательно использовать встроенный механизм валидации форм Yii).
* Согласно RFC 2616 метод POST должен быть использован для любого контекста, в котором запрос не идемпотентен: то есть, он вызывает изменение состояния сервера каждый раз при выполнении, такие как отправка комментария к сообщению в блоге или интернет-голосование. Метод GET для идемпотентных действий и для нульпотентных, то есть без побочных эффектов (в отличие от «без побочных эффектов при втором и последующих запросах» как с идемпотентными операциями).
* Процент покрытия кода тестами не меньше, чем в текущем master (в gitlab раздел builds, сравниваем coverage последнего билда с master).
* Веб-страница содержит защиту от 5xx при любых входных данных.
* Пример корректного приёма данных на сервер при запросе на изменение
```php
$model = new Feedback();
 
if ($model->load(Yii::$app->request->post()) && $model->validate()) {
            $sendStatus = $model->save();
}

```
* Миграции добавлены корректно: добавлены новые файлы миграций, а не отредактированы старые.

* Код не содержит бесконечных циклов (даже потенциальных).
* Вызов API должен быть независим. Не доступность одного API не должно блокировать вызов другого API 

* Не группировать все параметры в один запрос кроме параметров которые зависят друг от друга;
* Отсутствуют последовательные запросы длительностью > 0,1 сек. - открываем в отладчике Yii изменяемый action и проверяем;
* Обрабатывать результаты каждого запроса на наличие данных, корректность.
* Перед использованием переменных проверить на ее существование.
* Максимальная длина строки 120 символов
* После приведения типа переменной обязателен пробел перед переменной. Например: (array) $array
* В вызовах функций у которых много параметров и строка более 120 символов каждый параметр (включая первый) с новой строки. Закрывающая скобка ); также на новой строке.
##SQL-запросы
* Использование ActiveRecord на ресурсах с проектируемой нагрузкой (Request per minute > 50) запрещено.
* Если в запросе используются Where, на поля фильтра должны быть повешаны индексы
* Запросы должны быть параметризованы, чтобы использовать кеш движка СУБД вместо повторного анализа структуры запроса
* В новых таблицах
##Базы данных
* Минимальная размерность поля для хранения AUTO_INCREMENT данных bigint(20) unsigned.
##Обработка исключений
* Обязательно логировать ошибки, которые происходят в приложении. Пример логирования:
```php
try {
  //Код, который вызывает исключение
} catch (\Exception $exc) {
  Yii::warning($exc->getMessage(), __METHOD__);
}
```
      
