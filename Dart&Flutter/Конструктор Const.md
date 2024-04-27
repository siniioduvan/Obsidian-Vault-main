#Flutter #Dart #ООП 

В предыдущем параграфе мы научились создавать примитивные константы, но Dart позволяет создавать константные объекты с помощью конструктора `const`.

Давайте пробовать:
```dart
void main() {
  const printer = Printer('Hello, World!');//тут будет ошибка!
}

class Printer {
  final String _valueToPrint;

  Printer(this._valueToPrint);

  void printValue() {
    print('Printing: $_valueToPrint');
  }
}
```

Запускаем и видим на этапе компиляции ошибку: `Cannot invoke a non-'const' constructor where a const expression is expected.`

Что она значит? На самом деле из неё мы можем почерпнуть два важных замечания:

1. в переменную `const` можно записать только «константное» выражение, то есть мы не сможем присвоить ей результат выполнения функции;
2. в переменную `const` можно записать только экземпляр класса, созданный с помощью конструктора`const`.

Давайте по порядку.

Для начала рассмотрим пример, объясняющий первый пункт. Здесь мы пытаемся записать в константную переменную результат вычисления функции, который будет известен только после запуска программы, даже если кажется, что результат будет всегда один:

```dart
void main() {
  const printer = getInteger();
}

int getInteger() => 1;
```

Получаем ошибку: `Const variables must be initialized with a constant value.`

Она появляется, потому что в константные переменные нельзя записать значения, которые не известны до начала выполнения программы.

Второе выражение нам говорит о необходимости использовать ключевое слово `const` при создании объекта.

Давайте пробовать:

```dart
void main() {
  const printer = const Printer('some String value');
}

class Printer {
  String valueToPrint;

  Printer(this.valueToPrint);

  void printValue() {
    print('Printing: $valueToPrint');
  }
}
```

Ошибка никуда не ушла, потому что у класса `Printer` нет конструктора `const`.

Пробуем его добавить. Синтаксис данного типа конструктора отличается от обычного только наличием `const` в начале.

```dart
void main() {
  const printer = const Printer('some String value');
}



class Printer{
  String valueToPrint;
  
  const Printer(this.valueToPrint);
  
  void printValue(){
    print('Printing: $valueToPrint');
  }
}
```

Все условия выполнены: есть конструктор `const` и создаётся константный экземпляр. Однако возникает новая ошибка: `Constructor is marked 'const' so all fields must be final.`

**************************************************************
Важно понимать, что в константную переменную нельзя записать значение, которое неизвестно до запуска программы или которое возможно изменить при исполнении программы.
**************************************************************

Чтобы решить последнюю проблему, мы должны изменить код и затем проверить его.

```dart
void main() {
  const printer = const Printer('some String value');
  printer.printValue();
}

class Printer {
  final String _valueToPrint;

  const Printer(this._valueToPrint);

  void printValue() {
    print('Printing: $_valueToPrint');
  }
}
```

И наконец мы добиваемся ожидаемого результата.