#Flutter #Dart 
Enhanced enum — класс со своими полями, методами и конструкторами, но с определёнными ограничениями:

- Поля класса должны быть `final`, включая те, что подмешаны через `with`;
- Все конструкторы должны быть константными;
- Все `factory`-конструкторы могут вернуть только один из объявленных экземпляров `Enum`;
- Нельзя переопределить `index`, `hashCode` или оператор равенства `==`;
- Нельзя использовать имя `values` для членов класса, потому что это конфликтует с геттером `values` из API стандартных `enum`;
- В начале `enum` нужно объявить все возможные значения. При этом в целом значений должно быть не менее одного.

Enhanced enums могут пригодиться во многих случаях. К примеру, при работе с иконками:
```dart
enum MyIcon {
	close(path: 'assets/close', width: 200),
	car(path: 'assets/car', width: 300),
	driver(path: 'assets/driver', width: 300);

	const MyIcon({
		required this.path,
		required this.width,
	});

	factory MyIcon.withName(String name) =>
		values.firstWhere((v) => v.name.contains(name), orElse: () => close);


	final String path;
	final double width;


	String getThemedPath(bool isDarkTheme) =>
		isDarkTheme ? '${path}_nigh.webp' : '$path.webp';
}
```