#Flutter #Dart #null 
В опасных местах можно пользоваться null-aware. Он буквально говорит компилятору: «Если переменная не `null`, то вызывай код, иначе — верни `null`».

```dart
final value = myClass?.foo();
```

Компилятор ругаться перестанет, и мы получим следующее:

Тип `value` — `int?`.

`myClass` не null — `value == 5`.

`myClass` не присвоено или null — `value == null`