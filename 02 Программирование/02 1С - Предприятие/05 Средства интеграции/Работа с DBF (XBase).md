# XBase

XBase в абсолютном понимании — это язык программирования. В контексте 1С — это объект со свойствами и методами, представляющий таблицу базы данных (DBF файл)

Объект XBase позволяет считывать, удалять и редактировать DBF таблицы

## Свойства объекта XBase

| Свойство | Тип | Описание |
|-|-|-|
| `<имя поля>` | `Число`, `Строка`, `Дата`, `Булево` | поле текущей записи |
| `АвтоСохранение` | `Булево` | Содержит признак автоматического сохранения изменений в базе данных |
| `Индексы` | `КоллекцияИндексовXBase` | Содержит коллекцию индексов таблицы базы данных формата DBF |
| `Ключ` | `КлючXBase` | Предоставляет доступ к объекту КлючXBase. Состав свойств объекта повторяет набор свойств полей объекта XBase. Значения свойств используются для вычисления выражения индекса при использовании метода НайтиПоКлючу |
| `Кодировка` | `КодировкаXBase` |  |
| `ОтображатьУдаленные` | `Булево` |  |
| `Поля` | `КоллекцияПолейXBase` | Содержит коллекцию полей таблицы базы данных формата DBF |
| `ТекущийИндекс` | `ИндексXBase` |  |

**`КоллекцияПолейXBase`** представляет собой типичную коллекцию 1С, поддерживающую обход в цикле. Каждый элемент коллекции представляет собой `ПолеXBase`

**`ПолеXBase`** имеет следующую структуру:

| Свойство | Тип | Описание |
|-|-|-|
| `Длина` | `Число` | длина поля в символах |
| `Имя` | `Строка` | наименование поля |
| `Тип` | `Строка` | принимает значения: `{S,N,D,L,F,M}` |
| `Точность` | `Число` | количество разрядов дробной части |

| Значение свойства Тип | Описание |
|-|-|
| `S` | `Строка`|
| `N`, `F` | `Число`|
| `D` | `Дата`|
| `L` | `Булево`|
| `M` | `Мемо (не поддерживается)`|

**Длина в зависимости от типа:**

- числовое поле ("N", "F") - максимальная длина поля 19 символов;
- строковое поле ("S") - максимальная длина поля 254 символа;
- поле типа дата ("D") - длина поля равна 8 символам;
- поле типа булево ("L") - длина поля равна одному символу.

## Методы XBase

| Метод | Возвращает | Описание |
|-|-|-|
| `<Конструктор>` (`XBase([путьКФайлуDBF], [путьКИндексу], [ТолькоЧтение])`) | `XBase` | Создает объект. При указании путей к файлам, они будут сразу открыты |
| `ВНачале()` | `Число` | указатель записи находиться перед первой записью |
| `ВКонце()` | `Число` | указатель записи находится за последней записью |
| `Добавить()` |  | Создает новую запись (для сохранения изменений требуется записать данные в эту запись) |
| `СоздатьФайл(путьКФайлуDBF, [путьКИндексу])` |  |  |
| `ОткрытьФайл(путьКФайлуDBF, [путьКИндексу], [ТолькоЧтение])` |  |  |
| `ЗакрытьФайл()` |  |  |
| `ОчиститьФайл()` |  | Удаляет все записи в таблице. При этом все существующие записи удаляются физически и не могут быть впоследствии восстановлены |
| `Записать()` |  | Сохраняет изменения совершенные над объектом XBase в файл DBF |
| `Сжать()` |  | Сжатие таблицы путем удаление помеченных записей (аналогично операции FoxPro `PACK`) |
| `НомерЗаписи()` | `Число` | номер текущей записи |
| `Перейти(номер)` | `Булево` | переход на запись с указанным номером |
| `УстановитьЗначениеПоля(имяПоля, значение)` |  |  |
| `Удалить()` |  | помечает текущую запись на удаление |
| `Восстановить()` |  | снимает пометку удаления |
| `Открыта()` | `Булево` |  |
| `Последняя()` | `Булево` | последняя запись получена |
| `Следующая()` | `Булево` | Инкрементирует указатель записи на +1. Истина: получена следующая запись; Ложь: конец выборки |

#1С #XBase #DBF #1С/Интеграция
