# Migrate from JavaScript to React

## Fundamental Concepts

### Mutating methods

Mutating methods are array methods that modify the original array.
Mutating methods are: push, pop, shift, unshift, splice, sort, reverse.

### Non-mutating methods

Non-mutating methods return a new array without modifying the original.
Non-mutating methods are: concat, slice, join, indexOf, lastIndexOf, includes, map, filter.

### Side Effect

Side effects is something that makes our function impure by default. Any asynchronous processes or working with things outside of Javascript are side effects.

Typical examples of side effects are:

Working with API,
Working with Localstorage,
Reading or updating DOM tree
Using web sockets
Printing data on screen
Writing data in logs.

### Pure Function

A pure function is a function that contains none side effects and always returns the same result if the same arguments are passed.

```
const PureFunction_calculatePriceWithTax_1 = (productPrice) => {
  const tax = 20;
  return (productPrice * tax) / 100 + productPrice;
};
```

or

```
const PureFunction_calculatePriceWithTax_2 = (productPrice, tax) => {
  return (productPrice * tax) / 100 + productPrice;
};
```

Pure functions are not allowed to mutate external state because this behavior is super difficult to debug.

```
const ImpureFunction_addToCart = (cart, item, quantity) => {
  cart.items.push({
    item,
    quantity,
  });
  return cart;
};
```

push mutates array outside of our function. It doesn't matter that we return cart here because with push we already changed external data (variable cart) that we got as an argument. It's even worse because seeing the return you might think that this function is pure and "just returns data".

Image that you debug a bug in application where some data is randomly changed. Most often it's a case for mutating external state like we do here. What is the solution? Never use Javascript functions that mutate data. Instead of push we can use spread operator and generate a new array.

```
const PureFunction_addToCart = (cart, item, quantity) => {
  return {
    ...cart,
    items: [...cart.items, { item, quantity }],
  };
};
```

Here we create new object and merge all properties of cart inside. After this we update our items array with new object of item and quantity. And yes this code looks much more difficult to understand than array push but it's much more stable, easier to debug and pure.

### Impure (Mutable) Function

An impure function is a function that contains one or more side effects. It mutates data outside of its lexical scope and does not predictably produce the same output for the same input.

Using variables from outside of the functions is one of the ways to make our function impure.

```
let tax = 20;
const ImpureFunction_calculatePriceWithTax = (productPrice) => {
  return (productPrice * tax) / 100 + productPrice;
};
```

Using Math.random or new Date() is also a side effect as every usage of them generates new data. Which means they can't return the same result on the second call.

```
const ImpureFunction_getRandomNumber = () => {
  return Math.random();
};
```

```
const ImpureFunction_getDate = () => {
return new Date()
};
```

### React Function
