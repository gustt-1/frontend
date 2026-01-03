# $instanceof$ 运算符用于检测 构造函数的 $prototype$ 属性 是否出现在 某个实例对象的原型链 上。


##  构造 $Animal, Dog, Cat$, 实例化 $dahung, tom$
``` JavaScript
function Animal() {}
Animal.prototype.Say = function() {
  console.log("I am an animal");
};

function Dog() {}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

function Cat() {}
Cat.prototype = Object.create(Animal.prototype);
Cat.prototype.constructor = Cat;

const dahuang = new Dog();
const tom = new Cat();
```

---

## $手写 Instanceof$

### $1.迭代$
``` JavaScript
const myInstanceof = (instance, target) => {
  if (typeof instance !== 'object' || instance === null) {
    return false;
  }
  
  let proto = instance.__proto__;
  const targetPrototype = target.prototype;
  
  while (true) {
    if (!proto) {
      return false;
    }
    if (proto === targetPrototype) {
      return true;
    }
    proto = proto.__proto__;
  }
}


const myInstanceof = (instance, target) => {
  const isObject = typeof instance === 'object' && instance !== null;
  const isFunction = typeof instance === 'function';
  
  if (!isObject && !isFunction) return false;

  let proto = Object.getPrototypeOf(instance);
  const targetPrototype = target.prototype;

  while (proto !== null) {
    if (proto === targetPrototype) return true;
    proto = Object.getPrototypeOf(proto);
  }

  return false;
}


console.log(myInstanceof(dahuang, Animal));  // true
console.log(myInstanceof(dahuang, Cat));  // false
```

---

### $2.递归$
``` JavaScript
const myInstanceof = (instance, target) => {
  if (typeof instance !== 'object' || instance === null) return false;

  const proto = Object.getPrototypeOf(instance);
  const targetPrototype = target.prototype;

  if (proto === null) return false;
  if (proto === targetPrototype) return true;

  return myInstanceof(proto, target);
}
```

---

### $3.标准 API 写法 (isPrototypeOf)$
``` JavaScript
const myInstanceof = (instance, target) => {
  if (typeof instance !== 'object' || instance === null) return false;
  
  return target.prototype.isPrototypeOf(instance);
}
```