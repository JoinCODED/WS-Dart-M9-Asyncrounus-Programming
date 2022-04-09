We learned how to use the `.then` method to fulfill a future, however, this is not the recommended way of dealing with futures.

We will use the `async` and `await` keywords this time.

Let's refactor our code a bit:

```dart
void main () {
    print("Program has started");
    final order = getUserOrder();
    print(order);
}
Future<String> getUserOrder () {
    return Future.delayed(Duration(seconds: 3),() => "Burger");
}
```

We will run into the same issue and the console will print an instance of future, but we can use the `await` keyword:

```dart
void main () {
    print("Program has started");
    final order = await getUserOrder();
    print(order);
}
Future<String> getUserOrder () {
    return Future.delayed(Duration(seconds: 3),() => "Burger");
}
```

Like this, we are telling dart to a`WAIT` for the future to be finished but we ran into another error:

```
The await expression can only be used in an async function.
```

Those are twins, `async` and `await`, you can't use `await` unless you mark your function as `async`, so let's do that:

```dart
void main () async {
    print("Program has started");
    final order = await getUserOrder();
    print(order);
}
Future<String> getUserOrder () {
    return Future.delayed(Duration(seconds: 3),() => "Burger");
}
```

We agreed we need to handle both cases, when a future resolves into a value, and when it resolves into an error.

So we will produce an error using the `throw` keyword like this:

```dart
void main () async {
    print("Program has started");
    final order = await getUserOrder();
    print(order);
}
Future<String> getUserOrder () {
    return Future.delayed(Duration(seconds: 3),() => throw Exception('Sorry we are closed'));
}
```

Output:

```
Program has started
: Exception: Sorry we are closed
  Error: Exception: Sorry we are closed
```

This is called unhandled exception, and it will cause our code/app to crash, to resolve this, we will use another twin called `try` and `catch`:

```dart
void main () async {
    print("Program has started");
  try {
    final order = await getUserOrder();
    print(order);
  } catch (e) {
    print(e);
  }
}
Future<String> getUserOrder () {
    return Future.delayed(Duration(seconds: 3),() => throw Exception('Sorry we are closed'));
}
```

By wrapping it with a `try` `catch` block, we are telling dart to `try` to do the future, and if it fails to do so for any reason, we will `catch` the error and do whatever we want with it, in this case we are printing it in the console and it won't crash our code/app because the execution will stop for the whole `try` block, for example:

```dart
void main () async {
    print("Program has started");
  try {
    final order = await getUserOrder();
    print(order);
    print("Your total is 5$");
  } catch (e) {
    print(e);
  }
}
Future<String> getUserOrder () {
    return Future.delayed(Duration(seconds: 3),() => throw Exception('Sorry we are closed'));
}
```

Output:

```
Program has started
Exception: Sorry we are closed
```

As you see, dart didn't execute this line:

```dart
    print("Your total is 5$");
```

So if you have any logic that depends on the result of the future and it fails, you are safe, nothing will execute.

So the main takeaways here:

1. Use `await` to wait until a future is completed.
2. You can only use `await` inside a function that is marked as `async`.
3. Use `try` and `catch` to handle exceptions and errors.
