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

Дисклеймер: Я справді розумію, що це багатомільярдні компанії, найкращі в індустрії, і я ніщо в порівнянні з ними. Я розумію, що їхні рекрутери не переймаються моїми відповідями — вони просто тиснуть "видалити" і йдуть далі. Я також розумію, що вони ніколи не побачать цього посту, і ця стаття може нічого й не змінити. Однак я мав її написати.

Ось що я їм відписую:

> _Дякую за ваше повідомлення. Мене воно дійсно зацікавило. Я не проти співбесіди. Але є одна умова: співбесіду має проводити той, на кого я буду працювати. Мій майбутній безпосередній менеджер._

Рекрутер, отримавши цю відповідь, ніколи більше до мене не звертається.

Чому я це посилаю?

Що ж, через урок, який я отримав два роки тому, коли мене спробував найняти Amazon. Я отримав повідомлення від компанії, що вони дуже вражені моїм профілем і не можуть дочекатися на співпрацю зі мною. Їм був потрібен я, більше ніхто. Я був наївний, повідомлення справді мені лестило.

Ми спланували співбесіду в штаб-квартирі в Сіетлі. Вони оплатили мій квиток на літак (з Сан-Франциско) і ніч у п'ятизірковому готелі. Я був вражений. Вони справді були зацікавлені. Як і я.

> **Вони оплатили мій квиток на літак і ніч у п'ятизірковому готелі. Я був вражений.**

Те, що відбулося на співбесіді, напевно, дуже нагадувало [історію](https://twitter.com/mxcl/status/608682016205344768) Макса Хауела [з Google'ом](https://news.ycombinator.com/item?id=9695102): якісь програмісти, які нічогісінько не знали про мій профіль, просили мене винайти якісь алгоритми на інтерактивній дошці протягом майже чотирьох годин. Чи я дав собі раду? Не думаю. Чи вони зробили мені офер? Ні.

Чого я навчився?

Це було марнування часу. Для обох сторін.

Їхня бюрократична машина спроєктована для обробки сотень кандидатів за місяць. Щоб їх надибати і заманити, працює армія ~~мавп~~ [рекрутерів](https://www.yegor256.com/2015/09/29/mayonnaise.html), які розсилають теплі повідомлення до таких людей, як я. Вони мають якось перевіряти кандидатів, але вони надто ледачі, щоб зробити цей процес ефективним і творчим. Вони просто проганяють їх крізь випадкових програмістів, котрі повинні ставити їм якнайскладніші запитання.

Я не кажу, що люди, які складуть їхній іспит, не є гарними програмістами. І не кажу, що я гарний програміст — гляньмо правді у вічі, я ж не склав тесту. Вірю, що ця система відбору є непоганою. Але я хочу сказати, що вона суперечить початковому повідомленню, яке я отримав від рекрутерки.

Якби вона почала повідомлення так: "Ми шукаємо експерта з алгоритмів", ми б далі не пішли й не змарнували б нашого часу. Ясен світ, я не експерт з алгоритмів. Нема змісту питати мене про проходження двійкових дерев; я не знаю відповідей, і мені ніколи не буде цікаво їх дізнатися. Я намагаюся бути експертом у дечому іншому, скажімо, в [об'єктно-орієнтованому дизайні](https://www.yegor256.com/2016/11/29/eolang.html).

Була очевидна розбіжність між моїм профілем і сподіваннями співбесідників. Я не звинувачую їх і не звинувачую її. Вони ж усі просто ~~раби~~ [підлеглі](https://www.yegor256.com/2015/10/06/how-to-be-good-office-slave.html). Я звинувачую себе, що не прояснив цього з ними наперед.

> **Нема змісту питати мене про проходження двійкових дерев; я не знаю відповідей, і мені ніколи не буде цікаво їх дізнатися.**

Я мав би їй сказати, що не хочу, щоб співбесіду проводили _якісь_ програмісти, інакше її провалю, швидше за все. Не було потреби пробувати. Я хотів, щоб співбесіду проводив той, кому я по-справжньому _потрібен_: мій майбутній начальник. Ця людина зрозуміє мій профіль і не ставитиме безглуздих запитань про алгоритми, просто тому що вона знає, які мене чекають обов'язки, і які проблеми я буду вирішувати, якщо мене наймуть.

На жаль, спостерігаючи протягом двох років за наслідками таких відповідей рекрутерам, я бачу, що вони нічого змінити не можуть. Вони зобов'язані забезпечити формальну і стандартну перевірку кожного, почавши з тих самих теплих обіцянок і лестощів.

Вибачте, рекрутери, стандартних співбесід більше не хочу.
