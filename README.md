# EffectiveJava

# 1.Content


- [1. Content](#1-content)
- [2. Creating and Destroying Objects](#2-creating-and-destroying-objects)
	 -	[Item 3 : Static Factory Methos ](#item-1--static-factory-methods)
	 -	[Item 2 : Builder](#item-2--builder) 
	 -	[Item 3 : Enforce Singleton with a private constructor or an enum type](#item-3--enforce-singleton-with-a-private-constructor-or-an-enum-type)


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
