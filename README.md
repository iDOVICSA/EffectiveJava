# EffectiveJava

# 1.Content


- [1. Content](#1-content)
- [2. Creating and Destroying Objects](#2-creating-and-destroying-objects)
	 -	[Item 1 : Static Factory Methods ](#item-1--static-factory-methods)
	 -	[Item 2 : Consider Builder when faced with many constructor parameters](#item-2--consider-builder-when-faced-with-many-constructor-parameters) 
	 -	[Item 3 : Enforce Singleton with a private constructor or an enum type](#item-3--enforce-singleton-with-a-private-constructor-or-an-enum-type)
	 -	[Item 4 : Enforce noninstantiability with a private constructor](#item-4--enforce-noninstantiability-with-a-private-constructor)
	 -	[Item 5 : Prefer dependency injection to hardwiring resources](#item-5--prefer-dependency-injection-to-hardwiring-resources)
	 -	[Item 6 :  Avoid creating unnecessary objects](#item-6--avoid-creating-unnecessary-objects)





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
	* Singleton
	* Remeberb
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
