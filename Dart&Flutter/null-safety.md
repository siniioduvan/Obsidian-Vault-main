#Flutter #Dart #null
Dart поддерживает `sound null-safety` как необязательную функцию на версиях с 2.12 по 2.19 и как обязательную часть с версии 3. Это значит, что разработчик защищен от `NullReferenceException`, так называемой «[ошибки на миллиард долларов](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare/)».

Подробнее о реализации `null-safety` в Dart можно прочитать в [документации](https://dart.dev/null-safety).

Мы же остановимся на основах.

С включенным `null-safety` в Dart все переменные не могут иметь значение `null`, если об этом не сказано явно с помощью `?`:
```dart
String nonNullVariable = "Hello, world!";

// При попытке присвоить null будет ошибка

nonNullVariable = null;

String? nullableVariable = "Hello, world!";

// Теперь ничего не мешает присвоить null

nullableVariable = null;
```
Сам компилятор постоянно следит за разработчиком и подсвечивает потенциально опасные места:
```dart
void main() {
  // Пусть у нас будет List, с nullable элементами
  final myList = <MyClass?>[MyClass()];

  // Получаем ошибку на этапе компиляции
  final value = myList.first.foo();
}

class MyClass {
  int foo() {
    return 5;
  }
}
```
Код просто не скомпилируется, если компилятор увидит потенциально опасный вызов.

Выходов из ситуации несколько:

1. [[Оператор `?` или null-aware]]
2. [[Оператор `!` или force unwrap]]
3. [[Вычисление типа компилятором]]
