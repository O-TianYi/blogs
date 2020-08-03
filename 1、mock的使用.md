### mock是能做什么

+ 解决api没开发好，前端可以根据需求的API文档，独立于后端开发。





#### 使用

+ 安装

  ```
  npm i mockjs
  ```

+ 编写返回数据

  ```
  mock.js:
  import Mock from 'mockjs'
   Mock.mock('http://localhost:8080/user',{
  	'name':"@name",//返回随机姓名
  	'age|0-10':6
  })
  
  使用：
  import mock from './mock.js'
  import axios from 'axios'
  
  getUsers(){
  	axios.get("http://localhost:8080/user").then(res=>{
  		console.log(res)
  	})
  }
  
  
  ```

  返回的结果图示：

  ![image-20200803163303824](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200803163303824.png)





#### mock语法

语法规则

```]
// 属性名   name
// 生成规则 rule
// 属性值   value
'name|rule': value
```

例子：

```
"name1":"@name",//随机生成姓名
'name2|2-5':'a',//生成大于2小于5的a
'name3|3':'b',//生成3次b的字符串

'age1|+1':2,//第一次触发生成原数据2，第二次触发生成2+1=3，以此类推
'age2|1-10':5,//生成大于1小于10的数，5只是用来确定类型
'age3|4-5.1-2':1//生成一个整数位大于4小于5，小数部分保留1-2位
'age4|5': 1//生成5，后面的1只是代表类型

'boolean1|1': true,//true和false的概率都为1/2
'boolean2|1-2':true,//出现true的概率为1/(2+1)=1/3，false的概率为2/3

'object1|2':obj,//从obj中获取2个属性
'object2|1-10':obj,//从obj对象中随机选取1-10个属性

'array1|3':arr,//复制arr数组三次，返回一个新数组（数组长度为:30--3*arr.length）
'array2|+1':arr,//顺序选取一个元素
'array3|3-5':arr,//选取大于3小于5个arr组成的新数组（数组长度为：30-50）
'array4|1':arr,//从arr中随机选取一个值

'function': new Date(),//执行函数，值为函数返回的值

'regexp':/\d{5}/,//生成符合该正则的字符串

'random1':'@first',
'random2':'@first @last',


输出：
name1: "Brian Clark"
name2: "aaa"
name3: "bbb"

age1: 2  //刚开始是原数据，再次点击就会执行加1操作，即输出为; 2,3,4,5...
age2: 5
age3: 4.64
age4: 5

boolean1: true
boolean2: true

object1: {sex: "男", age: 10}
object2: {text2: "1", text3: "1", text4: "1", name: "mike", sex: "男", …}

array1: (30) [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
array2: 1
array3: (30) [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
array4: 4

function: "2020-08-03T09:14:58.279Z"

regexp: "66746"

random1: "Gary"
random2: "Gary Williams"
```





#### 其他语法

Mock.mock()[https://github.com/nuysoft/Mock/wiki/Mock.mock()]

```
Mock.mock( rurl?, rtype?, template|function( options ) )
+ rurl就是需要拦截的请求，可以为正则和字符串
+ rtype是需要拦截的ajax请求类型，例如get,post,put
+ template是数据模板，例如{ 'data|1-10':[{}] }、'@EMAIL'
+ fuctio是被拦截请求的时候触发的方法，生成响应数据的函数
```



Mock.setup(settings)配置拦截时的行为，支持的配置项有：timeout

```
Mock.setup({
    timeout: 400
})//被拦截的ajax请求的响应时间为400毫秒，即400毫秒后才返回数据
Mock.setup({
    timeout: '200-600'
})//200-600毫秒后才返回数据
```



Mock.Random是一个工具类，用于生成各种随机数据，即在数据模板中的@

```
var Random = Mock.Random
Random.email()
// => "n.clark@miller.io"
Mock.mock('@email')
// => "y.lee@lewis.org"
Mock.mock( { email: '@email' } )
// => { email: "v.lewis@hall.gov" }

自定义扩展方法：
Random.extend({
    constellation: function(date) {
        var constellations = ['白羊座', '金牛座', '双子座', '巨蟹座', '狮子座', '处女座', '天秤座', '天蝎座', '射手座', '摩羯座', '水瓶座', '双鱼座']
        return this.pick(constellations)
    }
})
Random.constellation()
// => "水瓶座"
Mock.mock('@CONSTELLATION')
// => "天蝎座"
Mock.mock({
    constellation: '@CONSTELLATION'
})
// => { constellation: "射手座" }
```

随机数据可以的值可以有，即@后面可以跟下面表显示的数据。

| Type          | Method                                                       |
| ------------- | ------------------------------------------------------------ |
| Basic         | boolean, natural, integer, float, character, string, range, date, time, datetime, now |
| Image         | image, dataImage                                             |
| Color         | color                                                        |
| Text          | paragraph, sentence, word, title, cparagraph, csentence, cword, ctitle |
| Name          | first, last, name, cfirst, clast, cname                      |
| Web           | url, domain, email, ip, tld                                  |
| Address       | area, region                                                 |
| Helper        | capitalize, upper, lower, pick, shuffle                      |
| Miscellaneous | guid, id                                                     |



Mock.valid(tempalte,data)检查tempalte和data是否匹配，即检查data是否与数据摸吧tempalte匹配

```
var template = {
    name: 'value1'
}
var data = {
    name: 'value2'
}
Mock.valid(template, data)
```

