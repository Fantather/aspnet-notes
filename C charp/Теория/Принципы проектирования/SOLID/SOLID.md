**SOLID** — это акроним, введенный Робертом Мартином (дядя Боб), который объединяет пять основных принципов:

1.  **S** - Single Responsibility Principle (Принцип единственной ответственности)
2.  **O** - Open/Closed Principle (Принцип открытости/закрытости)
3.  **L** - Liskov Substitution Principle (Принцип подстановки Барбары Лисков)
4.  **I** - Interface Segregation Principle (Принцип разделения интерфейсов)
5.  **D** - Dependency Inversion Principle (Принцип инверсии зависимостей)

Это принципы проектирования программы
Нужно знать их, что бы стать успешным разработчиком
Так же можно посмотреть языки программирования (например UML)

### 1. Принцип единственной ответственности (Single Responsibility Principle - SRP)

**Формулировка:** У класса должна быть только одна причина для изменения. Или: Класс должен отвечать только за одну задачу или функциональность.

**Суть:** Этот принцип направлен на разделение ответственности. Если класс берет на себя слишком много обязанностей, он становится сложным для понимания, тестирования и модификации. Изменение в одной из его функциональностей может повлечь за собой непредвиденные последствия в других.

**Детальное объяснение:**
Представьте, что у вас есть класс `Order`, который:
1.  Рассчитывает итоговую стоимость заказа.
2.  Сохраняет заказ в базу данных.
3.  Отправляет email-уведомление клиенту.

Это нарушает SRP, так как у класса три совершенно разные причины для изменения:
1.  Изменилась бизнес-логика расчета (например, добавилась новая скидка).
2.  Изменился способ сохранения (перешли с MySQL на PostgreSQL).
3.  Изменился формат email-уведомления или сервис его отправки.

**Пример нарушения:**

```java
class Order {
    public void calculateTotalSum() {...}
    public void saveToDatabase() {...}
    public void sendEmailNotification() {...}
}
```

**Исправление (соответствие SRP):**
Мы разделяем ответственность на три отдельных класса.

```java
// Отвечает только за данные и логику заказа
class Order {
    public void calculateTotalSum() {...}
}

// Отвечает только за взаимодействие с базой данных
class OrderRepository {
    public void save(Order order) {...}
}

// Отвечает только за отправку уведомлений
class NotificationService {
    public void sendEmail(Order order) {...}
}
```
Теперь каждый класс имеет только одну зону ответственности и одну причину для изменения.

---

### 2. Принцип открытости/закрытости (Open/Closed Principle - OCP)

**Формулировка:** Программные сущности (классы, модули, функции и т.п.) должны быть **открыты для расширения, но закрыты для модификации**.

**Суть:** Вы должны проектировать систему так, чтобы для добавления новой функциональности вам не приходилось изменять уже существующий, стабильно работающий код. Вместо этого вы должны его расширять (через наследование, композицию и т.д.).

**Детальное объяснение:**
Допустим, у вас есть класс `AreaCalculator`, который рассчитывает площадь различных фигур. Если добавлять новую фигуру через `if/else` или `switch`, мы нарушаем OCP, потому что придется каждый раз модифицировать этот класс.

**Пример нарушения:**

```java
class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.radius * circle.radius;
        } else if (shape instanceof Square) {
            Square square = (Square) shape;
            return square.side * square.side;
        }
        // Добавление нового `else if` для прямоугольника -> МОДИФИКАЦИЯ!
        throw new IllegalArgumentException("Unknown shape");
    }
}
```

**Исправление (соответствие OCP):**
Мы создаем общий абстрактный интерфейс `Shape` с методом `calculateArea()`. Теперь класс `AreaCalculator` зависит от абстракции, а не от конкретных реализаций. Чтобы добавить новую фигуру, мы просто создаем новый класс, реализующий `Shape`, не трогая при этом код `AreaCalculator`.

```java
// Абстракция, закрытая для модификации
interface Shape {
    double calculateArea();
}

// Конкретные реализации
class Circle implements Shape {
    public double radius;
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Square implements Shape {
    public double side;
    @Override
    public double calculateArea() {
        return side * side;
    }
}

// Класс открыт для расширения (новыми фигурами), но закрыт для модификации
class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea(); // Не нужно ничего менять здесь!
    }
}
```

---

### 3. Принцип подстановки Барбары Лисков (Liskov Substitution Principle - LSP)

**Формулировка:** Объекты в программе должны быть заменяемыми на экземпляры их подтипов без изменения правильности выполнения программы.

**Суть:** Наследующий класс должен дополнять, а не замещать поведение базового класса. Проще говоря, если у вас есть код, работающий с базовым классом `Animal`, то он должен корректно работать и с любым его наследником (например, `Dog` или `Cat`), не зная о конкретном типе объекта.

**Детальное объяснение:**
Классический пример нарушения — иерархия "Прямоугольник-Квадрат". С математической точки зрения квадрат — это частный случай прямоугольника. Но если мы сделаем `Square` наследником `Rectangle`, мы столкнемся с проблемой.

**Пример нарушения:**

```java
class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) { this.width = width; }
    public void setHeight(int height) { this.height = height; }
    public int getArea() { return width * height; }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        super.setWidth(width);
        super.setHeight(width); // Нарушение! У квадрата стороны равны.
    }

    @Override
    public void setHeight(int height) {
        super.setHeight(height);
        super.setWidth(height); // Нарушение!
    }
}

// Клиентский код, который ожидает работу с прямоугольником
public void testArea(Rectangle rect) {
    rect.setWidth(5);
    rect.setHeight(4);
    assert rect.getArea() == 20; // Этот тест упадет, если передать Square!
    // Для Square площадь будет 4 * 4 = 16, а не 20.
}
```
Класс `Square` изменяет поведение сеттеров родительского класса, что приводит к неожиданным результатам. Клиентский код, рассчитанный на `Rectangle`, ломается при работе с `Square`.

**Следствие:** LSP — это гарантия того, что **инварианты** (условия, которые всегда истинны) и **контракты** базового класса (например, определенные поведением методов) будут сохранены в классах-потомках.

---

### 4. Принцип разделения интерфейсов (Interface Segregation Principle - ISP)

**Формулировка:** Клиенты не должны зависеть от методов, которые они не используют. Много специализированных интерфейсов лучше, чем один универсальный.

**Суть:** Не нужно заставлять клиента реализовывать методы, которые ему не нужны. Это приводит к "раздутым" интерфейсам и появлению пустых методов в классах-реализациях.

**Детальное объяснение:**
Представьте монструозный интерфейс `MultiFunctionDevice` для устройства "3-в-1".

**Пример нарушения:**

```java
interface MultiFunctionDevice {
    void print();
    void scan();
    void fax();
}

class OldPrinter implements MultiFunctionDevice {
    @Override
    public void print() { ... } // Ему это нужно

    @Override
    public void scan() {
        throw new UnsupportedOperationException(); // Ему это НЕ нужно!
    }

    @Override
    public void fax() {
        throw new UnsupportedOperationException(); // Ему это НЕ нужно!
    }
}
```
`OldPrinter` вынужден реализовывать методы `scan` и `fax`, хотя физически не может этого делать.

**Исправление (соответствие ISP):**
Разделяем "жирный" интерфейс на несколько более специфичных.

```java
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

interface Fax {
    void fax();
}

// Современное МФУ реализует все интерфейсы
class MultiFunctionMachine implements Printer, Scanner, Fax {
    public void print() { ... }
    public void scan() { ... }
    public void fax() { ... }
}

// Старый принтер реализует только то, что ему нужно
class OldPrinter implements Printer {
    public void print() { ... }
}
```
Теперь клиенты зависят только от тех интерфейсов, которые им действительно необходимы.

---

### 5. Принцип инверсии зависимостей (Dependency Inversion Principle - DIP)

**Формулировка:**
1.  Модули верхнего уровня не должны зависеть от модулей нижнего уровня. И те, и другие должны зависеть от абстракций.
2.  Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.

**Суть:** Прямые зависимости внутри системы должны вести на абстракции (интерфейсы или абстрактные классы), а не на конкретные реализации. Это делает систему гибкой, позволяя легко заменять один компонент другим.

**Детальное объяснение:**
В традиционной архитектуре высокоуровневые модули (например, бизнес-логика) напрямую зависят от низкоуровневых модулей (например, база данных, API). DIP разрывает эту прямую связь.

Принцип инверсии зависимостей (пятый принцип) является следствием второго (OCP) и третьего (LSP) принципов SOLID

**Пример нарушения:**

```java
// Низкоуровневый модуль
class MySQLDatabase {
    public void saveData(String data) { ... }
}

// Высокоуровневый модуль напрямую зависит от низкоуровневого
class BusinessLogic {
    private MySQLDatabase database; // Прямая зависимость!

    public BusinessLogic() {
        this.database = new MySQLDatabase(); // Жесткая привязка!
    }

    public void processData(String data) {
        // ... какая-то логика ...
        database.saveData(data);
    }
}
```
Проблема: если мы захотим перейти с MySQL на PostgreSQL, нам придется изменять класс `BusinessLogic`.


Есть два способа реализации этого
**Factory method**
Если мы создаём объект какого-то класса, мы 
![[Pasted image 20250927152857.png]]


**Dependency Injection**
Внедрение зависимостей
Не просто инвертирует сильные зависимости от реализации в слабые зависимости от абстракции, но и выносит их из класса, предлагает внедрять их в класс извне

То есть вместо конкретного класса, нужно передавать в методы Абстрактный класс, который наследует нужный объект

![[Pasted image 20250927152953.png]]


**Исправление (соответствие DIP):**
1.  Создаем абстракцию (интерфейс) `Database`.
2.  Класс `BusinessLogic` будет зависеть только от этой абстракции.
3.  Конкретные реализации баз данных (`MySQLDatabase`, `PostgreSQLDatabase`) также будут зависеть от этой абстракции, реализуя ее.

```java
// Абстракция (Интерфейс)
interface Database {
    void save(String data);
}

// Низкоуровневый модуль зависит от абстракции
class MySQLDatabase implements Database {
    @Override
    public void save(String data) { ... }
}

class PostgreSQLDatabase implements Database {
    @Override
    public void save(String data) { ... }
}

// Высокоуровневый модуль зависит от абстракции, а не от деталей
class BusinessLogic {
    private Database database; // Зависимость от абстракции!

    // Внедрение зависимости через конструктор (Dependency Injection)
    public BusinessLogic(Database database) {
        this.database = database;
    }

    public void processData(String data) {
        // ... какая-то логика ...
        database.save(data);
    }
}

// Где-то в конфигурационном коде (например, в Main) собираем приложение
class Application {
    public static void main(String[] args) {
        Database db = new PostgreSQLDatabase(); // Легко меняем реализацию!
        BusinessLogic logic = new BusinessLogic(db); // Внедряем зависимость
        logic.processData("my data");
    }
}
```
Теперь для смены базы данных нужно изменить всего одну строку в коде сборки, не трогая бизнес-логику

### Итог

Принципы SOLID — это не строгие правила, а руководства, которые помогают проектировать систему, которая:
*   **Легко понимается** и поддерживается (SRP, ISP).
*   **Устойчива к изменениям** и легко расширяется (OCP, DIP).
*   **Ведет себя предсказуемо** и надежно (LSP).

Их совместное применение приводит к созданию чистого, модульного и слабосвязанного кода, который легко тестировать и адаптировать под changing requirements (меняющиеся требования).