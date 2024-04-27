#Dart #Flutter #future 

Выше мы сказали следующее:
***********
В Dart есть `Isolates` — аналоги потоков или нитей(`Threads`).
*************

Пришло время объяснить что это значит.

Изолят:

1. Это способ реализации псевдопараллелизма в Dart.
2. У него свой `Event Loop` и участок памяти.
3. Можно создавать сколько угодно изолятов.

Почему псевдопараллелизм и почему `Isolate` — это не совсем поток, поговорим в Advanced-главе.

Сейчас же остановимся на их пользе:

Как мы говорили выше в блоке про EventLoop, тяжёлые операции могут наглухо заблокировать работу Dart-кода. С помощью изолята мы можем выполнить какую-то сложную операцию «асинхронно», а затем обработать результат в основном изоляте.

Есть два простых способа создать изолят:

1. С помощью [`Isolate.run`](https://api.dart.dev/stable/2.19.4/dart-isolate/Isolate/run.html)
```dart
int slowFib(int n) =>
	n <= 1 ? 1 : slowFib(n - 1) + slowFib(n - 2);
Isolate.run(() => slowFib(40));
```

2. С помощью функции `compute`

```dart
Future<bool> isPrime(int value) {
	return compute(_calculate, value);
}
bool _calculate(int value) {
	if (value == 1) {
		return false;
	}
	for (int i = 2; i < value; ++i) {
		if (value % i == 0) {
			return false;
		}
	}
	return true;
}
```

Функция [compute](https://api.flutter.dev/flutter/foundation/compute-constant.html) за нас создаст изолят, выполнит код внутри коллбека, вернет результат и почистит изолят в конце.

