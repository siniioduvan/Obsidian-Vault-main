#Flutter #Dart 
Ключевое слово `late` позволяет объявить переменную и не инициализировать её значение сразу.

Это полезно, когда хотим объявить переменную `non-nullable` (о `null-safety` поговорим ниже), а значение ей задать позже.
```dart
late String name;

void main(){
	name = 'Dart';
	print('Hello, $name');
}
```