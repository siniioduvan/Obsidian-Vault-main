#Flutter #Dart #ООП #Наследование

Ключевое слово `extends` позволяет расширить поведение класса через создание подкласса-наследника.

При этом наследник может переопределять части интерфейса родителя и обязан задать свою реализацию для каждой нереализованной части.

Рассмотрим пример с наследованием от абстрактного класса:

```dart
void main() {
  final myObject = MyClass('String value');

  myObject.foo();
  myObject.bar();
  myObject.baz();
  myObject.myPrint();
}

abstract class MyAbstractClass {
  final String someValue;

  MyAbstractClass(this.someValue);
  
  void foo() {
    print('MyAbstractClass foo: $someValue');
  }

  void bar();

  void baz() {
    print('MyAbstractClass baz: $someValue');
  }
}

class MyClass extends MyAbstractClass {
  MyClass(super.someValue);

  @override
  void bar() {
    print('MyClass bar: $someValue');
  }

  @override
  void baz() {
    print('MyClass baz: $someValue');
  }

  void myPrint() {
    print('My class print: $someValue');
  }
}
```

Данный пример работает и для расширения обычных классов, за исключением обязательной реализации абстрактных частей интерфейса — вы не сможете сделать обычный класс с нереализованными методами.

Давайте зафиксируем, что мы можем извлечь из этого примера:

1. подкласс наследует поведение суперкласса;
2. подкласс может переопределить поведение суперкласса;
3. если у суперкласса есть конструктор, подкласс должен иметь конструктор, который вызывает суперконструктор родителя;
4. интерфейс переменной определяет её тип, а реализацию определяет класс, который создаём.

Попробуем поменять тип переменной:

```dart
void main() {
		// Теперь тип myObject - MyAbstractClass
	final MyAbstractClass myObject = MyClass('String value');
	myObject.foo();
	myObject.bar();
	myObject.baz();
	// IDE и компилятор подсветят здесь ошибку, потому что у MyAbstracClass нет метода myPrint()
	myObject.myPrint();
}
```


