---
title: pug使用
tag: pug nodejs
notebook: wiki
---
## pug中常用的一些代码

- [pug中常用的一些代码](#pug中常用的一些代码)
  - [router 中传递JSONarray数据](#router-中传递jsonarray数据)
  - [pug中接收数据](#pug中接收数据)
  - [express router代码](#express-router代码)
    - [index.js](#indexjs)
    - [home.js](#homejs)
    - [index.pug](#indexpug)

### router 中传递JSONarray数据

```pug
res.render('home/index', { 
 items: [ 
  { id:'1', name:'hp', quantity:'3' }, 
  { id:'2', name:'dell', quantity:'2' }, 
  { id:'3', name:'sony', quantity:'5' } 
  ] 
 });
```

### pug中接收数据

```pug
 each item in items 
 tr 
  td #{item.id} 
  td #{item.name} 
  td #{item.quantity}
```

### express router代码

```js
exports.test = function (req,res){  
 let array = [];  
 for (let item in jsondata) {  
 // console.log("item", jsondata\[item\]\['ExtParam'\])  
 // console.log("item", jsondata\[item\]\['ExtParam'\]) console.log(item+":"+jsondata\[item\]);  
 let result={  
   ExtParam: jsondata\[item\]\['ExtParam'\],  
   PageIdParam: jsondata\[item\]\['PageIdParam'\],  
   codeParam: jsondata\[item\]\['codeParam'\]  
  } 
  array.push(result);  
 // res.render('index',{ExtParamdata:result.ExtParam,PageId:result.PageIdParam,code:result.codeParm});  
 }  
 console.log("====>",array)  
 res.setHeader('Pragma', 'no-cache');  
 res.setHeader('Expires', '0');  
 res.setHeader('Cache-Control', 'no-cache, no-store, must-revalidate'); 
 res.render('index',{result:array});  
  
}
```

#### index.js

```js
var express = require('express'), 
 path = require('path'), 
 home = require('./routes/home.js'); 
var app = express(); 
app.set('views', \_\_dirname + '/views'); 
app.set('view engine', 'jade'); 
app.use('/home', home); 

app.listen(3000); console.log('Access through http://localhost:3000/');
```

#### home.js

```js
var express = require('express'); 
var router = express.Router(); 
router.get('/home', function(req, res) { 
 res.render('home/index', 
 { id:'1',name:'hp', quantity:'3'},
 { id:'2',name:'dell', quantity:'2'},
 { id:'3',name:'sony', quantity:'5'}); }); 
 
module.exports = router;
```

#### index.pug

```pug
doctype
 html 
   head 
    title =title 
   body 
      h3 Comments 
      table 
      tr 
       th Id 
    th Name 
    th Quantity 
   each item in item 
    tr 
     td #{item.id} 
     td #{item.name} 
     td #{item.quantity}

```
