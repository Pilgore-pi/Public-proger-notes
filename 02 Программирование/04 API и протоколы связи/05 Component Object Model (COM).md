
> **The Microsoft Component Object Model (COM)** — созданная в 1990-е годы, объектно-ориентированная система для создания бинарных программных компонентов, с которыми могут взаимодействовать другие программы.

COM — это основа для таких технологий, как Microsoft OLE, ActiveX и других.

COM технология позволяет взаимодействовать нескольким независимым программам между собой

- В рамках одного процесса;
- Из разных процессов;
- По сети (распределенный COM, **Distributed COM (DCOM)**)

Технология COM представляет собой архитектуру для создания и использования компонентов программного обеспечения, позволяющую взаимодействовать между различными языками программирования и приложениями на платформе Windows. Технология COM основана на технологии RPC (Remote Procedure Call)

> COM-технология доступна только в ОС семейства **Windows**

Интерфейсы COM строго типизированы и хорошо совместимы с .Net и C++, но также технологию можно использовать с интерпретируемыми языками с динамической типизацией

#### Идентификаторы объектов

Любой интерфейс или класс в рамках технологии COM имеет свой 128-битный (16-байтный) бинарный идентификатор **`GUID`** (globally unique identifier) или **`UUID`** (Universally Unique ID). GUID, в зависимости от типа, к которому относится идентификатор, именуется по-разному:

- **IID** — Interface ID, идентификатор интерфейсов COM
- **CLSID** — class ID, идентификатор COM-класса
- **ProgID** — идентификатор на естественном языке, который может прочитать человек. В рамках одной программы уникален, но в рамках COM-архитектуры `ProgID` идентификатором не является

Идентификаторы необходимы, чтобы не возникало коллизий между COM-сущностями с одинаковыми названиями.

При создании нового интерфейса COM, необходимо определить для него `GUID`. При использовании интерфейса внешним сервисом, этот сервис должен обращатся к уникальному идентификатору, чтобы избежать любые возможные конфликты имен.

При создании нового COM интерфейса можно создать определение интерфейса с помощью специального языка определения интерфейсов — **`IDL`** (interface definition language). При определении интерфейса с применением `IDL`, формируются файлы заголовков для использования внешними сервисами.

COM интерфейсы поддерживают концепцию ООП. COM интерфейсы, в целом, представляют классическую концепцию интерфейса в ООП. Сответственно COM интерфейсы обладают следующими свойствами:

- Интерфейсы могут наследоваться и использоваться для полиморфизма.
- Один интерфейс может быть унаследован только от одного другого интерфейса, при этом в дочерний интерфейс копируются все методы родительского интерфейса.
- Экземпляр интерфейса не может существовать сам по себе. Можно создавать только экземпляры класса, реализующего интерфейс.
- Класс, реализующий интерфейс должен реализовать все методы интерфейса

COM класс идентифицируется 128-битной последовательностью, называемой **`CLSID`** (Class ID). `CLSID` — это тоже `GUID`. Этот идентификатор связан с определенным классом, расположенным в файловой системе в виде файла DLL или EXE.

COM поддерживает работу с указателями на функции

## Интерфейс IUnknown

Все COM интерфейсы включены в иерархию, во главе которой находится интерфейс **`IUnknown`**. `IUnknown` интерфейс содержит минимально необходимый набор функций для обеспечения механизма полиморфизма и управления временем жизни объекта в памяти.

`IUnknown` интерфейс имеет 3 функции:

| Функция            | Описание                                                                                                                         |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| `QueryInterface()` | Обеспечивает механизм полиморфизма для COM-объекта                                                                               |
| `ULONG AddRef()`   | Увеличивает счетчик указателей на интерфейс и возвращает его. Метод должен вызываться, когда создается копия ссылки на интерфейс |
| `ULONG Release()`  | Уменьшает счетчик указателей и возвращает новое значение счетчика                                                                |

Функции `AddRef()` и `Release()` контролируют время жизни COM-объекта путем изменения счетчика ссылок. Когда количество ссылок опускается до 0, COM-объект может быть удален в любой момент, так как других объектов, ссылающихся на этот COM-объект нет.

В ОС Windows COM-объекты всегда содержаться в одном из двух типов файлов:

- Dynamic Linked Library (DLL)
- Executable (EXE)

Все идентификаторы CLSID и адреса связанных с ними файлов находятся в единой базе данных, которая содержит информацию обо всех GUID COM-объектов в системе. Пользователю COM-объекта достаточно знать его GUID, чтобы использовать его. При обращении к COM-объекту, происходит запрос к базе данных, после чего происходит получение информации о COM-объекте.

Взаимодействие с COM-объектами и их пользователями реализовано согласно модели **клиент-сервер**. В роли клиента выступает пользователь объекта, а в роли сервера выступает модуль, которому подчинен COM-объект

To enable creating a COM object, a COM server must provide an implementation of the [**IClassFactory**](https://learn.microsoft.com/en-us/windows/win32/api/unknwn/nn-unknwn-iclassfactory) interface. Clients can call the [**CreateInstance**](https://learn.microsoft.com/en-us/windows/desktop/api/Unknwn/nf-unknwn-iclassfactory-createinstance) method to request a new instance of a COM object, but usually such requests are encapsulated in the [**CoCreateInstance**](https://learn.microsoft.com/en-us/windows/desktop/api/combaseapi/nf-combaseapi-cocreateinstance) function.

Для включения доступа к создания COM-объектов, COM-сервер должен реализовывать интерфейс `IClassFactory`. Клиенты могут вызывать метод `CreateInstance()`, чтобы создать новый объект, но обычно создание инкапсулировано функцией `CoCreateInstance()`

## Концепция COM простыми словами

Допустим, у нас есть автомобиль и семья из 3 человек. Каждый член семьи может пользоваться машиной.

В данном случае, машина — это COM-класс (COM-сервер), модуль, содержащий набор методов согласно принятому контракту (интерфейсу). Например, отец в семье захотел поехать на работу на своей машине. Отец — это программа-клиента, использующая COM-сервер. Отец имеет доступ к ограниченному функционалу.

К примеру:

1. Отец может **сесть** за водительское место и **поехать**
2. Еще он может сесть за пассажирское заднее сидение, чтобы **взремнуть**. При этом на переднем сиденье он спать не может, так как мешает коробка передач. Другими словами передние места недоступны для действия "**вздремнуть**"
3. На переднем пассажирском месте отец может **взять обед** из бордачка и **начать есть**

Мы выделили 3 опции, доступных для отца, 3 метода COM-сервера, доступные для клиента.
Важно отметить, что у автомобиля обязательно есть регистрационный номер **`о777оо`** и модель **`Maserati Quattroporte`** (допустим, техническое название модели — `HR570-RX`). Как видно, наша семья в деньгах не нуждается. Регистрационный номер является идентификатором автомобиля и не может повторяться, а название модели может повторяться, но не факт, что рядом найдется водитель такого же дорогого автомобиля.

| Аналогия                                                                   | Суровая реальность            |
| -------------------------------------------------------------------------- | ----------------------------- |
| Зарегестрированная машина конкретной модели `Maserati Quattroporte o777oo` | COM-объект                    |
| Модель `Maserati Quattroporte`                                             | COM-класс                     |
| Техническое название модели `HR570-RX`                                     | **`CLSID`**                   |
| Автомобили фирмы `Maserati`                                                | COM-интерфейс                 |
| Полное название фирмы `Maserati S.p.A.`                                    | **`IID`**                     |
| Действие `Сесть и поехать`                                                 | метод `SitAndDrive()`         |
| (Идентификатор действия `Сесть и поехать`)                                 | `GUID` метода `SitAndDrive()` |
| Семья: отец, мать и сын                                                    | Клиенты                       |
| Папина машина                                                              | Сервер                        |

## Реестр

Реестр — это системная база данных, содержащая инфу о конфигурации устройств компьютера, ПО и пользователях системы. Любая программа, разработанная под винду может добавлять записи с нужной инфой в реестр и получать данные обратно. Клиенты (семья) могут также обращаться к реестру для получения нужной инфы.

Реестр содержит всю информацию о COM-объектах, зарегестрированных в системе

Ключ реестра **`HKEY_CLASSES_ROOT\CLSID`** предоставляет всю информацию необходимую для перечисления всех COM-объектов, включая **CLSID** и **ProgID**.

Список всех идентификаторов **CLSID** может быть получен в PowerShell:

```powershell
New-PSDrive -PSProvider registry -Root HKEY_CLASSES_ROOT -Name HKCR
Get-ChildItem -Path HKCR:\CLSID -Name | Select -Skip 1 > clsids.txt
```

При выполнении этого скрипта в файл `clsids.txt` будут записаны идентификаторы COM-классов в подобном формате:

```
{0000002F-0000-0000-C000-000000000046}
{00000300-0000-0000-C000-000000000046}
{00000301-A8F2-4877-BA0A-FD2B6645FB94}
{00000303-0000-0000-C000-000000000046}
{00000304-0000-0000-C000-000000000046}
{00000305-0000-0000-C000-000000000046}
{00000306-0000-0000-C000-000000000046}
{00000308-0000-0000-C000-000000000046}
{00000309-0000-0000-C000-000000000046}
{0000030B-0000-0000-C000-000000000046}
{00000315-0000-0000-C000-000000000046}
{00000316-0000-0000-C000-000000000046}
```

С помощью полученных идентификаторов можно сгенерировать экземпляр любого из зарегестрированных COM-классов.

## Зарегестрированные COM-классы

Многие приложения при установке самостоятельно регистрируются как COM-объекты

| Приложение           | ProgID                   |
| -------------------- | ------------------------ |
| Microsoft Word       | `Word.Application`       |
| Microsoft Excel      | `Excel.Application`      |
| Microsoft PowerPoint | `PowerPoint.Application` |
| Microsoft Outlook    | `Outlook.Application`    |
| Microsoft Access     | `Access.Application`     |
| Microsoft Publisher  | `Publisher.Application`  |
| Microsoft Project    | `MSProject.Application`  |
| Microsoft Visio      | `Visio.Application`      |
| Microsoft OneNote    | `OneNote.Application`    |
| Microsoft FrontPage  | `FrontPage.Application`  |
| Microsoft InfoPath   | `InfoPath.Application`   |
| Microsoft SharePoint | `SharePoint.Application` |
| Microsoft Teams      | `Teams.Application`      |
| Microsoft Paint      | `Paint.Picture`          |
| Microsoft PhotoDraw  | `PhotoDraw.Picture`      |
| Microsoft Sway       | `Sway.Application`       |
| Adobe Acrobat        | `Acrobat.Document`       |
| AutoCAD              | `AutoCAD.Application`    |
| CorelDRAW            | `CorelDRAW.Application`  |
| Notepad++            | `Notepad++.Application`  |

[[02 Работа с Excel через COM|Работа с Excel в 1С через COM-объект]]

#COM #API #RPC #OS/Windows