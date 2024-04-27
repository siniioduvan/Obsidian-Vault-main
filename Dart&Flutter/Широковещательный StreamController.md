#Dart #Flutter #stream 

`factory`-конструктор `StreamController.broadcast()` создаст `StreamController`, который отдает `Broadcast Stream`. Что это значит:

1. У `Stream` может быть больше одного подписчика.
2. Все события отправляются активным слушателям в момент вызова методов `add`, `addAll` или `addError` у объекта `Sink`.
3. У такого `StreamController` нет буфера, так что события могут быть пропущены.
4. `StreamController` должен быть закрыт, если его никто не слушает, чтобы избежать утечки ресурсов
5. `Stream` не должен порождать события, пока на него не подпишется кто-то.

Давайте рассмотрим конструктор подробнее и увидим несколько коллбеков:

1. `onListen` - cрабатывает, когда кто-то подписывается на `Stream`впервые.
2. `onCancel` - cрабатывает, когда `Stream` закрывается и больше его никто не слуашет.
3. `onResume` - cрабатывает, когда `Stream` уходит с паузы.
4. `onPause` - cрабатывает, когда `Stream` встает на паузу, события добавляются в буфер, но подписчик их не получает.

Последние два есть только у `Single-subscription`, поскольку буфер событий есть только у него.

По умолчанию каждый из конструкторов создает асинхронный `StreamController`. Это значит, что каждое событие проходит через `EventLoop` как `microtask`, что вызывает небольшую задержку.

У обоих конструкторов есть параметр `sync` , если он `true`, то мы получим синхронный `StreamController`, который стоит использовать с осторожностью.

Пример создания синхронных и асинхронных `StreamController`

```dart
// Асинхронные контроллеры
final asyncController = StreamController();
final asyncBroadcastController = StreamController.broadcast();

// Синхронные контроллеры
final syncController = StreamController(sync: true);
final syncBroadcastController = StreamController.broadcast(sync: true);
```
Давайте разберём подробнее синхронный контроллер.
[[SynchronousStreamController]]
