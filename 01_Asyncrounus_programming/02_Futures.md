`Future` represents the result of an asynchronous operation.

`Futures` can have two states, complete and incomplete.

It can resolve into two different values, a potential value (success), and we get the value we need, or an error (fail) if anything went wrong. We should always handle those two cases.

Let's start by creating a code for a restaurant where a user can place an order, and when it's ready, we can tell the user that the order is complete.

We'll start by creating a function that returns a `Future`.

```dart
void main () {
}
Future getUserOrder(){}
```

To simulate a delay of 3 seconds, for example, we can use `Future.delayed` like this:

```dart
void main () {
}
Future getUserOrder(){
    Future.delayed(Duration(seconds: 3));
}
```

We passed a `Duration` object to the `Future.delayed` method and specified a 3-second delay.

As a second argument for `Future.delayed`, we can use an anonymous function that runs after the delay.

```dart
void main () {
}
Future getUserOrder(){
    Future.delayed(Duration(seconds: 3),()=> "Burger");
}
```

Because we are returning a `String`, we must specify the return type to be a `Future` `String`:

```dart
void main () {

}
Future<String> getUserOrder () {
    return Future.delayed(Duration(seconds: 3),() => "Burger");
}
```

Let's call this function and see what happens:

```dart
void main () {
    print("Program has started");
    print(getUserOrder());
}
Future<String> getUserOrder () {
    return Future.delayed(Duration(seconds: 3),() => "Burger");
}
```

```
Program has started
Instance of '_Future<String>'
```

Hmm, `Burger` is not printed.. that's because our code runs synchronously. By the time that we called our function, there was a delay of 3 seconds. Dart won't wait for that, it will execute the code immediately and as a result, it gets an instance of a `Future`. It's like Dart is telling you that something will happen in the future, and I don't know it now, what I can tell you is that it's a future.

To resolve this issue, we can use `.then` method that takes an anonymous function as shown below:

```dart
void main () {
    print("Program has started");
    getUserOrder().then((order)=>print(order));
}
Future<String> getUserOrder () {
    return Future.delayed(Duration(seconds: 3),() => "Burger");
}
```

Output:

```dart
Program has started
// after 3 seconds
Burger
```

We're telling Dart: don't execute this code now, wait until it's finished then get me the result. That's why you see a delay of 3 seconds until you get `Burger` in the console.
