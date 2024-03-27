## Monads (Haskell)

- Type constructor `m`: Creates a polymorphic type (`m a`) from another type (`a`);
	- Haskell: type constructor `Maybe`. `Maybe a -> Just a | Nothing`
	- C++: Akin to a template, e.g. type constructor `std::vector`, e.g. `std::vector<int>`. Don't get this confused with generics i.e. the general case `std::vector<U>` - monads can apply to both.


A type constructor can be made monadic by defining two operations for interacting with the type constructor and its underlying type:

- Return function: `a -> m a` - This lifts our base type up to our polymorphic type. E.g. `Maybe int` or the constructor of `std::vector<int>`.
- Bind function: `m a -> (a -> m b) -> m b` : unwraps `m a` to `a`, passes it to `a -> m b`, returns `m b`; can choose to perform any additional computation around any of these stages.
	- Depending on the result of the bind function performing `m a->a` it may choose to not execute `a->mb` and instead return a value for `m b`;

```
instance Monad Maybe where
    Nothing  >>= f = Nothing
    (Just x) >>= f = f x
    return         = Just
```

TODO better example is list find


## Higher Order Functions & templating(C++)

A *HOF* is a function that has a function as an argument or return type;


#### Function Objects

```
template<class T, class U>
auto sum(T x, U y)
{
    return x + y;
}

//equivalent Function Object
struct sum_f
{
    template<class T, class U>
    auto operator()(T x, U y) const
    {
        return x + y;
    }
};

//or as wrapped:
struct sum_f
{
    template<class T, class U>
    auto operator()(T x, U y) const
    {
        return sum<T,U>(x,y)
    }
};
```