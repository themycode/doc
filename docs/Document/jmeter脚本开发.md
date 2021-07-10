---
title: jmeter脚本使用
tags: 测试
notebook: 压测
---
 
- [1. jmeter脚本编写介绍](#1-jmeter脚本编写介绍)
- [2. groovy编写线程共享变量](#2-groovy编写线程共享变量)
  - [2.0.1. Groovy处理JMeter中的请求参数](#201-groovy处理jmeter中的请求参数)
  - [2.1. JSR223 后置处理程序设置](#21-jsr223-后置处理程序设置)
    - [2.2. 读取文件参数，读取参数传參给接口](#22-读取文件参数读取参数传參给接口)
- [3. JSR223 后置处理程序，返回数据存入文件中](#3-jsr223-后置处理程序返回数据存入文件中)
- [4. JSR223 Sampler采样器使用](#4-jsr223-sampler采样器使用)
- [5. debug方式](#5-debug方式)
- [6. 参数化获取脚本](#6-参数化获取脚本)
- [常用脚本](#常用脚本)

### 1. jmeter脚本编写介绍

jmeter 脚本可以使用groovy语言开发。jmeter中已经带有groovy语言编译相关工具，在处理程序中需要选择预处理程序的语言 groovy 

### 2. groovy编写线程共享变量

* 在接口前置处理器中创建jsr223 预处理程序

`props.get（"MY",test)` 可以传一个函数给一个变量当参数  
`vars.put("MY1","FunTester")` 定义一个变量接受一个固定参数
`def my_var = vars.get("MY1")` 获取MY1 关键字的值

``` groovy
log.error '输出JMeter控制台错误'
vars.put("MY1","FunTester")
def my_var = vars.get("MY1");

props.put("MY",test())
log.info(props.get("MY"))

def test(){
	"funtest" + fan()
}

def fan(){
	System.currentTimeMillis()
}

```

* 接口共享变量方法

在接口中设置接受不了变量的参数用__P接收参数 ` /getconfig/sodar?sv=${__P(my)}&tid=gda&tv=${MY}`

##### 2.0.1. Groovy处理JMeter中的请求参数

在脚本中添加内容：

``` groovy
sampler.addArgument("name","data")
log.info(sampler.getArgument().toString())

//  sample example
sampler.addArgument("st","env")
log.info(sampler.getArguments().toString())
```

经过测试这个方法对于get和post请求均有效，包括post请求的不同参数类型

![img](https://i.imgur.com/CP6e69D.png)

查看日志中url打印的参数，最后的参数是我们脚本中的变量说明添加成功：

![note-sample](https://i.imgur.com/8jIc7IF.png)

#### 2.1. JSR223 后置处理程序设置

* 在后置处理器中添加JSR223后置处理程序，在脚本输入框添加脚本

``` groovy
import groovy.json.*

log.info("线程组名字 " + prev.getThreadName())
def end_time = prev.getEndTime()
log.info("结束时间 " + (new Date(end_time).toString()))
log.info("响应结果: " + prev.getTime().toString())
log.info("Connect Time is: " + prev.getConnectTime().toString())
log.info("Latency is: " + prev.getLatency().toString())
log.info("响应大小" + prev.getBytesAsLong().toString())
log.info("请求url " + prev.getURL())
log.info("测试结果是 " + prev.isSuccessful().toString())

def response = prev.getResponseDataAsString()
log.info("响应内容是:" + response)

def json = new JsonSlurper().parseText(response)
log.info("json:"+ json)
//log.info("响应code" + json.success)

assert 200 == json.success

log.info("响应头响应行是 " + prev.getResponseHeaders())
```

#### 2.2. 读取文件参数，读取参数传參给接口

在预处理JSR223中配置

``` groovy
import org.apache.jmeter.services.FileServer
OUT. println '获取文件参数'

def inputPath = FileServer.getFileServer().getBaseDir()+"/api_params.csv"
log.info("inputPath" + inputPath);
File inputFile = new File(inputPath)
if(inputFile.exists()){
	log.info("File"+ inputPath + " exists")
//	vars.put("assertionValue",inputFile)
	inputFile.eachLine{ line ->
		words = line.split(",")
		vars.put("data1",words[0])
		vars.put("data2",words[1])
		def data1 = vars.get("data1")
		log.info("*******" + data1)
		def data2 = vars.get("data2")
		log.info("=======" + data2)
		
	}
}

```

### 3. JSR223 后置处理程序，返回数据存入文件中

``` groovy
import org.apache.jmeter.services.FileServer

//通过使用定义的路径
def inputPath = FileServer.getFileServer().getBaseDir() + "/responseFile.txt"
log.info("inputPath"+ inputPath);
//Get the body from the response:
def body = prev.getResponseDataAsString();
//Create the response file:
def responseFile = new File(inputPath);
//Get the thread name:
def sample = prev.getThreadName();
//Write the thread name:
responseFile << "---------------------------------------------------------------------------------------- \n";
responseFile << "                              " + sample + "\n";
responseFile << "---------------------------------------------------------------------------------------- \n";
//Write the data into the file:
responseFile << body;

```

### 4. JSR223 Sampler采样器使用

``` groovy
//Get current thread
Thread thread = Thread.currentThread();
//Debug using log.info
log.info("Runnable Job is being run by " + thread.getName() + " (" + thread.getId() + ")");
//Get thread Id
Long threadNum = thread.getId();
//If thread Id is even, request Blazedemo page, if not, request JMeter page
if (threadNum.mod(2) == 0){
	vars.put("server","www.blazedemo.com");
} else {
	vars.put("server","jmeter.apache.org");
}
return "Next request will to server: " + vars.get("server");
```

### 5. debug方式

``` 
if (inputFile.exists()) {
...
}
else  log.error("Unable to open " + inputPath)
```


### 6. 参数化获取脚本

```groovy
import org.apache.jmeter.services.FileServer
import java.util.Random;
import java.util.Date; 
import java.text.SimpleDateFormat;

String str = "";
SimpleDateFormat sdf = new SimpleDateFormat("YYYYMMDDHHmmss");
Calendar lastDate = Calendar.getInstance();
str = sdf.format(lastDate.getTime());
Date date =sdf.parse(str);
str=date.getTime();  
log.info( "str======"+ str)
vars.put("datestr",str)
def datestr = vars.get("datestr")
log.info("*****" + datestr)


OUT. println '获取文件参数'

def inputPath = FileServer.getFileServer().getBaseDir()+"/api_params.csv"
log.info("inputPath" + inputPath);
File inputFile = new File(inputPath)
if(inputFile.exists()){
	log.info("File"+ inputPath + " exists")
//	vars.put("assertionValue",inputFile)
	inputFile.eachLine{ line ->
		words = line.split(",")
		vars.put("data1",words[0])
		vars.put("data2",words[1])
		def data1 = vars.get("data1")
		log.info("*******" + data1)
		def data2 = vars.get("data2")
		log.info("=======" + data2)
		
	}
}

for(int i=0;i<1000;i++){
    def a = Math.abs(new Random().nextInt() % 900000) +100000
//    println "for循环：${a}"

    vars.put("data3",Integer.toString(a))
    def data3 = vars.get("data3")
    log.info("for 循环"+ data3)
}
```

### 常用脚本
```Groovy
// 打印请求参数
log.info("试算请求参数****"+sampler.getArguments().toString())
// 随机生成身份证号码
def id = []
def rand = new Random()
18.times { id << rand.nextInt(10) }
def s = 0;
def w = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2, 1]
def v = [1, 0, 'X', 9, 8, 7, 6, 5, 4, 3, 2]
id.eachWithIndex { n, i -> s += n * w[i]; print n }
log.info("###"+id)
StringBuffer sb = new StringBuffer();
for (int i = 0; i < id.size(); i++) {
	sb.append(id.get(i));
	log.info("###"+ sb)
}
```

``` groovy
// 随机生成手机号码
public class getTel{
    public static int getNum(int start,int end) {
    return (int)(Math.random()*(end-start+1)+start);
    }
    public String getPhone() {
        String[] telFirst="134,135,136,137,138,139,150,151,152,157,158,159,130,131,132,155,156,133,153".split(",");
        String time=(String.valueOf(System.currentTimeMillis()).substring(7));
        String randNo1=String.valueOf(getNum(0,9));
        String randNo2=String.valueOf(getNum(0,9));
 
        int index=getNum(0,telFirst.length-1);
            String first=telFirst[index];
 
        String Phone = (new StringBuilder()).append(first).append(randNo1).append(time).append(randNo2).toString();
        return Phone;
    }
}
getTel p1 = new getTel();
vars.put("phoneNumber",p1.getPhone());
log.info("phoneNUmber:"+vars.get("phoneNumber"));
```

```groovy
// 随机生成身份证
import java.util.*;  
StringBuilder generater = new StringBuilder();  
int sex=0; // 1为男  0 为女
int age=1979; //1979为大于18岁  2000小于18岁
Map areaCode = new HashMap();  
areaCode.put("北京市", 110000);
areaCode.put("市辖区", 110100);
areaCode.put("东城区", 110101);
areaCode.put("西城区", 110102);
areaCode.put("崇文区", 110103);
areaCode.put("宣武区", 110104);
areaCode.put("朝阳区", 110105);
//地区号  
String  randomAreaCode="";  
int index = (int) (Math.random() * areaCode.size());  
Collection values = areaCode.values();  
Iterator it = values.iterator();  
int i = 0;  
int code = 0;  
while (i < index && it.hasNext()) {  
 i++;  
 randomAreaCode = it.next().toString();  
}  
generater.append(randomAreaCode);  

//生日  
String randomBirthday="";  
Calendar birthday = Calendar.getInstance();  
birthday.set(Calendar.YEAR, (int) (Math.random() * 20) + age);  
birthday.set(Calendar.MONTH, (int) (Math.random() * 12));  
birthday.set(Calendar.DATE, (int) (Math.random() * 31));  
StringBuilder builder = new StringBuilder();  
builder.append(birthday.get(Calendar.YEAR));  
long month = birthday.get(Calendar.MONTH) + 1;  
if (month < 10) {  
 builder.append("0");  
}  
builder.append(month);  
long date = birthday.get(Calendar.DATE);  
if (date < 10) {  
 builder.append("0");  
}  
builder.append(date);  
randomBirthday= builder.toString();  
generater.append(randomBirthday);  
//随机码  
String randomCode="";  

int code = (int) (Math.random() * 1000);  
if (code < 10) {  
//    randomCode= "00" + code;
    if(code%2==sex)
    {

    randomCode= "00" + code;
    }
    else
    {
    code=code +  1;
    randomCode= "00" + code;
    }
   
 
} else if (code < 100) {  
// randomCode= "0" + code;  
 if(code%2==sex)
    {
    randomCode= "0" + code;
    }
    else
    {
    code=code +  1;
    randomCode= "0" + code;
    }
} else {  
// randomCode= "" + code;  
  if(code%2==sex)
    {
    randomCode= "" + code; 
    }
    else
    {
    code=code +  1;
    randomCode= "" + code;

    }
}  


generater.append(randomCode);  
//验证码  
char[]  chars= generater.toString().toCharArray();  
int[] c = { 7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2 };  
char[] r = { '1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2' };  
int[] n = new int[17];  
int result = 0;  
for (int i = 0; i < n.length; i++) {  
 n[i] = Integer.parseInt(chars[i] + "");  
}  
for (int i = 0; i < n.length; i++) {  
 result += c[i] * n[i];  
}  
char validateCode = r[result % 11];  
generater.append(validateCode);  


vars.put("idCard",generater.toString());  
SampleResult.setResponseData(generater.toString());  

```


```Groovy
var vareaCode="${regionId}";


var vrandNum="${__Random(000,999)}";


var vbirthday=${__time(YYYY,py)}+parseInt(Math.floor(Math.random()*48+18)+"${__time(MMdd,pmd)}";


var videntifyCode=vareaCode+vbirthday+vrandNum;

 

//生成合法的身份证号
(function(){
var vcount=0;

//加权因子
var vweight=[7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2];

var vCode=[1,0,"X",9,8,7,6,5,4,3,2];

//获取身份证前17位数字
var vtempCode=videntifyCode.split("");


for(var i=0;i<vtempCode.length;i++)
{
vcount+=vtempCode[i]*vweight[i];
}
var vresult=vCode[vcount%11];

videntifyCode+=vresult;
});

//将生成的身份证号放到参数中，以便后面的请求可以使用
vars.put("videntifyCode",videntifyCode);
```