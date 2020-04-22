## 手写一切（1）

### 手写new
```js
function _new(fn, ...args) {
    const obj = Object.create(fn.prototype); // 以fn.prototype为原型创建空对象
    // 等价于以下写法
    // const obj = {};
    // obj.__proto__ = fn.prototype;
    
    const res = fn.apply(obj, args);
    return res instanceof Object ? res : obj;
}
```

### 手写继承
```js
// 父类
function Animal(name) {
    // 实例属性
    this.name = name;
    this.species = 'animal';
    // 实例方法
    this.sleep = function() {
        console.log(`${this.name} is sleeping!`);
    }
}
// 原型方法
Animal.prototype.eat = function(food) {
    console.log(`${this.name} eat ${food}`);
}

// 1. 构造继承
function Cat(name) {
    Animal.apply(this, arguments);
    this.name = name;
}

// 2. 原型链继承，父类的实例作为子类的原型
function Dog() {
    
}
Dog.prototype = new Animal('dog');
Dog.prototype.constructor = Dog;    // 修改原型的constructor方法

// 3. 利用空对象做中介实现继承，相较于第二种，节省了创建Animal的消耗
function Pig() {
    
}
function Mid() {}
Mid.prototype = Animal.prototype;
Pig.prototype = new Mid();
Pig.prototype.constructor = Pig;
```

### 手写apply/call
```js
Function.prototype._apply = function(scope) {
    scope = scope || window;
    scope.fn = this;
    const res = scope.fn(...[...arguments].slice(1));
    delete(scope.fn);
    return res;
}

```

### 手写bind
```js
Function.prototype._bind = function(scope) {
    const _this = this;
    const args = [...arguments].slice(1)
    const fn = function(){
        if (this instanceof fn) {
            return new _this(...args, ...arguments)
        }
        return _this.apply(scope, args.concat(...arguments))
    }
    
    return fn
}
```

### 手写中间件
```js
const mids = [];
const app = {
    use: function(fn) {
        mids.push(fn);
        return this;
    },
    compose: (midwares) => (ctx) => {
        function dispatch(idx) {
            const fn = midwares[idx];
            if (fn) {
                return fn(ctx, () => dispatch(idx + 1));
            }
            return;
        }
        return dispatch(0);
    },
    run: function() {
        const ctx = {name: 'test'};
        this.compose(mids)(ctx);
    }
}

app.use(async (ctx, next) => {
    console.log('mid-1 start');
    await next();
    console.log('mid-1 end');
});

app.use(async (ctx, next) => {
    console.log('mid-2 start');
    await next();
    console.log('mid-2 end');
});

app.run();

```

### 手写节流
```js
function throttle(func, idle) {
    let prev = Date.now();
    return function() {
        if (Date.now() - prev < idle) {
            return;
        }
        func(...arguments);
        prev = Date.now();
    }
}

```

### 手写防抖
```js
function debounce(func, idle) {
    let handler;
    return function() {
        if (handler) {
            clearTimeout(handler);
        }
        handler = setTimeout(() => {
            func(...arguments);
        }, idle);
    }
}
```