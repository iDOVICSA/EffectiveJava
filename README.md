# EffectiveJava
_Comments on some important items from the book : `Effective Java : Third Edition by Joshua Bloch`._
_Diaeddin BOUIDAINE_
# 1.Content


- [1. Content](#1-content)
- [2. Creating and Destroying Objects](#2-creating-and-destroying-objects)
	 -	[Item 1 : Static Factory Methods ]
	 -	[Item 2 : Builder](#item-2--builder)
	 -	[Item 3 : Enforce Singleton with a private constructor or an enum type](#item-3--enforce-singleton-with-a-private-constructor-or-an-enum-type)
	 -	[Item 4 : Enforce noninstantiability with a private constructor](#item-4--enforce-noninstantiability-with-a-private-constructor)
	 -	[Item 5 : Prefer dependency injection](#item-5--prefer-dependency-injection)
	 -	[Item 6 : Avoid creating unnecessary objects](#item-6--avoid-creating-unnecessary-objects)
	 -	[Item 7 : Eliminate obsolete objects](#item-7--eliminate-obsolete-objects)
	 -	[Item 8 : Avoid finalizers and cleaners](#item-8--avoid-finalizers-and-cleaners)
	 -	[Item 9 : Prefer try-with-resources to try-finally](#item-9--prefer-try-with-resources-to-try-finally)
- [3. Methods Common to All Objects](#3-methods-common-to-all-objects)
	 -	[Item 11 : Always override hashCode when overriding equals](#item-11--always-override-hashcode-when-overriding-equals)
	 -	[Item 13 : Override clone judiciously](#item-13--override-clone-judiciously)
- [4. Classes and Interfaces](#4-classes-and-interfaces)
	 -	[Item 15 : Minimize the accessibility of classes and members](#item-15--minimize-the-accessibility-of-classes-and-members)
	 -	[Item 18 : Favor composition over inheritance](#item-18--favor-composition-over-inheritance)
	 -	[Item 24 : favor static member classes over nonstatic](#item-24--favor-static-member-classes-over-nonstatic)
	 -	[Item 25 : limit source file to a single top-level class](#item-25--limit-source-file-to-a-single-top-level-class)
- [5. Generics](#5-generics)
	 -	[Item 26 : Don’t use raw types](#item-26--dont-use-raw-types)
	 -	[Item 27 : Eliminate unchecked warnings](#item-27--eliminate-unchecked-warnings)
	 -	[Item 28 : Prefer lists to arrays](#item-28--prefer-lists-to-arrays)
- [6. Enums an annotations](#6-enums-an-annotations)
	 -	[Item 34 : Use enums instead of int constants](#item-34--use-enums-instead-of-int-constants)
	 -	[Item 35 : Use instance fields instead of ordinals](#item-35--use-instance-fields-instead-of-ordinals)
- [8. Methods](#8-methods)
	 -	[Item 54 : Return empty collections or arrays , not nulls](#item-54--return-empty-collections-or-arrays--not-nulls)
- [9. General Programming](#9-general-programming)
	 -	[Item 58 : Prefer for-each loops to traditional for loops](#item-58--prefer-for-each-loops-to-traditional-for-loops)
	 -	[Item 61 : Prefer primitive types to boxed types](#item-61--prefer-primitive-types-to-boxed-types)
	 -	[Item 63 : Beware of performance of strings concatenation](#item-63--beware-of-performance-of-strings-concatenation)

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
## Item 4 : Enforce noninstantiability with a private constructor
*   To ensure a class is noninstantiable, we could make it abstract.
*   This is violated when it has subclasses that will be instantiated.
*   A private constructor could resolve the problem.
## Item 5 : Prefer dependency injection
*   Some classes depend on one or many resources.
*   Some people implement these classes as static or singletons, this implementation is wrong because it’s inflexible and untestable.
*   For this, we use dependency injection, we inject the resource into the constructor, or in dependent methods.
## Item 6 : Avoid creating unnecessary objects
*   An example of an unnecessary object is doing as follows : `String s = new String(“this is a string”)` where it could’ve been done simply by this : `String s = “this is a string”`.
*   If the first implementation occurs in a for loop, millions of String objects could be created, that could’ve been avoided.
*   Another example of creating unnecessary objects is when we match regular expressions by using `String.matches(re)` every time. We could initialize a matcher, and use it when in need.
*   Also, we must be aware about the difference between primitives and boxed primitives, and to watch out for autoboxing.
## Item 7 : Eliminate obsolete objects
*   While in Java allocation and liberation of resources is done automatically, and we also have the Garbage Collector for the liberation of resources, programmers must be aware whenever a class is managing its memory.
*   An example of this are arrays that contain objects, we must be aware to nullify the array on indexes that are no more in use, otherwise we will have a memory leak.
*   Another example of memory leaks are caches, we mustn't forget about references that are put in cache.
*   Also, listeners and callbacks could be sources of memory leaks. We mustn’t forget to deregister callbacks, otherwise they will accumulate.
## Item 8 : Avoid finalizers and cleaners
*   Finalizers and cleaners are used to manually free space in memory.
*   They are usually associated with many errors due to their execution (they are put into a stack).
*   It’s better to not use them because they will degrade the performance significantly.
## Item 9 : Prefer try-with-resources to try-finally
*   Whenever we have resources that must be opened and closed, we must use try-with-resources instead of using try-finally, because try and finally blocks are both capable of throwing exceptions so the call for closing may not be reached.

# 3. Methods Common to All Objects
## Item 11 : Always override hashCode when overriding equals
*   There’re many things that must be respected in the hashCode method, one important thing is that objects that are equal must have the same hashCode.
*   Overriding equals means defining the criterias of equality between two objects, this implies that the hashCode method should also be adapted to this change.
## Item 13 : Override clone judiciously
*   Classes that implement cloneable are meant to provide a well functioning public clone method, and to copy the cloned object field-by-field. Otherwise, `CloneNotSupportedException` must be thrown.
*   Immutable classes should never provide a clone method.
*   The returned class should be returned by calling super.clone method, but we mustn't forget to use casting to cast the class.
*   We must be aware when we have fields in objects that refer to other mutable objects, because if we do the clone they will refer to the same memory reference, these mutable objects must also be cloned alongside the object (when cloning arrays we don’t need casting).
*   We might have a problem with final fields when we copy and modify these fields.
*   We could use alternatives such as copying constructor, or Copy Factory, these have many advantages over the others and could enable us to avoid mentioned problems (casting, final fields, exceptions, conventions).

# 4. Classes and Interfaces
## Item 15 : Minimize the accessibility of classes and members
*   Each class or member must be as inaccessible as possible.
*   Four levels of accessibility for members ( fields, methods, nested classes, and nested interfaces):
	 *   private : accessible only in the same class or superclasses.
	 *   private-package : members are accessible in classes in the same package.
	 *   protected : members are accessible in classes in the same package as well as subclasses.
	 *   public : members are accessible from anywhere.
*   Fields should rarely be public.
*   Arrays shouldn’t be declared as public final fields, because they are mutable, instead, we could use unmodifiablelist or get them by cloning.
## Item 18 : Favor composition over inheritance
*   Inheritance violates the encapsulation principle because inherited classes depend on the implementation of their parents.
*   On the other hand, composition provides better encapsulation than inheritance. Also, in composition we have more flexibility as we could change the behavior of the composed object.
*   Sometimes subclasses inherit unnecessary implementation details from superclasses.
## Item 24 : favor static member classes over nonstatic
*   Four types of nested classes (non static, static, anonymous and local).
*   Static member class is a class nested in another class. It obeys the same rules as of membership because it’s itself a member (can access other private members).
*   We must always favor using static member classes. For this, whenever we have a nested class that doesn’t require any access to other members of its enclosing class (means don’t have to have an instance of the enclosing class), we should make this nested class static.
## Item 25 : limit source file to a single top-level class
*   Never put more than one class at the same source file, this could create problems while compiling. 
*   This ensures not having multiple definitions for the same class at compile time.

# 5. Generics
## Item 26 : Don’t use raw types
*   Generic classes or interfaces are the ones who have one or more types as generic, ex.: `List<E>`.
*   Using raw types could generate or lead to errors in code.
*   Always favor the use of parameterized types (this also enables us to avoid casting and to ensure type safety).
*   Ex. : Type safety is ensured when we use `List<String>` instead of only using the raw type List.
*   Unbounded Wildcard Types ex. `Set<?>` , are used when we need generic types but don’t care or don’t know about actual types.
*   (TO-ADD) table in the book that explains types ……
## Item 27 : Eliminate unchecked warnings
*   This rule is clear, we must try our best to eliminate any warning. If we eliminate all warnings, we ensure that our program is typesafe. 
*   If we can’t eliminate the warning, but we proved that it’s well written and that we can’t get Exceptions related to typesafe at runtime, we could suppress them. This must be done on the smallest scope possible, which means for example not to do it on a whole class because it could mask important unchecked warnings.
*   Every time we use warning suppression, we must add a comment to explain why it’s suppressed.
## Item 28 : Prefer lists to arrays
*   Arrays are covariant: that means if Sub is a subtype of Super, then Sub[] is also a subtype of Super[].
*   In contrast, Generics aren’t. They are invariant : for any two types Type 1 and Type2 (whether they are related or not), List<Type1> is neither subtype nor supertype to List<Type2>.
*   For this, arrays are deficient, for example, if we have the following declaration: 
	```java
            Object[] objectArray = new Long[1];
            objectArray[0] = "I don't fit in";
    ```
*   And this : 
	```java
        List<Object> ol = new ArrayList<Long>();
	    ol.add("I don't fit in");
    ```
*   The first one (with arrays) will fail at runtime ! but the second will fail at compile time (better).
*   Arrays are reified, meaning that they enforce their element type at runtime, but Lists are implemented by erasure, meaning they enforce element type at compile time but erase this and ignore it at runtime.
*   For these reasons, arrays and generics don’t mix will, ex., it’s illegal to declare an array of generic type, a parameterized type, or a type parameter. Ex. for illegal declarations : `new List<E>[]`, `new List<String>[]`, `new E[]`.

# 6. Enums an annotations
## Item 34 : Use enums instead of int constants
*   Using int enum pattern (declare enums as int constants) is not good in expressiveness, also not good because if they change, the program must be recompiled. Another shortcoming is that there’s no way to iterate through enums of the same group.
*   To overcome these problems, Java provides the enum type, enums in Java are different than others in other languages, because they’re full-fledged classes in Java : they are classes that exports one instance for each enumeration constant via public static final field, enums classes are final because they could not be created as instances, or extended.
*   Enums are associated with Singleton (a generalization of Singleton), see Item 3.
*   Enums provide compile-time type safety.
*   To associate data with enum constants, we declare instance fields and write a constructor that takes the data and stores it in the fields.
## Item 35 : Use instance fields instead of ordinals
*   enums are associated with the `ordinal()` method that gives the order of enum.
*   Instead of using this method, it’s better to associate enums with orders ex.: 
    ```java
        public enum Ensemble {
            SOLO(1), //we associated SOLO with 1
            …………
        }
    ```

# 8. Methods
## Item 54 : Return empty collections or arrays , not nulls
*   It’s always better to return empty arrays or collections, instead of returning null, because there’s no reason to special-case for emptiness, and returning null will lead to special-case when client is dealing with code.
*   If we want to return an array for cheese considering also emptiness, we could do as following: `return cheese.toArray(new Cheese[0])`, in toArray parameter we pass a zero-length array if type Cheese to indicate the type, the last instruction could be optimized by pre-allocation of empty array as final field and passing it to toArray method, to avoid allocating empty arrays every time.

# 9. General Programming
## Item 58 : Prefer for-each loops to traditional for loops
*   Some tasks are better accomplished with streams, others with iteration.
*   Because for loops require more expressions, and more declarations, this may result in errors or mistakes.
*   Their are some situations where it’s not possible to use for-each loops : 
	 *   Destructive filtering : When we need to remove elements while iterating, we must use the Iterator to use its remove method.
	 *   Transforming : we need the list iterator or the array index to modify elements.
	 *   Parallel iteration : If we need to traverse multiple collections in parallel, we need explicit control over the iterator.
*   For-each lets us iterate through any object that implements the Iterable interface, which consists of a single method (`iterator()`).
*   In summary, for-each loops provide flexibility, clarity, and bug prevention, without penalty on performance.
## Item 61 : Prefer primitive types to boxed types
*   Java  has two-part type system : 
	 *   Primitives: such as `int`, `double` …etc.
	 *   Reference types : such as `List`…etc.
*   Each primitive has its corresponding reference type, called boxed primitive. Ex: Corresponding boxed primitive for `int`, `double` and `boolean` is `Integer`, `Double` and `Boolean`.
*   Differences between primitives and boxed primitives:
	 *   Primitives have only their values, while boxed primitives have also their identities alongside their values.
	 *   Primitives have only functional values, while boxed primitives could have `null`.
	 *   Primitives are more time- and space-efficient than boxed primitives.
*   When comparing boxed primitives, the reference is compared, to avoid this, we must save the boxed primitive in a primitive variable (auto-unboxing is done), and then proceed to the comparison.
*   Boxed primitives are initialized with `null` when declared, in a case such as comparison with a primitive, the boxed primitive will be auto-unboxed, and a `NullPointerException` will be thrown (auto-unboxing of `null`).
*   As an example of the effect of boxed primitives on performance, refer to item 6 where the boxed primitive is repeatedly boxed and unboxed resulting in severe degradation of performance.
## Item 63 : Beware of performance of strings concatenation
*   Using the string concatenation operator (`+`) to concatenate n strings requires a time that is quadratic in n. 
*   This is caused by the fact that strings are immutable, when they are concatenated, the contents of both are copied.
*   To avoid this problem, we use `StringBuilder` to concatenate strings using the append method.
*   The following rule is important: Don't use a concatenation operator to combine more than a few strings, because time will increase quadratically.
