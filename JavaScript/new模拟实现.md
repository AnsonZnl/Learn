

# 模拟实现new
new的四个步骤
1. 新建一个新对象
2. 把新对象的原型对象指向构造函数的prototype（改变constructor）
3. 使用apply执行构造函数，使this指向新对象
4. 返回这个对象

```
function create(Con, ...values){
    //判断是不是函数 如果不是直接throw TypeError
    if(typeof Con !== 'function'){
        throw TypeError('type error'); 
    }
    //创建一个新的对象
    let obj = {};
    //把新对象的原型指向构造函数的prototype
    obj.__proto__ = Con.prototype;
    //使用apply调用得到构造函数返回值
    let res = Con.apply(obj, values);
    //如果构造函数设置的返回值是对象的话，优先返回构造函数返回的对象，如果没有或者返回的不是对象类型的，就返回obj
    return res instanceof Object ? res : obj;
}
//test code 无返回值
function person(name,age){
     this.name = name;
     this.age = age;
}
let o = create(person, 'znl', 18)
console.log(o.constructor == person)//true
//test code 有返回值
function person(name,age){
     this.name = name;
     this.age = age;
     return {}
}
let o = create(person, 'znl', 18)
console.log(o instanceof person)//false
```

参考：
- [冴羽的博客](https://github.com/AnsonZnl/LY-sBlog)
- [深度解析new 原理及模拟实现](https://muyiy.cn/blog/3/3.5.html#%E5%AE%9A%E4%B9%89)
- [头条面试官问我的几种源码实现，还好我都会](https://mp.weixin.qq.com/s?__biz=MzUyNDYxNDAyMg==&mid=2247484853&idx=1&sn=f06695a7cf39485fbfd95fefbfde1d34&chksm=fa2be55ccd5c6c4a1ebce86926832dae5f75e08ed2380ade9283b384bf7503cabebf28245ff9&mpshare=1&scene=1&srcid=&sharer_sharetime=1574169496046&sharer_shareid=cfcd208495d565ef66e7dff9f98764da&key=5387d86ee537ed0dfac37e6ec7a9c764bdfbaa9cc55cca6e25157d572b5442cdd4435894d776a84ac513c6e77a9ff501012c34d4501ec6a4ed2670855dfa482d01d40f4b26de62296d424540d0a25088&ascene=1&uin=MjY5MTk2ODkxOQ%3D%3D&devicetype=Windows+7&version=62070155&lang=zh_CN&pass_ticket=P7ZbLc%2FhzAknmsen4DS%2BZNGhFvZtTtUJd3s%2FwYj3Q4CM2MN9%2FWnkLLfWJl5bHo2e)