#Dart #Flutter #future 
Если во время работы `Future` возникнет ошибка, то `Event Loop`отправит ее в `catchError-коллбек`:
```dart
void main() {
	Future(() => print('Future print'))
		.then((_) => print('Print after future'))
		.catchError((error, stacktrace) => print(error));
}
```

У `then` есть тоже опциональный `onError-коллбек`, его приоритет «выше», чем у `catchError`:

```dart
void main() {
  Future(() => throw Exception())
      .then(
        (_) => print('Print after future'),
        onError: (error, stacktrace) => print('Then: $error'),
      )
      .catchError((error, stacktrace) => print('catchError: $error'));
}
```

А еще есть метод `onError()` под капотом он вызывает `catchError()`

Отличается тем, что:

1. Позволяет изменить структуру ошибки
2. Гарантирует, что в результате ошибка будет такого же типа, что и изначальная

Это может пригодиться, например, при обработке ошибки сетевого запроса.

Давайте представим, что сервер в ответ на запрос присылает различные коды ошибок в теле ответа, помимо стандартных кодов HTTP:

```dart
void main() {
	Future(() => performNetworkRequest())
		.onError((error, stackTrace){
			if(error.body.code == 1){
			//Еще один конструктор Future, немедленно отстрелит переданную ошибку через Event Loop
			return Future.error(CodeOneError());
		}
		return error;
	});
}
```

[[Обработка ошибки с помощью try-catch]]

