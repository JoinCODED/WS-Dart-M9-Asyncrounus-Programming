We learned how to use the `.then` method to fulfill a future. However, this is not the recommended way to deal with futures.

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

We will run into the same issue and the console will print an instance of future, but this time, we can use the `await` keyword:

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

We are telling Dart to `WAIT` for the future to be finished, but we ran into another error:

```
The await expression can only be used in an async function.
```

These are twins, `async` and `await`. You can't use `await` unless you mark your function as `async`. Let's do that:

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

We will produce an error using the `throw` keyword as follows:

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

This is called unhandled exception. It will cause our code/app to crash. To resolve this, we will use another twin called `try` and `catch`:

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

By wrapping our code with a `try` `catch` block, we are telling Dart to `try` to do the future, and if it fails, `catch` the error and do whatever we want to do with it. In this case, we are printing it in the console and it won't crash our code/app because the execution will stop for the whole `try` block, for example:

```dart
void main () async {
    print("Program has started");
  try {
    final order = await getUserOrder();
    print(order);
    print("Your total is 5\$");
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

As you can see, Dart didn't execute the following line:

```dart
    print("Your total is 5$");
```

If you have any logic that depends on the future result and it fails, you will be safe, nothing will be executed.

The main takeaways here:

1. Use `await` to wait until a future is complete.
2. You can only use `await` inside a function that is marked as `async`.
3. Use `try` and `catch` to handle exceptions and errors.
