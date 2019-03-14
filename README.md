# Object.assign

## Learning Goals

- Use `Object.assign` to merge object properties

## Introduction
JavaScript provides us access to a global `Object`. `Object` that has a bunch of
helpful methods we can use. One of those methods is `Object.assign()`, which
allows us to combine properties from multiple `Object`s into a single `Object`.
In this lab, we're going to learn how to use `Object.assign` to do just that.

### `Object.assign()`

 The first argument passed to `Object.assign()` is the first `Object` into which
 all of the properties are merged. Every additional argument is an `Object`
 whose properties we want to merge into the first `Object`:

```js
Object.assign(firstObject, secondObject, thirdObject, ...);
```

The return value of `Object.assign()` is the first `Object` after all of the
additional `Object`s' properties have been merged in:

```js
Object.assign({ eggs: 3 }, { flour: '1 cup' });
// => { eggs: 3, flour: "1 cup" }

Object.assign({ eggs: 3 }, { chocolateChips: '1 cup', flour: '2 cups' }, { flour: '1/2 cup' });
// { eggs: 3, chocolateChips: "1 cup", flour: "1/2 cup" }
```

Pay attention to the `flour` property in the above example. **If multiple
`Object`s have a property with the same key, the last key to be defined wins
out**. Essentially, the last call to `Object.assign()` in the above snippet is
wrapping all of the following assignments into a single line of code:

```js
const recipe = { eggs: 3 };

recipe.chocolateChips = '1 cup';

recipe.flour = '2 cups';

recipe.flour = '1/2 cup';
```

### Use `Object.assign()` to Non-destructively Assign New Data

A common pattern for `Object.assign()` is to provide an empty `Object` as the
first argument. That way we're providing an entirely new `Object` instead of
modifying or overwriting the properties of an existing `Object`. This pattern
allows us to rewrite the above `destructivelyUpdateObject()` function in a
nondestructive way:

```js
function nondestructivelyUpdateObject(obj, key, value) {
  return Object.assign({}, obj, { [key]: value });
}

const recipe = { eggs: 3 };

const newRecipe = nondestructivelyUpdateObject(recipe, 'chocolate', '1 cup');
// => { eggs: 3, chocolate: "1 cup" }

newRecipe;
// => { eggs: 3, chocolate: "1 cup" }

recipe;
// => { eggs: 3 }
```

It's important that we merge everything into a new, empty `Object`. Otherwise, we
would be modifying the original `Object`. In your browser's console, test what
happens if the body of the above function were `return Object.assign(obj, {
[key]: value });`. Uh oh, back to being destructive!

Let's write a function for a restaurant that accepts an old menu and some
changes we want to make and returns a new menu **without modifying the old
menu**:

```js
function createNewMenu (oldMenu, menuChanges) {
  return Object.assign({}, oldMenu, menuChanges);
}

const tuesdayMenu = {
  cheesePlate: {
    soft: 'Chèvre',
    semiSoft: 'Gruyère',
    hard: 'Manchego'
  },
  fries: 'Sweet potato',
  salad: 'Caesar'
};

const newOfferings = {
  cheesePlate: {
    soft: 'Brie',
    semiSoft: 'Fontina'
  },
  salad: 'Southwestern'
};

const wednesdayMenu = createNewMenu(tuesdayMenu, newOfferings);

wednesdayMenu;
// => { cheesePlate: { soft: "Brie", semiSoft: "Fontina", hard: "Manchego" }, fries: "Sweet potato", salad: "Southwestern" }

tuesdayMenu;
// => { cheesePlate: { soft: "Chèvre", semiSoft: "Gruyère", hard: "Manchego" }, fries: "Sweet potato", salad: "Caesar" }
```

Bon appétit!

## Conclusion

As you can see, `Object.assign` can be useful both in a destructive and non-destructive capacity, depending on how you need to use it. Play around with it in console and see what happens when you use different data types when you try to combine objects - the results might surprise you!

## Resources

[Object.assign]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
