#Dart #Flutter #stream

Концепция реактивного или декларативного программирования строится на асинхронных последовательностях событий.

Суть концепции может представлена в виде подобной схемы:

![fluttern_1.2.3_4.svg](https://yastatic.net/s3/ml-handbook/admin/fluttern_1_2_3_4_ec3927bcf1.svg)

Есть некий генератор асинхронных событий, и есть подписчики, слушающие эти события.

В Dart такой последовательностью называется `Stream` . Создать `Stream` можно можно несколькими способами:

1. С помощью конструктора.
    
```dart
final stream = Stream.fromIterable([1, 2, 3, 4, 5]);
```
    
    Про другие конструкторы можно почитать в [документации](https://api.dart.dev/stable/2.19.4/dart-async/Stream-class.html)
    
2. С помощью асинхронного генератора. Его принцип работы такой же как и обычного генератора, только он возвращает `Stream`, а не `Iterable`

```dart
Stream<int> generateStream() async* {
	int count = 1;
	while(count <= 5){
		yield count++;
	}
}
```

3. С помощью `StreamController`. На этом задержимся подольше ниже, а пока пример простейшего `Stream`, созданного `StreamController`:

```dart
class StreamProvider {
	final _controller = StreamController<int>();
	Stream get stream => _controller.stream;
	void startPushingEvents() {
		for (int i = 0; i < 20; i++) {
			_controller.sink.add(i);
		}
	}
	void stopPushingEvents() {
		_controller.close();
	}
}
```

В примере выше `StreamProvider` предоставляет `stream`, который отдает последовательность чисел от 1 до 19. Помимо `stream`появляется `Sink` и необходимость закрывать `controller`. Зачем это необходимо, давайте разберемся ниже.

[[StreamController]]
[[Single-subscription StreamController]]
[[Широковещательный StreamController]]
[[SynchronousStreamController]]
[[Отправка событий в StreamController]]
[[Обработка событий Stream]]
[[Обработка ошибок в Stream]]



