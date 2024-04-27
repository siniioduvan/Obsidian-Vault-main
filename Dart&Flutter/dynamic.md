#Flutter #Dart 
В Dart есть ключевое слово `dynamic`, отключающее проверки типов для переменной. Им стоит пользоваться, если тип переменной не известен до запуска программы и его нельзя вывести. Давайте заменим `var` из предыдущего примера на `dynamic` и слегка модифицируем код:
```dart
void main() {
  dynamic helloString = 'Hello,world!';
  print(helloString);

  // Ошибки теперь не будет
  helloString = 4;
  print(helloString);

  //А теперь запишем в переменную строчку и попробуем использовать с ней математическую операцию
  helloString = 'I am string';
  print(helloString / 4);
}
```