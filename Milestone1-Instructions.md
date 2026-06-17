# Milestone 1 Tutorial: Project Setup and the Core Card Model

## What You Are Building

Welcome to the first real milestone of the Blackjack project.

In this milestone, you are not building the whole game yet. You are building the foundation that the whole game will stand on:

- A Java project that Maven can build
- A tiny `Main` program so the project can run
- A `Card` class
- A `Rank` enum
- A `Suit` enum
- A `Deck` class
- A `Shoe` class
- A `ShoeMgr` class
- Unit tests that prove your card, deck, and shoe logic works

Think of this milestone like building the Lego pieces before building the castle. If the individual pieces are shaped correctly, the castle is much easier to build later.

## The Big Learning Goal

The goal is not just to make Java compile.

The goal is to understand:

- What a Java class is
- What an object is
- What a constructor does
- Why private fields exist
- Why methods exist
- How one object can contain other objects
- How lists work
- How enums prevent invalid values
- How unit tests help you trust your code
- Why a real program is split across many files

If you have only used Scratch, Blockly, or another block-based tool, imagine this:

- A **class** is like creating a new kind of block.
- An **object** is one actual block made from that design.
- A **method** is an action the block can perform.
- A **field** is information stored inside the block.
- A **constructor** is the setup code that runs when the block is created.

Blackjack gives us a perfect project for learning this because the real world already has objects:

- A card
- A deck
- A shoe
- A hand
- A wager
- A player
- A dealer

In this milestone, we only build the first few.

## How This Tutorial Uses `directions.txt`

The original `directions.txt` is the backbone of this tutorial. It starts in the same way many real projects start: with a simple idea that gets improved step by step.

The early directions begin with a very simple `Card`:

> Treat a Card as a random number between 1 and 13.

That is not the final design, but it is a great learning step.

So this tutorial will do two things:

1. Preserve the original learning sequence so you understand the ideas.
2. End with the cleaner Milestone 1 design required by `Specifications.md`.

You will see some "learning versions" of code first. Later, you will replace them with better versions. That is normal. Programming is often a process of improving your design as you understand the problem better.

## Files You Will Create by the End

Production files in `src/main/java`:

- `Main.java`
- `Rank.java`
- `Suit.java`
- `Card.java`
- `Deck.java`
- `Shoe.java`
- `ShoeMgr.java`

Test files in `src/test/java`:

- `CardTest.java`
- `DeckTest.java`
- `ShoeTest.java`
- `ShoeMgrTest.java`

The repository already contains:

- `pom.xml`
- `src/main/java/CardFaceCreator.java`
- image files in `src/main/resources`

Do not edit `CardFaceCreator.java` in this milestone. It is supplied helper code for drawing graphical cards later.

Important: `CardFaceCreator.java` refers to `Rank` and `Suit`, so the full project will not compile until you create `Rank.java` and `Suit.java`.

## Part 1: Project Setup

Original `directions.txt` setup:

```text
Laptop setup if not using Replit:
a. Install VS code
b. Install Extensions for Java
c. Install maven from maven.apache.org (download binaries and add folder to PATH env variable)
d. Can run 'mvn -version' to check install
e. Can run 'mvn clean javafx:run'
```

### What This Means

Java source code is written in `.java` files. But a real Java project usually needs more than just source files:

- A way to compile the code
- A way to run tests
- A way to download libraries
- A way to launch the program

This project uses **Maven** for that.

Maven is a build tool. A build tool is like a project manager for your code. It knows:

- Where the source files live
- Where the test files live
- Which libraries the project needs
- How to compile the program
- How to run tests

The Maven configuration file is `pom.xml`.

You do not need to understand every line of `pom.xml` yet. For now, know this:

- Maven reads `pom.xml`.
- Maven compiles everything in `src/main/java`.
- Maven runs tests in `src/test/java`.
- Maven knows this project uses JavaFX and JUnit.

### Check Maven

Open a terminal in the project folder and run:

```powershell
mvn -version
```

If Maven is installed, you should see version information.

Do not worry if `mvn test` fails at the very beginning. The project already has `CardFaceCreator.java`, and that file expects `Rank` and `Suit` to exist. You will create those soon.

### Maven Folder Mental Model

Use this structure:

```text
src/
  main/
    java/        Java files for the real program
    resources/   images and other files used by the program
  test/
    java/        Java files that test the real program
```

Common beginner mistake:

- Putting tests in `test/java`

Correct:

- Put tests in `src/test/java`

Maven expects the full path.

## Part 2: The First Tiny Program

Original `directions.txt` step:

```text
1. Create a new project based on the Java template.
```

In this repository, the project already exists. Your job is to create the first runnable Java file.

### What Is `Main`?

Every program needs a starting point.

In many beginner Java programs, the starting point is:

```java
public static void main(String[] args)
```

JavaFX programs are slightly different. They use a class that extends `Application`, and JavaFX calls a method named `start`.

For now, do not worry about every JavaFX detail. Just understand this:

- `Main` starts the app.
- `start` creates the first window.
- Later, the window will show the Blackjack table.
- For now, the window can be simple.

### Create `Main.java`

Create this file:

```text
src/main/java/Main.java
```

Complete code:

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.stage.Stage;

public class Main extends Application {
    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        Label label = new Label("Blackjack project starting...");
        Scene scene = new Scene(label, 400, 200);

        primaryStage.setTitle("Blackjack");
        primaryStage.setScene(scene);
        primaryStage.show();
    }
}
```

### Line-by-Line Walkthrough

```java
import javafx.application.Application;
```

This imports JavaFX's `Application` class. Your `Main` class will inherit from it.

```java
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.stage.Stage;
```

These imports let you create a basic JavaFX window:

- `Stage` means the actual app window.
- `Scene` means the contents inside the window.
- `Label` means a small piece of text.

```java
public class Main extends Application {
```

This creates a class named `Main`.

`extends Application` means `Main` is a special kind of JavaFX application.

```java
public static void main(String[] args) {
    launch(args);
}
```

This is the normal Java entry point. It tells JavaFX to start.

```java
@Override
public void start(Stage primaryStage) {
```

JavaFX calls this method after `launch(args)`.

`primaryStage` is the main window.

```java
Label label = new Label("Blackjack project starting...");
```

This creates a text label.

The word `new` means "create a new object."

```java
Scene scene = new Scene(label, 400, 200);
```

This creates a scene containing the label. The window content is 400 pixels wide and 200 pixels tall.

```java
primaryStage.setTitle("Blackjack");
primaryStage.setScene(scene);
primaryStage.show();
```

These lines:

1. Put a title on the window.
2. Put the scene inside the window.
3. Show the window.

### Checkpoint

After you create `Rank` and `Suit` later, run:

```powershell
mvn clean javafx:run
```

Expected result:

- A small window opens.
- The window says `Blackjack project starting...`.

If it does not work yet, do not panic. The project may still need `Rank` and `Suit` because `CardFaceCreator.java` already exists.

## Part 3: Original First Card Idea

Original `directions.txt` step:

```text
2. Write a program to draw() a Card. I.e., create a Card class with a draw() method. Instantiate the Card from main() and call draw(). Output the Card's identifying information. Treat a Card as a random number between 1 and 13. This id instance variable can be set in the default contructor. (To pick a random number between 1 and 13, use this code: "id = new Random().nextInt(13) + 1;" You'll also need to "import java.util.Random;")
```

### Why This Step Exists

Before building Blackjack, you need to understand the smallest object in the game:

```text
Card
```

A real card has two important properties:

- Rank, such as Ace, 2, 3, Jack, Queen, King
- Suit, such as Hearts or Spades

But the original directions start simpler:

- Ignore suits.
- Treat the rank as a number from 1 to 13.

This lets you practice:

- Creating a class
- Creating an object
- Creating a field
- Writing a constructor
- Using randomness
- Calling a method

### What Is a Class?

A class is a blueprint.

Example:

```text
Class: Card
Object: Ace of Hearts
Object: 7 of Clubs
Object: King of Spades
```

The class describes what all cards have in common. Each object is one actual card.

### What Is a Field?

A field is information stored inside an object.

For the first simple card, the field is:

```java
private int id;
```

This means each card object stores an integer named `id`.

### What Is a Constructor?

A constructor is setup code that runs when an object is created with `new`.

If you write:

```java
Card card = new Card();
```

Java creates a new `Card` object and runs the `Card()` constructor.

### Learning Version of `Card`

This is a learning version, not the final version.

Create `Card.java`:

```java
import java.util.Random;

public class Card {
    private int id;

    public Card() {
        id = new Random().nextInt(13) + 1;
    }

    public void draw() {
        System.out.println("You drew card #" + id);
    }
}
```

### Line-by-Line Walkthrough

```java
import java.util.Random;
```

Java has a built-in `Random` class for random numbers. This import lets you use it.

```java
public class Card {
```

This defines a new class named `Card`.

```java
private int id;
```

This creates a private field.

`private` means other classes cannot directly change it.

`int` means integer.

`id` stores the card's number.

```java
public Card() {
```

This is the constructor. It has the same name as the class.

```java
id = new Random().nextInt(13) + 1;
```

This line chooses a random number.

Break it down:

- `new Random()` creates a random number generator.
- `.nextInt(13)` gives a number from 0 through 12.
- `+ 1` shifts the range to 1 through 13.

So the final result is:

```text
1, 2, 3, ..., 13
```

```java
public void draw() {
```

This creates a method named `draw`.

`void` means the method does not return an answer. It just does something.

```java
System.out.println("You drew card #" + id);
```

This prints the card number.

### Try It from `Main`

For a simple console experiment, you could temporarily use:

```java
public class Main {
    public static void main(String[] args) {
        Card card = new Card();
        card.draw();
    }
}
```

This is not the final JavaFX `Main`. It is a learning experiment.

### Common Mistakes

Mistake:

```java
new Random().nextInt(13)
```

This gives 0 through 12, not 1 through 13.

Correct:

```java
new Random().nextInt(13) + 1
```

Mistake:

```java
public card() {
```

Constructors must match the class name exactly, including capitalization:

```java
public Card() {
```

Mistake:

```java
private int id
```

Java statements need semicolons:

```java
private int id;
```

### Verify the Step

Run the program several times.

Expected result:

- It prints one random card number.
- The number is always between 1 and 13.

## Part 4: Asking Whether to Draw Again

Original `directions.txt` step:

```text
3. Continually ask the user if they want to draw() another Card. (You can use "Scanner reader = new Scanner(System.in);". Remember to "reader.close();" at the end of your program.) The program ends when they choose No. [Use do..while] (For example, "System.out.println("Do you want to draw another hand? (1 for yes, 0 for no)"); proceed = reader.nextInt();"))
```

### Why This Step Exists

Games usually repeat actions.

In Blackjack, you will eventually repeat many things:

- Deal another card
- Play another hand
- Start another round

So the original directions introduce a loop.

### What Is a Loop?

A loop repeats code.

A `do...while` loop means:

1. Do the code once.
2. Then check whether to do it again.

That is perfect here, because you want to draw at least one card before asking whether to continue.

### What Is `Scanner`?

`Scanner` reads input from the keyboard.

The original snippet:

```java
Scanner reader = new Scanner(System.in);
```

This creates a scanner that reads from `System.in`, which means keyboard input.

### Learning Version of `Main`

This is still a console learning experiment:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner reader = new Scanner(System.in);
        int proceed;

        do {
            Card card = new Card();
            card.draw();

            System.out.println("Do you want to draw another card? (1 for yes, 0 for no)");
            proceed = reader.nextInt();
        } while (proceed == 1);

        reader.close();
    }
}
```

### Line-by-Line Walkthrough

```java
import java.util.Scanner;
```

This lets the program use `Scanner`.

```java
Scanner reader = new Scanner(System.in);
```

This creates the object that reads keyboard input.

```java
int proceed;
```

This variable stores the user's answer.

```java
do {
```

Start a `do...while` loop.

```java
Card card = new Card();
card.draw();
```

Create and draw a card.

```java
proceed = reader.nextInt();
```

Read an integer from the keyboard.

```java
} while (proceed == 1);
```

If the user typed 1, repeat.

```java
reader.close();
```

Close the scanner when finished.

### Common Mistakes

Mistake:

- Forgetting to import `Scanner`.

Mistake:

- Forgetting `reader.close();`.

Mistake:

- Using `=` instead of `==`.

This is wrong:

```java
while (proceed = 1)
```

This is correct:

```java
while (proceed == 1)
```

In Java:

- `=` means assign a value.
- `==` means compare two values.

### Verify the Step

Expected behavior:

- The program draws one card.
- It asks whether to draw another card.
- If you type `1`, it draws again.
- If you type `0`, it stops.

## Part 5: Handling Bad Input

Original `directions.txt` step:

```text
4. Enhance the program to not crash if a non-numeric is entered by the user. Replace the while clause with "(reader.hasNextInt() && reader.nextInt() == 1)". You no longer need the 'proceed' variable.
```

### Why This Step Exists

Beginners often write programs that work only if the user behaves perfectly.

Real users do not always behave perfectly.

If the program expects a number and the user types:

```text
hello
```

then `reader.nextInt()` can crash.

This step teaches defensive programming: writing code that handles mistakes gracefully.

### Original Condition

The original directions suggest:

```java
reader.hasNextInt() && reader.nextInt() == 1
```

This uses two ideas:

- `hasNextInt()` checks whether the next input can be read as an integer.
- `nextInt()` actually reads the integer.

The `&&` means "and."

Java evaluates this left to right. If `hasNextInt()` is false, Java does not try `nextInt()`.

### Learning Version

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner reader = new Scanner(System.in);

        do {
            Card card = new Card();
            card.draw();

            System.out.println("Do you want to draw another card? (1 for yes, 0 for no)");
        } while (reader.hasNextInt() && reader.nextInt() == 1);

        reader.close();
    }
}
```

### Common Mistake

This condition is compact but subtle:

```java
reader.hasNextInt() && reader.nextInt() == 1
```

It both checks and consumes input.

That means you should not also call `reader.nextInt()` somewhere else for the same answer.

### Verify the Step

Try:

- `1`
- `0`
- `hello`

Expected result:

- `1` continues.
- `0` stops.
- non-numeric input stops instead of crashing.

## Part 6: Displaying Real Rank Names

Original `directions.txt` step:

```text
5. Modify the program to display "Jack" instead of 11, "Queen" instead of 12, "King" instead of 13, and "Ace" instead of 1. Add a toString() method to Card to perform this conversion. Use a switch statement. Note that for the default case (cards 2-10), you can use "Integer.toString(id)". In main(), display the Card that was drawn. Note that you do not need to add "toString()" in your System.out.println call.
```

### Why This Step Exists

Humans do not usually say:

```text
I drew card #13.
```

They say:

```text
I drew a King.
```

This step teaches `toString()`.

### What Is `toString()`?

`toString()` is a special method Java uses when it needs a text version of an object.

If you write:

```java
System.out.println(card);
```

Java automatically calls:

```java
card.toString()
```

### Learning Version of `Card`

```java
import java.util.Random;

public class Card {
    private int id;

    public Card() {
        id = new Random().nextInt(13) + 1;
    }

    @Override
    public String toString() {
        switch (id) {
            case 1:
                return "Ace";
            case 11:
                return "Jack";
            case 12:
                return "Queen";
            case 13:
                return "King";
            default:
                return Integer.toString(id);
        }
    }
}
```

### Line-by-Line Walkthrough

```java
@Override
```

This tells Java you are replacing a method that already exists in the parent `Object` class.

Every Java object has a default `toString()`, but the default is not very useful. You are creating a better one.

```java
public String toString()
```

This method returns a `String`, which means text.

```java
switch (id)
```

A `switch` chooses among several cases.

```java
case 1:
    return "Ace";
```

If `id` is 1, return `Ace`.

```java
default:
    return Integer.toString(id);
```

For 2 through 10, convert the number into text.

### Learning Version of `Main`

```java
public class Main {
    public static void main(String[] args) {
        Card card = new Card();
        System.out.println(card);
    }
}
```

Notice:

```java
System.out.println(card);
```

not:

```java
System.out.println(card.toString());
```

Both work, but Java automatically calls `toString()` for you.

### Common Mistakes

Mistake:

- Forgetting `return` in a `case`.

Mistake:

- Forgetting the `default` case.

Mistake:

- Returning `id` directly from `toString()`.

This is wrong because `id` is an `int`, but `toString()` must return a `String`:

```java
return id;
```

Correct:

```java
return Integer.toString(id);
```

### Verify the Step

Run the program several times.

Expected output examples:

```text
Ace
7
Queen
King
```

You should never see:

```text
1
11
12
13
```

## Part 7: Adding Suits

Original `directions.txt` step:

```text
6. Modify the program to give each card a suit. Use a second variable called "int suitId = new Random().nextInt(4) + 1;". Modify toString() to say, for example "Ace of Hearts". Treat 1 as Hearts, 2 as Diamonds, 3 as Spades, and 4 as Clubs. (Don't worry about drawing 2 of the same exact card; pretend we are playing with lots of combined decks.)
```

### Why This Step Exists

A real playing card has:

- Rank
- Suit

So far, you only have rank.

This step adds the second piece of information.

### Learning Version of `Card`

```java
import java.util.Random;

public class Card {
    private int id;
    private int suitId;

    public Card() {
        id = new Random().nextInt(13) + 1;
        suitId = new Random().nextInt(4) + 1;
    }

    @Override
    public String toString() {
        return getRankName() + " of " + getSuitName();
    }

    private String getRankName() {
        switch (id) {
            case 1:
                return "Ace";
            case 11:
                return "Jack";
            case 12:
                return "Queen";
            case 13:
                return "King";
            default:
                return Integer.toString(id);
        }
    }

    private String getSuitName() {
        switch (suitId) {
            case 1:
                return "Hearts";
            case 2:
                return "Diamonds";
            case 3:
                return "Spades";
            case 4:
                return "Clubs";
            default:
                return "Unknown";
        }
    }
}
```

### Why Split into Helper Methods?

This code has:

```java
private String getRankName()
```

and:

```java
private String getSuitName()
```

These are helper methods.

A helper method does a small job so the main method stays easy to read.

Now `toString()` is simple:

```java
return getRankName() + " of " + getSuitName();
```

That reads almost like English.

### Common Mistakes

Mistake:

- Using `nextInt(4)` without `+ 1`, which gives 0 through 3.

Correct:

```java
new Random().nextInt(4) + 1
```

Mistake:

- Accidentally mapping suit numbers inconsistently.

The original directions use:

```text
1 Hearts
2 Diamonds
3 Spades
4 Clubs
```

Be consistent.

### Verify the Step

Run several times.

Expected output examples:

```text
Ace of Hearts
9 of Clubs
Queen of Diamonds
King of Spades
```

## Part 8: Why the Learning Version Is Not Enough

The random `Card()` constructor was useful for learning, but it has a problem.

Suppose you want a specific card:

```text
Ace of Hearts
```

With the learning version, you cannot ask for that. The constructor always chooses randomly.

That is a problem for a real deck.

A real `Deck` should create:

```text
Ace of Hearts
Ace of Diamonds
Ace of Clubs
Ace of Spades
2 of Hearts
...
King of Spades
```

To build a real deck, `Card` must represent a specific rank and suit.

So now we refactor.

### What Is Refactoring?

Refactoring means changing the design of code without changing the overall goal.

You are not throwing away what you learned. You are improving the design.

The final Milestone 1 design will use:

- `Rank` enum instead of `int id`
- `Suit` enum instead of `int suitId`
- `Card(Rank rank, Suit suit)` instead of random `Card()`

## Part 9: Creating `Rank`

Original `directions.txt` step:

```text
34. In our Card class, id is an int. Invalid ints could be specified, and the result is undefined. For example, what does it mean for a card to have id = 100? "enum"s address this, by defining a Type with specifically allowed value. Create a type called Rank as follows:
public enum Rank
{
  ACE,
  TWO,
  THREE,
  FOUR,
  FIVE,
  SIX,
  SEVEN,
  EIGHT,
  NINE,
  TEN,
  JACK,
  QUEEN,
  KING
}
In Card, replace "int id" with a Rank instance variable. It's now impossible for a client to instantiate a Card with a bogus rank. A Card now comprises a Rank and a Suit.
```

### Why This Step Exists

An `int` can be almost any number:

```text
-100
0
14
999
```

But a card rank can only be one of thirteen values.

An enum solves that.

### What Is an Enum?

An enum is a type with a fixed list of allowed values.

For example:

```java
Rank.ACE
Rank.TWO
Rank.KING
```

You cannot accidentally create:

```java
Rank.PIZZA
```

because it is not in the enum.

### Original Simple `Rank`

The original directions first introduce:

```java
public enum Rank
{
  ACE,
  TWO,
  THREE,
  FOUR,
  FIVE,
  SIX,
  SEVEN,
  EIGHT,
  NINE,
  TEN,
  JACK,
  QUEEN,
  KING
}
```

That version is useful, but the next original direction makes it smarter.

Original `directions.txt` step:

```text
35. In most programming languages, an enum is just a constrained list of integers. E.g., ACE is 0 behind the scenes, TWO is 1, etc. To pick a random Rank, we would pick a random number between 0 and 13, and cast it to the Enum. However, in Java, enums are more powerful. They are really a set of Singleton classes! I.e., there's a class called ACE of type Rank. These enums can have fields and methods just like classes. Add private fields "int value" and "String name". Add 2 accessors: getValue and toString. Add 2 constructors, one that takes a value and another that takes a value and name. Update the enum list as follows:
ACE(11, "Ace"),
TWO(2),
THREE(3),
FOUR(4),
FIVE(5),
SIX(6),
SEVEN(7),
EIGHT(8),
NINE(9),
TEN(10),
JACK(10, "Jack"),
QUEEN(10, "Queen"),
KING(10, "King");
```

### Final `Rank.java` for This Milestone

Create:

```text
src/main/java/Rank.java
```

Complete code:

```java
public enum Rank {
    ACE(11, "A", "Ace"),
    TWO(2, "2", "2"),
    THREE(3, "3", "3"),
    FOUR(4, "4", "4"),
    FIVE(5, "5", "5"),
    SIX(6, "6", "6"),
    SEVEN(7, "7", "7"),
    EIGHT(8, "8", "8"),
    NINE(9, "9", "9"),
    TEN(10, "10", "10"),
    JACK(10, "J", "Jack"),
    QUEEN(10, "Q", "Queen"),
    KING(10, "K", "King");

    private final int value;
    private final String shortName;
    private final String displayName;

    Rank(int value, String shortName, String displayName) {
        this.value = value;
        this.shortName = shortName;
        this.displayName = displayName;
    }

    public int getValue() {
        return value;
    }

    public String getShortName() {
        return shortName;
    }

    @Override
    public String toString() {
        return displayName;
    }
}
```

### Line-by-Line Walkthrough

```java
public enum Rank {
```

This creates an enum named `Rank`.

```java
ACE(11, "A", "Ace"),
```

This creates the `ACE` enum constant and gives it:

- value `11`
- short name `"A"`
- display name `"Ace"`

```java
KING(10, "K", "King");
```

The last enum constant ends with a semicolon because fields and methods come after it.

```java
private final int value;
```

This stores the Blackjack value.

`final` means it cannot be changed after construction.

```java
Rank(int value, String shortName, String displayName) {
```

This is the enum constructor. It runs once for each enum constant.

```java
this.value = value;
```

`this.value` means the field.

`value` means the constructor parameter.

```java
public int getValue() {
    return value;
}
```

This accessor returns the Blackjack value.

```java
@Override
public String toString() {
    return displayName;
}
```

This controls how the rank appears as text.

### Verify `Rank`

Expected:

```java
Rank.ACE.getValue()      // 11
Rank.KING.getValue()     // 10
Rank.QUEEN.getShortName() // "Q"
Rank.THREE.toString()    // "3"
```

Common mistakes:

- Forgetting the semicolon after `KING(...)`
- Making enum fields non-final without a reason
- Returning `shortName` from `toString()` when you meant `displayName`

## Part 10: Creating `Suit`

Original `directions.txt` step:

```text
31. We will work with Suits. Suits have a symbol and a color. Make a Suit class with these attributes. For Color, import the type "import javafx.scene.paint.Color;". Symbol is a String. Also give the Suit class a String for its name. Add get accessors for the 3 fields. Add a constructor to set these fields: public Suit(String name, Color color, String symbol). Add a getSuit() accessor.
```

The original directions first tried a regular class with four predefined suit objects. Then they moved to an enum.

Original `directions.txt` step:

```text
38. The way we designed the Suit class is a normal technique for languages such as C++ or C#. But Java's enum power renders it unnecessary. Refactor Suit class to be an enum.
```

### Why `Suit` Should Be an Enum

There are exactly four suits.

You do not want invalid suits like:

```text
Stars
Moons
Pizza
```

So `Suit` should be an enum.

### Why `Suit` Uses JavaFX `Color`

Later, `CardFaceCreator` draws cards on the screen. It needs to know:

- Hearts are red.
- Diamonds are red.
- Clubs are black.
- Spades are black.

So each `Suit` stores a JavaFX `Color`.

### Final `Suit.java` for This Milestone

Create:

```text
src/main/java/Suit.java
```

Complete code:

```java
import javafx.scene.paint.Color;

public enum Suit {
    HEARTS("Hearts", Color.RED, "\u2665"),
    DIAMONDS("Diamonds", Color.RED, "\u2666"),
    CLUBS("Clubs", Color.BLACK, "\u2663"),
    SPADES("Spades", Color.BLACK, "\u2660");

    private final String name;
    private final Color color;
    private final String symbol;

    Suit(String name, Color color, String symbol) {
        this.name = name;
        this.color = color;
        this.symbol = symbol;
    }

    public String getName() {
        return name;
    }

    public Color getColor() {
        return color;
    }

    public String getSymbol() {
        return symbol;
    }

    @Override
    public String toString() {
        return name;
    }
}
```

### Line-by-Line Walkthrough

```java
import javafx.scene.paint.Color;
```

This imports JavaFX's color type.

```java
HEARTS("Hearts", Color.RED, "\u2665"),
```

This creates a suit with:

- name `"Hearts"`
- color `Color.RED`
- symbol `"\u2665"`

The `\u2665` text is a Unicode escape sequence. It represents the heart symbol.

```java
private final String symbol;
```

This stores the suit symbol.

```java
public String getSymbol() {
    return symbol;
}
```

This lets other classes ask the suit for its symbol.

`CardFaceCreator` will use this later.

### Verify `Suit`

Expected:

```java
Suit.HEARTS.getName()    // "Hearts"
Suit.SPADES.getColor()   // Color.BLACK
Suit.DIAMONDS.getSymbol() // diamond symbol
Suit.CLUBS.toString()    // "Clubs"
```

Compile checkpoint:

```powershell
mvn test
```

Now that `Rank` and `Suit` exist, `CardFaceCreator.java` should at least be able to find those types.

If Maven still fails, read the first error carefully. The first error is usually the most important.

## Part 11: Creating the Final `Card`

Original `directions.txt` step:

```text
36. Update Card to have a Rank instead of int rankId. Refactor most methods, including the constructor. isAce now looks like:
public boolean isAce()
{
  return rank == Rank.ACE;
}
```

### Why This Step Exists

The learning version used:

```java
private int id;
private int suitId;
```

That worked, but it allowed invalid values.

The final version uses:

```java
private Rank rank;
private Suit suit;
```

Now a card can only have a valid rank and suit.

### Final `Card.java` for This Milestone

Replace the learning version of `Card.java` with this final version:

```java
public class Card {
    private final Rank rank;
    private final Suit suit;

    public Card(Rank rank, Suit suit) {
        this.rank = rank;
        this.suit = suit;
    }

    public Rank getRank() {
        return rank;
    }

    public Suit getSuit() {
        return suit;
    }

    public int getValue() {
        return rank.getValue();
    }

    public boolean isAce() {
        return rank == Rank.ACE;
    }

    @Override
    public String toString() {
        return rank + " of " + suit;
    }
}
```

### Line-by-Line Walkthrough

```java
private final Rank rank;
private final Suit suit;
```

Each card stores exactly one rank and one suit.

`final` means the card cannot change from Ace of Hearts into 7 of Clubs later.

```java
public Card(Rank rank, Suit suit) {
```

This constructor requires the caller to specify exactly which card to create.

```java
this.rank = rank;
this.suit = suit;
```

These lines save the constructor inputs into the fields.

```java
public int getValue() {
    return rank.getValue();
}
```

The card's Blackjack value comes from its rank.

```java
public boolean isAce() {
    return rank == Rank.ACE;
}
```

Enums can be compared using `==`.

```java
return rank + " of " + suit;
```

This works because both `Rank` and `Suit` have `toString()` methods.

### Verify `Card`

Expected:

```java
new Card(Rank.ACE, Suit.HEARTS).toString()
```

returns:

```text
Ace of Hearts
```

Expected:

```java
new Card(Rank.KING, Suit.SPADES).getValue()
```

returns:

```text
10
```

## Part 12: Creating a Real Deck

Original `directions.txt` step:

```text
16. Make a real Deck with 52 cards. The deck class shall have a List of Cards. The constructor of the Deck class shall populate the Deck; use a nested for-loop to create 13 cards of each of the 4 suits, and add these to your List of Cards.
```

### Why This Step Exists

So far, a card can exist. But Blackjack does not create random cards from nothing.

Real Blackjack draws from a deck or shoe.

A standard deck contains:

```text
13 ranks * 4 suits = 52 cards
```

### What Is a List?

A `List` is a collection that stores items in order.

You can:

- Add items
- Remove items
- Ask how many items are inside
- Get an item by index

In Java, we often write:

```java
List<Card> cards = new ArrayList<>();
```

This means:

- The variable type is `List<Card>`.
- It stores `Card` objects.
- The actual object is an `ArrayList`.

### What Are Generics?

The angle brackets tell Java what kind of thing the list stores:

```java
List<Card>
```

This is a generic type.

It prevents mistakes like adding a `String` to a list of cards.

### Final `Deck.java` for This Milestone

Create:

```text
src/main/java/Deck.java
```

Complete code:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class Deck {
    private final List<Card> cards = new ArrayList<>();
    private final Random random = new Random();

    public Deck() {
        for (Rank rank : Rank.values()) {
            for (Suit suit : Suit.values()) {
                cards.add(new Card(rank, suit));
            }
        }
    }

    public int size() {
        return cards.size();
    }

    public Card drawCard() {
        if (cards.isEmpty()) {
            return null;
        }

        int index = random.nextInt(cards.size());
        return cards.remove(index);
    }
}
```

### Line-by-Line Walkthrough

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Random;
```

These imports give you:

- `ArrayList` for storing cards
- `List` as the general list type
- `Random` for choosing a random card

```java
private final List<Card> cards = new ArrayList<>();
```

This deck owns a list of cards.

```java
private final Random random = new Random();
```

This deck owns one random number generator.

That is better than creating a new `Random` every time you draw.

```java
for (Rank rank : Rank.values()) {
```

Loop through every rank.

```java
for (Suit suit : Suit.values()) {
```

For each rank, loop through every suit.

```java
cards.add(new Card(rank, suit));
```

Create one card for that rank/suit pair and add it to the deck.

```java
public int size() {
    return cards.size();
}
```

This tells other classes how many cards remain.

```java
if (cards.isEmpty()) {
    return null;
}
```

If the deck has no cards, there is nothing to draw.

```java
int index = random.nextInt(cards.size());
```

Choose a valid random index.

```java
return cards.remove(index);
```

Remove and return the card.

This is important. If you only returned the card without removing it, the same exact card could be drawn again from the same deck.

### Verify `Deck`

Expected:

```java
Deck deck = new Deck();
deck.size(); // 52
```

After:

```java
Card card = deck.drawCard();
```

Expected:

```java
card != null
deck.size() == 51
```

After 52 draws:

```java
deck.size() == 0
deck.drawCard() == null
```

## Part 13: Drawing from a Deck

Original `directions.txt` step:

```text
17. Add a drawCard() method to the Deck class to remove a random Card from the deck, rather than creating a new card. (Use the "remove()" method of the List interface.). Update Main to use the deck. Handle the case of an empty deck by returning null. When the deck is empty, create a new deck (simulating shuffling) and gracefully continue. This is not how real blackjack works, because of a cut card. We'll fix that later.
```

You already added:

```java
public Card drawCard()
```

The important concept is this:

```java
cards.remove(index)
```

does two jobs:

1. Removes the card from the list.
2. Returns the removed card.

### Why Not Just Create a New Random Card?

If you create a random card each time, you might draw:

```text
Ace of Hearts
Ace of Hearts
Ace of Hearts
```

from the same deck.

That is impossible in one real deck.

A deck must draw without replacement. "Without replacement" means once a card is drawn, it is gone until the deck is reset.

### Common Random Index Mistake

Wrong:

```java
random.nextInt(cards.size() - 1)
```

If `cards.size()` is 52, this gives indexes 0 through 50. It never chooses index 51.

Correct:

```java
random.nextInt(cards.size())
```

If `cards.size()` is 52, this gives indexes 0 through 51.

## Part 14: The Cut Card Idea

Original `directions.txt` step:

```text
18. A cut card is an index in the Shoe. When the cut card is drawn the shoe is reshuffled after the round completes. Simulate a cut card. Use a position of 12 cards from the bottom per deck (so a cut card position for a shoe size of 8 is 96 cards from the bottom). At the end of each round, if too few cards remain, reshuffle the shoe before the next hand. This practically eliminates the possiblity of running out of cards mid round.
```

### Why This Step Exists

Casinos do not usually deal until the very last card.

They use a cut card to mark when the shoe should be reshuffled soon.

For this project:

```text
cut-card threshold = 12 cards per deck
```

For 8 decks:

```text
12 * 8 = 96
```

When the shoe has 96 or fewer cards left, the game should replace the shoe before the next round.

You will implement that in `ShoeMgr`.

## Part 15: Creating a Shoe

Original `directions.txt` step:

```text
19. In Blackjack, they typically play with multiple decks (called a Shoe). Create a Shoe class in a new file. The Shoe contains a List of Decks. The constructor shall take a "deckCount" and populate the List of Decks with new Decks. (Typically, a Shoe contains 6 or 8 Decks.)
```

### Why This Step Exists

Blackjack often uses several decks mixed together.

That group of decks is called a shoe.

In this project, the default shoe has 8 decks.

```text
8 decks * 52 cards = 416 cards
```

### Final `Shoe.java` for This Milestone

Create:

```text
src/main/java/Shoe.java
```

Complete code:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class Shoe {
    private final List<Deck> decks = new ArrayList<>();
    private final Random random = new Random();

    public Shoe(int deckCount) {
        for (int i = 0; i < deckCount; i++) {
            decks.add(new Deck());
        }
    }

    public int size() {
        int total = 0;

        for (Deck deck : decks) {
            total += deck.size();
        }

        return total;
    }

    public Card draw() {
        int totalSize = size();

        if (totalSize == 0) {
            return null;
        }

        int randomIndex = random.nextInt(totalSize);
        Deck selectedDeck = determineDeck(randomIndex);
        return selectedDeck.drawCard();
    }

    private Deck determineDeck(int randomIndex) {
        for (Deck deck : decks) {
            if (randomIndex < deck.size()) {
                return deck;
            }

            randomIndex -= deck.size();
        }

        throw new IllegalStateException("Could not determine deck for index.");
    }
}
```

### Line-by-Line Walkthrough

```java
private final List<Deck> decks = new ArrayList<>();
```

A shoe owns several decks.

```java
public Shoe(int deckCount) {
```

The constructor accepts the number of decks to create.

```java
for (int i = 0; i < deckCount; i++) {
    decks.add(new Deck());
}
```

This loop creates the requested number of decks.

```java
public int size() {
```

This method counts how many cards are left across all decks.

```java
for (Deck deck : decks) {
    total += deck.size();
}
```

Ask each deck how many cards it has, then add them together.

```java
int randomIndex = random.nextInt(totalSize);
```

Pick a random card position across the whole shoe.

```java
Deck selectedDeck = determineDeck(randomIndex);
```

Find which deck owns that random position.

```java
return selectedDeck.drawCard();
```

Draw from that deck.

## Part 16: The Biased Shoe Algorithm Problem

Original `directions.txt` step:

```text
20. Implement a draw() method to draw a random Card from the Shoe. In the real world, the Shoe would be shuffled, then draw would always remove the top card. It is easier to implement drawing a random card from the unshuffled shoe, as implementing a shuffle() method is inefficient. Choose a random Deck, and then draw() from that Deck. If the Deck is empty, or if the Shoe is empty, return null. (Note that this algorithm is flawed. Pause and see if you can figure out the problem. We'll fix it later.)
```

Original `directions.txt` step:

```text
22. The algorithm for drawing a random Card is flawed. Why? Imagine a Shoe with 2 Decks. Imagine that draw() randomly draws from the 2nd Deck 51 times in a row (of course, this is statistically unlikely); the 2nd Deck has only 1 Card left in it, while the 1st Deck is still full. The next time draw() is called, we are equally likely to draw from the 2nd Deck (with 1 card in it) as we are from the 1st Deck (with 52 cards in it). I.e., Cards in smaller Decks are more likely to be drawn, which doesn't reflect the real world; in the real world, each Card in the Shoe has an equal likelihood of being drawn. Let's fix the algorithm. Create a size() method in Shoe, which determines the number of cards remaining in all the Decks in the Shoe. Then choose a random number from that count. That's our index. Be careful of boundary conditions. Now create a private helper method called determineDeck(randomIndex), which returns the Deck corresponding to the randomly chosen index. To do this, loop through each Deck. If index is less than the Deck's size, this is our Deck. If not, decrement the Deck size from index, and try again on the next Deck. Once the correct Deck is found, it's actually not necessary to draw the exact index Card from that Deck; we can simply call Deck.draw() to get a random Card from that Deck. Stated differently, the random index is only needed to select the Deck (essentially via a weighted average), not to select an exact Card within a Deck.
```

### Why This Is a Great Algorithm Lesson

At first, it seems reasonable to choose a random deck, then choose a random card from that deck.

But that is biased.

Imagine:

```text
Deck 1 has 52 cards.
Deck 2 has 1 card.
```

If you choose randomly between decks:

```text
Deck 1 has a 50% chance.
Deck 2 has a 50% chance.
```

That gives the one lonely card in Deck 2 way too much chance of being drawn.

Instead, every card should have equal chance.

The weighted index method fixes this.

Mental model:

```text
Deck 1: indexes 0 through 51
Deck 2: index 52
```

Now Deck 1 is selected for 52 possible indexes, and Deck 2 is selected for 1 possible index.

That matches the number of cards in each deck.

### Verify `Shoe`

Expected:

```java
new Shoe(1).size() // 52
new Shoe(8).size() // 416
```

After one draw from an eight-deck shoe:

```java
size() // 415
```

## Part 17: Creating `ShoeMgr`

Original `directions.txt` step:

```text
25. We have some Shoe management logic spread between Round and main(). Consolidate in a ShoeMgr class. The class has a shoe member variable and 3 methods: a constructor - public ShoeMgr(int deckCount), public Card drawCard(), and public void checkCutCard(). Upon completion, all access to Deck is via Shoe (i.e. Shoe encapsulates Deck), and ShoeMgr encapsulates Shoe.
```

### Why This Step Exists

`Shoe` represents the cards.

`ShoeMgr` manages the active shoe.

That means `ShoeMgr` is responsible for:

- Creating the shoe
- Letting other classes draw cards
- Replacing the shoe after the cut-card threshold

This keeps shoe-management logic in one place.

### Final `ShoeMgr.java` for This Milestone

Create:

```text
src/main/java/ShoeMgr.java
```

Complete code:

```java
public class ShoeMgr {
    private final int deckCount;
    private Shoe shoe;

    public ShoeMgr(int deckCount) {
        this.deckCount = deckCount;
        this.shoe = new Shoe(deckCount);
    }

    public Card drawCard() {
        return shoe.draw();
    }

    public int size() {
        return shoe.size();
    }

    public void checkCutCard() {
        int cutCardThreshold = 12 * deckCount;

        if (shoe.size() <= cutCardThreshold) {
            shoe = new Shoe(deckCount);
        }
    }
}
```

### Line-by-Line Walkthrough

```java
private final int deckCount;
```

The manager remembers how many decks belong in a fresh shoe.

```java
private Shoe shoe;
```

This is the current active shoe.

It is not `final` because it can be replaced.

```java
public ShoeMgr(int deckCount) {
```

The constructor receives the number of decks.

```java
this.shoe = new Shoe(deckCount);
```

Create the first shoe.

```java
public Card drawCard() {
    return shoe.draw();
}
```

Other classes will call `ShoeMgr.drawCard()` instead of touching `Shoe` directly.

```java
int cutCardThreshold = 12 * deckCount;
```

Compute the cut-card threshold.

For 8 decks:

```text
12 * 8 = 96
```

```java
if (shoe.size() <= cutCardThreshold) {
    shoe = new Shoe(deckCount);
}
```

If too few cards remain, replace the shoe.

### Verify `ShoeMgr`

Expected:

```java
ShoeMgr shoeMgr = new ShoeMgr(8);
shoeMgr.size(); // 416
```

After one draw:

```java
shoeMgr.drawCard();
shoeMgr.size(); // 415
```

After drawing down to 96 or fewer cards:

```java
shoeMgr.checkCutCard();
shoeMgr.size(); // 416
```

## Part 18: Testing

The original directions introduce formal unit testing later, but for this repository we will start testing early.

### Why Test Now?

You are building foundation classes.

If `Deck` is wrong, the whole game will be wrong later.

Unit tests help you catch mistakes while the code is still small.

### What Is JUnit?

JUnit is a Java testing library.

A test is a method that checks whether your code behaves as expected.

Example:

```java
assertEquals(52, deck.size());
```

This means:

```text
I expect deck.size() to be 52.
```

If it is not 52, the test fails.

### Create `CardTest.java`

Create:

```text
src/test/java/CardTest.java
```

Complete code:

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class CardTest {
    @Test
    public void aceHasValueEleven() {
        Card card = new Card(Rank.ACE, Suit.HEARTS);

        assertEquals(11, card.getValue());
    }

    @Test
    public void kingHasValueTen() {
        Card card = new Card(Rank.KING, Suit.SPADES);

        assertEquals(10, card.getValue());
    }

    @Test
    public void aceIsAce() {
        Card card = new Card(Rank.ACE, Suit.CLUBS);

        assertTrue(card.isAce());
    }

    @Test
    public void nonAceIsNotAce() {
        Card card = new Card(Rank.TEN, Suit.DIAMONDS);

        assertFalse(card.isAce());
    }

    @Test
    public void cardHasReadableText() {
        Card card = new Card(Rank.ACE, Suit.HEARTS);

        assertEquals("Ace of Hearts", card.toString());
    }
}
```

### Test Walkthrough

```java
import org.junit.jupiter.api.Test;
```

This imports the `@Test` annotation.

An annotation gives extra information to Java or a tool.

`@Test` tells JUnit:

```text
Run this method as a test.
```

```java
assertEquals(11, card.getValue());
```

This compares:

- expected value: `11`
- actual value: `card.getValue()`

### Create `DeckTest.java`

Create:

```text
src/test/java/DeckTest.java
```

Complete code:

```java
import org.junit.jupiter.api.Test;

import java.util.HashSet;
import java.util.Set;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertNull;

public class DeckTest {
    @Test
    public void newDeckHasFiftyTwoCards() {
        Deck deck = new Deck();

        assertEquals(52, deck.size());
    }

    @Test
    public void drawingOneCardReducesDeckSize() {
        Deck deck = new Deck();

        Card card = deck.drawCard();

        assertNotNull(card);
        assertEquals(51, deck.size());
    }

    @Test
    public void drawingAllCardsProducesFiftyTwoUniqueCards() {
        Deck deck = new Deck();
        Set<String> drawnCards = new HashSet<>();

        for (int i = 0; i < 52; i++) {
            Card card = deck.drawCard();

            assertNotNull(card);
            drawnCards.add(card.toString());
        }

        assertEquals(52, drawnCards.size());
        assertEquals(0, deck.size());
    }

    @Test
    public void drawingFromEmptyDeckReturnsNull() {
        Deck deck = new Deck();

        for (int i = 0; i < 52; i++) {
            deck.drawCard();
        }

        assertNull(deck.drawCard());
    }
}
```

### Why Use a `Set`?

A `Set` is like a list that refuses duplicates.

If you add the same card text twice, the set only keeps one copy.

So after drawing all 52 cards:

```java
assertEquals(52, drawnCards.size());
```

proves there were no duplicate card names.

### Create `ShoeTest.java`

Create:

```text
src/test/java/ShoeTest.java
```

Complete code:

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertNull;

public class ShoeTest {
    @Test
    public void oneDeckShoeStartsWithFiftyTwoCards() {
        Shoe shoe = new Shoe(1);

        assertEquals(52, shoe.size());
    }

    @Test
    public void eightDeckShoeStartsWithFourHundredSixteenCards() {
        Shoe shoe = new Shoe(8);

        assertEquals(416, shoe.size());
    }

    @Test
    public void drawingOneCardReducesShoeSize() {
        Shoe shoe = new Shoe(8);

        Card card = shoe.draw();

        assertNotNull(card);
        assertEquals(415, shoe.size());
    }

    @Test
    public void emptyShoeReturnsNull() {
        Shoe shoe = new Shoe(1);

        for (int i = 0; i < 52; i++) {
            assertNotNull(shoe.draw());
        }

        assertNull(shoe.draw());
    }
}
```

### Create `ShoeMgrTest.java`

Create:

```text
src/test/java/ShoeMgrTest.java
```

Complete code:

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class ShoeMgrTest {
    @Test
    public void managerStartsWithFullShoe() {
        ShoeMgr shoeMgr = new ShoeMgr(8);

        assertEquals(416, shoeMgr.size());
    }

    @Test
    public void drawingThroughManagerReducesShoeSize() {
        ShoeMgr shoeMgr = new ShoeMgr(8);

        shoeMgr.drawCard();

        assertEquals(415, shoeMgr.size());
    }

    @Test
    public void checkCutCardReplacesLowShoe() {
        ShoeMgr shoeMgr = new ShoeMgr(1);

        while (shoeMgr.size() > 12) {
            shoeMgr.drawCard();
        }

        shoeMgr.checkCutCard();

        assertEquals(52, shoeMgr.size());
    }

    @Test
    public void checkCutCardDoesNotReplaceTooEarly() {
        ShoeMgr shoeMgr = new ShoeMgr(1);

        shoeMgr.drawCard();
        shoeMgr.checkCutCard();

        assertEquals(51, shoeMgr.size());
    }
}
```

### Run Tests

Run:

```powershell
mvn test
```

Expected result:

- All tests pass.

If tests fail:

1. Read the first failure.
2. Look at the expected value.
3. Look at the actual value.
4. Find the smallest piece of code that could cause that difference.

## Part 19: Final JavaFX Check

Run:

```powershell
mvn clean javafx:run
```

Expected result:

- A simple JavaFX window opens.

This proves:

- Maven can compile the main project.
- JavaFX is wired correctly.
- `CardFaceCreator.java` can compile with your `Rank` and `Suit`.

## Common Beginner Mistakes

### Putting Too Much in `Main`

`Main` should start the application. It should not own deck logic or shoe logic.

Good mental model:

```text
Main starts the program.
Deck manages cards.
Shoe manages decks.
ShoeMgr manages the active shoe.
```

### Keeping the Random `Card()` Constructor

The first `Card()` constructor was a learning step.

The final `Card` should not randomly choose its rank and suit.

Good:

```java
new Card(Rank.ACE, Suit.HEARTS)
```

Avoid in the final model:

```java
new Card()
```

### Forgetting That Java Is Case-Sensitive

These are different:

```java
HEARTS
Hearts
hearts
```

Enum constants should use all caps:

```java
HEARTS
SPADES
```

### Forgetting `values()`

Use:

```java
for (Rank rank : Rank.values())
```

and:

```java
for (Suit suit : Suit.values())
```

This is how you loop through every enum constant.

### Creating Fewer Than 52 Cards

You need both loops:

```text
13 ranks * 4 suits = 52 cards
```

If your deck has 13 cards, you probably forgot suits.

If your deck has 4 cards, you probably forgot ranks.

### Returning Without Removing

`drawCard()` must remove the card.

Wrong idea:

```java
return cards.get(index);
```

Correct idea:

```java
return cards.remove(index);
```

### Off-by-One Errors

Valid indexes for a list of 52 cards:

```text
0 through 51
```

Correct:

```java
random.nextInt(cards.size())
```

### Biased Shoe Drawing

Do not choose randomly among decks equally if the decks have different numbers of cards remaining.

Use the weighted random index method.

### Mixing UI and Model

Do not put JavaFX card nodes inside:

- `Card`
- `Deck`
- `Shoe`
- `ShoeMgr`

These are model classes. They should be testable without opening a window.

`Suit` uses JavaFX `Color` only because `CardFaceCreator` needs it later.

## Milestone 1 Completion Checklist

You are done when:

- `Main.java` starts a minimal JavaFX window.
- `Rank.java` exists and passes rank tests.
- `Suit.java` exists and supports symbols/colors.
- `Card.java` represents a specific rank and suit.
- `Deck.java` creates 52 unique cards.
- `Deck.drawCard()` removes cards.
- `Shoe.java` supports one or more decks.
- `Shoe.draw()` uses weighted deck selection.
- `ShoeMgr.java` owns and replaces the active shoe at the cut-card threshold.
- Unit tests cover the core model.
- `mvn test` passes.
- `mvn clean javafx:run` opens the minimal app.

Do not implement hands, hand scoring, dealer play, betting, split logic, or JavaFX card rendering yet. Those belong to later milestones.



