# .:: Item 15 ::.

**Item 15: Minimize the accessibility of classes and members**

 A well-designed component hides all its implementation details, cleanly separating its API from its implementation. 
 
 Components then communicate only through their APIs and are oblivious to each others’ inner workings. This concept, known as information hiding or encapsulation, is a fundamental tenet of software design.
 
 The access control mechanism
 [JLS, 6.6] specifies the accessibility of classes, interfaces, and members.
 
##### [public,private,protected]
 
 The rule of thumb is simple: make each class or member as inaccessible as possible. 
 
 In other words, use the lowest possible access level consistent with the proper functioning of the software that you are writing.
 
 For top-level (non-nested) classes and interfaces, there are only two possible access levels:
 
  package-private and public. 
 
 If you declare a top-level class or interface with the public modifier, it will be public; otherwise, it will be packageprivate.
 
 
 If a top-level class or interface can be made package-private, it should 
  be.
 
 
 By making it package-private, you make it part of the implementation rather thanthe exported API,and you can modify it, replace it, or eliminate it in a subsequent release without fear of harming existing clients.
 
 If you make it public, you are obligated to support it forever to maintain compatibility.
 
 
######  For members (fields, methods, nested classes, and nested interfaces), there are four possible access levels, listed here in order of increasing accessibility:
 
 • private—The member is accessible only from the top-level class where it is
 declared.
 
 • package-private—The member is accessible from any class in the package
 where it is declared. Technically known as default access, this is the access
 level you get if no access modifier is specified (except for interface members,
 which are public by default).
 
 • protected—The member is accessible from subclasses of the class where it is
 declared (subject to a few restrictions [JLS, 6.6.2]) and from any class in the
 package where it is declared.
 
 • public—The member is accessible from anywhere.
 
 
Only if another class in the same package really needs to access a member should you remove the private modifier, making the member package-private.

If you find yourself doing this often, you should reexamine the design of your system to see if another decomposition might yield classes that are better decoupled from one another.

 That said, both private and package-private members are part of a class’s implementation and do not normally impact its exported API. 

These fields can, however, “leak” into the exported API if the class implements Serializable (Items 86 and 87).

This is necessary to ensure that an instance of the subclass is usable anywhere that an instance of the superclass is usable (the Liskov substitution principle, see Item 15). 

A special case of this rule is that if a class implements an interface, all of the class methods that are in the interface must be declared public in the class.

To facilitate testing your code, you may be tempted to make a class, interface, or member more accessible than otherwise necessary.
 This is fine up to a point. 
 
 افزایش سطح دسترسی برای اینکه قابلیت تست داشته باشد جایز نیست
 
In other words, it is not acceptable to make a class, interface, or member a part of a package’s exported API to facilitate testing. 

Instance fields of public classes should rarely be public (Item 16).

If an instance field is nonfinal or is a reference to a mutable object, then by making it public, 
you give up the ability to limit the values that can be stored in the field.

This means you give up the ability to enforce invariants involving the field. 


Also, you give up the ability to take any action when the field is modified, so classes with public mutable fields are not generally thread-safe.
Even if a field is final and refers to an immutable object, by making it public you give up the flexibility to switch to a new internal data representation in which the field does not exist.

###### The same advice applies to static fields, with one exception.

You can expose constants via public static final fields, assuming the constants form an integral part of the abstraction provided by the class. 

By convention, such fields have names consisting of capital letters, with words separated by underscores (Item 68). 

a field containing a reference to a mutable object has all the disadvantages of a nonfinal field. 
While the reference cannot be modified, the referenced object can be modified—with disastrous results.


Note that a nonzero-length array is always mutable, so it is wrong for a class
to have a public static final array field, or an accessor that returns such a
field.
If a class has such a field or accessor, clients will be able to modify the contents
of the array. This is a frequent source of security holes:

// Potential security hole!

public static final Thing[] VALUES = { ... };


Beware of the fact that some IDEs generate accessors that return references to private array fields, resulting in exactly this problem.
 There are two ways to fix the problem. You can make the public array private and add a public immutable list:

private static final Thing[] PRIVATE_VALUES = { ... };
public static final List<Thing> VALUES =
Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));


Alternatively, you can make the array private and add a public method that
returns a copy of a private array:


private static final Thing[] PRIVATE_VALUES = { ... };
public static final Thing[] values() {
return PRIVATE_VALUES.clone();
}




 
 
 
 