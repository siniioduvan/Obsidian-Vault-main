#Flutter #Dart 

### final

Ключевое слово `final` запрещает переопределение переменной после инициализации.

**Важно.** Переменная, помеченная `final`, — не константа, просто её значение нельзя переопределить после инициализации.

Давайте создадим переменную `final` и попытаемся изменить её значение.
```dart
void main() {
  final a;

  a = 5;

  print('Integer variable: $a');

  a = 6;
  print('Integer variable 2: $a');

  a = 'Some string';
  print('String variable: $a');
}
```