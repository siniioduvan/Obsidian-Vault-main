#Dart #Flutter 

`Event Loop` — это механизм обработки событий, который позволяет писать неблокирующий код. В этом блоке пойдёт много теории, но ее стоит понять, потому что все инструменты, рассматриваемые в статье, строятся на приниципах работы `Event Loop`.

Его принцип работы можно представить в виде следующей схемы:
![fluttern_1.2.3_2.svg](https://yastatic.net/s3/ml-handbook/admin/fluttern_1_2_3_2_d139e9e9a9.svg)

Или, если кому-то удобнее читать код, то вот пример:

```dart
bool programWorks = false;

void main(){
	startEventLoop();
	// Ваша программа
	// ...
}

void startEventLoop(){
		programWorks = true;
		while(programWorks){
			eventLoopIteration();
		}
}

void stopEventLoop(){
	programWorks = false;
		while(programWorks && !eventLoopQueueIsEmpty()){
			eventLoopIteration();
	}
disposeProgram();
}
```

За **итерацию** EventLoop делает примерно следующее:

```dart
void eventLoopIteration() {
	if (microtaskQueue.isNotEmpty()) {
		fetchFirstMicroTaskFromQueue();
		executeThisMicroTask();
		return;
	}

	if (eventQueue.isNotEmpty()) {
		fetchFirstEventFromQueue();
		handleThisEvent();
	}
}
```

1. Программа запускается
2. `EventLoop` разбирает всю очередь микрозадач (Microtask Queue) — в FIFO-порядке.
3. Затем переходит к очереди событий (Event Queue) и обрабатывает следующее событие из очереди.
4. Затем цикл переходит к началу — п.2
5. Как только очередь событий опустеет, программа может завершать работу

Выше мы узнали два новых понятия: `Microtask` и `Event`. Напрямую при написании кода мы с ними не взаимодействуем, так что давайте разберёмся, какие наши действия порождают [[Event]] и [[Microtask]].

