# .:':. Item 17 .:':.
 
**Minimize mutability**

To make a class immutable, follow these five rules:

1. Don’t provide methods that modify the object’s state (known as mutators).

2. Ensure that the class can’t be extended. (Preventing subclassing is generally accomplished by making the class final, but there is an alternative that we’ll discuss later.)

3. Make all fields final. (Also, it is necessary to ensure correct behavior if a reference to a newly created instance is passed from one thread to another without
                           synchronization, as spelled out in the memory model [JLS, 17.5; Goetz06, 16].)

4. Make all fields private.  

5. Ensure exclusive access to any mutable components. 


on the other hand, can have arbitrarily complex state spaces.
 If the documentation does not provide a precise description of the state transitions performed by mutator methods, it can be difficult or impossible to use a mutable class reliably.
 
  The BigInteger and BigDecimal
 classes did not obey this naming convention, and it led to many usage errors.
 The functional approach may appear unnatural if you’re not familiar with it,
 but it enables immutability, which has many advantages. Immutable objects are
 simple. An immutable object can be in exactly one state, the state in which it was
 created. 
 
######  Immutable objects are inherently thread-safe; they require no synchronization. 
                         
