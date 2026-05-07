# Why is `any` a "Type Safety Hole" and Why is `unknown` the Safer Choice?

## Introduction

TypeScript ব্যবহারের সবচেয়ে বড় সুবিধা হলো type safety — কোড run হওয়ার আগেই ভুল ধরা যায়। কিন্তু `any` ব্যবহার করলে এই সুবিধাটাই নষ্ট হয়ে যায়। অন্যদিকে `unknown` unpredictable data handle করে কিন্তু safety ধরে রাখে। এই blog-এ আমরা দেখব কেন `any` বিপজ্জনক, কেন `unknown` বেশি নিরাপদ, এবং type narrowing কীভাবে কাজ করে।

---

## `any` কেন "Type Safety Hole"?

যখন কোনো variable define করার সময় type বলা হয় না, TypeScript সেটাকে automatically `any` ধরে নেয়।

````ts
let x; // TypeScript এটাকে any ধরেছে
x = "hello";
x = 42;
x = true;
````

`any` use করলে TypeScript তার সব checking বন্ধ করে দেয় — কোনো warning নেই, কোনো error নেই।

````ts
let value: any = "Hello";
value = true;
value.toUpperCase(); // TypeScript কিছু বলছে না, কিন্তু runtime-এ crash!
````

`true.toUpperCase()` বাস্তবে কাজ করে না — কিন্তু TypeScript চুপ করে থাকে। এই কারণেই `any`-কে বলা হয় **"Type Safety Hole"**।

---

## `unknown` কেন Safer Choice?

বাইরে থেকে যখন কোনো user input আসে, আমরা আগে থেকে জানি না সেটা কী type। `unknown` ব্যবহার করলে TypeScript runtime-এ যাওয়ার আগেই থামিয়ে দেয় এবং data verify করতে বলে।

````ts
const userInput: unknown = getUserInput();
userInput.toUpperCase(); // TypeScript এখানেই error দেবে
````

`any` হলে TypeScript চুপ থাকত। `unknown` হলে বাধ্য করে আগে check করতে। এটাই সবচেয়ে বড় পার্থক্য।

---

## Type Narrowing কী?

`unknown` বা union type নিয়ে কাজ করার সময় TypeScript নিজে থেকে বুঝতে পারে না data কী type। তাই একটা condition দিতে হয় — TypeScript সেই block-এর ভেতরে type "narrow" করে নেয়। এই process-টাকেই বলে **Type Narrowing।**

### ১. `typeof` দিয়ে Narrowing

````ts
function printData(data: unknown) {
  if (typeof data === "string") {
    console.log(data.toUpperCase()); // TypeScript জানে এটা string
  }

  if (typeof data === "number") {
    console.log(data.toFixed(2)); // TypeScript জানে এটা number
  }
}
````

`if` block-এর বাইরে `data` হলো `unknown`, ভেতরে গেলে TypeScript নিজেই type বুঝে নেয়।

### ২. `instanceof` দিয়ে Narrowing

````ts
function handleError(error: unknown) {
  if (error instanceof Error) {
    console.log(error.message);
  }
}
````

Object বা class-এর ক্ষেত্রে `typeof` কাজ করে না, তখন `instanceof` ব্যবহার করতে হয়।

---

## Conclusion

`any` দেখতে সহজ মনে হলেও এটা TypeScript-এর type safety নষ্ট করে দেয়। `unknown` ব্যবহার করলে data verify করতে বাধ্য হতে হয়, ফলে runtime error কমে এবং code অনেক বেশি reliable হয়। আর type narrowing হলো সেই উপায় যেটা দিয়ে `unknown` data-কে safely ব্যবহার করা যায়। Unpredictable data handle করার সময় সবসময় `any`-এর বদলে `unknown` ব্যবহার করাই সঠিক approach।

````
````