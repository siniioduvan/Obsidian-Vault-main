#Flutter #Dart #typedef
Это ключевое слово позволяет переименовать какой-то тип.
```dart
typedef JSON = Map<String, Object?>;

typedef Converter = String Function(int value); // Что это такое? см. Функции

String convertToString(int value, Converter converter) => converter?.call(value);

class MyModel {

final int value;

MyModel(this.value);

factory MyModel.fromJson(JSON json) => MyModel(json['value'] ?? 0);

}
```