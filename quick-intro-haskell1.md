<!-- $theme: default -->

Scala / Haskell
===

A quick introduction to Haskell for Scala programmers.

---

### GHCi - REPL for the Glasgow  Haskell Compiler

```text
stack ghci OR ghci (depending on how GHC was installed)

:q - quit
:t - get the type of an expression
:k - get the kind for a type constructor

`let` when defining variables and functions with < GHC 8 
> let x = 1

```

---

### Immutable variables

```scala
val x = 1
```

```haskell 
x = 1
```
* No mutable variables in Haskell.

---

Functions
===

### Function declaration

```scala
def f(x: Int): Int = x + 1
```
* Input type must be provided in Scala.
* Return type can be inferred.

```haskell 
f :: Int -> Int
f x = x + 1
```
* Type signature is optional in Haskell. 
* Full type inference (in standard Haskell).

---

### Function application

```scala
@ def g(x: Int): Int = x + 2
@ f(3)
res13: Int = 4
@ f(g(3))
res19: Int = 6
```
* Scala also has conventional OOP method calls. Haskell does not.

```haskell 
> g :: Int -> Int; g x = x + 2
> f 3
4
> f (g 3)
5
> f $ g 3
6
```
* No parentheses when applying a function to value!
* `$` is often used in place of using parentheses. This often more readable.

---

### Function composition

```scala
def h = f _ compose g _
```

Pointfree
```haskell 
h = f . g
```
Not pointfree
```haskell 
h x = f . g $ x
```
* Point free notation can be more concise and better convey meaning. The opposite can also be true!

---

### Anonymous Functions

```scala
@ ((x: Int) => x + 1)(3)
res5: Int = 4

```

```haskell
> (\x -> x + 1) 3
4
```
* Tip: Backslash looks like lambda.

```
> (+1) 3
4
```
* Know as a section.

---

### Parametric polymorphism

```scala
import scalaz._, Scalaz._

def myLength[A, F[_]: Foldable](xs: F[A]): Int = { 
  xs.foldRight(0)((_, acc) => 1 + acc)
}
```
* `Foldable` type class provided by Scalaz.

```haskell
myLength :: (Foldable t, Num a1) => t a -> a1
myLength xs = foldr (\ _ acc -> 1 + acc) 0 xs
```
* Polymorphic in element, container and return type!

---

### Currying

```scala
def add(x: Int)(y: Int) = x + y
```

```haskell
add x y = x + y
```
* Functions in Haskell are curried by default.

---

Types
===

### Simple wrapper


```scala
case class Age(age: Int) extends AnyVal

@ Age(3) < Age(30)
Compilation Failed

```

```haskell
newtype Age = Age { getAge :: Int }
  deriving (Show, Read, Eq, Ord)
  
> Age 3 < Age 30
True
```  
* No runtime penalty.
* No mechanism for type class derivation in standard Scala.

---

### Product types

```scala
case class Person(name: String, age: Age)

```
```haskell
data Person = Person String Age
  deriving (Show, Read, Eq, Ord)
```
OR
```haskell
data Person = Person
  { name :: String
  , age  :: Age
  } deriving (Show, Read, Eq, Ord)
```
* Record syntax - defines functions to extract fields.
---

### Sum types

###### Enumeration types


```scala
trait Boolean
case object True extends Boolean
case object False extends Boolean

```

```haskell
data Bool = True | False
  deriving (Show, Read, Eq, Ord, Enum, Bounded)
```
* Can derive  `Enum` and `Bounded` type class instances for enum types.
---

###### Sum types (cont.)

```scala
trait List[+A] 
case object Nil extends List[Nothing]
case class Cons[A](head: A, tail: List[A]) extends List[A]
```

```haskell
data List a = Nil | Cons a (List a)
  deriving (Show, Eq, Ord, Functor, Foldable, Traversable)
```
* With language extensions, we can also derive `Functor`, `Foldable` and `Traversable`.

---

### Summary

* Haskell is typically less verbose and has greater clarity than Scala when programming in a functional style.

* Haskell's syntax is very light weight!

* Full type inference and type class derivation are really nice features in Haskell.

* Next session, we should go into pattern matching and type classes.
