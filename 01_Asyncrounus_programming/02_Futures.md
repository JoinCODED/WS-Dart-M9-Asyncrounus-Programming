A `Future` represents the result of an asynchronous operation.

Futures can have two states, completed and uncompleted.

And can resolve into 2 different values, it's either everything went fine and we got the value we need, or if anything wrong happens, it will resolve into an error. And we should always handle those two cases.

Let's start by creating a code for a restaurant, where a user can place an order and when it's ready we can tell the user the order is completed.

We'll start by creating a function that returns a `Future`.

```dart
void main () {
}
Future getUserOrder(){}
```

To simulate a delay of 3 seconds for example, we can use `Future.delayed` like this:

```dart
void main () {
}
Future getUserOrder(){
    Future.delayed(Duration(seconds: 3));
}
```

We passed a `Duration` object to the `Future.delayed` method and we specified a 3 seconds delay.

And as a second argument for `Future.delayed` we can use an anonymous function that runs after the delay.

```dart
void main () {
}
Future getUserOrder(){
    Future.delayed(Duration(seconds: 3),()=> "Burger");
}
```

And because we are returning a `String` we must specify the return type to be a `Future` `String`:

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

Hmm, `Burger` is not printed.. that's because our code runs synchronously and by the time that we called our function, there is a delay of 3 seconds, and dart won't wait for that, it will execute immediately and as a result, it gets an instance of a `Future`, it's like dart is telling you that its something that will happen in the future, And I don't know it now, what I can tell you is that it's a future.

To resolve this issue we can use `.then` method that takes an anonymous function like this:

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

It's like telling dart don't execute this know, wait until it's finished then get me the result, that's why you see a delay of 3 seconds until you get `Burger` in the console.
