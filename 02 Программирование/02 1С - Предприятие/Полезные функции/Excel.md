## Имя колонки в номер колонки

```bsl
Функция ExcelColumnNameToNumber(имяКолонки)
    ЛатАлфавит = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    длинаНомера = СтрДлина(имяКолонки);
    
    номерКолонки = 0;
    
    Для счет = 1 по длинаНомера Цикл
        поз = Найти(ЛатАлфавит, Сред(имяКолонки, (длинаНомера + 1 - счет), 1));
        номерКолонки = номерКолонки + поз * Pow(26, счет - 1);
    КонецЦикла;
    
    Возврат номерКолонки;
КонецФункции
```

## Номер колонки в имя колонки

```bsl
Функция NumberToExcelColumnName(Знач номерКолонки)
    ЛатАлфавит = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    имяКолонки = "";
    
    Пока номерКолонки > 0 Цикл
    сстаток = (номерКолонки - 1) % 26;
    
    буква = Сред(ЛатАлфавит, остаток + 1, 1);
        имяКолонки = буква + имяКолонки;
        
        номерКолонки = Цел((номерКолонки - остаток) / 26);
    КонецЦикла;
    
    Возврат имяКолонки;
КонецФункции
```

#1С #1С/Полезные_функции #Utils