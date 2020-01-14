---
title: express use()和中间件
date: 
        2019-08-22 18:13:29
tags: 前端
---
# express use()和中间件
`express.js`
```javascript
function express () {
	const app = () => {
	}
	app.static = () => {
		// ...
	}
	// 内部存在一个任务数组 每次调用use 则将use对于的中间件存入task中
	// 每次接受到请求，则依次从任务中走一遍，直至走完或者遇到res.send()、res.render()、res.end()等
	let task = []
	app.use = (p1, p2) => {
		// 如果p1是方法就执行
		// 如果p1是字符串等 并且 p1 匹配上p2的req 则执行p2
		// 如果p1是数字，则返回状态码
		task.push({
			path: // p1 是字符串就塞入path 如果是方法就是null，
			fn: // p1 或者 p2 ，即中间件方法
		})
	}
}
```
`p2`
```javascript
function p2 (req, res, next) {
	...
}
```

`next()`
```javascript
function next (req, res, next) {
	...
	task[i++]
}
```
