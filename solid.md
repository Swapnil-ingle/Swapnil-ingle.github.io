## SOLID programming principles

The SOLID principles is a framework one should follow in order to write a readable, maintainable and extendable code.

### SOLID is an acronym for:

#### S --> Single Responsibility Principle
#### O --> Open/Closed Principle
#### L --> Liskov Substitution Principle
#### I --> 
#### D --> 

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
1. Add ```else if ``` statement in the pay() method
2. Add a new method ```payUsingCashOnDelivery(Request request)``` in Payment class.

However, this violates the Open/Closed principle. It make look like the above classes are not dependent on each other, but adding method to one class will lead you to editing the method in other class and overtime this will make your code much more tangled.

Therefore, here we can use [factoryDesignPattern](https://www.tutorialspoint.com/design_pattern/factory_pattern.htm) to ensure the Open/Closed principle.

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

### References:
* [Becoming a better developer by Katerina Trajchevska](https://www.youtube.com/watch?v=rtmFCcjEgEw)
