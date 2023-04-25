---
title: map or set
date: 2023-04-04 10:20:28
tags: map  set
---

## Map
Map本身是一个构造函数
1、Map是一组键值对的结构，具有极快的查找速度。

```
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);

m.get('Michael'); // 95

```

对于初始化 一个  new Map  空的  常用 api

```
var m = new Map(); // 空Map

m.set('Adam', 67); // 添加新的key-value

m.set('Bob', 59);

m.has('Adam'); // 是否存在key 'Adam': true

m.get('Adam'); // 67   Adam 对应value

m.delete('Adam'); // 删除key 'Adam'

m.get('Adam'); // undefined
```

** 由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉：**

```
var m = new Map();

m.set('Adam', 67);

m.set('Adam', 88);

m.get('Adam'); // 88
```

使用 for..of 方法迭代 Map

```
var myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");
for (var [key, value] of myMap) {
  console.log(key + " = " + value);
}
// 将会显示两个log。一个是"0 = zero"另一个是"1 = one"

for (var key of myMap.keys()) {
  console.log(key);
}
// 将会显示两个log。 一个是 "0" 另一个是 "1"

for (var value of myMap.values()) {
  console.log(value);
}
// 将会显示两个log。 一个是 "zero" 另一个是 "one"

for (var [key, value] of myMap.entries()) {
  console.log(key + " = " + value);
}
// 将会显示两个log。 一个是 "0 = zero" 另一个是 "1 = one"

```

使用  forEach  迭代 Map



json转map
```
 let json = {"name":"ES6","day":"2014","feature":"新特性"};
	//json 2 map
	let map = new Map();
	for(let i in json){
		map.set(i,json[i]);
	}
```

map转json
```
	//map 2 json
	let map = new Map();
	map.set("name","ES6");
	map.set("day","2014");
	map.set("feature","新特性");
	let json = {};
	for(let [k,v] of map){
		json[k]=v;
	}
	console.log(json);
```