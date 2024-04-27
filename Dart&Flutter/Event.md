#Dart  #Flutter 
Это какое-то внешнее событие, которое попадает в `Event Queue` и обрабатывается `Event Loop`.

![fluttern_1.2.3_1.svg](https://yastatic.net/s3/ml-handbook/admin/fluttern_1_2_3_1_92f0df9ee8.svg)

Например:

- операции ввода/вывода
- жесты
- рисование
- таймеры
- ...
- [[Future]]


Напрямую добавляеть новые события в `Event Queue` мы не можем, исключение — `Future API`. О них мы и поговорим ниже.