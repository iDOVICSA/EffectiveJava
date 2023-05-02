# EffectiveJava

# 1.Content


- [1. Content](#1-content)
- [2. Creating and Destroying Objects](#2-creating-and-destroying-objects)
	 -	[Item 1 : Static Factory Methods ](#item-1-static-factory-methods)
	 -	[Item 2 : Consider Builder when faced with many constructor parameters](#item-2-consider-builder-when-faced-with-many-constructor-parameters) 
	 -	[Item 3 : Enforce Singleton with a private constructor or an enum type](#item-3-enforce-singleton-with-a-private-constructor-or-an-enum-type)
	 -	[Item 4 : Enforce noninstantiability with a private constructor](#item-4-enforce-noninstantiability-with-a-private-constructor)
	 -	[Item 5 : Prefer dependency injection to hardwiring resources](#item-5-prefer-dependency-injection-to-hardwiring-resources)
	 -	[Item 6 :  Avoid creating unnecessary objects](#item-6-avoid-creating-unnecessary-objects)
	 -	[Item 7 :  Eliminate obsolete object references](#item-7-eliminate-obsolete-object-references)
	 -	[Item 8 :  Avoid finalizers and cleaners](#item-8-avoid-finalizers-and-cleaners)
	 -	[Item 9 :  Prefer try-with-resources to try-finally](#item-9-prefer-try-with-resources-to-try-finally)


- [3. Methods Common to All Objects ](#3-methods-common-to-all-objects)
	 -	[Item 11 :  Always override hashCode when you override equals](#item-11-Always-override-hashCode-when-you-override-equals)
	 -	[Item 13 :  Override clone judiciously](#item-13-override-clone-judiciously)

- [4. Classes and interfaces ](#4-classes-and-interfaces)
	 -	[Item 15 :  Minimize the accessibility of classes and members](#item-15-minimize-the-accessibility-of-classes-and-members)
	 -	[Item 18 :  Favor composition over inheritance ](#item-18-favor-composition-over-inheritance)
	 -	[Item 24 :  Favor static member classes over nonstatic ](#item-24-favor-static-member-classes-over-nonstatic)
	 -	[Item 25 : Limit source files to a single top-level class](#item-25-limit-source-files-to-a-single-top-level-class)

- [5. Generics ](#5-generics)
	 -	[Item 26 : Don’t use raw types](#item-26-don’t-use-raw-types)
	 -	[Item 27: Eliminate unchecked warnings](#item-27-eliminate-unchecked-warnings)
	 -	[Item 28 : Prefer Lists to arrays](#item-28-prefer-Lists-to-arrays)


- [6. Enums and annotations ](#5-enums-and-annotations)
	 -	[Item 34 : Use enums instead of int constants](#item-34-use-enums-instead-of-int-constants)
	 -	[Item 35: Use instance fields instead of ordinals](#item-35-use-instance-fields-instead-of-ordinals)

- [7. Lambdas and Streams ](#7-lambdas-and-streams)

- [8. Methods ](#8-methods)
	 -	[Item 54 : Return empty collections or arrays, not nulls](#item-54-return-empty-collections-or-arrays-not-nulls)

- [9. General Programming ](#9-general-programming)
	 -	[Item 58 : Prefer for-each loops to traditional for loops](#item-58-prefer-for-each-loops-to-traditional-for-loops)
	 -	[Item 61 : Prefer primitive types to boxed types](#item-61-prefer-primitive-types-to-boxed-types)
	 -	[Item 63 : Beware of performance of strings concatenation](#item-63-beware-of-performance-of-strings-concatenation)

# 2. Creating and Destroying Objects
## Item 1 : Static Factory Methods 
* Retrun an instance of a class via a static method 
* Traditionnaly we use public contructor to do the same job

* ### Advantages :
	* static methods have names resulting in ease reading of the code 
	* overcome the problem of creating multiple constructors with order different signatures by creating multiple static methods with well chosen names  
	* static methods allows returning an object of any subtype of their return type

* ### Limitations : 
	* classes without constructors cannot be subclassed
	
## Item 2 : Consider Builder when faced with many constructor parameters
* Builder pattern is used when we deal with classes that have multiple parameters in which some of them are optional so instead of writing multiple constructors to meet every single combination of the parameters or at least desired combinations we use the builder pattern which consist of providing static factories that add parameters and return the Builder class and finally calls a build method that instanciate the object and returns it.
	
## Item 3 : Enforce Singleton with a private constructor or an enum type
* Singleton is simply a class that is instantiated exactly once
* Export a final member (method or field) to to provide access to the singleton instance
* Another way to implement a singleton is to declare a single-element enum

## Item 4 : Enforce noninstantiability with a private constructor
* Making a class abstract does not resolve noninstantiability issue because it can be subclassed then instanciate the subclass
* To enforce noninstantiability we declare a private constructor instead
* This is used for utilities classes

## Item 5 : Prefer dependency injection to hardwiring resources
* When a class depends on one or more ressources its behaviour is parameterized 
* Using singletons or utilities is unsatisfactory
* As a solution we can inject (pass) these resources (dependencies) in the class constructor when creating a new instance


## Item 6 : Avoid creating unnecessary objects
* Avoid creating unnecessary objects by using static factory methods
* The constructor must create a new object each time it’s called, while the factory method is never required to do so  
* Cache "expensive objects" to be reused instead of expensive creations repeatdely 
* prefer primitives to boxed primitives, and watch out for unintentional autoboxing

## Item 7 : Eliminate obsolete object references
* Avoid memory leaks scenarios by : nulling out references once they become obsolete
* Nulling out object references should be the exception rather than the norm.
* Generally, whenever a class manages its own memory, the programmer should be alert for memory leaks


## Item 8 : Avoid finalizers and cleaners
* Cleaners are less dangerous than finalizers, but still unpredictable, slow, and generally unnecessary
* there is no guarantee they’ll be executed 
* Never do anything time-critical in a finalizer or cleaner
* It’s better to not use them 

## Item 9 : Prefer try-with-resources to try-finally
* Use try with ressources when dealing with resources that must be closed
* try-with-resources versions shorter and more readable than the originals (try finaly blocks)


# 3. Methods Common to All Objects

## Item 11 : Always override hashCode when you override equals
* Equal objects according to equals method must have equal hash codes
* Unequal objects according to equals method doesn't need to produce distinct integer results
* A hashCode method that returns a fixed value is legal as it preserves the first two rules but it degenerate a hashMap to linkedList as all objects will be hashed to the same bucket
* a hash function should distribute any reasonable collection of unequal instances uniformly across all int values
* Consider caching the hash code in the object rather than recalculating it each time it is requested if the hash function computing time is significant.

## Item 13 : Override clone judiciously 
* if a class implements Cloneable, Object’s clone method returns a field-by-field copy of the object otherwise it throws CloneNotSupportedException
* immutable classes should never provide a clone method
* We must cast the result of super.clone from Object to the actual class before returning it to avoid casting in the client
* If an object contains fields that refer to mutable objects, a clone of that object will refer to the same mutable objects (fields) , a proper implementation of clone method should also clone those mutable objects before returning the final clone result
* We should ensure that clone method does no harm to the original object.
* A better approach to object copying is to provide a copy constructor or copy factory.  


# 4. Classes and interfaces

## Item 15 : Minimize the accessibility of classes and members
* information hiding (encapsulation) decreases the risk in building large systems because individual components may prove successful even if the system does not.
* The general rule for encapsulation is to make each class or member as inaccessible as possible.
* For top level classes and interfaces there is only two possibilities, either public or package private so if they can be package private then they should be.
* If a package-private top-level class or interface is used by only one class, consider making the top-level class a private static nested class of the sole class that uses it
* If a method overrides a superclass method, it cannot have a more restrictive access level in the subclass than in the superclass
* Instance fields of public classes should rarely be public
* It is wrong for a class to have a public static final array field
* Module is a grouping of packages that was introduced by java 9

## Item 18 : Favor composition over inheritance
* A subclass depends on the implementation details of its superclass
* Inheritance violates the encapsulation principle
* Composition is giving the new class a private field that references an instance of the existing class instead of extending it
* A class B should extend a class A only if an “is-a” relationship exists between the two classe if not use composition is prefered

## Item 24 : Favor static member classes over nonstatic
* A nested class is a class defined within another class, it servers only the enclosing class
* There are four types of nested classes : static-member, non-static-members, anonymous and local classes
* static-members classes : it has access to all of the enclosing class’s members 
* non-static-members classes : Each instance of a nonstatic member class is implicitly associated with an enclosing instance of its containing class 
* it is impossible to create an instance of a nonstatic member class without an enclosing instance.
* The association between a nonstatic member class instance and its enclosing instance is established when the member class instance is created


## Item 25 :  Limit source files to a single top-level class
* Never put more than one top level java class in one single java source file.


# 5. Generics

## Item 26 : Don’t use raw types
* A generic class or interface is a class or interface whose declaration has one or more type parameters (List<E>)
* A raw type is the name of a generic class or interface without any type arguments.
* We should never use raw types over generics because we lose

## Item 27 : Eliminate unchecked warnings 
* Eliminate as much as possible warnings when programming using generics
* If you can’t eliminate a warning, but you can prove that the code that provoked the warning is typesafe, then (and only then) 
suppress the warning with an @SuppressWarnings("unchecked") annotation

## Item 28 : Prefer Lists to arrays
* Arrays are covariant: that means if Sub is a subtype of Super, then Sub[] is also a subtype of Super[].
* I contrast, Generics are invariant for any two types Type 1 and Type2 (whether they are related or not), List<Type1> is neither subtype nor supertype to List<Type2>.
* It is illegal to create an array of a generic type. 



# 6. Enums and Annotations

## Item 34 :  Use enums instead of int constants
* An enumerated type is a type whose legal values consist of a fixed set of constants
* int enum pattern provides nothing in the way of type safety and little in the way of expressive power
* To avoid the previous shortcommings we should use enum type instead
* Use enums any time you need a set of constants whose members are known at compile time.
* Enum types with identically named constants coexist peacefully because each type has its own namespace
* Enums provide compile-time type safety.
* Enums are classes that export one instance for each enumeration constant (they cannot be instantiated )
* Use constant-specific method enums to associate  different behavior with each constant


## Item 35 : Use instance fields instead of ordinals
* `ordinal()` method returns the ordinal of this enumeration constant (its position in its enum declaration)
* Never derive a value associated with an enum from its ordinal; store it in an instance field instead


# 7. Lambdas and Streams


# 8. Methods

## Item 54 : Return empty collections or arrays, not nulls
* Returning null instead of empty collections in a function is error prone on the client side using that function
* Never return null in place of an empty array or collection

# 9. General Programming

## Item 58 : Prefer for-each loops to traditional for loops
* for-each loop is the preferred idiom for iterating over collections and arrays
* There are some situations where we cannot use the for-each loops for example : Destructive filtering, Transforming, Parallel iteration

## Item 61 : Prefer primitive types to boxed types
* Every primitive type has a corresponding reference type called a boxed primitive
* Primitives are more time- and space-efficient than boxed primitives
* Applying the `==` operator to boxed primitives is almost always wrong
* When we mix primitives and boxed primitives in an operation, the boxed primitive is auto-unboxed

# Item 63 : Beware of performance of strings concatenation 
* Using the string concatenation operator `+` repeatedly to concatenate n strings requires time quadratic in n
* To achieve acceptable performance, use a StringBuilder in place of a String



# Exception Handling : 
* Exceptions are anomalous situations during a program’s execution. 
* Exception handling is an error-handling mechanism that prevents the application from crashing, it takes the steps necessaries in order to either recover from the error or fail gracefully.

## Best practices for exception handling in java
* Use a try with resource statement (item 9)
* Prefer Specific Exceptions : provide as much information as possible in the thrown exceptions
* Document the Exceptions You Specify : make sure to add a @throws declaration to your Javadoc and to describe the situations that can cause the exception.
* Throw Exceptions With Descriptive Messages
* Catch the Most Specific Exception First for example if we catch IllegalArgumentException first, we will never reach the catch block that should handle the more specific NumberFormatException because it’s a subclass of the IllegalArgumentException. (order of catch blocks matters)
* Don’t Catch Throwable : Throwable is the superclass of all exceptions and errors. You can use it in a catch clause, but you should never do it!
* Don’t Ignore Exceptions
* Don't log and throw because it will write multiple error messages for the same exception.
* Wrap the Exception Without Consuming It : It’s sometimes better to catch a standard exception and to wrap it into a custom one.

# Immutability
* An immutable object is an object whose internal state remains constant after it has been entirely created
* In Java, variables are mutable by default, meaning we can change the value they hold
* `final` keyword only forbids us from changing the reference the variable holds, it doesn't protect us from changing the internal state of the object
* Java guarantees that primitives types values will not change.
* Immutable objects don't change their internal state in time, they are thread-safe and side-effects free  

# Equals and hashCode 
* Equals violations happen most often if we extend a class that has overridden equals().
* To avoid symmetry criteria violation favor composition over inheritance (item 18).
* Override the hashCode() method so that it adheres to the contract; equal objects return the same hashCode.
* If we want to check whether our implementations adhere to the Java SE contracts, and also to best practices, we can use the EqualsVerifier library.



# Java 8 :

## Default methods
* Default methods are methods that are implemented directly in the interface do not need to be implemented in the classes.
* Interfaces default methods can be overridden in the classes (but do not need to)
* A class that implements several interfaces that have the same default method implemented, has to implement this method itself to avoid the diamond problem
* Java 8 offers also the possibility to define static methods inside interfaces 
* To call these static methods we should prefix them with the interface name, not the class implementing the interface.
* Default methods can only invoke other default or static methods from the same interface

## Java Lambda Expressions
* A way to represent a functional interface using an expressio
* They also improve some functionality in Collection libraries such as iterate, filter and extracting data from a Collection.
* A lambda expression is included three parts : argument list, arrow token and body for example
	* `(int x, int y) -> x+y `
* Lambda expressions are a useful replacement for anonymous inner classes to simplify the code.
* A Functional Interface includes only an abstract method, if we add more it throws compile time error.
* There are number of different functional interfaces in `java.util.function`
* Predicate is one of the functional interface in this package which includes a test method which is useful for filtering mechanisms.
 
## Java Method Reference 
* Method Reference reduces the code written in a lambda expression
* Double-colon operator `(::)` is used for method references.
* When a Lambda expression is invoking an already defined method, developers can replace it with a reference to that method.
* We cannot pass arguments to the method reference

## Java 8 Comparator 
* Comparator now supports declarations via lambda expressions for example :
* `Comparator<Book> descPriceComp = (Book b1, Book b2) -> (int) (b2.getPrice() - b1.getPrice());`
* The comparator instance is then passed into the Collections.sort() method as normal.

## Consumer and Supplier
* `Consumer<T>` and `Supplier<T>` are  in-built functional interfaces  introduced in Java8 in the `java.util.function package`
* The consumer can be used in all contexts where an object needs to be consumed
* The supplier can be used in all contexts where there is no input but an output is expected.

## Java 8 Base64
* In old Java versions, it was needed to use external libraries like commons-codec or sum.misc.BASE64Decoder for encoding and decoding Strings
* Now, the package java.util already contains a couple of classes that support this : `java.util.Base64`, `java.util.Base64.Decoder`, `java.util.Base64.Encoder`

## Java 8 Streams 
* Streams are sequences of elements that support concatenated operations
* Intermediate operations are the ones that produce a stream : `filter`, `map`, `distict` or `flatMap` 
* Terminal operations are the ones that produce value of other type,  for example `forEach`, `collect`, `reduce` and `min` or `max`
* Only one terminal operation is possible, if a terminal operation is executed the stream will be closed and cannot be used again.
* In order to create an stream, we need always a source (Arrays, Collections ...)
* Once a stream is closed it cannot be used unless we recreate it.

## Map Sorting
* Sorting a Map by Key : `Map.entrySet().stream().sorted(Map.Entry.comparingByKey())`
* Sorting a Map by Value : `Map.entrySet().stream().sorted(Map.Entry.comparingByValue())`

## Convert a Stream to List 
* The standard way to collect the result of a stream in a container i.e. List, Set or any other Collection is to use `collect()` method : `List listOfStream = streamOfString.collect(Collectors.toList());`
* Using `forEach` to go through all elements of Stream and add them to list 

## Join arrays 
* Using Stream.concat after converting arrays to stream
* Using System.arraycopy

## Java 8 Streams: allMatch(), anyMatch(), noneMatch()
* `Stream.allMatch()` method returns true if all the elements of the stream match the provided predicate condition
* `Stream.anyMatch()` method returns true if at least 1 of the elements of the stream match the provided predicate
* `Stream.noneMatch()` method returns true if none of the elements of the stream match the provided predicate condition. 

## Java 8 Collect vs Reduce
* Reducing in the context of Java8 Streams refers to the process of combining all elements in the stream repeatedly to produce a single value which is returned as the result of the reduction operation for example summation, finding maximum ... 
* The reduction operation's participant elements are :
	* Identity :  element that is the initial value of the reduction operation and the default result if the stream is empty
	* Accumulator : a function that takes two parameters: a partial result of the reduction operation and the next element of the stream
	* Combiner : a function used to combine the partial result of the reduction operation when the reduction is parallelized or when there's a mismatch between the types of the accumulator arguments and the types of the accumulator implementation

## Java 8 flatMap
* A programming operation that takes a single function as argument which will be applied to each element of this stream to construct a stream of new values 
* The flatMap() method is a combination of Map and Flattening operation
* 

## Java Read File
* Java8 has added the Files.lines() method to read the file data using the Stream
* `Stream<String> lines = Files.lines(Paths.get(fileName))`



# Git tutorials 


## Git basics
* Git is a distributed version control system used to keep track of file changes history in term of :

	* when it was changed
	* who made the changes
	* why he made the changes
	* what files were changed and the actual changes that happened

* Git allows developers to collaborate on projects, maintain different versions of the codebase, and track changes made by each contributor

## Git basic commands

* git init: initializes a new local repository.
* git status: shows files that need to be committed
* git add : adding files to staging area (we use "." to express all files)
* git commit :creates the files snapshot to be saved in version control history
* git push : pushes commits to a remote repository
* git pull : pulls changes from a remote repository
* git branch : creates a new branch 
* git checkout branchName: switch from the current branch to branchName branch
* git checkout -b : creates and switches the newly created branch 
* git log : lists the version control history (all previous commits)
* git merge branchName: merges changes made in branchName with the master branch

## Git Cherry Pick
* Git command that enables arbitrary Git commits to be picked by reference and appended to the current working HEAD
* It is used for : team collaboration, bug hotfixes, undoing changes and restoring lost commits
* Execute the cherry-pick with the following command: `git cherry-pick commitSha`

## Git branching
* Branching means you diverge from the main line of development and continue to do work without messing with that main line 
* Use `git branch branchName` to create a new branch
* Git knows what branch we’re currently on by keeping a special pointer called HEAD
* Use `git checkout branchName` to switch branches (moves HEAD to point the branchName)
* Use `git branch -d branchName` to delete a branch
* Use `git branch --move oldName newName` to rename a branch then delete oldName branch from the remote using `git push origin --delete oldName` 
