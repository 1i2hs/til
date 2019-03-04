# Scala
## Conversions between Java and Scala collections
It can be done using JavaConverters

The following conversions are supported via asScala and asJava:
```
Iterator               <=>     java.util.Iterator
Iterator               <=>     java.util.Enumeration
Iterable               <=>     java.lang.Iterable
Iterable               <=>     java.util.Collection
mutable.Buffer         <=>     java.util.List
mutable.Set            <=>     java.util.Set
mutable.Map            <=>     java.util.Map
mutable.ConcurrentMap  <=>     java.util.concurrent.ConcurrentMap
```
The following conversions are supported via asScala and through specially-named extension methods to convert to Java collections, as shown:
```
scala.collection.Iterable    <=> java.util.Collection   (via asJavaCollection)
scala.collection.Iterator    <=> java.util.Enumeration  (via asJavaEnumeration)
scala.collection.mutable.Map <=> java.util.Dictionary   (via asJavaDictionary)
```
The following one-way conversions are provided via asJava:
```
scala.collection.Seq         => java.util.List
scala.collection.mutable.Seq => java.util.List
scala.collection.Set         => java.util.Set
scala.collection.Map         => java.util.Map
```
The following one way conversion is provided via asScala:
```
java.util.Properties => scala.collection.mutable.Map
```
In scala `JavaConverters` class's conversion methods are imported as implicit method, so the methods such as asJava can be used directly from scala collection instance(e.g. list.asJava). However in Java, the class methods are given as static method, so you have to call conversion methods directly from the class(e.g. `JavaConverters.asScalaBufferConverter(<some java collection object as parameter>).asScala()`) .

## How to import package object of Scala into Java code?
example scala code:
```
package com.inho

package object a {
  def one():Integer = 1
}
```
example java code which uses scala code above:
```
package com.something;

import com.a.package$;

public class B {
    public static void main(String[] args) {
        System.out.println(package$.MODULE$.one());
    }
}

```