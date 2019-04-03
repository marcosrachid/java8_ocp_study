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
public class A<T> {
	T obj;
}
```

It's possible to use generic to a specific method. ex:
```
...
	public static <T> void call(T t) {}
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
Wildcard with lower bound  | ? super type  | List<? super Exception> l = new ArrayList<Object>();

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
Supplier<T> | 0 | T | get
Consumer<T> | 1(T) | void | accept
BiConsumer<T, U> | 2(T, U) | void | accept
Predicate<T> | 1(T) | boolean | test
BiPredicate<T, U> | 2(T, U) | boolean | test
Function<T, R> | 1(T) | R | apply
BiFunction<T, U, R> | 2(T, U) | R | apply
UnaryOperator<T> | 1(T) | T | apply
BinaryOperator<T> | 2(T, T) | T | apply

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
findAny()/findFirst() | Terminates | Optional<T> | N
forEach() | Does not terminate | void | N
min()/max() | Does not terminate | Optional<T> | Y
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
ToDoubleFunction<T> | 1(T) | double | applyAsDouble
ToIntFunction<T> | 1(T) | int | applyAsInt
ToLongFunction<T> | 1(T) | long | applyAsLong
ToDoubleBiFunction<T, U> | 2(T, U) | double | applyAsDouble
ToIntBiFunction<T, U> | 2(T, U) | int | applyAsInt
ToLongBiFunction<T, U> | 2(T, U) | long | applyAsLong
DoubleToIntFunction | 1(double) | int | applyAsInt
DoubleToLongFunction | 1(double) | long | applyAsInt
IntToDoubleFunction | 1(int) | double | applyAsDouble
IntToLongFunction | 1(int) | long | applyAsLong
LongToDoubleFunction | 1(long) | double | applyAsDouble
LongToIntFunction | 1(long) | int | applyAsInt
ObjDoubleConsumer<T> | 2(T, double) | void | accept
ObjIntConsumer<T> | 2(T, int) | void | accept
ObjLongConsumer<T> | 2(T, long) | void | accept

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

## Java File I/O (NIO.2)

## Java Concurrency

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
Java Exam Classes(en)  |  <not yet>
Java Exam Classes(pt-br)  |  <not yet>
Exam Simulation(en)  | <not yet>
Exam Simulation(pt-br)  | <https://www.udemy.com/metodologia-de-certificacao-java-8-ocp-simulados/learn/v4/overview>
