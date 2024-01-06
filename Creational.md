## Creational Design Patterns
Creational Design Patterns as outlined by the Gang of Four (GoF) in their seminal book "Design Patterns: Elements of Reusable Object-Oriented Software," focus on ways to create objects while hiding the creation logic, rather than instantiating objects directly. This allows the program to be more independent of how its objects are created, composed, and represented.

Here's an overview of the five creational design patterns:

### 1. Singleton Pattern
- **Purpose**: Ensures a class has only one instance and provides a global point of access to it.
- **Example**: A database connection class.

```java
public class DatabaseConnection {
    private static DatabaseConnection instance = new DatabaseConnection();

    private DatabaseConnection() {}

    public static DatabaseConnection getInstance() {
        return instance;
    }

    // Other database methods
}
```

### 2. Factory Method Pattern
- **Purpose**: Defines an interface for creating an object but lets subclasses alter the type of objects that will be created.
- **Example**: A simple factory for creating different types of shapes.

```java
public abstract class ShapeFactory {
    abstract Shape createShape(String shapeType);
}

public class ConcreteShapeFactory extends ShapeFactory {
    @Override
    Shape createShape(String shapeType) {
        switch (shapeType) {
            case "Circle":
                return new Circle();
            case "Rectangle":
                return new Rectangle();
            default:
                return null;
        }
    }
}

interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() {
        // Draw circle
    }
}

class Rectangle implements Shape {
    public void draw() {
        // Draw rectangle
    }
}
```

### 3. Abstract Factory Pattern
- **Purpose**: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
- **Example**: A factory for creating different types of UI elements for different platforms.

```java
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

public class WinFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WinButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WinCheckbox();
    }
}

public class MacFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new MacButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}

interface Button {
    void paint();
}

interface Checkbox {
    void paint();
}

class WinButton implements Button {
    public void paint() {
        // Windows button paint
    }
}

class MacButton implements Button {
    public void paint() {
        // Mac button paint
    }
}
// Similar implementations for WinCheckbox and MacCheckbox
```

### 4. Builder Pattern
- **Purpose**: Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.
- **Example**: Imagine a scenario where we need to create a user profile with various optional fields such as address, age, phone number, etc.

```java
public class UserProfile {
    // Required parameters
    private final String firstName;
    private final String lastName;

    // Optional parameters
    private final int age;
    private final String phone;
    private final String address;

    private UserProfile(UserProfileBuilder builder) {
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.age = builder.age;
        this.phone = builder.phone;
        this.address = builder.address;
    }

    public static class UserProfileBuilder {
        private final String firstName;
        private final String lastName;
        private int age = 0;
        private String phone = "";
        private String address = "";

        public UserProfileBuilder(String firstName, String lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }

        public UserProfileBuilder age(int age) {
            this.age = age;
            return this;
        }

        public UserProfileBuilder phone(String phone) {
            this.phone = phone;
            return this;
        }

        public UserProfileBuilder address(String address) {
            this.address = address;
            return this;
        }

        public UserProfile build() {
            return new UserProfile(this);
        }
    }
}
```

Usage:

```java
UserProfile userProfile = new UserProfile.UserProfileBuilder("John", "Doe")
                                        .age(30)
                                        .phone("1234567890")
                                        .address("123 Main St")
                                        .build();
```

### 5. Prototype Pattern
- **Purpose**: Used to create duplicate objects while keeping performance in mind. It lets you copy existing objects without making your code dependent on their classes.
- **Example**: A prototype manager to clone different types of shapes.

Suppose we have an interface `Shape` and want to clone a specific shape.

```java
public interface Shape extends Cloneable {
    Shape clone();
}

public class Rectangle implements Shape {
    private int width;
    private int height;

    // Constructors, getters, setters

    @Override
    public Shape clone() {
        return new Rectangle(this.width, this.height);
    }
}

public class Circle implements Shape {
    private int radius;

    // Constructors, getters, setters

    @Override
    public Shape clone() {
        return new Circle(this.radius);
    }
}
```

Usage:

```java
Shape originalRectangle = new Rectangle(10, 20);
Shape clonedRectangle = originalRectangle.clone();

Shape originalCircle = new Circle(5);
Shape clonedCircle = originalCircle.clone();
```
