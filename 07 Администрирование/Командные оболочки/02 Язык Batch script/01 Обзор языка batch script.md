> Язык **batch script** — это сценарный язык командной строки Windows (`CMD.EXE`), используемый для автоматизации задач по администрированию

| Характеристика                           | Значение                                            |
| ---------------------------------------- | --------------------------------------------------- |
| **Дата появления**                       | 1981 (batch), 1989 (cmd.exe)                        |
| **Разработчик**                          | IBM, Microsoft                                      |
| Типизация (статическая/динамическая)     | Динамическая                                        |
| Типизация (слабая/сильная)               | Слабая                                              |
| Компилируемый/интерпретируемый           | Интерпретируемый                                    |
| Поддерживаемые парадигмы                 | Процедурная                                         |
| Однострочные и многострочные комментарии | Односточные: `:: ...`, `rem ...`; Многострочных нет |
| Чувствительность к регистру              | Нет                                                 |
| Назначение                               | Администрирование, автоматизация задач пользователя |
| Рефлексия                                | Нет                                                 |
| Естественный язык написания кода         | Английский                                          |
| Синтаксис                                | Специфический                                       |
| Точка с запятой в конце строки           | Не нужна                                            |
| Расширения файла с кодом                 | `.bat`                                              |
| Платформы                                | Windows                                             |

Существуют более современные языкы сценариев — [[01 Обзор языка PowerShell|PowerShell]] и Python.

Пакетный файл (batch file) — это файл с расширением `.bat`, который выполняется построчно командной строкой.

Этот язык позволяет использовать условия, циклы, объявления переменных, а также любую из команд cmd.exe.

>Язык не чувствителен к регистру, **как и имена всех файлов и каталогов в Windows**

## Стандартный шаблон скрипта

```batch
@echo off
chcp 65001
:: code
pause
```

`@` — говорит о том, что следующая комадна не должна отображаться в командной строке

`@ echo off` — говорит о том, что все последующие команды не должны отображаться в командной строке.

`chcp 65001` — указание кодировки UTF-8.

`pause` — ожидание ввода. Нужно, чтобы cmd не закрывался после выполнения скрипта.

* Чтобы вывести символ `%`, его нужно экранировать так: `%%`
* Чтобы вывести пустую строку, нужно ввести `echo .`

## Управление командами

==Оператор перенаправления результата== одной команды в другую: **`|`**

```
tasklist | find "notepad"

echo %USERNAME% | clip
```

==Оператор объединения команд==: **`&`**. Позволяет записывать сразу несколько команд. Каждая команда выполняется независимо.

```
md new_folder & copy file.txt new_folder\file.txt
```

При применении **`&&`** следующая команда выполниться, только если предыдущая выполнилась успешно.

Оператор ==**`||`**== имеет противоположную логику. Если первая команда завершится ошибкой, то вторая команда выполнится.

### Переменные

Команда **`set`** позволяет объявлять переменные. Существует 2 вида переменных:

1) Переменные, существующие в рамках рекущей сессии работы cmd.exe (`setlocal`)
2) Переменные пользователя, сохраненные в реестре `set`

```batch
:: текст:
set variable=2+2
> 2+2

:: число:
set \a variable=2+2
> 4
```

Чтение переменной:

```batch
set \a a=3

set \a b=%a%+%a%
echo %b%
```

Ввод значения переменной

```batch
@echo off
set \a a=Enter str
echo %a%
pause
```

- [`setlocal`](https://ab57.ru/cmdlist/setlocal.html) — установка локальных переменных в командном файле
- [`sleep`](https://ab57.ru/cmdlist/sleep.html), `timeout` — задержка по времени в пакетном файле

#### Типы переменных

Операторы сравнения

| `equ` | `==` |
| ----- | ---- |
| `neq` | `!=` |
| `lss` | `<`  |
| `leq` | `<=` |
| `gtr` | `>`  |
| `geq` | `>=` |

### Условия

3. **IF**: Используется для выполнения команд в зависимости от условий. Оператор `IF` не может быть вложен в другой оператор

**Синтаксис**:

```batch
IF [NOT] condition command
```

```batch
IF EXIST filename.txt echo File exists
IF NOT EXIST filename.txt echo File does not exist
IF %var%==value echo Variable equals value
```

4. **IF...ELSE Синтаксис**:

```batch
IF condition (
   command1
) ELSE (
   command2
)
```

```batch
IF %var%==value (
   echo Variable equals value
) ELSE (
   echo Variable does not equal value
)
```

5. **IF DEFINED**: Проверяет, определена ли переменная.

**Синтаксис**:

```batch
IF DEFINED var command
```

```batch
IF DEFINED myVar echo Variable is defined
```

### Циклы

6. **FOR синтаксис**:

```batch
FOR %%variable IN (set) DO command
```

```batch
FOR %%i IN (1 2 3) DO echo %%i
FOR %%f IN (*.txt) DO echo %%f
```

7. **`FOR /L`**: Цикл с шагом.

**Синтаксис**:

```batch
FOR /L %%variable IN (start,step,end) DO command
```

```batch
FOR /L %%i IN (1,1,10) DO echo %%i
```

8. **`FOR /R`**: Рекурсивный цикл по каталогам.

**Синтаксис**:

```batch
FOR /R [[drive:]path] %%variable IN (set) DO command
```

```batch
FOR /R C:\Users\ %%f IN (*.txt) DO echo %%f
```

9. **`FOR /F`**: Цикл для обработки строк из файла или команды.

**Синтаксис**:

```batch
FOR /F ["options"] %%variable IN (file|command|string) DO command
```

```batch
FOR /F "tokens=1,2 delims=," %%i IN (file.txt) DO echo %%i %%j
FOR /F "usebackq" %%i IN (`command`) DO echo %%i
```

### Встроенные команды

10. **`ECHO`**: Вывод текста на экран.

```batch
ECHO [message]
ECHO Hello, World!
```

11. **`SET`**: Установка и отображение переменных окружения.
  
  **Синтаксис**:

```batch
SET variable=value
SET /P variable=Prompt text
```

```batch
SET myVar=Hello
SET /P myVar=Enter value:
```

12. **`PAUSE`**: Приостановка выполнения скрипта до нажатия клавиши.

13. **`GOTO`**: Переход к метке в скрипте.
   - **Синтаксис**:

```batch
GOTO label
```

```batch
GOTO end
:end
ECHO End of script
```

14. **`CALL`**: Вызов другого batch-файла или метки

- **Синтаксис**:

```batch
CALL batchfile [arguments]
CALL :label [arguments]
```

```batch
CALL otherbatch.bat
CALL :subroutine
```

15. **`EXIT`**: Завершение выполнения скрипта.

- **Синтаксис**:

```batch
EXIT [/B] [exitCode]
```

```batch
EXIT /B 0
```

#OS #OS/Windows #CMD #Batch