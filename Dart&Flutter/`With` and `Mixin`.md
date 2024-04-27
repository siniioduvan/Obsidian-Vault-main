#Flutter #Dart #ООП 

Выше мы сказали, что у класса не может быть больше одного родителя или суперкласса. Но ключевое слово `with` позволяет расширять функционал класса больше чем одним классом.

Команда Dart старается по максимуму защитить разработчика от совершения ошибок — такая схема спасает от проблем множественного наследования. Например, от [ромбовидного наследования](https://ru.wikipedia.org/wiki/%D0%A0%D0%BE%D0%BC%D0%B1%D0%BE%D0%B2%D0%B8%D0%B4%D0%BD%D0%BE%D0%B5_%D0%BD%D0%B0%D1%81%D0%BB%D0%B5%D0%B4%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5).

`Mixin` — любой класс без конструктора и не наследующийся от других классов.

Вот это `Mixin`:

```dart
class MyMixin {
	void foo() {
		print('Mixed foo');
	}
}
```

И это `Mixin`:

```dart
abstract class MyAbstractMixin {
	void bar();
}
```

Их можно подмешать в новый класс с помощью `with`:

```dart
class MyMixin {
	void foo() {
		print('Mixed foo');
	}
}

abstract class MyAbstractMixin {
	void bar();
}

class A with MyMixin, MyAbstractMixin {
	@override
		/// bar обязаны реализовать, потому что это абстрактный метод
	void bar() {
		// TODO: implement bar
	}
}
```

А теперь `MyMixin` уже не `Mixin`, а обычный класс:

```dart
class B {}

class MyMixin extends B {
	void foo() {
		print('Mixed foo');
	}
}
```

`MyMixin` уже подмешать не выйдет, так что ситуация с ромбовидным наследованием через примеси тоже исключена:

![fluttern_1.2.2_1.svg](https://yastatic.net/s3/ml-handbook/admin/fluttern_1_2_2_1_437289f0eb.svg)

Поведение примеси не может быть расширено, но реализовывать другие интерфейсы `Mixin` может:

```dart
class MyMixin implements MyAbstractMixin {
	void foo() {
		print('Mixed foo');
	}
	
	@override
		// Мы обязаны реализовать абстрактный bar, т. к. MyMixin — это обычный класс
	void bar() {
		// TODO: implement bar
	}
}

// У интерфейса класса A появились два метода — foo(), bar()
// А реализацию он отнаследовал от примеси MyMixin
class A with MyMixin {
}
```

В примерах выше `MyMixin` можно по-прежнему использовать как обычный класс. Так, мы можем создавать его экземпляры и расширять.

Но задача `Mixin` — расширять поведение **без** создания новых классов. На помощь приходит ключевое слово `mixin`:

```dart
mixin MyMixin implements MyAbstractMixin {
	void foo() {
		print('Mixed foo');
	}
// Реализовывать абстрактный bar уже не обязательно, потому что экземпляр mixin не может быть создан.
// Следовательно, и гарантировать реализацию всех членов интерфейса не обязательно
}

// У интерфейса класса A появились два метода — foo(), bar()
// А реализацию он отнаследовал от примеси MyMixin
class A with MyMixin {
@override
	void bar() {
		// TODO: implement bar
	}
}
```

А ещё мы можем ограничить спектр классов, в которые `Mixin` можно подмешать с помощью ключевого слова `on`.

Оно разрешает расширять наследников суперкласса с помощью `Mixin`.

Чтобы лучше разобраться в этом, рассмотрим схему:
![fluttern_1.2.2_2.svg](https://yastatic.net/s3/ml-handbook/admin/fluttern_1_2_2_2_f16555005a.svg)

```dart
class Super {

	final int a;

	Super(this.a);

	void foo() {
		print('Super foo');
	}
}

mixin MyMixin on Super {
	void baz() {
		print('Mixed baz');
	}
}

// Класс А — наследник Super и подмешивает Mixin. MyMixin — тоже наследник Super.
class A extends Super with MyMixin {
	A(super.a);
}

// А вот уже в класс B подмешать MyMixin не получится.
class B with MyMixin {

}
```

В примере выше мы можем переопределить `foo()` внутри `MyMixin`, тогда становится не ясно, какую реализацию в итоге выберет язык.

Ещё непонятнее становится при следующей схеме наследования:
![fluttern_1.2.2_3.svg](https://yastatic.net/s3/ml-handbook/admin/fluttern_1_2_2_3_aae3527e0a.svg)

Если в расширяемых классах встречаются разные реализации одного и того же метода, Dart считает верной ту, что была объявлена последней, — будь то переопределение в самом классе или последний подмешанный `Mixin`.

Теперь, когда мы познакомились со всем инструментарием ООП В Dart, попробуйте решить несколько задач.

Попробуйте угадать, что вызовет следующий код:
```dart
class P {
  String getMessage() => 'P';
}

mixin A {
  String getMessage() => 'A';
}

mixin B {
  String getMessage() => 'B';
}

class AB extends P with A, B {}

class BA extends P with B, A {}

void main() {
  String message = '';
  
  AB ab = AB();
  
  message += ab.getMessage();
  
  BA ba = BA();
  
  message += ba.getMessage();
  
  print(message);
}
```
***********
Ответ
ВА
***********

Типы работают так же, как и при наследовании классов.

Попробуйте понять, что выведет следующий код:

```dart
class P {
  String getMessage() => 'P';
}

mixin A {
  String getMessage() => 'A';
}

mixin B {
  String getMessage() => 'B';
}

class AB extends P with A, B {}

class BA extends P with B, A {}

void main() {
  AB ab = AB();
  print(ab is P);
  print(ab is A);
  print(ab is B);

  BA ba = BA();
  print(ba is P);
  print(ba is A);
  print(ba is B);
}
```

***********
Ответ
6 true
**************

