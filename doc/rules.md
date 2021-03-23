[< Содержание](/README.md)
# Общие требования по доработке типовых конфигураций на платформе 1С Предприятие 8

* [Модификация типовых конфигураций](#модификация-типовых-конфигураций)
  * [Подготовка типовой конфигурации перед началом разработки](#подготовка-типовой-конфигурации)
  * [Правила добавления и изменения объектов метаданных](#правила-добавления-новых-объектов)
  * [Доработка форм](#доработка-форм)
  * [Доработка общих модулей, модулей объектов, модулей менеджеров](#доработка-модулей)
   * [Роли](#роли)
   * [Интерфейс](#интерфейс)

* [Повторное использование кода, стандартные библиотеки.](#повторное-использование-кода)
* [Регистрация ошибок, сообщения и предупреждения](#сообщения-и-предупреждения)



<a id="markdown-модификация-типовых-конфигураций" name="модификация-типовых-конфигураций"></a>

## Модификация типовых конфигураций

<a id="markdown-подготовка-типовой-конфигурации" name="подготовка-типовой-конфигурации"></a>

### Подготовка типовой конфигурации перед началом разработки

После создания хранилища для типовой конфигурации архитектор проекта или ведущий разработчик выполняет следующие действия:

* Включить возможность изменения. В диалоговом окне настройки правил поддержки включить флаги "Объект поставщика не редактируется".

![](/img/repsettings.png)

* Изменить режим поддержки для корня конфигурации. Установить "Объект поставщика редактируется с сохранением поддержки".

* Включить [подсистему СлужебныеФункцииГрадум](/doc/subsysfunc.md) в состав основной конфигурации.  

<a id="markdown-правила-добавления-новых-объектов" name="правила-добавления-новых-объектов"></a>

### Правила добавления и изменения объектов метаданных

* Снятие объектов типовой конфигурации с поддержки должно быть согласовано с архитектором

* Все изменения, связанные со схемой данных, выполняются в основной конфигурации. Добавлять реквизиты, объекты метаданных в расширения **запрещено**. Более подробно работа с расширениями описана в разделе ["Применение расширений"](/doc/useexts.md)

* Новым объектам, реквизитам, переменным в типовом модуле и т.п. в начале названия добавляем префикс "гр", например: грСумма; если добавили табличную часть грМастера, то реквизиты ТЧ пишем без префикса; если добавили функцию грФункция, то локальные переменные пишем без префикса.

* Синоним добавленных объектов не должен содержать префикс «гр». Если синоним совпадает с синонимом типового объекта, то в конец синонима добавлять «(Градум)». Для новых регистров, отчетов, обработок, перечислений, ролей в конце синонима добавлять «(Градум)». Например: «Отчет ДДС (Градум)»

* Все добавленные объекты включаются в подсистему грДобавленныеОбъекты

* Все измененные типовые объекты добавляются в подсистему грИзмененныеОбъекты

* При добавлении отчетов указывать хранилище вариантов, используемое в конфигурации

* При добавлении объектов конфигурации необходимо назначать права на объект. Если в задаче нет дополнительных указаний, права на чтение и запись давать роли грБазовыеПрава.

* При добавлении регламентного задания проверять, что в конфигураторе признак «Использование» = ЛОЖЬ

* У типовых подписок на события не изменять источники данных, вместо этого создавать копию подписки с префиксом «гр» и в неё добавляем свои источники данных.

* Новые справочники, документы и отчеты к типовым механизмам БСП: Свойства, запрет редактирования, версионирование, РЛС, варианты отчетов.

* Объекты метаданных сортируются в дереве конфигурации по имени и по возрастанию.


<a id="markdown-доработка-форм" name="доработка-форм"></a>

### Доработка форм

* Основные изменения типовых форм выполняются программно. Шаблоны разработки модификации форм размещены в разделе [Шаблоны разработки](/doc/devpatterns.md#доработка-типовых-форм). Если есть потребность значительного изменения типовой формы, доработка выполняется непосредственно в форме, программно переопределяются события типовых элементов.

* Новым реквизитам и элементам формы добавлять префикс "гр"

* Настройку условного оформления форм и динамических списков необходимо делать в коде формы. Подробно в разделе [Шаблоны разработки](/doc/devpatterns.md#видимость-доступность-элементов-формы)

* Настройки условного оформления должны производится при создании формы и потом не должны модифицироваться

* Механизмы управления видимостью и доступностью элементов формы реализуются программно согласно [шаблону разработки](/doc/devpatterns.md#видимость-доступность-элементов-формы )

* Поля  реквизитов должны выравниваться по опорной линии.

Неправильно:
![](/img/1a1.png)

Правильно:
![](/img/2a1.png)

<a id="markdown-доработка-модулей" name="доработка-модулей"></a>

### Модули конфигурации

* Необходимо группировать все процедуры/функции в модулях по их назначению с использованием областей. А также с разделением на интерфейс модуля и служебную часть [см. стандарты разработки на ИТС](https://its.1c.ru/db/v8std#content:455:hdoc)


* В модуле менеджера и модуле объекта должна быть прописана директива препроцессора: “#Если Сервер Или ТолстыйКлиентОбычноеПриложение Или ВнешнееСоединение Тогда”. 

* Добавление общих модулей согласуется с архитектором

* Добавлять суффикс в имени общего модуля в зависимости от свойства модуля Клиент, ВызовСервера, ПовтИсп. Создание новый общих модулей выполнять в соответствии с [правилами создания общих модулей](https://its.1c.ru/db/v8std#content:469:hdoc@5e224c3e)

* Новому серверному модулю устанавливать свойство "Внешнее соединение"

* Для модулей со свойством «Сервер» и «Внешнее соединение» не добавлять свойство «Вызов сервера», а при необходимости создавать новый модуль и из него вызывать функции серверного.

* В типовых модулях конфигурации недопустимо создание функций и процедур. Для этого создается общий модуль, например, если вносятся изменения в модуль объекта документа ПоступлениеТоваровУслуг, то создается общий модуль 1.1.	грПоступлениеТоваровУслугМодульОбъекта, для типового общего модуля гр<Имя модуля>Расширение, и в нем реализуются необходимые процедуры и функции. В дорабатываемом модуле должны быть только вызовы процедур/функций. 

* В дорабатываемом/разрабатываемом модуле должно применяться автоформатирование (alt+shift+F). Разработанный код должен быть выровнен: знаки "=" друг под другом, табуляции, отступы после функциональных блоков - код должен быть удобочитаемым

* Все методы добавленных подписок на события должны вызываться из модуля грПодпискиНаСобытия. Процедуры модуля грПодпискиНаСобытия не должны содержать реализацию, а только вызов процедур из других модулей.  
Например, создали подписку **грДоговорыКонтрагентовПередЗаписью**, назначаем метод **грПодпискиНаСобытия.ДоговорыКонтрагентовПередЗаписью**.  
Реализация метода:

```bsl
Процедура ДоговорыКонтрагентовПередЗаписью(Источник, Отказ) Экспорт
	грДоговорыКонтрагентов.ДоговорыКонтрагентовПередЗаписью( Источник, Отказ );
КонецПроцедуры
```
<a id="markdown-роли" name="роли"></a>

### Роли

* Типовые роли конфигурации не изменяются. Если есть необходимость доработать типовую роль, создать новую с префиксом `гр` и выполнить модификацию.

<a id="markdown-интерфейс" name="интерфейс"></a>

### Интерфейс

* Все добавленные объекты должны быть доступны через интерфейс пользователя.

<a id="markdown-повторное-использование-кода" name="повторное-использование-кода"></a>

## Повторное использование кода, стандартные библиотеки.

* При разработке придерживаться принципов, принятых в дорабатываемой конфигурации. Например, если в новом документе реализуется обработка проведения, то структура процедуры должна соответствовать структуре, принятой в конфигурации. Если документ делает движения по регистрам, то должна быть возможность в пользовательском режиме просматривать эти движения, аналогично функционалу дорабатываемой конфигурации

* Перед началом разработки нового функционала провести анализ на предмет возможности повторного использования кода. Например, часть необходимого функционала может быть реализована в БСП или других библиотеках. 

* Для доопределения типовой функциональности используем механизм типовых переопределяемых общих модулей выявить возможность вставки кода в модуля с постфиксом Переопределяемый. 


<a id="markdown-сообщения-и-предупреждения" name="сообщения-и-предупреждения"></a>

## Регистрация ошибок, сообщения и предупреждения

* Все сообщения (предупреждения, уведомления) должны быть достаточно информативными и содержательными. Имена объектов конфигурации в сообщениях (предупреждениях, уведомлениях) должны даваться так, как они представлены в пользовательском интерфейсе

* Конфигурация должна выдавать предупреждения с подробными пояснениями перед выполнением процедур, занимающих продолжительное время

* При выдаче в окно сообщений информации, связанной с конкретным объектом информационной базы, должно быть явно указано, какой объект информационной базы вызвал появление сообщения

* При выдаче пользователю вопросов с несколькими вариантами выбора ответа, по умолчанию должен предлагаться ответ, выбор которого вызывает действия, либо наиболее безопасные для информационной базы, либо предусматривающие контроль пользователя за выполнением действий.

* Модальные диалоги, вопросы, предупреждения не должны вызываться внутри транзакций записи и проведения.

* Для вывода в окно сообщений требуется использовать метод ОбщегоНазначение.СообщитьПользователю

* В тексте сообщений об ошибках или информационных объекты ИБ должны быть выделены символами «<>». Пример текста ошибки:
 «По договору <№РП122345 от 21.10.2018> задолженность <25'4567.00 руб.>  превышает допустимую <1’000.00 руб.> »

 * При записи в журнал регистрации в переменную  `ИмяСобытия` процедуры `ЗаписьЖурналаРегистрации` передавать значение, котороые получено функцией `ИмяСобытияЖурналаРегистраций<ИмяСобытия>` из общего модуля `грОбщегоНазначения` (входит в [подсистему СлужебныеФункцииГрадум](/doc/subsysfunc.md) ). Если требуемой функции нет, необходимо создать по шаблону `ИмяСобытияЖурналаРегистраций<ИмяСобытия>` в разделе `МенеджерРегистрацииОшибок` общего модуля `грОбщегоНазначения`

* Если используется `Попытка` `Исключение` при возникновении ошибки, делать запись в журнал регистрации, с указанием текста ошибки. 

* При регистрации ошибки в журнале регистраций не следует использовать функцию ОписаниеОшибки, т.к. она неинформативна для разработчика, потому что не возвращает стек в тексте ошибки. Правильно записывать в журнал регистрации подробное представление исключения, а краткое представление добавлять в текст сообщения пользователю:

```bsl
&НаСервере
Процедура ВыполнитьОперацию()
  Попытка 
    // код, приводящий к вызову исключения
    ....
  Исключение
    // Запись события в журнал регистрации для системного администратора.
    грОбщегоНазначения.ИмяСобытияЖурналаРегистрацийЗаполнениеТЧТовары(),
       УровеньЖурналаРегистрации.Ошибка,,,
       ПодробноеПредставлениеОшибки(ИнформацияОбОшибке()));
    ВызватьИсключение;
  КонецПопытки;
КонецПроцедуры
```

```bsl

&НаКлиенте
Процедура КомандаВыполнитьОперацию()

    Попытка 
        ВыполнитьОперацию();
    Исключение
        ТекстСообщения = КраткоеПредставлениеОшибки(ИнформацияОбОшибке());
        ПоказатьПредупреждение(,НСтр("ru = 'Операция не может быть выполнена по причине:'") + Символы.ПС + ТекстСообщения);
    КонецПопытки;

КонецПроцедуры
```