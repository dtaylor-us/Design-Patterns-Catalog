# Structural Design Patterns in Java

Structural design patterns are concerned with how classes and objects are composed to form larger structures. These patterns simplify the structure by identifying the relationships between the entities. Structural patterns ensure that if one part of the system changes, the entire system doesn't need to do the same. They help in creating class hierarchies and composing interfaces to build complex objects.

Below are examples of common structural design patterns implemented in Java.

## Patterns

### 1. Adapter Pattern
- **Purpose**: Allows the interface of an existing class to be used as another interface. It is often used to make existing classes work with others without modifying their source code.
- **Example**:
  ```java
  // Target Interface
  public interface MediaPlayer {
      public void play(String audioType, String fileName);
  }

  // Advanced Media Player
  public interface AdvancedMediaPlayer {
      public void playVlc(String fileName);
      public void playMp4(String fileName);
  }

  // Concrete implementation of AdvancedMediaPlayer
  public class VlcPlayer implements AdvancedMediaPlayer {
      @Override
      public void playVlc(String fileName) {
          System.out.println("Playing vlc file. Name: " + fileName);
      }

      @Override
      public void playMp4(String fileName) {
          // do nothing
      }
  }

  public class Mp4Player implements AdvancedMediaPlayer {
      @Override
      public void playVlc(String fileName) {
          // do nothing
      }

      @Override
      public void playMp4(String fileName) {
          System.out.println("Playing mp4 file. Name: " + fileName);
      }
  }

  // Adapter
  public class MediaAdapter implements MediaPlayer {
      AdvancedMediaPlayer advancedMusicPlayer;

      public MediaAdapter(String audioType) {
          if (audioType.equalsIgnoreCase("vlc")) {
              advancedMusicPlayer = new VlcPlayer();
          } else if (audioType.equalsIgnoreCase("mp4")) {
              advancedMusicPlayer = new Mp4Player();
          }
      }

      @Override
      public void play(String audioType, String fileName) {
          if (audioType.equalsIgnoreCase("vlc")) {
              advancedMusicPlayer.playVlc(fileName);
          } else if (audioType.equalsIgnoreCase("mp4")) {
              advancedMusicPlayer.playMp4(fileName);
          }
      }
  }

  // Using the adapter
  public class AudioPlayer implements MediaPlayer {
      MediaAdapter mediaAdapter;

      @Override
      public void play(String audioType, String fileName) {
          if (audioType.equalsIgnoreCase("mp3")) {
              System.out.println("Playing mp3 file. Name: " + fileName);
          } else if (audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4")) {
              mediaAdapter = new MediaAdapter(audioType);
              mediaAdapter.play(audioType, fileName);
          } else {
              System.out.println("Invalid media. " + audioType + " format not supported");
          }
      }
  }

  // Usage
  AudioPlayer audioPlayer = new AudioPlayer();
  audioPlayer.play("mp3", "beyond the horizon.mp3");
  audioPlayer.play("mp4", "alone.mp4");
  audioPlayer.play("vlc", "far far away.vlc");
  audioPlayer.play("avi", "mind me.avi");
  ```

### 2. Composite Pattern
- **Purpose**: Composes objects into tree structures to represent part-whole hierarchies. It allows clients to treat individual objects and compositions of objects uniformly.
- **Example**:
  ```java
  import java.util.ArrayList;
  import java.util.List;

  public interface Component {
      void add(Component component);
      void remove(Component component);
      void display();
  }

  public class Composite implements Component {
      private List<Component> children = new ArrayList<Component>();

      @Override
      public void add(Component component) {
          children.add(component);
      }

      @Override
      public void remove(Component component) {
          children.remove(component);
      }

      @Override
      public void display() {
          for (Component component : children) {
              component.display();
          }
      }
  }

  public class Leaf implements Component {
      private String name;

      public Leaf(String name) {
          this.name = name;
      }

      @Override
      public void add(Component component) {
          // Leaf does not implement this method
      }

      @Override
      public void remove(Component component) {
          // Leaf does not implement this method
      }

      @Override
      public void display() {
          System.out.println(name);
      }
  }

  // Usage
  Component root = new Composite();
  Component leaf1 = new Leaf("Leaf 1");
  Component leaf2 = new Leaf("Leaf 2");
  root.add(leaf1);
  root.add(leaf2);
  root.display();
  ```

### 3. Proxy Pattern
- **Purpose**: Provides a surrogate or placeholder for another object to control access to it. This pattern is used to create a representative object that controls access to another object, which may be remote, expensive to create, or in need of securing.
- **Example**:
  ```java
  public interface Image {
      void display();
  }

  public class RealImage implements Image {
      private String fileName;

      public RealImage(String fileName) {
          this.fileName = fileName;
          loadFromDisk(fileName);
      }

      @Override
      public void display() {
          System.out.println("Displaying " + fileName);
      }

      private void loadFromDisk(String fileName) {
          System.out.println("Loading " + fileName);
      }
  }

  public class ProxyImage implements Image {
      private RealImage realImage;
      private String fileName;

      public ProxyImage(String fileName) {
          this.fileName = fileName;
      }

      @Override
      public void display() {
          if (realImage == null) {
              realImage = new RealImage(fileName);
          }
          realImage.display();
      }
  }

  // Usage
  Image image = new ProxyImage("test_10mb.jpg");
  //image will be loaded from disk
  image.display(); 
  //image will not be loaded from disk
  image.display(); 
  ```

### 4. Flyweight Pattern
- **Purpose**: Reduces the cost of creating and manipulating a large number of similar objects.
- **Example**:
  ```java
  import java.util.HashMap;
  import java.util.Map;

  interface Shape {
      void draw();
  }

  class Circle implements Shape {
      private String color;
      private int x;
      private int y;
      private int radius;

      public Circle(String color) {
          this.color = color;
      }

      public void setX(int x) {
          this.x = x;
      }

      public void setY(int y) {
          this.y = y;
      }

      public void setRadius(int radius) {
          this.radius = radius;
      }

      public void draw() {
          System.out.println("Circle: Draw() [Color : " + color + ", x : " + x + ", y :" + y + ", radius :" + radius);
      }
  }

  class ShapeFactory {
      private static final Map<String, Shape> circleMap = new HashMap<>();

      public static Shape getCircle

(String color) {
          Circle circle = (Circle)circleMap.get(color);

          if(circle == null) {
              circle = new Circle(color);
              circleMap.put(color, circle);
              System.out.println("Creating circle of color : " + color);
          }
          return circle;
      }
  }

  // Usage
  public class FlyweightPatternDemo {
      private static final String colors[] = { "Red", "Green", "Blue", "White", "Black" };

      public static void main(String[] args) {

          for (int i = 0; i < 20; ++i) {
              Circle circle = (Circle)ShapeFactory.getCircle(getRandomColor());
              circle.setX(getRandomX());
              circle.setY(getRandomY());
              circle.setRadius(100);
              circle.draw();
          }
      }

      private static String getRandomColor() {
          return colors[(int)(Math.random()*colors.length)];
      }

      private static int getRandomX() {
          return (int)(Math.random()*100 );
      }

      private static int getRandomY() {
          return (int)(Math.random()*100);
      }
  }
  ```

### 5. Facade Pattern
- **Purpose**: Provides a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
- **Example**:
  ```java
  class SubsystemOne {
      public void methodOne() {
          System.out.println(" SubsystemOne Method");
      }
  }

  class SubsystemTwo {
      public void methodTwo() {
          System.out.println(" SubsystemTwo Method");
      }
  }

  class SubsystemThree {
      public void methodThree() {
          System.out.println(" SubsystemThree Method");
      }
  }

  class Facade {
      private SubsystemOne one;
      private SubsystemTwo two;
      private SubsystemThree three;

      public Facade() {
          one = new SubsystemOne();
          two = new SubsystemTwo();
          three = new SubsystemThree();
      }

      public void methodA() {
          System.out.println(" MethodA() ---- ");
          one.methodOne();
          two.methodTwo();
      }

      public void methodB() {
          System.out.println(" MethodB() ---- ");
          two.methodTwo();
          three.methodThree();
      }
  }

  // Usage
  Facade facade = new Facade();
  facade.methodA();
  facade.methodB();
  ```

### 6. Bridge Pattern
- **Purpose**: Decouples an abstraction from its implementation so that the two can vary independently.
- **Example**:
  ```java
  // Implementor
  interface DrawAPI {
      public void drawCircle(int radius, int x, int y);
  }

  // Concrete Implementor
  class RedCircle implements DrawAPI {
      public void drawCircle(int radius, int x, int y) {
          System.out.println("Drawing Circle[ color: red, radius: " + radius + ", x: " + x + ", " + y + "]");
      }
  }

  class GreenCircle implements DrawAPI {
      public void drawCircle(int radius, int x, int y) {
          System.out.println("Drawing Circle[ color: green, radius: " + radius + ", x: " + x + ", " + y + "]");
      }
  }

  // Abstraction
  abstract class Shape {
      protected DrawAPI drawAPI;
      
      protected Shape(DrawAPI drawAPI) {
          this.drawAPI = drawAPI;
      }
      public abstract void draw();
  }

  // Refined Abstraction
  class Circle extends Shape {
      private int x, y, radius;

      protected Circle(int x, int y, int radius, DrawAPI drawAPI) {
          super(drawAPI);
          this.x = x;
          this.y = y;
          this.radius = radius;
      }

      public void draw() {
          drawAPI.drawCircle(radius,x,y);
      }
  }

  // Usage
  Shape redCircle = new Circle(100,100, 10, new RedCircle());
  Shape greenCircle = new Circle(100,100, 10, new GreenCircle());
  redCircle.draw();
  greenCircle.draw();
  ```

### 7. Decorator Pattern
- **Purpose**: Allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class.
- **Example**:
  ```java
  interface Coffee {
      double getCost();
      String getDescription();
  }

  class SimpleCoffee implements Coffee {
      public double getCost() {
          return 1;
      }

      public String getDescription() {
          return "Simple coffee";
      }
  }

  class MilkCoffee implements Coffee {
      protected Coffee coffee;

      public MilkCoffee(Coffee coffee) {
          this.coffee = coffee;
      }

      public double getCost() {
          return coffee.getCost() + 0.5;
      }

      public String getDescription() {
          return coffee.getDescription() + ", milk";
      }
  }

  class WhipCoffee implements Coffee {
      protected Coffee coffee;

      public WhipCoffee(Coffee coffee) {
          this.coffee = coffee;
      }

      public double getCost() {
          return coffee.getCost() + 0.7;
      }

      public String getDescription() {
          return coffee.getDescription() + ", whip";
      }
  }

  class VanillaCoffee implements Coffee {
      protected Coffee coffee;

      public VanillaCoffee(Coffee coffee) {
          this.coffee = coffee;
      }

      public double getCost() {
          return coffee.getCost() + 0.3;
      }

      public String getDescription() {
          return coffee.getDescription() + ", vanilla";
      }
  }

  // Usage
  Coffee myCoffee = new SimpleCoffee();
  System.out.println(myCoffee.getCost()); // 1
  System.out.println(myCoffee.getDescription()); // Simple coffee

  myCoffee = new MilkCoffee(myCoffee);
  System.out.println(myCoffee.getCost()); // 1.5
  System.out.println(myCoffee.getDescription()); // Simple coffee, milk

  myCoffee = new WhipCoffee(myCoffee);
  System.out.println(myCoffee.getCost()); // 2.2
  System.out.println(myCoffee.getDescription()); // Simple coffee, milk, whip

  myCoffee = new VanillaCoffee(myCoffee);
  System.out.println(myCoffee.getCost()); // 2.5
  System.out.println(myCoffee.getDescription()); // Simple coffee, milk, whip, vanilla
  ```