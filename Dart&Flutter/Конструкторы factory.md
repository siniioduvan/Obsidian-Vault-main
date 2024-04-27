#Flutter #Dart #ООП 

Ещё одна отличительная черта Dart — в нём нельзя переопределять функции, методы и конструкторы.

Следующий код просто не скомпилируется:

```dart
int foo() {
	return 5;
}

int foo(int value) {
	return value * 2;
}
```

Данное правило не позволяет разработчику создать огромное количество нечитаемых классов с перегруженными методами вроде:

```dart
class Mathematic {
	int compute() {
		return 5;
	}

	int compute(int value) {
		return value * value;
	}

	int compute(int left, int right) {
		return left * right;
	}

	String compute(int left, int right, String operator) {
		return '$left$operator$right';
	}

}
```

Поскольку конструктор тоже метод, для него правило такое же. Чтобы его соблюсти, в языке есть конструкторы `factory`.

Давайте ниже рассмотрим, где может пригодиться этот инструмент.

- Можно определить специфический интерфейс для создания экземпляров:

```dart
class Logger {

	String? _tag;

	Logger(this._tag);

	factory Logger.withBeautifulTag(String tag) => Logger('[$tag]');

	factory Logger.withDoubleTag(String tag) => Logger('$tag$tag');

	void log(Object? message) {
		print("$_tag:$message");
	}
}
```

- Делегировать создание экземпляров подтипов конструкторам `factory` , что позволяет в определённой степени реализовать Union-тип.

*************************************************************
Union-тип — объединение нескольких типов под одним. В Dart нет полноценной поддержки Union-типов, но реализовать такую концепцию возможно и существующими средствами. Или вы можете воспользоваться сторонними пакетами — например, [Freezed](https://pub.dev/packages/freezed#union-types-and-sealed-classes).
*************************************************************

Пример Union-типа на конструкторах `factory`:

```dart
// Тип A — это Union Type

class A {
		// Экземпляр A создать нельзя
	A._();
	// Но мы можем вместо этого с помощью конструкторов класса A
	// создать экземпляры классов B и C
	factory A.withB(int bValue) = B;
	factory A.withC(String cValue) = C;

}

class B extends A {
	final int bValue;
	B(this.bValue) : super._();
}

class C extends A {
	final String cValue;
	C._(this.cValue) : super._();
	factory C(String value) => C._(value);

}
```

- Можно использовать ключевое слово `const` для таких конструкторов.

```dart
class A {
	const A();
		// const factory-конструктор A.asConstFactory() вызывает конструктор const B()
		const factory A.asConstFactory() = B;
}

class B extends A {
	const B() : super();
}
```

