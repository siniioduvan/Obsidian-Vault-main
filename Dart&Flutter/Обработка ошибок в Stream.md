#Dart #Flutter #stream 
Выше мы рассмотрели принцип работы `Stream` в Dart, вы уже могли заметить, что он сильно отличается от асинхронных функций и `Isolate`. Так и с ошибками, обычный `try/catch` не отловит все исключения.

Любая ошибка в потоке `Stream` приведет к тому, что подписчик не получит событие и не обработает его в `onData`. А все необработанные исключения считаются unhandled и попадают в обработчик `зоны`.

Исключение может подстерегать нас в нескольких случаях:

- Один из методов-модификаторов выбрасывает исключение. В этом случае можно воспользоваться:
    1. `try/catch` внутри функции;
    2. `handleError()` ниже по `Stream`;
    3. Коллбек `onError` в `StreamController` и в `StreamSubscription`.

Ошибка долетает в порядке выше, т.е. если `handleError()` поймал где-то ошибку, то `StreamSubscrtiption` ее не получит.

- В `Stream` появилось событие ошибки. Кто-то явно его добавил через `addError()` . В этом случае:

1. `handleError()` ниже по `Stream`;

```dart
Stream.periodic(const Duration(seconds: 1), (count) {
	if (count == 2) {
		throw Exception('Exceptional event');
	}
	return count;
}).take(4).handleError(print).forEach(print);
```

1. Коллбек `onError` в `StreamController` и в `StreamSubscription`. Ошибка долетает в порядке выше, т.е. если `handleError()` поймал где-то ошибку, то `StreamSubscrtiption` ее не получит.
    
    - Один из коллбеков `StreamSubscription` выбрасывает исключение. В этом случае только `try/catch`, в `onError`исключение не попадет.
