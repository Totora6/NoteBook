## 1. JS基础部分

### 1.1 从原型到原型链
```js
function Person() {

}
var person = new Person();
person.name = 'Totora6';
console.log(person.name) //Totora6
```
其中`Person`是一个构造函数，使用`new`创建了一个实例对象`person`。
#### prototype
```js
function Person() {

}
//每个函数都有一个prototype属性，并且该属性是函数才有的属性。
Person.prototype.name = 'Totora6';
var person1 = new Person();
var person2 = new Person();
console.log(person1.name); //Totora6
console.log(person2.name); //Totora6
```
函数的prototype指向了一个`对象`(调用该构造函数而创建的实例的`原型`【每一个JavaScript对象在创建的时候就会与之关联另一个对象，这个对象就是原型，每个对象都会从原型“继承”属性】)。
**继承**：继承意味着复制操作，但是在JavaScript中默认并不会复制对象的属性，而是在两个对象之间创建一个关联，这样一个对象就可以通过委托访问另一个对象的属性和函数，所以相对于说是继承，委托的方法应该更加恰当。
![image-20211024161435490](JS笔记.assets/image-20211024161435490.png)

#### __proto__
`__proto__`是每一个JavScript对象都具有的一个属性，该属性会指向该对象的原型。
```js
function Person() {

}
var person = new Person();
console.log(person.__proto__ === Person.prototype);
```
![image-20211024162719525](JS笔记.assets/image-20211024162719525.png)

#### constructor

该属性是由原型指向其关联的构造函数。
```js
function Person() {

}
console.log(Person === Person.prototype.constructor); //true
```

![image-20211024162656756](JS笔记.assets/image-20211024162656756.png)

```js
function Person() {

}
var person = new Person();
console.log(person.__proto__ == Person.prototype); //true
console.log(Person.prototype.constructor == Person); //true
//ES5的方法获得一个对象的原型
console,log(Object.getPrototypeOf(person) === Person.prototype); //true
```
#### 实例与原型
```js
function Person() {

}
Person.prototype.name = 'Totora6';
var person = new Person();
person.name = 'Baymin';
console.log(person,name); //Baymin

delete person.name;
console.log(person.name);//Totora6
```
原型的原型
```js
var obj = new Object();
obj.name = 'Totora6';
console.log(obj.name); //Totora6
console.log(Object.prototype.__proto__ === null); //true
```

![image-20211024170131488](JS笔记.assets/image-20211024170131488.png)
```js
function Person() {

}
var person = new Person();
console.log(person.constructor === Person); //true
//由于person中并没有constructor属性，所以会从person的原型中读取，而原型中存在该属性，所以返回true。
person,constructor === Person.prototype.constructor;
```
`__proto__`绝大部分浏览器会支持该非标准方法访问原型，但是它实际上并不存在于`Person.prototype`中，它实际上来自于`Object.prototype`，它更像是一个getter/setter，当使用`obj.__proto__`时，可以理解成返回了`Object.getPrototypeOf(obj)`。