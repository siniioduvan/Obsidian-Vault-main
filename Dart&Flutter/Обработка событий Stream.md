#Dart #Flutter #stream 

Начать обработку ивентов можно двумя способами:

1. Метод `listen()`
2. Асинхронный цикл `for` (`await for`)

Рассмотрим `await for`.

По сравнению с `for in` циклом у него три отличия:

1. Перед `for` нужно добавить `await`
2. Итерироваться он может только по событиям `Stream`
3. Использовать его можно только в асинхронной функции

Для лучшего понимания давайте решим задачу:

Нам нужно считывать весь контент файла и складывать его в одну строку.

Для этого напишем две функции, которые будут складывать строковый массив в полноценную строку для итерируемых коллекций(`Iterable`) и для `Stream`:

```dart
String concatStrings(Iterable<String> collection) {
	final stringBuffer = StringBuffer();
	for (final string in collection) {
		stringBuffer.write(string);
	}
	return stringBuffer.toString();
}

Future<String> conctatStreams(Stream<String> stream) async* {
	final stringBuffer = StringBuffer();
	await for (final string in stream) {
		stringBuffer.write(string);
}
	return stringBuffer.toString();
}
```

Так, мы получили два решения одной и той же задачи, но с помощью `conctatStreams()` мы можем асинхронно обрабатывать контент файла и не блокировать исполнение дальнейшего кода.
***********
Заметим, что события ошибки `await for`обрабатывать не умеет. В случае оной цикл просто прекратит исполнение.
**********

Теперь рассмотрим метод `listen()`

Этот метод позволяет создать подписчика у Stream в любом месте.
**************
Напоминаем, что для single-subscription `Stream`нельзя создать больше одного подписчика. Компилятор за этим не следит, так что вы увидите исключение только во время работы приложения.
****************

Давайте посмотрим на пример кода с вызовом метода `listen()`

```dart
StreamSubscription<T> createStreamSubcription<T>(Stream<T> stream) {
	final streamSubscription = stream.listen(
		_handleStreamEvent,
		onError: _handleOnError,
		onDone: _handleOnDone,
		cancelOnError: true,
	);
	return streamSubscription;
}

void _handleStreamEvent<T>(T event) {
	print('Got new event $event');
}

void _handleOnError(Object? error, StackTrace? stackTrace) {
	print('Got stream error');
}

void _handleOnDone() {
	print('Stream subscription closed');
}
```

Первое, что мы видим — `listen()` возвращает объект [`StreamSubscription`](https://api.dart.dev/stable/3.0.7/dart-async/StreamSubscription-class.html). Это и есть подписка. Подписку можно поставить на паузу(`pause()`), восстановить(`resumse()`) и закрыть(`cancel()`).

Если флаг `cancelOnError == true`, тогда подписка закроется после первого события ошибки.

***************
Будьте внимательны с `cancel()` — закрытую подписку уже нельзя восстановить, а `Stream` сам за вас ее не закроет. Если забыть про это, то события могут протечь в неожиданных для вас сценариях.
******************
В момент подписки вы можете определить несколько callback:

1. `onData` — будет вызван каждый раз, когда `StreamSubscription`обрабатывает событие `Stream`.
2. `onError` — будет вызван при появлении ошибки в `Stream`. Если не определить, все ошибки будут обработаны как `unhandled` и отправятся в обработчик текущей `зоны` (о том что такое зоны мы будем говорить в параграфах ниже)
3. `onDone` — будет вызван, если `Stream` закроется и отправит об этом событие. Т.е. завершится без ошибок. Если не определить, ничего не произойдет.
***************
Мы рассмотрели только основные способы обработки событий, в библиотеке [dart:async](https://api.dart.dev/stable/3.0.7/dart-async/dart-async-library.html) есть еще больше [методов-модификаторов](https://dart.dev/tutorials/language/streams#modify-stream-methods), которые строятся на всем вышесказанном. А для более сложных сценариев используйте пакет [rxdart](https://pub.dev/packages/rxdart).
*****************
