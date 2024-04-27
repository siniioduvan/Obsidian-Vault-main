#Dart #Flutter #future 

Для этого нужно зарегистрировать `then-коллбек`. Он будет вызван сразу после выполнения функции внутри Future.

```dart
void main() {
	Future(() {
		print('Future print');
	}).then((_) => print('Print after future'));
}