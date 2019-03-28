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

Methods from map that uses lambda:
* 

## Lambda Built-in Functional Interfaces

## Exceptions and Assertions

## Use Java SE 8 Date/Time API

## Java I/O Fundamentals

## Java File I/O (NIO.2)

## Java Concurrency

## Building Database Applications with JDBC

## Localization


Source  | Data
------------- | -------------
Take Exam  | <https://education.oracle.com/pt_BR/java-se-8-programmer-ii/pexam_1Z0-809>
Java Exam Classes(en)  |  <not yet>
Java Exam Classes(pt-br)  |  <not yet>
Exam Simulation(en)  | <not yet>
Exam Simulation(pt-br)  | <https://www.udemy.com/metodologia-de-certificacao-java-8-ocp-simulados/learn/v4/overview>
