#Flutter #Dart 

Dart поддерживает вывод типов (type-inference), так что указывать тип переменной сразу необязательно. На помощь приходит ключевое слово **`var`:**
```dart
void main() {
  var helloString = 'Hello, world!';

  print(helloString);

  //Код ниже выбросит compile time исключение, потому что на строке 2 для переменной был вычислен тип String
  helloString = 4;
  print(helloString);
}
```