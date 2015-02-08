# Design Patterns -- 1 (Strategy Patterns)

###### Reference：Head First Design Patterns

### Design Prinsples:
* Identify the aspects of your application that **vary** and **seperate them from what stays the same**.
* **Program to an interface(supertype)**, not to an implementation.
* Favor **composition** oner inheritance.

### Inheritance

```java
public abstract class Duck() {
	public void fly() { 
		// fly behavior
	}
}

public class RedHeadDuck() extends Duck {
	// nothing
}

public class MallardDuck() extends Duck {
	// nothing
}
```

**Advantage:**
* It is easy to add a new method (ex: `quack()`)

```java
public abstract class Duck() {
	public void fly();
	public void quack();
}
```

**Disadvantage:**
* If there is 80 kinds of duck cannot fly, we have to **check every kinds of duck, and decide wether to `override` the `fly()` method or not.**

* For example, if we want to add a new kind of duck which is not able to fly, ex: RubberDuck. We have to add a new class and `@Override` the `fly()` method.

```java
public class RubberDuck() extends Duck {
	@Override
	public fly() {
		// do nothing
	}
}
```

### Interface

```java
public interface Flyable {
	public void fly();
}

public interface Quackable {
	public void quack();
}

public class RedHeadDuck implements Flyable, Quackable {
	public void fly() {
		// fly behavior
	}
	public void quack() {
		// quack behavior
	}
} 

public class RubberDuck implementation Quackable {
	public void quack() {
		// quack behavior
	}
}
```

**Advantage:**
* Seperate each kind of duck.

**Disadvantage:**
* **Destroy the code reuse**, each `fly()` might be the same.

### Strategy Pattern

```java
public class FlyWithWings implements Flyable {
	public void fly() {
		// fly with wings behavior
	}
}

public class NoFly implements Flyable {
	public void fly() {
		// nothing
	}
}

public class Quack implements Quackable {
	public void quack() {
		// quack
	}
}

public class MuteQuack implements Quackable {
	public void quack() {
		// mute, nothing
	}
}

public abstract class Duck {
	FlyBehavior flyBehavior;
	QuackBehavior quackBehavior;

	public void performFly() {
		flyBehavior.fly();
	}

	public void performQuack() {
		quackBehavior.quack();
	}
}

public class RedHeadDuck extends Duck {
	public RedHeadDuck() {
		this.flyBehavior = new FlyWithWings();
		this.quackBehavior = new Quack();
	}
}

public class RubberDuck extends Duck {
	public RubberDuck() {
		this.flyBehavior = new NoFly();
		this.quackBehavior = new MuteQuack();
	}
}

/////////////////
// Client side //
/////////////////

public void main() {
	Duck hduck = new RedHeadDuck();
	hduck.performFly();
	hduck.performQuack();
	Duck rduck = new RubberDuck();
	rduck.performFly();
	rduck.performQuack();
}

```
**Feature:**
* **FlyBehavior** and **QuackBehavior**

**Advantage:**
* Seperate client side's code and the model.

> We don't have to change codes in client side anymore.

**Disadvantage:**
* Every new behavior means a new class.

## Summary

###Why should I know Strategy Pattern?###
Short answer: Strategy Patterns makes the code **easier to be reused and maintained**.

By isolating algorithms from client-side implementation, different client applications can **share the same implementation of an algorithm**.

At the same time, by isolating algorithms from each other, maintenance of the code can be done with **less impact to other components**. From development point of view, the maintenance can be done independently - by different people at different time (or even better, at the same time!)

###When should I use Strategy Pattern, and when not to?###
Theoretically, this pattern should be applied all the time.

Yet, when time and resource are limited, you might want to think twice before doing the right thing. Poor design does not necessarily comes from poor decisions.


