```java
// Duck.java
abstract class Duck {

	FlyBehavior flybehavior;
	QuackBehavior quackBehavior;
	
	void performQuack() {
		quackBehavior.quack();
	}
}
```

This minimizes the changes needed to create a new duck class, as you would only need to add a `FlyBehavior` and a `QuackBehavior`.

This also allows us to change the strategy used at run time.
****
## Strategy Pattern
Defines a family of algorithms, encapsulates each one, and makes them interchangeable.
This lets the algorithm vary independently from clients that use it.
****
**Tip**
- When using that, use a name that tells so.
	i.e. instead of naming the base class `FlyBehavior` call it `FlyStrategy` so you can communicate that you are using this pattern to other developers.
- Notice that we can simply inject behaviors as configurations and not need to create any concrete classes for any ducks.
	 i.e. `Duck mallardDuck = new Duck(mallardFlyBehavior, mallardQuackBehavior);`
****

