## Строга типізація без типів

(Оригінал цієї статті знаходиться [тут](https://www.yegor256.com/2020/11/10/typing-without-types.html).)

10 листопада 2020 р. Москва, Росія.

Автор: [Єгор Бугаєнко](https://www.yegor256.com).

В 1974 році Лісков (Liskov) і Зилес (Zilles) [дали визначення](https://dl.acm.org/doi/abs/10.1145/942572.807045) мови зі [строгою типізацією](https://en.wikipedia.org/wiki/Strong_and_weak_typing) як такої, в якій "кожного разу, коли об'єкт передається від викликаючої функції до викликуваної, його тип мусить бути сумісним із типом, оголошеним у викликуваній функції". Строга [перевірка типів](https://en.wikipedia.org/wiki/Type_system), поза сумнівом, зменшує кількість [помилок, пов'язаних із типами](https://en.wikipedia.org/wiki/Type_system#Type_errors), а це підвищує якість. Однак є питання: чи нам потрібно вказувати типи, щоб отримати строгу типізацію?

![Redirected](/redirected.jpg)
(Image: :copyright: Redirected (2014) by Emilis Velyvis)

Наприклад, тут ми очікуємо, що нам прилетить примірник Java-[інтерфейсу](https://docs.oracle.com/javase/tutorial/java/concepts/interface.html) `Book`:

```java
void print(Book b) {
    System.out.printf(
        "The ISBN is: %s%n", b.isbn()
    );
}
```

Тип `Book` може виглядати так:

```java
interface Book {
  String isbn();
}
```

Якщо об'єкт, який не імплементує інтерфейсу `Book`, передається в метод `print()`, компілятор викине помилку про неузгодження типів (type mismatch). Програмісту важко зробити помилку і передати об'єкт типу, скажімо, `Car` в метод `print()`. І все-таки це можливо при використанні динамічного приведення типів:

```java
Car car = new Car("Mercedes-Benz G63");
print(Book.class.cast(car)); // Отут!
```

Цей код компілюється без помилок, але в час виконання ми отримаємо [`ClassCastException`](https://docs.oracle.com/javase/7/docs/api/java/lang/ClassCastException.html), бо неможливо привести `Car` до типу `Book`.

сь що я їм відписую:

> _Дякую за ваше повідомлення. Мене воно дійсно зацікавило. Я не проти співбесіди. Але є одна умова: співбесіду має проводити той, на кого я буду працювати. Мій майбутній безпосередній менеджер._
