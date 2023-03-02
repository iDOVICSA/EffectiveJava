# EffectiveJava

# 1.Content


- [1. Content](#1-content)
- [2. Creating and Destroying Objects](#2-creating-and-destroying-objects)
	 -	[Item 3 : Static Factory Methos ](#item-3--static-factory-methods)
	 -	[Item 2 : Builder](#item-2--builder) 
	 -	[Item 3 : Enforce Singleton with a private constructor or an enum type](#item-3--enforce-singleton-with-a-private-constructor-or-an-enum-type)


# 2. Creating and Destroying Objects
## Item 1 : Static Factory Methods 
*   Static factory methods replace constructors in classes, so they are used to create an instance of a class.
*   Unlike constructors, they have names, so they are more trivial to use.
*   Not required to create a new object each time they are invoked.
*   They give us the flexibility to return an object of subtype of their type.
*   the class of returned object could not exist the moment the class of caller has been created.
*   Disadvantage : classes without public constructors can’t be subclassed (good when favoring composition over inheritance).
*   Disadvantage : hard for a programmer to find.
*   Generally like this : `from()`, `of()`, `valueOf()`, `instance()`, `getInstance()`, `create()`,`newInstance()`,`getType()`,`newType()`,`type()`.
## Item 2 : Builder
*   If we have a constructor with so many parameters, this will make it more difficult to write a proper constructor if some params are optional.
*   One solution is to provide setters and getters (JavaBeans pattern), but this could not make us ensure the correctness of the implementation. Also, with JavaBeans, class immutability is precluded (setters and getters).
*   Using builder is a good solution, we provide methods that add a parameter every time and return the builder, and finally a build method that checks the correctness of the process. At the end we will have a call like this that is prone to error:
`new NutritionFacts.Builder(240,8).calories(100).sodium(35).carbohydrate(27).build()`;
## Item 3 : Enforce Singleton with a private constructor or an enum type
