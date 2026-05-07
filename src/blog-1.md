# The Four Pillars of OOP in TypeScript

## Introduction

বড় TypeScript project-এ কাজ করতে গেলে একটা সময় পর complexity এত বেড়ে যায় যে সামলানো কঠিন হয়ে পড়ে। যত বেশি feature আসে, তত বেশি code বাড়ে, তত বেশি জট পাকায়। এই জায়গায় OOP-এর চারটি pillar — Inheritance, Polymorphism, Abstraction, এবং Encapsulation — মিলে একটা solid foundation তৈরি করে।

---

## ১. Inheritance

বড় project-এ অনেক similar class থাকে। প্রতিটার জন্য আলাদা করে একই code লেখা মানে duplication। Inheritance ব্যবহার করলে common logic একটি parent class-এ লেখা যায়, বাকি class গুলো সেটা inherit করে নেয়।

````ts
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  move() {
    console.log(`${this.name} is moving`);
  }
}

class Dog extends Animal {
  bark() {
    console.log("Woof!");
  }
}

const dog = new Dog("Rex");
dog.move(); // parent থেকে নিয়েছে
dog.bark();
````

একই code বারবার লিখতে হচ্ছে না। Parent class-এ change করলে সব জায়গায় reflect হয়।

---

## ২. Polymorphism

একই method আলাদা আলাদা class-এ আলাদাভাবে কাজ করে। ফলে লম্বা if-else chain লিখতে হয় না।

````ts
class Shape {
  area(): number {
    return 0;
  }
}

class Circle extends Shape {
  radius: number;
  constructor(radius: number) {
    super();
    this.radius = radius;
  }
  area(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  width: number;
  height: number;
  constructor(width: number, height: number) {
    super();
    this.width = width;
    this.height = height;
  }
  area(): number {
    return this.width * this.height;
  }
}
````

প্রতিটি class নিজের মতো করে `area()` implement করছে। আলাদা condition লেখার দরকার নেই।

---

## ৩. Abstraction

Hard logic লুকিয়ে রাখে। Developer শুধু জানে কী করতে হবে, ভেতরে কীভাবে হচ্ছে সেটা নিয়ে ভাবতে হয় না।

````ts
abstract class Payment {
  abstract processPayment(amount: number): void;

  printReceipt() {
    console.log("Payment successful!");
  }
}

class BkashPayment extends Payment {
  processPayment(amount: number) {
    console.log(`Bkash-এ ${amount} টাকা পাঠানো হয়েছে`);
  }
}
````

`processPayment` ভেতরে কীভাবে কাজ করছে সেটা না জেনেও ব্যবহার করা যাচ্ছে।

---

## ৪. Encapsulation

Data safe রাখে। বাইরে থেকে সরাসরি data change করার সুযোগ থাকে না, যার কারণে unexpected bug অনেক কমে যায়।

````ts
class BankAccount {
  private balance: number = 0;

  deposit(amount: number) {
    this.balance += amount;
  }

  getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount();
account.deposit(500);
console.log(account.getBalance()); // 500
// account.balance = 99999;  সরাসরি access করা যাবে না
````

`balance` private থাকায় বাইরে থেকে সরাসরি কেউ বদলাতে পারছে না।

---

## Conclusion

বড় TypeScript project-এ এই চারটি pillar একসাথে কাজ করে complexity অনেকটা কমিয়ে দেয়। Inheritance duplication কমায়, Polymorphism condition কমায়, Abstraction জটিলতা লুকিয়ে রাখে, আর Encapsulation data safe রাখে। আলাদাভাবে প্রতিটাই কাজের, কিন্তু চারটা একসাথে ব্যবহার করলে codebase অনেক বেশি গোছানো এবং maintainable থাকে।

````
````

