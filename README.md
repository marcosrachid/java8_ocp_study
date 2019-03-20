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

## Advanced Java Class Design

## Generics and Collections

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
