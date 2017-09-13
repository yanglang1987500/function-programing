# function-programing
```javascript
function compose(...funcs){
	if(funcs.length == 0)
		return ()=>{};
	if(funcs.length == 1)
		return funcs[0];
	/*return funcs.reduce((a,b)=>{
		return (...args)=>{
			return a(b(...args));
		};
	})*/
	const last = funcs[funcs.length-1];
	const rest = funcs.slice(0, -1);
	return (...args)=>rest.reduceRight((composed,next)=>next(composed), last(...args))
}

function greeting(s){
	return s = 'hello,'+s;
}
function upper(s){
	return s.toUpperCase();
}
console.log(compose(upper,greeting)('yanglang'))
```



reduce 从左到右 但是必须倒序执行。。。所以，
先reduce进行迭代，最后返回一个方法给外部调用作为入口
reduce迭代的话，获得参数的顺序肯定是从左往右的，于是a - b - c ... - e
`return (...args)=>a(b(...args))`
返回`a(b(c(d(e(arguments)))))`


reduceRight 从右到左 已经是倒序执行了，所以直接返回一个方法给外部调用
`return (...args)=>{ reduceRight()  }`
...args已经是第二次调用传入的参数了，也就是'yanglang'
所以必须先把最后一个last函数给执行了
`last(...args)`返回值作为 reduceRight的初始值放入，在回调里取倒数第二个函数传入并执行
