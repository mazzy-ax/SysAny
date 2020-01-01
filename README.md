# Any

[project]:https://github.com/mazzy-ax/SysAny
[license]:https://github.com/mazzy-ax/SysAny/blob/master/LICENSE

[Any][project] &ndash; это класс Any на языке X++ для работы со значениями любого типа (AnyType) в [Microsoft Dynamics AX 2009](ax2009), [Microsoft Dynamics AX 2012](ax2012) и [Axapta 4.0](ax4).

## Проблемы имеющихся `Anytype` и `SysAnyType`

Аксапта позволяет установить произвольное значение в переменную типа `Anytype` только один раз.
Во время первой инициализации тип переменной фиксируется. В дальнейшем можно установить новое значение такого же типа, но тип изменить уже нельзя.

```java
Anytype var = 1;
var = 2;         // ok
var = '';        // результат не определен.
```

Чтобы обойти эту проблему в Аксапту ввели класс `SysAnyType`, который действительно позволяет в любой момент установить любое значение. Но ради этого класс сделали очень тяжелым &mdash; в куче хранится `map`, `key` и сам `anytype`-объект в качестве `value`.
Кроме того, каждое обращение к `value` &mdash; это `lookup` внутри `map`.

```java
SysAnyType var = new SysAnyType(1); // хранит число 1
var.value('');                      // меняет тип и хранит пустую строку
```

## Класс `Any`

Представленный в данном проекте класс `Any` предлагает более легкую реализацию, чем `SysAnyType`,
при этом программист может работать со значениями разного типа.

Сам класс не позволяет изменять значение хранимого внутри объекта.
Поэтому данный класс может просто хранить простой ref на объект произвольного типа.

Но! можно просто пересоздать объект `Any`:

```java
Any var = new Any(1);        // ok
var = new Any('');           // ok
```

У Any есть несколько специализированных конструкторов и обычный construct:

```java
Any var = Any::constuct(1);  // ok
var = Any::constuct('');     // ok
```

Класс проявляется в цикле (обратите внимание на специализированный конструктор conpeek):

```java
container con = [1, '', 31\01\2019];
Any var;
for(i=1; i<=conlen(con); ++i)
{
    var = Any::conpeek(con, i);
}
```

## Отличия для ax4 и ax2009, 2012

* В `ax4` нет типа `utcDateTime`
* В `ax4` тип `DateTime` соответствует типу `Time` в `ax2009`, `ax2012`

## ChangeLog

* [CHANGELOG.md](CHANGELOG.md)
* <https://github.com/mazzy-ax/SysAny/releases>

## Помощь проекту

Буду признателен за ваши замечания, предложения и советы по проекту как в разделе [Issues](https://github.com/mazzy-ax/SysAny/issues), так и в виде письма на адрес <mazzy@mazzy.ru>

Мазуркин Сергей (mazzy)
