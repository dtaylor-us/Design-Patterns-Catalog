
# Behavioral Design Patterns

Behavioral design patterns are concerned with algorithms and the assignment of responsibilities between objects. They help in defining how objects interact with each other to complete tasks, allowing for flexible and efficient communication and delegation of responsibilities. These patterns focus on improving communication between disparate objects in a system.

Below are examples of some common behavioral design patterns implemented in Java.

## Patterns

### 1. Strategy Pattern
- **Purpose**: Allows a family of algorithms to be defined and encapsulated within a class. The algorithm can be changed at runtime without client intervention.
- **Example**:
  ```java
  public interface Strategy {
      public void execute();
  }

  public class ConcreteStrategyA implements Strategy {
      public void execute() {
          System.out.println("Executing strategy A");
      }
  }

  public class ConcreteStrategyB implements Strategy {
      public void execute() {
          System.out.println("Executing strategy B");
      }
  }

  public class Context {
      private Strategy strategy;

      public Context(Strategy strategy) {
          this.strategy = strategy;
      }

      public void executeStrategy() {
          strategy.execute();
      }
  }

  // Usage
  Context context = new Context(new ConcreteStrategyA());
  context.executeStrategy();
  context = new Context(new ConcreteStrategyB());
  context.executeStrategy();
  ```

### 2. Observer Pattern
- **Purpose**: Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
- **Example**:
  ```java
  import java.util.ArrayList;
  import java.util.List;

  public interface Observer {
      void update();
  }

  public class ConcreteObserver implements Observer {
      public void update() {
          System.out.println("Observer notified");
      }
  }

  public class Subject {
      private List<Observer> observers = new ArrayList<Observer>();

      public void attach(Observer observer) {
          observers.add(observer);
      }

      public void notifyAllObservers() {
          for (Observer observer : observers) {
              observer.update();
          }
      }
  }

  // Usage
  Subject subject = new Subject();
  Observer observer = new ConcreteObserver();
  subject.attach(observer);
  subject.notifyAllObservers();
  ```

### 3. Command Pattern
- **Purpose**: Encapsulates a request as an object, thereby allowing for parameterization of clients with different requests, queue or log requests, and support undoable operations.
- **Example**:
  ```java
  public interface Command {
      void execute();
  }

  public class ConcreteCommand implements Command {
      private Receiver receiver;

      public ConcreteCommand(Receiver receiver) {
          this.receiver = receiver;
      }

      public void execute() {
          receiver.action();
      }
  }

  public class Receiver {
      public void action() {
          System.out.println("Action performed");
      }
  }

  public class Invoker {
      private Command command;

      public void setCommand(Command command) {
          this.command = command;
      }

      public void executeCommand() {
          command.execute();
      }
  }

  // Usage
  Receiver receiver = new Receiver();
  Command command = new ConcreteCommand(receiver);
  Invoker invoker = new Invoker();
  invoker.setCommand(command);
  invoker.executeCommand();
  ```

### 4. Iterator Pattern
- **Purpose**: Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
- **Example**:
  ```java
  public interface Iterator {
      boolean hasNext();
      Object next();
  }

  public interface Container {
      Iterator getIterator();
  }

  public class ConcreteContainer implements Container {
      private String[] items = {"Item1", "Item2", "Item3"};

      public Iterator getIterator() {
          return new ConcreteIterator();
      }

      private class ConcreteIterator implements Iterator {
          int index;

          public boolean hasNext() {
              return index < items.length;
          }

          public Object next() {
              if (this.hasNext()) {
                  return items[index++];
              }
              return null;
          }
      }
  }

  // Usage
  Container container = new ConcreteContainer();
  for (Iterator iter = container.getIterator(); iter.hasNext();) {
      String item = (String)iter.next();
      System.out.println("Item: " + item);
  }
  ```

Absolutely, let's detail the additional behavioral design patterns with their purposes and Java examples.

### 5. State Pattern
- **Purpose**: Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.
- **Example**:
  ```java
  public interface State {
      void doAction(Context context);
  }

  public class StartState implements State {
      public void doAction(Context context) {
          System.out.println("In start state");
          context.setState(this);
      }

      public String toString(){
          return "Start State";
      }
  }

  public class StopState implements State {
      public void doAction(Context context) {
          System.out.println("In stop state");
          context.setState(this);
      }

      public String toString(){
          return "Stop State";
      }
  }

  public class Context {
      private State state;

      public Context() {
          state = null;
      }

      public void setState(State state) {
          this.state = state;
      }

      public State getState() {
          return state;
      }
  }

  // Usage
  Context context = new Context();
  StartState startState = new StartState();
  startState.doAction(context);
  System.out.println(context.getState().toString());
  StopState stopState = new StopState();
  stopState.doAction(context);
  System.out.println(context.getState().toString());
  ```

### 6. Template Method Pattern
- **Purpose**: Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.
- **Example**:
  ```java
  public abstract class Game {
      abstract void initialize();
      abstract void startPlay();
      abstract void endPlay();

      //template method
      public final void play() {
          initialize();
          startPlay();
          endPlay();
      }
  }

  public class Cricket extends Game {
      void initialize() {
          System.out.println("Cricket Game Initialized! Start playing.");
      }

      void startPlay() {
          System.out.println("Cricket Game Started. Enjoy the game!");
      }

      void endPlay() {
          System.out.println("Cricket Game Finished!");
      }
  }

  public class Football extends Game {
      void initialize() {
          System.out.println("Football Game Initialized! Start playing.");
      }

      void startPlay() {
          System.out.println("Football Game Started. Enjoy the game!");
      }

      void endPlay() {
          System.out.println("Football Game Finished!");
      }
  }

  // Usage
  Game game = new Cricket();
  game.play();
  game = new Football();
  game.play();
  ```

### 7. Mediator Pattern
- **Purpose**: Reduces coupling between classes by handling communication between them, effectively promoting loose coupling.
- **Example**:
  ```java
  import java.util.ArrayList;
  import java.util.List;

  public interface ChatMediator {
      void sendMessage(String msg, User user);
      void addUser(User user);
  }

  public class ChatMediatorImpl implements ChatMediator {
      private List<User> users;

      public ChatMediatorImpl(){
          this.users = new ArrayList<>();
      }

      @Override
      public void addUser(User user){
          this.users.add(user);
      }

      @Override
      public void sendMessage(String msg, User user) {
          for(User u : this.users){
              // message should not be received by the user sending it
              if(u != user){
                  u.receive(msg);
              }
          }
      }
  }

  public abstract class User {
      protected ChatMediator mediator;
      protected String name;

      public User(ChatMediator med, String name){
          this.mediator = med;
          this.name = name;
      }

      public abstract void send(String msg);

      public abstract void receive(String msg);
  }

  public class UserImpl extends User {
      public UserImpl(ChatMediator med, String name) {
          super(med, name);
      }

      @Override
      public void send(String msg){
          System.out.println(this.name+": Sending Message="+msg);
          mediator.sendMessage(msg, this);
      }

      @Override
      public void receive(String msg) {
          System.out.println(this.name+": Received Message:"+msg);
      }
  }

  // Usage
  ChatMediator mediator = new ChatMediatorImpl();
  User user1 = new UserImpl(mediator, "John");
  User user2 = new UserImpl(mediator, "Lisa");
  User user3 = new UserImpl(mediator, "Sophia");
  User user4 = new UserImpl(mediator, "David");
  mediator.addUser(user1);
  mediator.addUser(user2);
  mediator.addUser(user3);
  mediator.addUser(user4);
  user1.send("Hi All");
  ```

### 8. Chain of Responsibility Pattern
- **Purpose**: Passes the request along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.
- **Example**:
  ```java
  public abstract class Logger {
      public static int INFO = 1;
      public static int DEBUG = 2;
      public static int ERROR = 3;

      protected int level;

      //next element in chain or responsibility
      protected Logger nextLogger;

      public void setNextLogger(Logger nextLogger){
          this.nextLogger = nextLogger;
      }

      public void logMessage(int level, String message){
          if(this.level <= level){
              write(message);
          }
          if(nextLogger != null){
              nextLogger.logMessage(level, message);
          }
      }

      abstract protected void write(String message);
  }

  public class ConsoleLogger extends Logger {
      public ConsoleLogger(int level){
          this.level = level;
      }

      @Override
      protected void write(String message) {
          System.out.println("Standard Console::Logger: " + message);
      }
  }

  public class ErrorLogger extends Logger {
      public ErrorLogger(int level){
          this.level = level;
      }

      @Override
      protected void write(String message) {
          System.out.println("Error Console::Logger: " + message);
      }
  }

  public class FileLogger extends Logger {
      public FileLogger(int level){
          this.level = level;
      }

      @Override
      protected void write(String message) {
          System.out.println("File::Logger: " + message);
      }
  }

  // Usage
  Logger consoleLogger = new ConsoleLogger(Logger.INFO);
  Logger errorLogger = new ErrorLogger(Logger.ERROR);
  Logger fileLogger = new FileLogger(Logger.DEBUG);
  consoleLogger.setNextLogger(fileLogger);
  fileLogger.setNextLogger(errorLogger);
  consoleLogger.logMessage(Logger.INFO, "This is an information.");
  consoleLogger.logMessage(Logger.DEBUG, "This is a debug level information.");
  consoleLogger.logMessage(Logger.ERROR, "This is an error information.");
  ```

### 9. Visitor Pattern
- **Purpose**: Allows adding new operations to existing object structures without modifying them.
- **Example**:
  ```java
  interface ComputerPart {
      void accept(ComputerPartVisitor computerPartVisitor);
  }

  class Keyboard implements ComputerPart {
      @Override
      public void accept(ComputerPartVisitor computerPartVisitor) {
          computerPartVisitor.visit(this);
      }
  }

  class Monitor implements

 ComputerPart {
      @Override
      public void accept(ComputerPartVisitor computerPartVisitor) {
          computerPartVisitor.visit(this);
      }
  }

  class Mouse implements ComputerPart {
      @Override
      public void accept(ComputerPartVisitor computerPartVisitor) {
          computerPartVisitor.visit(this);
      }
  }

  class Computer implements ComputerPart {
      ComputerPart[] parts;

      public Computer(){
          parts = new ComputerPart[] {new Mouse(), new Keyboard(), new Monitor()};
      }

      @Override
      public void accept(ComputerPartVisitor computerPartVisitor) {
          for (int i = 0; i < parts.length; i++) {
              parts[i].accept(computerPartVisitor);
          }
          computerPartVisitor.visit(this);
      }
  }

  interface ComputerPartVisitor {
      void visit(Computer computer);
      void visit(Mouse mouse);
      void visit(Keyboard keyboard);
      void visit(Monitor monitor);
  }

  class ComputerPartDisplayVisitor implements ComputerPartVisitor {
      @Override
      public void visit(Computer computer) {
          System.out.println("Displaying Computer.");
      }

      @Override
      public void visit(Mouse mouse) {
          System.out.println("Displaying Mouse.");
      }

      @Override
      public void visit(Keyboard keyboard) {
          System.out.println("Displaying Keyboard.");
      }

      @Override
      public void visit(Monitor monitor) {
          System.out.println("Displaying Monitor.");
      }
  }

  // Usage
  ComputerPart computer = new Computer();
  computer.accept(new ComputerPartDisplayVisitor());
  ```

### 10. Memento Pattern
- **Purpose**: Provides the ability to restore an object to its previous state (undo via rollback).
- **Example**:
  ```java
  class Memento {
      private String state;

      public Memento(String state){
          this.state = state;
      }

      public String getState(){
          return state;
      }
  }

  class Originator {
      private String state;

      public void setState(String state){
          this.state = state;
      }

      public String getState(){
          return state;
      }

      public Memento saveStateToMemento(){
          return new Memento(state);
      }

      public void getStateFromMemento(Memento Memento){
          state = Memento.getState();
      }
  }

  class Caretaker {
      private List<Memento> mementoList = new ArrayList<Memento>();

      public void add(Memento state){
          mementoList.add(state);
      }

      public Memento get(int index){
          return mementoList.get(index);
      }
  }

  // Usage
  Originator originator = new Originator();
  Caretaker caretaker = new Caretaker();
  originator.setState("State #1");
  originator.setState("State #2");
  caretaker.add(originator.saveStateToMemento());
  originator.setState("State #3");
  caretaker.add(originator.saveStateToMemento());
  originator.setState("State #4");

  System.out.println("Current State: " + originator.getState());
  originator.getStateFromMemento(caretaker.get(0));
  System.out.println("First saved State: " + originator.getState());
  originator.getStateFromMemento(caretaker.get(1));
  System.out.println("Second saved State: " + originator.getState());
  ```

### 11. Interpreter Pattern
- **Purpose**: Provides a way to evaluate language grammar or expression for a language.
- **Example**:
  ```java
  interface Expression {
      boolean interpret(String context);
  }

  class TerminalExpression implements Expression {
      private String data;

      public TerminalExpression(String data){
          this.data = data;
      }

      public boolean interpret(String context) {
          return context.contains(data);
      }
  }

  class OrExpression implements Expression {
      private Expression expr1 = null;
      private Expression expr2 = null;

      public OrExpression(Expression expr1, Expression expr2) {
          this.expr1 = expr1;
          this.expr2 = expr2;
      }

      public boolean interpret(String context) {
          return expr1.interpret(context) || expr2.interpret(context);
      }
  }

  class AndExpression implements Expression {
      private Expression expr1 = null;
      private Expression expr2 = null;

      public AndExpression(Expression expr1, Expression expr2) {
          this.expr1 = expr1;
          this.expr2 = expr2;
      }

      public boolean interpret(String context) {
          return expr1.interpret(context) && expr2.interpret(context);
      }
  }

  // Usage
  Expression person1 = new TerminalExpression("John");
  Expression person2 = new TerminalExpression("Doe");
  Expression isSingle = new OrExpression(person1, person2);

  Expression viktor = new TerminalExpression("Viktor");
  Expression committed = new TerminalExpression("Committed");
  Expression isCommitted = new AndExpression(viktor, committed);

  System.out.println("John is single? " + isSingle.interpret("John"));
  System.out.println("Viktor is a committed person? " + isCommitted.interpret("Committed Viktor"));
  ```