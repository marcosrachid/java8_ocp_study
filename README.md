# JAVA 8 OCP STUDY TIPS

Tips for OCP exam

## Java Class Design

Access modifiers: public, protected, private and default(package-private):

Can access  | private  | default(package-private)  | protected  | public
------------- | ------------- | ------------- | ------------- | -------------
Member in the same class  | Y  | Y  | Y  | Y
Member in another class in the same package  | N  | Y  | Y  | Y
Member in a superclass in a different package  | N  | N  | Y  | Y
Method/field in a class (that is not superclass) in a different package | N  | N  | N  | Y

Overloaded methods rules:

* methods must have the same name.
* methods must have different argument list.
* method can or cannot have different return types.
* method can or cannot have a different access modifier.
* method can or cannot have a different checked or unchecked exception.

Overridden method rules:

* methods must have the same name.
* methods must have the same arguments.
* methods must have the same return type or its covariants. 
* methods must not restrict more the access, but can change to a broader access modifier(if parent --> protected then child --> private is not allowed)(if parent --> protected then child --> public is allowed).
* methods must not throw new or broader exception but can remove(if parent --> throws RuntimeException --> child throws Exception is not allowed)(if parent --> throws RuntimeException --> child does not throw nothing is allowed).
* only inherited methods can be overridden.
* Constructors and private methods are not inherited, so they cannot be overridden.
* Abstract methods must be overridden by the first concrete (non-abstract) subclass.
* final methods cannot be overridden.
* A subclass can use super.overridden_method() to call the superclass version of an overridden method.

Abstract classes are non instantiable, only extendable classes that can define abstract methods that will be implemented on the concrete class.

static modifier makes a variable shared between every instance of a class at class level and uses the class name to refer to a method.

final modifier prevents a variable from changing and a method from being overriden.

"import" key word to import usable class and "import static" to import static methods from classes.

"instanceof" key word is used to find if an object is of a specific type or child of a specific type.

virtual methods are methods from interfaces or abstract classes that can use the implementation of its sons without the need of specifying the concrete class.

@Override annotation force a method to be and overridden method, otherwise will it will not compile.

"toString()" a native method from Object class that every class has in its structure, it is called when trying to cast an object as String or print the object on console. It can be overriden in the class and return a custom string format for the class.

"equals()" is the substitute for "==" on objects, it returns a boolean based on the implementation of equals to know if two objects are alike on the object context.

"equals()" constract:

* It is reflexive: for any non-null reference value x, x.equals(x) should return true;
* It is symmetric: for any non=null reference values x and y, x.equals(y) should return true if and only if y.equals(x) is true;
* It is transitive: for any non-null reference x, y and z, if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) should return true;
* It is consistent: for any non-null reference values x and y, multiple invocations of x.equals(y) consistently return true or consistently return false, provided no information used in equals comparisons on the objects is modified;
* For any non-null reference value x, x.equals(null) should return false.

"hashCode()" is a number that puts instances of a class into finite number of categories.

"hashCode()" contract:

* Within the same program, the result of hashCode() must not change. This means that you shouldn't include variables that change in figuring out the hash code.
* If "equals()" returns true when called with two objects, calling hashCode() on each of those objects must return the same result.
* If "equals()" returns false when called with two objects, calling hashCode() on each of those objects does not have to return a different result.

encapsulation is the idea of combining fields and methods in a class such that the methods operate on the data, as opposed to the users of the class accessing the fields directly. In Java, it is commonly implementated with private instance members that hae public methods to retriee or modify data, commonly referred to as getters and setters, respectively.

"is-a" relationship is a principle that identifies a class A being a sub-class of B, also known as inheritance test, and can be found throught "instanceof" test.

"has-a" relationship is a principle that identifies if a class A contains a property or value B, B being a object or primitive. This relationship is also known as the object composition test.

polymorphism is the ability of a single interface to support multiple underlying forms. Ex:
```
public interface Animal { void roar(); }

public class Dog implements Animal {
	public void roar() {
		System.out.println("barf!");
	}
}

public class Cat implements Animal {
	public void roar() {
		System.out.println("meow!");
	}
}

public class Nature {
	public void checkRoar(Animal animal) {
		animal.roar();
	}
	public static void main(String[] args) {
		Nature nature = new Nature();
		nature.checkRoar(new Dog()); // barf!
		nature.checkRoar(new Cat()); // meow!
	}
}
```

"singleton" is the design pattern that guarantees that a class will have just a single instance on program.

lazy instantiation of singleton, means that the instance of singleton will only be created when "getInstance()" or the instance return method defined is called for the first time.

Immutable objects are read-only objects that the state does not change after they are created:
* Use constructor to set all properties of the object;
* Mark all of the instance variable private and final;
* Don't define any setter methods;
* Don't allow referenced mutable objects to be modified or accessed directly;
* Prevent methods from being overriden.

## Advanced Java Class Design

enum is acceptable on switch, but case options must always be only from enum type. Notice that an int is not an enum type.

enum accepts abstract methods, but it must be implemented in every enum type.

effectively final are local variables that althought they are not stated with final modifier, it does not change its state throught execution.

nested class is a class that is defined within another class.

compilation generates a class file on format Outer$Inner.class.

private interfaces are legal as nested inner.

nested class types:

* "inner class" is a class defined at the same leel as instance ariables and is not static:
	* Can be declared as public, priate, protected or default;
	* Can extend any class and implement interfaces;
	* Can be abstract or final;
	* Cannot declare static fields or methods;
	* Can access members of the outer class including private members.
	
```
public class A {
	private interface Example {
		public void ok();
	}
	private int x = 10;
	class B {
		private int x = 20;
		class C {
			private int x = 30;
			public void allX() {
				System.out.println(x); //30
				System.out.println(this.x); //30
				System.out.println(B.this.x); //20
				System.out.println(A.this.x); //10
			}
		}
	}
	public static void main(String[] args) {
		A a = new A();
		A.B b = a.new B();
		A.B.C c = b.new C();
		c.allX();
	}
}
```
	
* "local inner class" is a class defined within a method:
	* They do not have an access specifier;
	* They cannot be declared static and cannot declare static fields or methods;
	* They have access to all fields and methods of the enclosing class;
	* They do not have access to local variables of a method unless those variables are final or effectively final;

* "anonymous inner class" is a special case of a local inner class(or interface) that does not have a name:

```
	public class A {
		private interface B {
			void whatever();
		}
		public void exec() {
			B b = new B() {
				public void whatever() {
					System.out.println("whatever");
				}
			};
		}
	}
```

* "static nested class" is a static class defined at the same level as static variables:
	* The nesting creates a namespace because the enclosing class name must be used to refer to it;
	* It can be made private or use one of the other access modifiers to encapsulate it;
	* The enclosing class can refer to the fields and methods of the static nested class.
	
```
	public class Enclosing {
		static class Nested {
			private int price = 6;
		}
		public static void main(String[] args) {
			Nested nested = new Nested();
			System.out.println(nested.price);
		}
	}
```

## Generics and Collections

Generics allow you to write and use parameterized types, for example you can specify that you want a ArrayList of String objects. ex:
```
public class A&lt;T&gt; {
	T obj;
}
```

It's possible to use generic to a specific method. ex:
```
...
	public static &lt;T&gt; void call(T t) {}
...
```
calling:
```
...
	A.<String>call("Something");
...
```

Bounds are useful to specify a generic type as a restricted type throught a wildcard position.

Bounds are only used on declarations.

Type of bound  | Syntax  | Example
------------- | ------------- | -------------
Unbounded wildcard  | ?  | List<?> l = new ArrayList<String>();
Wildcard with an upper bound  | ? extends type  | List<? extends Exception> l = new ArrayList<RuntimeException>();
Wildcard with lower bound  | ? super type  | List<? super Exception> l = new ArrayList<ObjecT&gt;();

Collection interface has three main sub-interfaces: List, Set and Queue.

LinkedList implements both List and Queue.

ArrayDeque methods:

Method  | Description  | for queue  | For stack
------------- | ------------- | ------------- | -------------
boolean add(E e) | Adds an element to the back of the queue and returns true or throws exception | Y | N
E element() | Returns next element or throws an exception if empty queue | Y | N
boolean offer(E e) | Adds an element to the back of the queue and returns wherer successful | Y | N
E remove() | Removes and returns next element or throws an exception if empty queue | Y | N
void push(E e) | Adds an element to the front of the queue | Y | Y
E poll() | Removes and returns next element or returns null if empty queue | Y | N
E peek() | Returns next element or returns null if empty queue | Y | Y
E pop() | Removes and returns next element or throws an exception if empty queue | N | Y

Collection and Map Classes attributes:

Type  | Framework Interface | Sorted? | Calls hashCode()? | Calls compareTo()?
------------- | ------------- | ------------- | ------------- | -------------
ArrayList | List | N | N | N
ArrayDeque | Queue | N | N | N
HashMap | Map | N | Y | N
HashSet | Set | N | Y | N
Hashtable | Map | N | Y | N
LinkedList | List, Queue | N | N | N
Stack | List | N | N | N
TreeMap | List | Y | N | Y
TreeSet | Set | Y | N | Y
Vector | List | N | N | N

Both Comparable and Comparator are Functional Interfaces.

Difference  | Comparable | Comparator
------------- | ------------- | -------------
Package Name | java.lang | java.util
Interface must be implemented by class comparing | Y | N
Method name in interface | compareTo | compare
Number of parameters | 1 | 2
Common to declare using a lambda | N | Y

Methods from collection that uses lambda:
* removeIf(Predicate<? super E> filter)
* replaceAll(UnaryOperator<E> o)
* forEach(Consumer<E> c)

## Lambda Built-in Functional Interfaces

Built-in Functional Interfaces

Functional Interfaces | Parameters | Return Type | Single Abstract Method
------------- | ------------- | ------------- | -------------
Supplier&lt;T&gt; | 0 | T | get
Consumer&lt;T&gt; | 1(T) | void | accept
BiConsumer&lt;T, U&gt; | 2(T, U) | void | accept
Predicate&lt;T&gt; | 1(T) | boolean | test
BiPredicate&lt;T, U&gt; | 2(T, U) | boolean | test
Function&lt;T, R&gt; | 1(T) | R | apply
BiFunction&lt;T, U, R&gt; | 2(T, U) | R | apply
UnaryOperator&lt;T&gt; | 1(T) | T | apply
BinaryOperator&lt;T&gt; | 2(T, T) | T | apply

Optional methods

Method | When Optional is Empty | When Optional Contains a Value
------------- | ------------- | -------------
get() | Throws an exception | Returns value
ifPresent(Consumer c) | Does nothing | Calls Consumer c with value
isPresent() | Returns false | Returns true
orElse(T other) | Returns other parameter | Returns value
orElseGet(Supplier s) | Returns result of calling Supplier | Returns value
orElseThrow(Supplier s) | Throws exception created by calling Supplier | Returns value

There are three parts to a stream pipeline:
* Source: Where the stream comes from;
* Intermediate operations: Transforms the stream into another one. There can be as few or as many intermediate operations as you'd like. Since streams use lazy evaluation, the intermediate operations do not run until the terminal operation runs;
* Terminal operation: Actually produces a result. Since streams can be used only once, the stream is no longer valid after a terminal operation completes.

Terminal stream operations

Method | What Happens for Infinite Streams | Return Value | Reduction
------------- | ------------- | ------------- | -------------
allMatch()/anyMatch()/noneMatch() | Sometimes terminates | boolean | N
collect() | Does not terminate | varies | Y
count() | Does not terminate | long | Y
findAny()/findFirst() | Terminates | Optional&lt;T&gt; | N
forEach() | Does not terminate | void | N
min()/max() | Does not terminate | Optional&lt;T&gt; | Y
reduce() | Does not terminate | varies | Y

There are three types of primitive streams:
* IntStream: Used for the primitive types int, short, byte and char;
* LongStream: Used for the primitive type long;
* DoubleStream: Used for the primitive types double and float.

Mapping methods between types of streams

Source Stream Class | To Create Stream | To Create DoubleStream | To Create IntStream | To Create LongStream
------------- | ------------- | ------------- | ------------- | -------------
Stream | map | mapToDouble | mapToInt | mapToLong
DoubleStream | mapToObj | map | mapToInt | mapToLong
IntStream | mapToObj | mapToDouble | map | mapToLong
LongStream | mapToObj | mapToDouble | mapToInt | map

Function parameters when mapping between types of streams

Source Stream Class | To Create Stream | To Create DoubleStream | To Create IntStream | To Create LongStream
------------- | ------------- | ------------- | ------------- | -------------
Stream | Function | ToDoubleFunction | ToIntFunction | ToLongFunction
DoubleStream | DoubleFunction | DoubleUnaryOperator | DoubleToIntFunction | DoubleToLongFunction
IntStream | IntFunction | IntToDoubleFunction | IntUnaryOperator | IntToLongFunction
LongStream | LongFunction | LongToDoubleFunction | LongToIntFunction | LongUnaryOperator

Optional types for primitives

Description | OptionalDouble | OptionalInt | OptionalLong
------------- | ------------- | ------------- | -------------
Getting as a primitive | getAsDouble() | getAsInt() | getAsLong()
orElseGet() parameter type | DoubleSupplier | IntSupplier | LongSupplier
Return type of max() | OptionalDouble | OptionalInt | OptionalLong
Return type of sum() | double | int | long
Return type of avg() | OptionalDouble | OptionalDouble | OptionalDouble

We have three Summary Statistics class that comes from primitive streams by calling method "summaryStatistics()", which perform many calculations about the stream, including minimum, maximum, average, size and the number of alues i the stream, they are:
* IntSummaryStatistics
* LongSummaryStatistics
* DoubleSummaryStatistics

Commom functional interfaces for primitives

Functional Interfaces | Parameters | Return Type | Single Abstract Method
------------- | ------------- | ------------- | -------------
DoubleSupplier | 0 | double | getAsDouble
IntSupplier | 0 | int | getAsInt
LongSupplier | 0 | long | getAsLong
DoubleConsumer | 1(double) | void | accept
IntConsumer | 1(int) | void | accept
LongConsumer | 1(long) | void | accept
DoublePredicate | 1(double) | boolean | test
IntPredicate | 1(int) | boolean | test
LongPredicate | 1(long) | boolean | test
DoubleFunction(R) | 1(double) | R | apply
IntFunction(R) | 1(int) | R | apply
LongFunction(R) | 1(long) | R | apply
DoubleUnaryOperator | 1(double) | double | applyAsDouble
IntUnaryOperator | 1(int) | int | applyAsInt
LongUnaryOperator | 1(long) | long | applyAsLong
DoubleBinaryOperator | 2(double, double) | double | applyAsDouble
IntBinaryOperator | 2(int, int) | int | applyAsInt
LongBinaryOperator | 2(long, long) | long | applyAsLong

Primitive-specific functional interfaces

Functional Interfaces | Parameters | Return Type | Single Abstract Method
------------- | ------------- | ------------- | -------------
ToDoubleFunction&lt;T&gt; | 1(T) | double | applyAsDouble
ToIntFunction&lt;T&gt; | 1(T) | int | applyAsInt
ToLongFunction&lt;T&gt; | 1(T) | long | applyAsLong
ToDoubleBiFunction&lt;T, U&gt; | 2(T, U) | double | applyAsDouble
ToIntBiFunction&lt;T, U&gt; | 2(T, U) | int | applyAsInt
ToLongBiFunction&lt;T, U&gt; | 2(T, U) | long | applyAsLong
DoubleToIntFunction | 1(double) | int | applyAsInt
DoubleToLongFunction | 1(double) | long | applyAsInt
IntToDoubleFunction | 1(int) | double | applyAsDouble
IntToLongFunction | 1(int) | long | applyAsLong
LongToDoubleFunction | 1(long) | double | applyAsDouble
LongToIntFunction | 1(long) | int | applyAsInt
ObjDoubleConsumer&lt;T&gt; | 2(T, double) | void | accept
ObjIntConsumer&lt;T&gt; | 2(T, int) | void | accept
ObjLongConsumer&lt;T&gt; | 2(T, long) | void | accept

## Exceptions and Assertions

Categories of exceptions

![Throwable structure](http://journals.ecs.soton.ac.uk/java/tutorial/java/exceptions/images/throwableHierarchy_trans.gif)

OCP exceptions

Exception | Used when | Checked or unchecked?
------------- | ------------- | -------------
java.text.ParseException | Converting a String to a number. |  checked
java.io.IOException | Dealing with IO and NIO.2 issues. | checked
java.io.FileNotFoundException | Dealing with IO and NIO.2 issues. | checked
java.io.NotSerializableException | Dealing with IO and NIO.2 issues. | checked
java.sql.SQLException | Dealing with database issues. | checked
java.lang.ArrayStoreException | Trying to store the wrong data type in an array. | unchecked
java.time.DateTimeException | Receiving an inalid format string for a date. | unchecked
java.util.MissingResourceException | Trying to access a key or resource bundle that does not exist. | unchecked
java.lang.IllegalStateException | Attempting to run an invalid operation in collections and concurrency | unchecked
java.lang.UnsupportedOperationException | Attempting to run an invalid operation in collections and concurrency | unchecked

It's possible to create multi-catch to handle more than one exception the same way.
```
try {
	// Implementation
} catch (Exception1 | Exception2 e) {
	// Exception handler
}
```
The OCP exam might try to fool you with different syntax like "Exception1 e1 | Exception2 e2", but the variable is shared between exception types.

The order of the multi-catch exceptions does not matter.

Java intends multi-catch to be used for exceptions that aren't related, and it prevents you from specifying redundant types in a multi-catch.
```
try {
	// Implementation
} catch (FileNotFoundException | IOException e) { // Does not compile since FileNotFoundException is an IOException 
	// Exception handler
}
```

multi-catch variable is effectively final. It won't compile if you try to reassign a new Exception to it.

try-with-resources a kind of try syntax that ensures resources(Autocloseable and Closeable) contained in "()" are closed

try-with-resources, differently from traditional try, does not need a catch or finally statement to compile.

resources from try-with-resources are closed at the end of try statement block execution.

Legal vs illegal configuration with a traditional try

x | 0 finally blocks | 1 finally block | 2 or more finally blocks
------------- | ------------- | ------------- | -------------
0 catch blocks | Not legal | Legal | Not legal
1 or more catch blocks | Legal | Legal | Not Legal

Legal vs illegal configuration with a try-with-resources

x | 0 finally blocks | 1 finally block | 2 or more finally blocks
------------- | ------------- | ------------- | -------------
0 catch blocks | Legal | Legal | Not legal
1 or more catch blocks | Legal | Legal | Not Legal

AutoCloseable vs Closeable
* AutoCloseable has method "public void close() throws Exception;" while Closeable has "public void close() throws IOException;";
* Closeable requires implementations to be idempotent.

assertion is a Boolean expression that you place at a point in your code where you expect something to be true.

assertion syntax might be of two forms:
* assert boolean_expression; -> assert(0>1); // "()" are not mandatory
* assert boolean_expression: error_message; -> assert(0>1): "0 lesser then 1"; // "()" are not mandatory

The three outcomes of an assert statement are as follows:
* if assertions are disabled, Java skips the assertion and goes on in the code.
* if assertions are enabled and the boolean expression is true, then our assertion has been validated and nothing happens. The program continues to execute in its normal manner.
* if assertions are enabled and the boolean expression is false, then our assertion is invalid and a java.lang.AssertionError is thrown.

Enabling assertions:
* java -enableassertions ProgramClass: enable assertion on full program
* java -ea ProgramClass: enable assertion on full program
* java -ea:com.my.demopackage... ProgramClass: enable assertion on a specific package
* java -ea:com.my.demopackage.AnotherClass ProgramClass: enable assertion on a specific class
* java -ea:com.my.demopackage.AnotherClass ProgramClass: enable assertion on a specific class
* java -ea:com.my.demopackage... -disableassertions:com.my.demopackage.AnotherClass ProgramClass: enable assertion on a specific package but disable on a specific class
* java -ea:com.my.demopackage... -da:com.my.demopackage.AnotherClass ProgramClass: enable assertion on a specific package but disable on a specific class

## Use Java SE 8 Date/Time API

There are 4 final classes when working dates and times:
* LocalDate: Contains just a date, no time and no zone;
* LocalTime: Contains just a time, no date and no zone;
* LocalDateTime: Contains both date and time, no zone;
* ZonedDateTime: Contains date, time and timezone.

Method "of" is the factory method that exists in all of the classes above. Below the overloaded methods:

* LocalDate
	* of(int year, int month, int dayOfMonth)
	* of(int year, Month month, int dayOfMonth)
* LocalTime
	* of(int hour, int minute)
	* of(int hour, int minute, int second)
	* of(int hour, int minute, int second, int nanos)
* LocalDateTime
	* of(int year, int month, int dayOfMonth, int hour, int minute)
	* of(int year, int month, int dayOfMonth, int hour, int minute, int second)
	* of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanos)
	* of(int year, Month month, int dayOfMonth, int hour, int minute)
	* of(int year, Month month, int dayOfMonth, int hour, int minute, int second)
	* of(int year, Month month, int dayOfMonth, int hour, int minute, int second, int nanos)
	* of(LocalDate date, LocalTime time)
* ZonedDateTime
	* of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanos, ZonedId zone)
	* of(LocalDate date, LocalTime time, ZonedId zone)
	* of(LocalDateTime dateTime, ZonedId zone)
	
Methods in LocalDate, LocalTime, LocalDateTime and ZonedDateTime

Method | Can Call on LocalDate? | Can Call on LocalTime? | Can Call on LocalDateTime or ZonedDateTime?
------------- | ------------- | ------------- | -------------
plusYears/minusYears | Y | N | Y
plusMonths/minusMonths | Y | N | Y
plusWeeks/minusWeeks | Y | N | Y
plusDays/minusDays | Y | N | Y
plusHours/minusHours | N | Y | Y
plusMinutes/minusMinutes | N | Y | Y
plusSeconds/minusSeconds | N | Y | Y
plusNanos/minusNanos | N | Y | Y

LocalDate, LocalDateTime and ZonedDateTime have "toEpochSecond()" to transform a date to a long unixtime value. The OCP will pretend that this method exist on LocalTime.

Period final class represent a date period in year or/and months or/and days

Period factories:
* ofYears(int years)
* ofWeeks(int weeks)
* ofMonths(int months)
* ofDays(int days)
* of(int years, int months, int days)

Period object can be used to "plus" and "minus" methods from LocalDate, LocalDateTime, ZonedDateTime and Period that returns a new object. The OCP will pretend that this method exist on LocalTime and Duration.

Period toString format:
```
	System.out.println(Period.ofYears(1));  // P1Y
	System.out.println(Period.ofWeeks(3));  // P21D
	System.out.println(Period.ofMonths(3)); // P3M
	System.out.println(Period.ofDays(2));   // P2D
	System.out.println(Period.of(1,2,3));   // P1Y2M3D
```

Duration final class represent a time duration in days or hours or minutes or seconds or milliseconds or nanoseconds.

Duration factories:
* ofDays(long days)
* ofHours(long hours)
* ofMillis(long millis)
* ofMinutes(long minutes)
* ofNanos(long nanos)
* ofSeconds(long seconds)
* ofSeconds(long seconds, long nanoAdjustment)
* of(long amount, TemporalUnit unit)

Duration object can be used to "plus" and "minus" methods from LocalTime, LocalDateTime, ZonedDateTime and Duration that returns a new object. The OCP will pretend that this method exist on LocalDate and Period.

Duration toString format:
```
	System.out.println(Period.ofDays(1));                  // PT24H
	System.out.println(Period.ofHours(1));                 // PT1H
	System.out.println(Period.ofMinutes(1));               // PT1M
	System.out.println(Period.ofSeconds(10));              // PT10S
	System.out.println(Period.ofMillis(1));                // PT0.001S
	System.out.println(Period.ofNanos(1));                 // PT0.000000001S
	System.out.println(Period.of(1, ChronoUnit.DAYS));     // PT24H
	System.out.println(Period.of(1, ChronoUnit.HOURS));    // PT1H
	System.out.println(Period.of(1, ChronoUnit.MINUTES));  // PT1M
	System.out.println(Period.of(10, ChronoUnit.SECONDS)); // PT10S
	System.out.println(Period.of(1, ChronoUnit.MILLIS));   // PT0.001S
	System.out.println(Period.of(1, ChronoUnit.NANOS));    // PT0.000000001S
```

Class | Can Use with Period? | Can Use with Duration?
------------- | ------------- | -------------
LocalDate | Y | N
LocalDateTime | Y | Y
LocalTime | N | Y
ZonedDateTime | Y | Y

ChronoUnit is a TemporalUnit.

ChronoUnit for differences:
```
LocalTime one = LocalTime.of(5, 15);
LocalTime two = LocalTime.of(6, 30);
LocalDate date = LocalDate.of(2016, 1, 20);

System.out.println(ChronoUnit.HOURS.between(one, two);    // 1
System.out.println(ChronoUnit.MINUTES.between(one, two);  // 75
System.out.println(ChronoUnit.MINUTES.between(one, date); // DateTimeException due to time vs date
```

Instant class represents a specific moment in time in the GMT timezone.

If you have a Date or ZonedDateTime, you can use "toInstant()" to get the Instant object. ZonedDateTime object will transform its current zone date and time to the GMT date and time.

Daylight savings time automatically adjusts depending on the zone.

DateTimeFormatter can use pre-defined formats or a pattern.

Pre-defined formats:
* DateTimeFormatter df = DateTimeFormatter.ISO_LOCAL_DATE: 2020-01-20
* DateTimeFormatter df = DateTimeFormatter.ISO_LOCAL_TIME: 11:12:34
* DateTimeFormatter df = DateTimeFormatter.ISO_LOCAL_DATE_TIME: 2020-01-20T11:12;34

Pre-defined formats using FormatStyle:
* DateTimeFormatter df = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT): '12.13.52' or '3:30pm'
* DateTimeFormatter df = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM): 'Jan 12, 1952
* DateTimeFormatter df = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG): January 12, 1952
* DateTimeFormatter df = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.FULL): Tuesday, April 12, 1952 AD' or '3:30:42pm PST

Using pattern seems like: 
```
DateTimeFormatter df = DateTimeFormatter.ofPattern("MMMM dd, yyyy, hh:mm");
```
* MMMM: M represents the month, being "M" like 1, "MM" like 01, MMM like "Jan" and MMMM like "January";
* dd: d represents day in the month, beind "d" like 1 and "dd" like 01. For double digit the "d" will output both digits;
* yyyy: y represents year, being "yy" two-digit year and "yyyy" four-digit year;
* hh: h represents hour, being "hh" to include leading zero;
* mm: m represents minute omitting the leading zero if present, being "m" the one-digit and "mm" the two-digit and more common.

format is a java.time object to String method. Ex:
```
LocalDateTime datetime = LocalDateTime.of(2020, Month.JANUARY, 20, 11, 12, 34);
DateTimeFormatter shortF = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT);
DateTimeFormatter mediumF = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM);
DateTimeFormatter f = DateTimeFormatter.ofPattern("MMMM dd, yyyy, hh:mm");
System.out.println(datetime.format(shortF)); // 1/20/20 11:12 AM
System.out.println(datetime.format(mediumF)); // Jan 20, 2020 11:12:34 AM
System.out.println(datetime.format(f)); // January 20, 2020, 11:12
System.out.println(datetime.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME)); // 2020-01-20T11:12:34
```

parse is a String to java.time object method. Ex:
```
DateTimeFormatter f = DateTimeFormatter.ofPattern("MM dd yyyy");
LocalDate date = LocalDate.parse("01 02 2015", f);
LocalTime time = LocalTime.parse("11:22");
System.out.println(date); // 2015-01-02
System.out.println(time); // 11:22
```

## Java I/O Fundamentals

File object receives a relative path. Ex: If current directoriy set to "/home/rachid" -> new File("data/anything.txt");

File separator are different for each os, it can be get with 'System.getProperty("file.separator")' or 'java.io.File.separator'.

java.io.File methods

Method name | Description
------------- | -------------
exists() | Returns true if the file or directory exists.
getName() | Returns the name of the file or directory denoted by this path.
getAbsolutePath() | Returns the absolute pathname string of this path.
isDirectory() | Returns true if the file denoted by this path is a directory.
isFile() | Returns true if the file denoted by this path is a file.
length() | Returns the number of bytes in the file. For performance reasons, the file system may allocate more bytes on disk than the file actually uses.
lastModified() | Returns the number of millisencods since the epoch when the file was last modified.
delete() | Deletes the file or directory. If this pathname denotes a directory, then the directory must be empty ir order to be deleted.
renameTo(File) | Renames the file denoted by this path.
mkdir() | Creates the directory named by this path.
mkdirs() | Creates the directory named by this path including any nonexistent parent directories.
getParent() | Returns the abstract pathname of this abstract pathname's parent or null if this pathname does not name parent directory.
listFiles() | Returns a File[] array denoting the files in the directory.

Differences between streams and readers/writers
* The stream classes are used for inputting and outputting all types of binary or byte data.
* The reader and writer classes are used for inputting and outputting only character and string data.

java.io class properties
* A class with the word InputStream or OutputStream in its name is used for reading or writing binary data, repectively.
* A class with the word Reader or Writer in its name is used for reading or writing character or string data, respectively.
* Most, but not all, input classes have a corresponding output class.
* low-level stream connects directly with the source of the data.
* high-level stream is built on top of another stream using wrapping.
* A class with Buffered in its name reads or writes data in groups of bytes or characters and often improves performance in sequential file systems.

java.io.stream classes

Class name | Low/High Level | Description
------------- | ------------- | -------------
InputStream | N/A | The abstract class all InputStream classes inherit from
OutputStream | N/A | The abstract class all OutputStream classes inherit from
Reader | N/A | The abstract class all Reader classes inherit from
Writer | N/A | The abstract class all Writer classes inherit from
FileInputStream | Low | Reads file data as bytes
FileOutputStream | Low | Writes file data as bytes
FileReader | Low | Reads file data as characters
FileWriter | Low | Writes file data as characters
BufferedReader | High | Reads character data from an existing Reader in a buffered manner, which improes efficiency and performance
BufferedWriter | High | Writes character data from an existing Writer in a buffered manner, which improes efficiency and performance
ObjectInputStream | High | Deserializes primitive Jaa data types and graphs of Jaa objects from an existing InputStream
ObjectOutputStream | High | Serializes primitive Jaa data types and graphs of Jaa objects from an existing OutputStream
InputStreamReader | High | Reads character data from an existing InputStream
OutputStreamWriter | High | Writes character data from an existing OutputStream
PrintStream | High | Writes formatted representations of Java objects to a binary stream
PrintWriter | High | Writes formatted representations of Java objects to a text-based output stream

streams are Closeable, so they are considered resources for try-with-resources syntax.

When data is written to an OutputStream, the underlying os does not necessarily guarantee that the data will make it to the file immediately, it can be chached in memory in some os. It means if the application terminates unexpectedly, the data would be lost, because it was neer written to the file system. To address this, java provides a "flush()" method, which requests that all accumulated data be written immediately to disk.

To serialize and deserialize an object with ObjectInputStream and ObjectOutputStream classes, the class to be serialized/deserialized must implements Serializable.

Thread or Stream classes are not recommended to be Serializable.

Known System streams

Modifier and Type | Class Name | Field and Description
------------- | ------------- | -------------
System.err | PrintStream | The "standard" error output stream.
System.in | InputStream | The "standard" input stream.
System.out | PrintStream | The "standard" output stream.

Both PrintStream and PrintWriter have the print based methods: "print()", "println()", "format()" and "printf()"

"format()" and "printf()" are similar methods.

We have a class to interact with user called java.io.Console. Example of Console use:
```
import java.io.Console;
public class ConsoleSample {
	public static void main(String[] args) {
		Console console = System.console();
		if (console != null) {
			String userInput = console.readLine();
			console.writer().println("You entered the following " + userInput);
		}
	}
}
```
Note that it's possible to System not return an instance of console, so it's not guaranteed the instance from "System.console()".

Console's reader() method returns an instance of Reader while Console's writer() method returns an instance of PrintWriter.

The methods "format()", "printf()" and "flush()" also exists on Console and works the same way as PrintWriter.

The method "readLine()" from Console reads a full line input from user and works exactly like BufferedReader's "readLine()" method.

 The method "readPassword()" from Console is similar to the "readLine()" method, except that echoing is disabled. By disabling echoing, the user does not see the text they are typing. Unlike "readLine()" method, the "readPassword()" returns an array of characters instead of a String.
 
 The reason for "readPassword()" returns an array of characters is because String alues are added to a shared memory pool for performance reasons in Java. This means that if a password that a user typed in were to be returned to the process as String, it might be available in the String pool long after the user entered it. That means that the password would be available on String pool of the memory in the application is ever dumped to disk.

## Java File I/O (NIO.2)

java.nio.file.Path object represents a hierarchical path on the storage system to a file or directory. In this manner, Path is a direct replacement for the legacy java.io.File class, and conceptually it contains many of the same properties.

The simples and most straightforward way to obtain a Path object is using the java.nio.file.Paths factory class, To obtain a reference to a file or directory, you would call the static method "Paths.get(String, String...)". Ex:
```
Path p1 = Paths.get("books/law.pdf"); // relative
Path p2 = Paths.get("c:\\employees\\jane.txt"); // absolute
Path p3 = Paths.get("/home/marcos/daemon.log"); // absolute
```
or
```
Path p1 = Paths.get("books", "law.pdf");
Path p2 = Paths.get("c:", "employees", "jane.txt");
Path p3 = Paths.get("/", "home", "marcos", "daemon.log");
```

Attention with Path and Paths, OCP might try to trick you, Path is the interface reference and Paths is the factory class.

Alternatively you can use a FileSystem object to retrive a Path object. Like "FileSystems.getDefault().getPath(String, String...)" or "FileSystems.getFileSystem(new URI("any valid uri")).getPath(String, String...)".

Also you can get a Path object from the legacy File instances. Like "new File(String).toPath()".

The backward also works. Like "Paths.get(String, String...).toFile()".

NIO.2 also includes helper classes such as java.nio.file.Files, whose primary purpose is to operate on instances of Path objects. Helpers differs from factories in that they are focused on manipulating or creating new objects from existing instances, whereas factory classes are focused primarily on object creation.

Common optional arguments in NIO.2

Enum Value | Usage | Description
------------- | ------------- | -------------
NOFOLLOW_LINKS | Test file existing, Read file data, Copy file, Move file | If provided, symbolic links when encountered will not be traversed. Useful for performing operations on symbolic links themselves rather than their target.
FOLLOW_LINKS | Traverse a directory tree | If proided, symbolic links when encountered will be traversed.
COPY_ATTRIBUTES | Copy file | If provided, all metadata about a file will be copied with it.
REPLACE_EXISTING | Copy file, Move file | If proided and the target file exists, it will be replaced; otherwise, if it is not provided, an exception will be thrown if the file already exists.
ATOMIC_MOVE | The operation is performed in an atomic manner within the file system, ensuring that any process using the file sees only a complete record. Method using it may throw an exception if the feature is unsupported by the file system.

The Path object's "toString()" method is the only method to return String, returning the String value of the path.

The Path object's "getName(int)" returns the component of the Path as a new Path object rather than a String.

The Path object's "getNameCount()" returns the number of names in a Path. Ex:
```
Path path = Paths.get("/land/hippo/harry.happy");
System.out.println("The path Name is " + path); // The path Name is /land/hippo/harry.happy

for (int i = 0; i<path.getNameCount(); i++) {
	System.out.println("Element " + i + " is: " + path.getName(i));
} 
// Element 0 is: land
// Element 1 is: hippo
// Element 2 is: harry.happy
```
Note that root element / is not included in the list of names. If the Path object represents the root element itself, then the number of names in the Path object returned by getNameCount() will be 0 and the relative path "land/hippo/harry.happy" must return the same as absolute "/land/hippo/harry.happy" path.

The Path object's "getFileName()" method returns a new Path instance of the filename, which is the farthes element from the root.

The Path object's "getParent()" method returns a Path instance representing the parent path or null if there is no such parent.

The Path object's "getRoot()" method returns the root element for the Path object or null if the Path object is relative.

```
// Consider path as:
// A: /zoo/armadillo/shells.txt
// B: armadillo/shells.txt
System.out.println("Filename is: " + path.getFileName()); // A is shells.txt and B is shells.txt
System.out.println("Root is: " + path.getRoot()); // A is / and B is null

Path currentParent = path;
while((currentParent = currentParent.getParent()) != null) {
	System.out.println("	Current parent is: " + currentParent);
}
// A returns:
//	Current parent is: /zoo/armadillo
//	Current parent is: /zoo
//	Current parent is: /
// B returns:
//	Current parent is: armadillo	
```

The Path object's "isAbsolute()" returns true if the path of the object references is absolute and false if the path object is relative.

The Path object's "toAbsolutePath()" method converts a relative Path object to an absolute Path object by joining it to the current working directory.

```
// my working directory is /home/rachid
Path path = Paths.get("jboss/log/server.log");
System.out.println("Path is absolute ? " + path.isAbsolute()); // false because jboss/log/server.log is relative
System.out.println("Absolute Path " + path.toAbsolutePath()); // /home/rachid/jboss/log/server.log
```

The method "subpath(int, int) returns a relative subpath of the Path object, referenced by an inclusive start index and an exclusive end index. ex:
```
Path path = Paths.get("/mammal/carnivore/raccoon.image");
System.out.println("Path is: " + path);

System.out.println(Subpath from 0 to 3 is: " + path.subpath(0, 3)); // mammal/carnivore/raccoon.image
System.out.println(Subpath from 1 to 3 is: " + path.subpath(1, 3)); // carnivore/raccoon.image
System.out.println(Subpath from 1 to 2 is: " + path.subpath(1, 2)); // carnivore
```

The Path object's "relativize(Path)" is for constructing the relative path from one Path object to another. Ex:
```
Path p1 = Paths.get("server-2019-04-18.log");
Path p2 = Paths.get("server-2019-04-17.log");
System.out.println(p1.relativize(p2)); // ../server-2019-04-17.log
System.out.println(p2.relativize(p1)); // ../server-2019-04-18.log
```
remember that both paths must be or both relative or both absolute, otherwise it will throw IllegalArgumentException

## Java Concurrency

Runnable is a functional interface that takes no arguments and returns no data with the method "void run()".

Callable is a functional interface that is siilar to Runnable except that its "V call() throws Exception" method returns a value and can throw a checked exception. 

The Callable interface was introduced as an alternative to the Runnable interface, since it allows more details to be retrieved easily from the task after it is completed.

Callable has a lambda ambiguity with Supplier, to solve this problem, you can explicitly cast the type to the lambda.

Defining the task, or work, tat a Thread instance will execute can be done two ways in java:
* Provide a Runnable object or lambda expression to the Thread constructor.
```
public class Printer implements Runnable {
	public void run() {
		System.out.println("Done.");
	}
	public static void main(String[] args) {
		(new Thread(new Printer())).start();
	}
}
```
* Create a class that extends Thread and overrides the "run()" method.
```
public class Printer implements Thread {
	public void run() {
		System.out.println("Done.");
	}
	public static void main(String[] args) {
		(new Printer()).start();
	}
}
```

Thread class is hardly used on OCP.

ExecutorService is the interface which creates and manages threads for you. You can get an instance throught Executors class factory methods.

You can shutdown a thread executor throught "shutdown()" method or "shutdownNow()". The difference between then is:
* "shutdown()" will firstly set to reject eery new task submitted to the thread executor while continuing to execute any previously submitted task. During this time, calling "isShutdown()" will return true, while "isTerminated()" will return false. If a new task is submitted to the thread executor while it shutting down, a "RejectedExecutionException" will be thrown. Once all actie tasks have been completed, "isShutdown()" and "isTerminated()" will both return true.
* "shutdownNow()" will attempt to stop all running tasks and discards any that have not been started yet and it returns a List<Runnable> of tasks that were submitted to the thread executor but that were never started.

ExecutorService submitting tasks methods:

Method name | Description
------------- | -------------
void execute(Runnable command) | Executes a Runnable task at some point the future
Future<?> submit(Runnable task) | Executes a Runnable task at some point the future and returns a Future respresenting the task
&lt;T&gt; Future&lt;T&gt; submit(Callable&lt;T&gt; task) | Executes a Caççançe task at some point in the future and returns a Future representing the pending results of the task
&lt;T&gt; List<Future&lt;T&gt;> invokeAll(Collection<? extends Callable&lt;T&gt;> tasks) throws InterruptedException | Executes the gien tasks, synchronously returning the results of all tasks as a Collection of Future objects, in the same order they were in the original collection
&lt;T&gt; T invokeAny(Collection<? extends Callable&lt;T&gt;> tasks) throws InterruptedException, ExecutionException | Executes the gien tasks, synchronously returning the result of one of finished tasks, cancelling any unfinished tasks

"execute()" and "submit()" methods are nearly identical when applied to Runnable expressions. The "submit()" method has the obvious advantage of doing the exact same thing "execute()" does, but with a return object that can be used to track the result.

Future methods:

Method name | Description
------------- | -------------
boolean isDone() | Returns true if the task was completed, threw an exception or was cancelled
boolean isCancelled() | Returns true if the task was cancelled before it completely normally
boolean cancel() | Attempts to cancel execution of the task
V get() | Retrieves the result of a task, waiting endlessly if it is not yet available
V get(long timeout, TimeUnit unit) | Retrieves the result of a task, waiting the specified amout of time. if the result is not ready by the time the timeout is reached, a checked TimeoutException will be thrown

Like ExecutorService, we have a proper interface to schedule tasks called "ScheduledExecutorService". Its instance is also obtained throught "Executors" factory methods.

ScheduledExecutorService methods

Method name | Description
------------- | -------------
schedule(Callable<V> callable, long delay, TimeUnit unit) | Creates and executes a Callable task after the given delay.
schedule(Runnable command, long delay, TimeUnit unit) | Creates and executes a Runnable task after the given delay.
scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit) | Creates and executes a Runnable task after the given initial delay creating a new task every period alue that passes.
scheduleAtFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit) | Creates and executes a Runnable task after the given initial delay and subsequently with the given delay between the termination of one execution and the commencement of the next.

Executors methods

Method name | Return Type | Description
------------- | ------------- | -------------
newSingleThreadExecutor() | ExecutorService | Creates a single-threaded execuor that uses a single worker thread operating off an unbounded queue. Results are processed sequentially in the order in which they are submitted.
newSingleThreadScheduledExecutor() | ScheduledExecutorService | Creates a single-threaded executor that can schedule commands to run after a given delay or to execute periodically.
newCachedThreadPool() | ExecutorService | Creates a thread pool that creates new threads as needed, but will reuse previously constructed threads wen they are available.
newFixedThreadPool(int nThreads) | ExecutorService | Creates a thread pool that reuses a fixed number of threads operating off a shared unbounded queue.
newScheduledThreadPool(int nThreads) | ScheduledExecutorService | Creates a thread pool that can schedule commands to run after a given delay or execute periodically.

Atomic Classes are thread-safe classes that has atomic property that synchronizes the execution access for each thread access.

Atomic Classes

Class Name | Description
------------- | -------------
AtomicBoolean | A boolean value that may be updated atomically
AtomicInteger | An int value that may be updated atomically
AtomicIntegerArray | An int array in which elements may be updated atomically
AtomicLong | A long value that may be updated atomically
AtomicLongArray | A long array in which elements may be updated atomically
AtomicReference | A generic object reference that may be updated atomically
AtomicReferenceArray | An array of generic object references array in which elements may be updated atomically

Common atomic methods

Method Name | Description
------------- | -------------
get() | Retrieve the current value
set() | Set the given value, equivalent to the assignment = operator
getAndSet() | Atomically sets the new Value and Returns the old value
incrementAndGet() | for numeric classes, atomic pre-increment operation equivalent to ++value
getAndIncrement() | For numeric classes, atomic post-increment operation equivalent to value++
decrementAndGet() | for numeric classes, atomic pre-increment operation equivalent to --value
getAndDecrement() | For numeric classes, atomic post-increment operation equivalent to value--

The purpose of the concurrent collection classes is to sole common memory consistency errors. A memory consistency error occurs when two threads hae inconsistent views of what should be the same data. Conceptually, we want writes on one thread to be aailable to another thread if it accesses the concurrent collection after the write has ocurred.

Concurrent collection classes

Class Name | Java COllection Framework Interface | Element Ordered? | Sorted? | Blocking?
------------- | ------------- | ------------- | ------------- | -------------
ConcurrentHashMap | ConcurrentMap  N | N | N
ConcurrentLinkedDeque | Deque | Y | N | N
ConcurrentLikendQueue | Queue | Y | N | N
ConcurrentSkipListMap | ConcurrentMap, SortedMap, NavigableMap | Y | Y | N
ConcurrentSkipListSet | SortedSet, NavigableSet | Y | Y | N
CopyOnWriteArrayList | List | Y | N | N
CopyOnWriteArraySet | Set | N | N | N
LinkedBlockingDeque | BlockingQueue, BlockingDeque | Y | N | Y
LinkedBlockingQueue | BlockingQueue | Y | N | Y

LinkedBlockingDeque and LinkedBlockingQueue are just regular Queue, except that it includes methods that will wait a specific amout of time to complete an operation.

BlockingQueue waiting methods

Method Name | Description
------------- | -------------
offer(E e, long timeout, TimeUnit unit) | Adds item to the queue waiting the specified time, returning false if time elapses before space if available
poll(long timeout, TimeUnit unit) | Retries and removes an item from the queuem waiting the specified time, returning null if the time elapses before the item is available

BlockingDeque waiting methods

Method Name | Description
------------- | -------------
offerFirst(E e, long timeout, TimeUnit unit) | Adds an item to the front of the queue, waiting a specified time, returning false if time elapses before space is available
offerLast(E e, long timeout, TimeUnit unit) | Adds an item to the tail of the queue, waiting a specified time, returning false if time elapses before space is available
pollFirst(long timeout, TimeUnit unit) | Retrives and removes an item from the front of the queue, waiting the specified time, returning null if the time elapses before the item is available
pollLast(long timeout, TimeUnit unit) | Retrives and removes an item from the tail of the queue, waiting the specified time, returning null if the time elapses before the item is available

The SkipList classes, ConcurrentSkipListSet and ConcurrentSkipListMap, are concurrent versions of their sorted counterparts, TreeSet and TreeMap, respectively.

CopyOnWriteArrayList and CopyOnWriteArraySet are classes that copy all of their elements to a new underlying structure anytime an element is added, modified, or removed from the collection. Ex:
```
List<Integer> list = mew CopyOnWriteArrayList<>(Arrays.asList(4,3,52));
for (Integer item : list) {
	System.out.print(item+" ");
	list.add(9);
} // 4 5 52
System.out.println();
System.out.println("Size: " + list.size()); // Size: 6
```
If we had used a regular ArrayList as for referece, a ConcurrentModificationException would have been thrown at runtime.

Synchronized collections are obtained throught Collections classe factory methods.

"parallel()" is the method used to retrieve a parallel stream from a Stream instance.

"parallelStream()" is the method used to retrieve a parallel stream from a Collection instance.

Parallel streams can improe performance because they rely on the property that many stream operations can be executed independently. This means that a intermediate operation will execute operations on different threads. Ex:
```
Arrays.asList("jackal","kangaroo","lemur")
	.parallelStream()
	.map(s -> s.toUpperCase())
	.forEach(System.out::println);
```
The order that each thread is executed is never guaranteed.

"reduce()" on parallel stream will guarantee the order the same way as a serial stream.

"reduce()" arguments:
* The identity must be defined such that for all elements in the stream u, combiner.apply(identity, u) is equal to u;
* The accumulator operator op must be associative and stateless such that (a op b) op c is equal to a op (b op c) being the accumulator a BiFunction;
* The combiner operator must also be associatie and stateless and compatible with the identity, such that for all u and t combiner.apply(u, accumulator.apply(identity, t)) os equal to accumulator.apply(u, t) being combiner a BinaryOperator.

"collect()" reduction on parallel stream will not guarantee the same order as the stream.

Requirements for Parallel Reduction with "collect()"
* The stream is parallel;
* The parameter of the collect operation has the Collector.Characteristics.CONCURRENT characteristic;
* Either the stream is unordered, or the collector has the characteristic Collector.Characteristics.UNORDERED.

Multiple thread tasks can be coordinated on concurrent API throught CyclicBarrier and ForkJoinPool classes.

Threading Problems:
* Deadlock: occurs when two or more thread are blocked forever, each waiting on the other.
* Starvation: occurs when a single thread is perpetually denied access to a shared resource or lock. The thread is still actie, but it is unable to complete its work as a result of other threads constantly taking the resource that they are trying to access.
* Livelock: occurs when two or more threads are conceptually blocked foreer, although they are each still active and trying to complete their task. Livelock is a special case of resource staration in which two or more threads actively try to acquire a set of locks, are unable to do so, and restart part of the process.
* Race Conditions: is an undesirable result that occurs when two tasks, which should be completed sequentially, are completed at the same time.

## Building Database Applications with JDBC

## Localization

Get current default locale with "Locale.getDefault()".

Set defaut locale with "Locale.setDefault(Locale locale)". The locale set will mantain till the end of program or till it is changed.

You can use constants Locales for language or language+country:
```
	System.out.println(Locale.GERMAN); // de
	System.out.println(Locale.GERMANY); // de_DE
```
or use Locale constructors to language or language+country:
```
	System.out.println(new Locale("de")); // de
	System.out.println(new Locale("de", "DE")); // de_DE
```
or you can use Locale builder:
```
	System.out.println(Locale.Builder.setLanguage("de").build()); // de
	System.out.println(Locale.Builder.setLanguage("de").setRegion("DE").build()); // de_DE
```

The case on language and region doesn't interfer.

not necessary to memorize codes for OCP

Locale has a third constructor "Locale(String language, String country, String variant)", that is not used on exam.

Resource Bundle contains the local specific objects to be used by a program. It is like a map with keys and values. The resource bundle can be in a property file or in a Java class.

Resource Bundle that will be used will depends on the Locale passed on the locale parameter or its default locale. Ex: 
Zoo_en.properties:
```
hello=Hello
open=The zoo is open
```
Zoo_fr.properties:
```
hello=Bonjour
open=Le zoo est ouvert
```
Code:
```
import java.util.*;
public class ZooOpen {
	public static void main(String[] args) {
		Locale us = new Locale("en", "US");
		Locale france = new Locale("fr", "FR");
		
		Resource rb = ResourceBundle.getBundle("Zoo", us);
		System.out.println(rb.getString("hello")); // Hello
		System.out.println(rb.getString("open")); // The zoo is open
		
		rb = ResourceBundle.getBundle("Zoo", france);
		System.out.println(rb.getString("hello")); // Bonjour
		System.out.println(rb.getString("open")); // Le zoo est ouvert
	}
}
```

Properties has some additional features, including being able to passa a default.

Return values of a Properties instance's getProperty():

Key Found? | Y | N
------------- | ------------- | -------------
getProperty("key") | Value | null
getProperty("key", "default") | Value | "default"

To create a Java class resource bundle it's necessary to create a class that extends "ListResourceBundle". Ex:
```
package animal;
import java.util.*;
public class Zoo_en_US extends ListResourceBundle {
	protected Object[][] getContents() {
		return new Object[][] {
			{ "hello", "Hello" },
			{ "open", "The zoo is open" }
		};
	}
	public static void main (String[] args) { // executable is not necessary for the resource bundle, it's only to ilustrate
		ResourceBundle rb = ResourceBundle.getBundle("animal.Zoo", Locale.US);
		System.out.println(rb.getString("hello")); // Hello
	}
}
```

Ilustration of resource bundle files priority, picking a resource bundle for French in France with default locale US English

Step | Looks For File | Reason
------------- | ------------- | -------------
1 | Zoo_fr_FR.java | The requested locale
2 | Zoo_fr_FR.properties | The requested locale
3 | Zoo_fr.java | The language we requested with no country
4 | Zoo_fr.properties | The language we requested with no country
5 | Zoo_en_US.java | The default locale
6 | Zoo_en_US.properties | The default locale
7 | Zoo_en.java | The default language with no country
8 | Zoo_en.properties | The default language with no country
9 | Zoo.java | No locale at all - the default bundle
10 | Zoo.properties | No locale at all - the default bundle
11 | If still not found, throw MissingResourceException | none

NumberFormat uses default locale and a specified locale to parse Number, Currency and Percentage based on country format.

Description | Using Default Locale and Specified Locale
------------- | -------------
A general purpose formatter with default Locale | NumberFormat.getInstance()
A general purpose formatter with specified Locale | NumberFormat.getInstance(locale)
Same as getInstance with default Locale | NumberFormat.getNumberInstance()
Same as getInstance with specified Locale | NumberFormat.getNumberInstance(locale)
For formatting monetary amounts with default Locale | NumberFormat.getCurrencyInstance()
For formatting monetary amounts with specified Locale | NumberFormat.getCurrencyInstance(locale)
For formatting percentages with default locale | NumberFormat.getPercentInstance()
For formatting percentages with specified locale | NumberFormat.getPercentInstance(locale)
Round decimal values before displaying with default Locale (not on exam) | NumberFormat.getIntegerInstance()
Round decimal values before displaying with specified Locale (not on exam) | NumberFormat.getIntegerInstance(locale)

Ex:
```
...
	int attendeesPerYear = 3_200_000;
	int attendeesPerMonth = attendeesPerYear / 12;
	NumberFormat us = NumberFormat.getInstance(Locale.US);
	System.out.println(us.format(attendeesPerMonth)); // 266,666
	NumberFormat g = NumberFormat.getInstance(Locale.GERMANY);
	System.out.println(us.format(attendeesPerMonth)); // 266.666
	NumberFormat ca = NumberFormat.getInstance(Locale.CANADA_FRENCH);
	System.out.println(us.format(attendeesPerMonth)); // 266 666
...
```

Source  | Data
------------- | -------------
Take Exam  | <https://education.oracle.com/pt_BR/java-se-8-programmer-ii/pexam_1Z0-809>
Java Exam Classes(en)  |  &lt;not yet&gt;
Java Exam Classes(pt-br)  |  &lt;not yet&gt;
Exam Simulation(en)  | &lt;not yet&gt;
Exam Simulation(pt-br)  | <https://www.udemy.com/metodologia-de-certificacao-java-8-ocp-simulados/learn/v4/overview>
