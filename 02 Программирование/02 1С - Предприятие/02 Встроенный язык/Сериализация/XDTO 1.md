https://infostart.ru/1c/articles/167459/

С появлением платформы 8.1 фирма “1С” представила механизм, носящий интригующее название XML Data Transfer Objects или, если коротко - XDTO

Платформа позволяет оперировать фрагментами XML-документов, как обыкновенными объектами, к которым можно, например, обращаться “через точку”

Создаваемая схема описывает типы данных, которые мы можем использовать. Обязательно задается пространство имен, которому наши типы будут принадлежать. Сами типы подразделяются на простые и составные

Для XDTO важно, какая модель данных (структура типов) используется. Можно создавать объекты конфигурации, называемые **XDTO-Пакеты**.

В конфигураторе, каждая схема XML может быть представлена в виде ПакетаXDTO, а вся совокупность типов, имеющихся в конфигурации составляет Модель данных конфигурации

>Простые типы XML представляются в виде объектов языка 1С с типом “ЗначениеXDTO”. Составные типы XML представляются в виде объектов языка 1С с типом “ОбъектXDTO”

Подобно тому, как в конфигурации есть метаданные, предоставляющие информацию о типах, в XDTO тоже есть подобные объекты - ТипЗначенияXDTO и ТипОбъектаXDTO. Эти объекты позволяют узнать информацию о типе данных - размер, число знаков после запятой, неотрицательность и т.п.

>Для XDTO пакета в конфе нужно указать URI-ссылку на пространство имен, в котором содержится информация о моделе типов. После указания ссылки, в XDTO-пакете должно отобразиться дерево типов:

![[XDTO_пакет.jpg]]

#1С #1С/Язык_программирования/Сериализация/XDTO #XML