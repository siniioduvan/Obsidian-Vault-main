#Dart #Flutter #future 

Осуществляется с помощью конструкции try/catch. Но есть нюанс:

******
Если результат асинхронной функции не ожидается, то в случае возникновения ошибки try/catch-блок её не отловит
*********

Почему так — поговорим подробно в advanced секции, а пока — пример.

```dart
void main() {
  badAsyncJob();
  goodAsyncJob();
}

badAsyncJob() async {
  try {
    print('Start badAsyncJob');
    throwAsyncJob();
  } catch (e, stacktrace) {
    print('Caught badAsyncJob: $e');
  }
}

goodAsyncJob() async {
  try {
    print('Start goodAsyncJob');

    await throwAsyncJob();
  } catch (e, stacktrace) {
    print('Caught goodAsyncJob: $e');
  }
}

throwAsyncJob() async {
  print('Start throwAsyncJob');
  throw Exception('Hello, i am async error');
}
```

Как видите, catch-блок функции `badAsyncJob()` не смог обработать исключение.

***********
Важно следить за ожиданием асинхронных операций, там где хотим отловить ошибки.
**************


