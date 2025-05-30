
Для работы с колекциями можно использовать два способа:

1) Операторы запросов LINQ
2) Методы расширений

## Операторы запросов

```cs
string[] people = { "Tom", "Bob", "Sam", "Tim", "Tomas", "Bill" };
 
// создаем новый список для результатов
var selectedPeople =
	from p in people //для каждого элемента p из people
	where p.ToUpper().StartsWith("T") //фильтрация по критерию
	orderby p  // упорядочиваем по возрастанию
	select p; // выбираем объект в создаваемую коллекцию
 
foreach (string person in selectedPeople)
    Console.WriteLine(person);
```

Синтаксис простейшего запроса:

```cs
from item in collection select item
```

Операция запроса перебирает все элементы коллекции и производит над ними некоторые операции, затем возвращает итоговый набор как новую коллекцию.

## Методы расширений

Для использования встроенных методов расширений — операторов запросов, необходимо подключить пространство имен `System.Linq`. Методы расширений LINQ, как правило, реализуют тот же функционал, что и операторы LINQ, такие как `where`, `orderby`.

```cs
var list = new List<string>
{
	"Первое",
	"Второе",
	"Третье",
	"Четвёртое",
	"Пятое",
};

var newList = list
	.Where(x => x.Contains("П"))  //Predicate
	.OrderByDescending(x => x)
	.ToList();
string Output = string.Join(Environment.NewLine, newList);
Console.WriteLine(Output);
```

Результат:

```
Пятое
Первое
```

Не каждый метод расширения имеет аналог оператора запроса LINQ.
Оба типа операторов можно совмещать

```cs
int number = (from p in people where p
			  .ToUpper()
			  .StartsWith("T") select p).Count();
Console.WriteLine(number); //3
```

## Встроенные методы расширений LINQ

| Метод                            | Описание                                                                               |
| -------------------------------- | -------------------------------------------------------------------------------------- |
| `Select(x => function(x))`       | меняет и возвращает элементы выборки                                                   |
| `Where(Func<T, bool> predicate)` | возвращает элементы выборки, удовлетворяющие указанному предикату                      |
| `OrderBy()`                      | сортирует выборку по возрастанию                                                       |
| `OrderByDescending()`            | сортирует по убыванию                                                                  |
| `ThenBy()`                       | задает дополнительные критерии для упорядочивания элементов возрастанию                |
| `ThenByDescending()`             | по убыванию                                                                            |
| `GroupBy()`                      | группирует по ключу\*                                                                  |
| `Join()`                         | соединяет 2 коллекции по определенному признаку                                        |
| `Reverse()`                      | обратный порядок элементов                                                             |
| `Distinct()`                     | выборка из неповторяющихся элементов                                                   |
| `Any(predicate)`                 | возвращает true, если выполнится условие для какого-либо элемента выборки              |
| `All(predicate)`                 | возвращает true, если каждый элемент удовлетворяет условию                             |
| `Contains()`                     | проверяет содержит ли коллекция указанный элемент (можно настроить механизм сравнения) |
| `First()`                        | первый элемент коллекции (генерирует исключение при остутсвии оного)                   |
| `FirstOrDefault()`               | первый элемент или значение по умолчанию (не генерирует исключение)                    |
| `Last()`                         | ...                                                                                    |
| `LastOrDefault()`                | ...                                                                                    |
| `Sum(\[Func\])`                  | посчитывает сумму числовых значений выборки                                            |
| `Average(\[Func\])`              | среднее                                                                                |
| `Min(\[Func\])`                  | минимальное зн-е                                                                       |
| `Max(\[Func\])`                  | ...                                                                                    |
| `Count(x => <condition>)`        | подсчитывает количество элементов, удовлетворяющих условию                             |
| `Skip(N)`                        | пропускает первые N элементов                                                          |
| `SkipLast(N)`                    | пропускает последние N элементов                                                       |
| `SkipWhile(predicate)`           | пропускает элемент, если он удовлетворяет условию                                      |
| `Take()`                         | по аналогии с Skip, но наоборот, выбирает какие элементы останутся в выборке.          |
| `SelectMany()`                   | рассматривает коллекцию коллекций как единую коллекцию                                 |

Далее: [[03 Проекция данных]]

#C-Sharp #C-Sharp/LINQ