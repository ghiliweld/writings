# haskell: what i learned and love about it
I took up learning [haskell](https://www.haskell.org/) over the winter 2020 break. haskell is a statically typed, and purely functional language. here's my experience and thoughts on haskell.

### clean, pretty and legible code
what initially sold me on haskell was how clean and pretty the code looked. a quick look at [a gentle intro to haskell](https://www.haskell.org/tutorial/index.html) will show you what i'm talking about. everything i do in haskell just seems much more succinct than in other languages which i appreciate.

a quick look at how quicksort is defined (w/ list comprehensions) shows just how succint functions in haskell can get:

```hs
quicksort :: [Int] -> [Int]
quicksort []           =  []
quicksort (x:xs)       =  quicksort [y | y <- xs, y<x ] ++ [x] ++ quicksort [y | y <- xs, y>=x]
```

the terminology and feel of haskell is pretty mathematical, given it's academic origins and i really like that for some reason, idk makes me feel cool.

you'll notice that a lot of haskell packages are severely lacking in documentation, which isn't that bad given that haskell code is really easy to understand. i've been able to read source code and comments and had an relaatively easy time understanding what was going on.

a big part of that is haskell's powerful type system.

### type system
aside from compile time guarantees and type inference, haskell also has a natural and elegant syntax for user-defined types which are classified as sum types or product types. note that types can also be sums of products. product types aren't that impressive, so i'll only show off sum (aka union) types.

user defined sum types are pretty intuitive so you'll likely grasp the next two predefined types in haskell immediately.

```hs
data Bool = False | True
data Maybe a = Nothing | Just a
```

the first is the type definition for `Bool`(booleans), which can either have the values `True` or `False`.
the second is the polymorphic Maybe type, a type-safe alternative for `null` or `undefined` in other languages. say a function might return a string or it might just be undefined, that function will have a return type of `Maybe String` where it either returns `Nothing` or `Just s` where `s` is a string.

polymorphic is just a fancy word for works with any type `a`. so we can use it for `String`, `Int`, etc...

we can also defined recursive types which is handy for data structures like trees:
```hs
data Tree a = Leaf a | Branch (Tree a) (Tree a)
```
what's happening is that trees are being defined recursively where a Tree element is either a Leaf or a Branch with subtrees.

#### type classes
another amazing thing about types in haskell are type classes, which allows you to group types by class. they're less like classes in OOP and more like interfaces.
you declare an interface for types and you can then instantiate a type into a class by implementing each method of the class.

let's take a look at the `Eq`(equality) class:
```hs
class Eq a where
  (==), (/=)            :: a -> a -> Bool
  x /= y                =  not (x == y)
```
so basically, any type under the `Eq` class will have to implement the `==` method, to be compared for equality. we can use the `Tree` type defined earlier as an example to see how we could instantiate it under the `Eq` type class. we'll show what conditions are needed for two elements of type `Tree` to be equal.
```hs
instance (Eq a) => Eq (Tree a) where 
  Leaf a         == Leaf b          =  a == b
  (Branch l1 r1) == (Branch l2 r2)  =  (l1==l2) && (r1==r2)
  _              == _               =  False
```
the top part of that is a little odd but it just means that given `a` implements `Eq` then `Eq (Tree a)` is defined like so. this is how class inheritance works.

### purity & monads
haskell is a pure language, which shouldn't have side effects. but that's not very practical given we'll want IO for anything useful. haskell handles this with monads. i won't explain too much about monads cause everyone and their dog has a post about them. i just think of it as a way of wrapping computation in a context such that it doesn't leave that context. i say it like this cause we don't want things happening in IO to leave the IO context since outside values shouldn't affect our programs or it'll be harder to ensure purity. it's hard to explain monads outside of outright defining them since that's all there is to it pretty much. makes more sense to explain each monad on a case by case basis.

if you're curious, Monads are a type class defined like so:
```hs
class  Monad m  where
    (>>=)            :: m a -> (a -> m b) -> m b
    (>>)             :: m a -> m b -> m b
    return           :: a -> m a
    fail             :: String -> m a

    m >> k           =  m >>= \_ -> k
```

the IO monad is for wrapping your computation in ann IO context such that it doesn't sneak in our program as a regular value. if something bad is happening, the problem is likely in the IO parts of your code (the compiler will catch errors in the pure parts of the code).

the Maybe monad (another popular one), is also for wrapping your computation in a Maybe (read uncertain) context. in this case you'll likely have succesive function calls of return type `Maybe a`. one important part is we also want a value of Nothing in one function to keep propagating down the succesive function calls. hence why instead of having a bunch of case statements checking if we get `Nothing` we just wrap the whole thing in a Maybe monad context.

the Maybe monad is instantiated like this:
```hs
 instance Monad Maybe where
    return = Just
    Nothing >>= f = Nothing
    (Just x) >>= f = f x
    fail _ = Nothing
```

so given `a x >>= b >>= c`, this will return `Nothing` if anything here returns `Nothing` even `a x`. the "uncertainty" of a return value propagates down in the context.

### cons
stack (the package manager and build tool) is kinda eh to work with, had a lot more fun first getting into Go

haskell is really fun until you have to deploy it, currently working on [vercel-hs](https://github.com/ghiliweld/vercel-hs) for that. it'll help with quick deploys of haskell code on [vercel](https://vercel.com/).
