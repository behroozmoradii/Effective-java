# .:':. Item 16 .:':.
 
**In public classes, use accessor methods, not public fields**

// Degenerate classes like this should not be public!

class Point {
public double x;
public double y;
}


Hard-line object-oriented programmers feel that such classes are anathema and should always be replaced by classes with
private fields and public accessor methods (getters) and, for mutable classes, mutators (setters):


Certainly, the hard-liners are correct when it comes to public classes: 

if a class is accessible outside its package, provide accessor methods to preserve the flexibility to change the class’s internal representation. 


If a public class exposes its data fields, all hope of changing its representation is lost because client code can be distributed far and wide.

However, if a class is package-private or is a private nested class, there is nothing inherently wrong with exposing its data fields—assuming they do an adequate job of describing the abstraction provided by the class.


