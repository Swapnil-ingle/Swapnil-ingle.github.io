## SOLID programming principles

The SOLID principles is a framework one should follow in order to write a readable, maintainable and extendable code.

### SOLID is an acronym for:

* S --> Single Responsibility Principle
* O --> Open/Closed Principle
* L --> Liskov Substitution Principle
* I --> Interface Segregation Principle
* D --> Dependency Inversion Principle (DIP)

### Single Responsibility Principle

"Single Responsibility Principle" states that one block of function (either method or a class) should be responsible for doing one thing and one thing only. If you find a reason for changing a class, extract that functionality out of the class and tear a new one.

Classes/Methods should be small, precise, appropriately-named and should not do more than one thing.

### Open/Closed Principle

Classes in your program should be open to extension but closed for modification.

The idea is that your core program/project should be unfailable.

Suppose, you have payment system that makes three different type of payments (UPI, NetBanking, Paypal). You've a function pay(Request req) which gets the type of payment and calls method of class Payment which have different types of payment logic.

```java
public void pay(Request request) {
  Payment payment = new Payment();
  
  if (req.getType() == "UPI") {
    payment.payUsingUPI(request);
  } else if (req.getType() == "NetBanking") {
    payment.payUsingNetBanking(request);
  } else if (req.getType() == "Paypal") {
    payment.payUsingPaypal(request);
  }
}
  
public class Payment {
  public void payUsingUPI(Request request)  {
    // TODO: pay using UPI
  }
  
  public void payUsingNetBanking(Request request)  {
    // TODO: pay using NetBanking
  }
  
  public void payUsingPayPal(Request request)  {
    // TODO: pay using PayPal
  }
}
```

Now, suppose if you want to add one more payment method "CashOnDelivery". With the above code, you'll need to do the following:
1. Add ```else if (...)``` statement in the pay() method
2. Add a new method ```payUsingCashOnDelivery(Request request) { ... }``` in Payment class.

However, this violates the Open/Closed principle. It make look like the above classes are not dependent on each other, but adding method to one class will lead you to editing the method in other class and overtime this will make your code much more tangled.

Therefore, here we can use [factoryDesignPattern](https://www.tutorialspoint.com/design_pattern/factory_pattern.htm) to ensure the Open/Closed principle is not violated.

```java
public void pay(Request request) {
  new PaymentFactory().initPayment(request.getType()).pay();
}

public interface Payable {
  public void pay();
}

public class UpiPayment implements Payable {
  @Override
  public void pay() {
    // TODO: Pay using UPI
  }
}

public class NetBankingPayment implements Payable {
  @Override
  public void pay() {
    // TODO: Pay using NetBanking
  }
}

public class PayPalPayment implements Payable {
  @Override
  public void pay() {
    // TODO: Pay using PayPal
  }
}

public class PaymentFactory {
  public Payable initPayment(PaymentType type) {
    if (req.getType() == "UPI") {
      return new UpiPayment();
    } else if (req.getType() == "NetBanking") {
      return new NetBankingPayment();
    } else if (req.getType() == "Paypal") {
      return new PayPalPayment();
    } else {
      throw new RuntimeException("Unsupported payment method");
    }
  }
}

```

Your goal should be "Adding Code" instead of "Chanding Code".

### Liskov Substitution Principle

The Liskov Substitution Principle states that:
> "Let $(x) be a property provalbe about objects x of type T. Then $(y) should be true for all objects y of type S where S is a subtype of T."

I didn't get that either. In a nutshell, what it says is _"Our program should run as it is, even if all the implementations of all defined interfaces are implemented by a different concrete class"_.

### Interface Segregation Principle

No client should be forced to depend on methods it doesn't use.

We can understand this with an example, suppose we have a class ```Notification``` which has ```public void sendNotification(Subscriber sub) { ... }``` method that gets the emailId from the subsriber and sends the Notification.

```java
public class Notification {
  public void notify(Subscriber sub) {
    Mail.to(sub.getNotifyEmail(), "Message");
  }
}
// This notify() method now depends upon the whole Subscriber object, and it didn't need to. It only needs getNotifyEmail() method.
// Note that, now notify method cannot be used by any other object which also has an emailID and to whom we can send notification,
// but who is NOT a subscriber.
```

To solve this, we can make an interface ```Notifiable``` and have the Subscriber class implement this. This is how the code now changes.

```java
public interface Notifiable {
  public String getNotifyEmail();
}

public class Subscriber implements Notifiable {
  @Override
  public String getNotifyEmail() {
    // TODO: Send notify email
  }
}

public class Notification {
  public void notify(Notifiable subscriber) {
    Mail.to(subscriber.getNotifyEmail(), "Message");
  }
}
```

Rather than making huge objects we make small/precise interfaces and implement those.

Note: Strike a balance between too monolithic objects v/s too segregated objects, both extremes are not beneficial.

### Dependency Inversion Principle (DIP)

1. High-Level module should not depend upon low-level modules. Both should depend upon abstractions. 
2. Abstractions should not depend on details, details should depend on abstractions.

During application developement, a very common practice is to build lower level module which are consumed by higher level modules. This creates a dependency of higher level modules on lower level components. This limits the reusability of higher-level components.

There are many vague terms used in the definition in the principle, this [blog](https://medium.com/@kedren.villena/simplifying-dependency-inversion-principle-dip-59228122649a) from Medium does a very good job of explaining this in details with good examples.

### Don't get trapped by SOLID

* SOLID design principle are principles, not rules.

* Use common sense while applying SOLID.

* Don't over fragment your code for the sake of SPR or SOLID.

* SOLID is tool, not a goal.

> Don't try to acheive SOLID, use SOLID to acheive maintainability.

### References:
* [Becoming a better developer by Katerina Trajchevska](https://www.youtube.com/watch?v=rtmFCcjEgEw)
* [Simplifying Dependency Inversion](https://medium.com/@kedren.villena/simplifying-dependency-inversion-principle-dip-59228122649a)
* [SOLID Principles - YouTube](https://www.youtube.com/watch?v=5WHKNOTqwsA)
* [SOLID Principles - WikiPedia](https://en.wikipedia.org/wiki/SOLID)
