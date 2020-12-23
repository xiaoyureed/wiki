---
title: Extjs学习笔记
date: 2017-08-03 11:56:34
tags: extJs
categories: frontend
---

<div align="center">
网上关于ExtJS的教程多不是最新的ExtJS6, 而且零零散散, 做个整理备查.
reference: [1](http://www.extjs-tutorial.com/), [2](http://www.extjs-tutorial.com/extjs-examples/extjs6-demo), [3](http://docs.sencha.com/extjs/6.5.1/modern/Ext.html), [4](https://www.tutorialspoint.com/extjs/index.htm)
</div>

<!--more-->

<!-- TOC -->

- [Extjs 介绍](#extjs-介绍)
- [配置环境](#配置环境)
- [帮助文档](#帮助文档)
- [Sencha Cmd](#sencha-cmd)
    - [cmd 能做的事](#cmd-能做的事)
    - [安装](#安装)
- [first APP](#first-app)
- [APP目录结构](#app目录结构)
- [命名规范](#命名规范)
- [extjs 类系统](#extjs-类系统)
    - [一些重要的方法](#一些重要的方法)
    - [重要的类](#重要的类)
- [component](#component)
- [container](#container)
- [layout](#layout)
- [define](#define)
    - [define class](#define-class)
    - [define constructor](#define-constructor)
    - [define private member](#define-private-member)
    - [define static member](#define-static-member)
- [extend 继承](#extend-继承)
- [mixins](#mixins)
- [config](#config)
- [自定义事件](#自定义事件)
- [使用DOM 跨浏览器问题](#使用dom-跨浏览器问题)
- [Ext.ComponentQuery](#extcomponentquery)
- [MessageBox](#messagebox)
- [MVVM架构](#mvvm架构)
- [Ext.data.Model](#extdatamodel)
    - [定义一个Model](#定义一个model)
    - [自定义field type](#自定义field-type)
    - [association between Model](#association-between-model)
        - [one to one](#one-to-one)
        - [one to many](#one-to-many)
        - [many to many](#many-to-many)
- [Ext.app.ViewModel](#extappviewmodel)
    - [简单示例](#简单示例)
    - [MVVM架构中](#mvvm架构中)
    - [links](#links)
- [Ext.app.ViewController](#extappviewcontroller)
- [Ext.Ajax](#extajax)
- [Ext.data.Session](#extdatasession)
- [Ext.data.ChainedStore, Ext.data.Store](#extdatachainedstore-extdatastore)
- [CRUD例子](#crud例子)
    - [在form中](#在form中)
    - [在Grid中](#在grid中)
- [sync（）](#sync)
- [Grid中的分页](#grid中的分页)

<!-- /TOC -->

# Extjs 介绍

* 一个基于JavaScript的前端开发框架  
* 开发企业应用前台的绝佳利器  
* 分收费版和免费版（Ext JS is a per-seat/per-server commercial license framework and open-source under GPL licence by Sencha.）  
* 屏蔽了大部分HTML和CSS的代码  
* 支持MVC/MVVM架构    

和AngularJS的对比：  
![](./Extjs学习笔记/extjs_vs_angularjs.png)



# 配置环境

* Extjs SDK: GPL 版本的ExtJS6 https://www.sencha.com/legal/gpl/
* sencha cmd: ALM工具，https://www.sencha.com/products/sencha-cmd/ `Win10下需要手动加入环境变量`
* IDE: Eclipse, necessary plugin: spket http://www.agpad.com/update/


# 帮助文档

API: http://docs.sencha.com/extjs/6.5.1/modern/Ext.html
EXMPLE: http://examples.sencha.com/extjs/
extjs在线编辑器：https://fiddle.sencha.com/#view/editor

# Sencha Cmd

## cmd 能做的事  

![](Extjs学习笔记/cmd.png)  

* 自动生成程序骨架
* 轻量级的web server
* 构建&部署工具

## 安装

* jdk or jre
* [Ruby](https://rubyinstaller.org/downloads/), 安装完验证：ruby -v
* [sencha cmd](https://www.sencha.com/products/sencha-cmd/), 验证：sencha
* 命令示例：`sencha -sdk pathToSenchaSDK generate app appName pathToAppFolder`生成APP骨架，appName和folder要相同




# first APP

* 生成骨架

`sencha -sdk pathToSenchaSDK generate app appName pathToAppFolder`生成APP骨架，appName和pathToAppFolder要相同

* 部署、访问

cmd也可做一个轻量级的web server，在APP目录下  
`sencha web –port xxxx start` xxx表示端口号  
  
访问：http://localhost:xxxx


# APP目录结构

![](Extjs学习笔记/app_folder.png)

![](Extjs学习笔记/app_folder1.png)

# 命名规范

* 遵循CamelCase的写法，常量大写,静态属性大写，私有变量以‘_’开头
* class name 至少要有一个namespace，eg:TopLevelNamespace.MyClassName
* 顶级namespace 和class name 要符合CamelCase,其他要小写，eg：TopNamespace.middnamespace.CustomClass
* classname映射class文件的路径，eg：Ext.util.Observable存储在路径/到/ src / Ext / util / Observable.js；MyTopLevelNamespace.util.JsonToXml被存储在路径/到/ src / MyTopLevelNamespace / util / JsonToXml.js中
* alias 命名规范：mvvm中，view的别名用widget.XX ， controller的别名用'controller.XX， viewmodel的别名用viewmodel.XX   


# extjs 类系统

## 一些重要的方法

Ext.application()  :Loads Ext.app.Application class and starts it up with given configuration after the page is ready.
Ext.apply(obj,{a:'', b:''})  :Copies all the properties of config to the specified object.
Ext.define()  
Ext.override()  
Ext.create()  
Ext.getCmp()  


## 重要的类

Ext: 封装了所有的类和实用方法  
Ext.Base：Ext.define()创建出的所有类的基类，Ext中所有类继承自它  
Ext.ClassManager: 管理所有类；处理类名到类对象的映射  
Ext.Loader: 动态加载相关  

# component

> 继承关系（子类&rArr;父类）：所有component，Ext.Component，Ext.Base  
> 生命周期：初始化&rArr; 渲染&rArr; 销毁

```javascript
Ext.onReady(function () {

    Ext.create('Ext.Component', {
        id:'myComponent',//所有component会注册到Ext.ComponentManager，通过id获取Ext.getCmp('myComponent')
        renderTo: Ext.getBody(),
        html:'Hello World!'
    });

});
```





# container

> container也是component，只是它能容纳其他component，而且有不同的layout
> 所有container继承自Ext.container.Container

```js
Ext.onReady(function () {
    var comp1 = Ext.create('Ext.Component', {
        html:'Component 1'
    });
          
    var comp2 = Ext.create('Ext.Component', {
        html: 'Component 2'
    });

    var comp3 = Ext.create('Ext.Component', {
        html: 'Component 3'
    });

    var comp4 = Ext.create('Ext.Component', {
        html: 'Component 4'
    });

    var container1 = Ext.create('Ext.container.Container', {
        style: { borderColor: 'Red', borderStyle: 'solid', borderWidth: '1px' },
        width: '50%',
        padding: '5 5 5 5',
        items: [comp3, comp4]
    });

    // adding compoents into container using items config
    var container2 = Ext.create('Ext.container.Container', {
        renderTo: Ext.getBody(),
        title: 'Container',
        border: 1,
        width: '50%',
        padding:'5 5 5 5',
        style: { borderColor: '#000000', borderStyle: 'solid', borderWidth: '1px' },
        items: [comp1, comp2]
    });

    // adding container into container 
    container2.add(container1);
});
```

> 重要的container列出如下：

* Ext.container.Viewport：一个专门的container，一个APP会有一个，代表APP的可视区域，定义东南西北
* Ext.container.Container： 最轻的容器，如果只是添加元素进去，排列，则使用
* Ext.panel.Panel：较重的容器，相较于Ext.container.Container会有一些额外功能，如：标题，工具栏
* Ext.form.Panel
* Ext.form.FieldContainer
* Ext.form.FieldSet
* Ext.grid.Panel
* Ext.container.ButtonGroup	
* Ext.tab.Panel
* Ext.tree.Panel
* Ext.menu.Menu
* Ext.toolbar.Toolbar


# layout

> 用于container中

```js
layout: 'auto'

//or

layout: { 
    type: 'auto'
}
```

> 常见布局如下：

* Anchor：根据容器的尺寸成比例的改变元素尺寸
* Absolute：继承自Anchor布局，可以根据x，y精确标定元素位置
* Accordion：一组可折叠的panel
* border :将容器分为五个区域:east,south,west,north,center
* card: 在同一时间，只能有一个child component可见，如：tab panel、Wizard向导页
* column：把整个容器看成一列,然后向容器放入子元素
* form：是一种专门用于管理表单中输入字段的布局，纵向依次排列
* fit：一个子元素将充满整个容器（如果多个子元素则只有一个元素充满整个容器）
* table：按照普通表格的方法布局子元素
* VBox：纵向充满
* hbox：横向充满

```js
Ext.onReady(function() {   
    var panel1 = new Ext.Panel({   
            title: "panel1",  
            height: 100,   
        anchor: '-50',    
        html: "高度等于100，宽度=容器宽度-50"    
    });
    var panel2 = new Ext.Panel({
            title: "panel2",
            height: 100,
            anchor: '50%',   
            html: "高度等于100,宽度=容器宽度的50%"   
    });      
    var panel3 = new Ext.Panel({   
        title: "panel3",   
        anchor: '-10, -250',   
        html: "宽度=容器宽度-10,高度=容器宽度-250"   
    });    
});


layout: 'absolute',
items:[{
    title: 'Panel 1',
    x: 50,
    y: 50,
    html: 'Positioned at x:50, y:50'
}]


layout:'border',
defaults: {
    collapsible: true,
    split: true,
    bodyStyle: 'padding:15px'
},
items: [{
    title: 'Footer',
    region: 'south',
    height: 150,
    minSize: 75,
    maxSize: 250,
    cmargins: '5 0 0 0'
},{
    title: 'Navigation',
    region:'west',
    margins: '5 0 0 0',
    cmargins: '5 5 0 0',
    width: 175,
    minSize: 100,
    maxSize: 250
},{
    title: 'Main Content',
    collapsible: false,
    region:'center',
    margins: '5 0 0 0'
}]


layout: {
    type: 'vbox'
    align : 'stretch',
    pack  : 'start',
},
items: [
    {html:'panel 1', flex:1},
    {html:'panel 2', height:150},
    {html:'panel 3', flex:2}
]

layout: {
    type: 'hbox',
    pack: 'start',
    align: 'stretch'
},
items: [
    {html:'panel 1', flex:1},
    {html:'panel 2', width:150},
    {html:'panel 3', flex:2}
]

```

# define

## define class

```javascript
//语法
Ext.define((String) className, (Object) classMembers, (Optional Function) onClassCreatedCallback);

//示例
Ext.define('Aaaa.bbbb.Student', 
{
    name : 'unnamed',
    getName : function(){
        return "Student name is" + this.name;
    }
}, function(){
    alert('Student object created');
});

var studentObj = Ext.create('Aaaa.bbbb.Student');
studentObj.getName();

```

## define constructor

```javascript
Ext.define('Student', {
    name : 'unnamed',
    getName : function(){
        return 'Student name is ' + this.name;
    },
    constructor : function(studentName){
        if(studentName)
            this.name = studentName;
    }
});

var studentObj = Ext.create('Student','XYZ');
//or
var studentObj = new Student('XYZ');
var name = studentObj.getName();
console.log(name);
//output: Student name is XYZ
```

## define private member

```javascript
Ext.define('Student', function(){
    var name = 'unnamed';
    
    return {
        constructor : function(name){
            this.name = name;
        },
        getName : function(){
            alert('Student name is' + this.name);
        }
    };
});

//create an object of Student class
var studentObj = Ext.create('Student','XYZ');
studentObj.getName();//只能这样获取
//You cannot access name directly as it is a private member. Use getName() method on student object to access name.
```

## define static member

```javascript
Ext.define('Student', 
{
    name : 'unnamed',
    getName : function(){
        alert('Student name is ' + this.name);
    },
    constructor : function(studentName){
        if(studentName)
            this.name = studentName;
    },
    
  statics : 
    {
        getSchoolName : function(){
            return "XYZ";
        }
    }  
});

//call static method
alert(Student.getSchoolName());
```

# extend 继承

> extend 会经常使用，方便使用各个component的默认功能，同事增加我们自己的功能（Most of the time you will have to extend Ext JS components in your application to use default functionality of the component and add custom functionality.）

```javascript
Ext.define('Person', 
{
    name : 'Unknown',
 
    constructor : function(name){
        if(name){
            this.name = name;
        }
    },
 
    getName : function(){
        alert("My name is " + this.name);
    }
});

Ext.define('Student', 
{
    extend : 'Person',
    schoolName : 'Unknown',
 
    constructor : function(name, schoolName){
        this.schoolName = schoolName || 'Unknown';
        
        //call 父类的constructor
        this.callParent(arguments);
    },
 
    getSchoolName : function(){
        alert("My school name is " + this.schoolName);
    }
});

var newStudent = new Student('XYZ', 'ABC School');
newStudent.getName(); //output: XYZ
newStudent.getSchoolName(); //output: ABC School
```

#  mixins

> 在当前类中想使用另外一个类中的function而不使用继承（Mixins allows us to use functions of one class as a function of another class without inheritance.）

```javascript
Ext.define('Person', {
    name: 'Unknown',

    constructor: function(name) {
        if (name) {
            this.name = name;
        }
    },

    getName: function() {
        alert("My name is " + this.name);
    },

    eat: function(foodType) {
        alert("I'm eating " + foodType);
    }

});

Ext.define('Student', {
    schoolName: '',

    constructor: function(schoolName) {
        this.schoolName = schoolName || 'Unknown'
    },

    mixins: {
        eat: 'Person'//key：function名称，不带引号，相当于在本类中定义了方法，value：class名称
    },

    getSchoolName: function() {
        alert("I am a student of " + this.schoolName);
    }

});

var studentObj = new Ext.create('Student', 'XYZ');
studentObj.eat('Sandwich');
```

# config

> 通过config声明的属性都是public的，这些属性能以一个json object的形式被赋值，这些属性自动拥有getter和setter方法，即使我们没有自己定义

```javascript
Ext.define('Student', {
    config : 
    {
        name : 'unnamed',
        schoolName : 'Unknown'
    },
    constructor : function(config){
        this.initConfig(config);//初始化config properties，同时自动生成getter和setter
    },

    //在config property “name”赋值前call
    applyName : function(name){
        return Ext.String.capitalize(name);//获取name前，使name变为大写
    },
   
    //在config property “name”赋值后call
    updateName : function(newValue, oldValue){
        alert('New value: ' + newValue + ', Old value: ' + oldValue);
    }
});

var newStudent = Ext.create('Student', { name: 'XYZ', schoolName: 'ABC School' });

newStudent.getName();//output: XYZ
newStudent.getSchoolName();//output: ABC School

newStudent.setName('John');
newStudent.setSchoolName('New School');

newStudent.getName();//output: John
newStudent.getSchoolName();//output: New School

newStudent.name = 'Steve'; //Not valid.无效，会创造一个普通属性name
newStudent.setName('Steve');//Valid
```


# 自定义事件

> 绑定自定义事件要用到 mixins

```javascript
Ext.define('Student', {
    config : {
        name : ''
    },
    mixins : 
    {
        observable : 'Ext.util.Observable'
    },
    constructor : function(config){
        this.mixins.observable.constructor.call(this, config);
    },
    updateName : function(newValue, oldValue){
        this.fireEvent('studentNameChanged', newValue);//声明自定义事件
    }
});

var newStudent = Ext.create('Student', { name: 'xyz' });

newStudent.on('studentNameChanged', function(name){//注册自定义事件
    alert('student Name ' + name + 'has been Chaged.');
});

newStudent.setName('John');
```

# 使用DOM 跨浏览器问题

> 为了解决跨浏览器问题，extjs有如下类来操作dom元素

* Ext.dom.Element：DOM元素在extjs中的包装
* Ext.dom.CompositeElement：表示一组Ext.dom.Element
* Ext.dom.Helper：包含一些方法，使用指定的属性，创建DOM元素
* Ext.dom.Query：用于查找DOM元素

```javascript
<script>
    Ext.onReady(function () {
        // get Ext.dom.Element object
        var txtFirst = Ext.get("txtFirstName");//根据ID获取DOM元素
        txtFirst.set({ 'value': 'Steve' });

        Ext.fly('txtLastName').set({ 'value': 'Jobs' });//效果相同，更省内存

        console.log(txtFirst.$className);

        // get all input elements and set their bgcolor to yellow
        var inputElements = Ext.select("input"); //inputElements是一个Ext.dom.CompositeElement
        
        inputElements.set({ 'style': 'background-color:yellow' });//处理inputElements中的所有Ext.dom.Element

        var dh = Ext.DomHelper; // ExtJS DomHelper Utility Class 
            var listItems = {
                id: 'dhlist',
                tag: 'ul',
                children: [{
                    tag: 'li',
                    html: 'item 1'
                }, {
                    tag: 'li',
                    html: 'item 2'
                }, {
                    tag: 'li',
                    html: 'item 3'
                }]
            }

            dh.append("div1", listItems);
        
    });
</script>
<body>
  First Name: <input type="text" id="txtFirstName" class="myclass"/> <br /><br />
  Last Name:  <input type="text" id="txtLastName" class="myclass" /> 
  <div id="div1">
  </div>
</body>
```

# Ext.ComponentQuery

> 用CSS selector的语法来获取component

![](Extjs学习笔记/query_rule.png)

```js
Ext.onReady(function () {

    Ext.create('Ext.container.Container', {
        id:'myContainer',
        renderTo: Ext.getBody(),
        padding: '5 5 5 5',
        layout: { type: 'vbox' },
        items: [
            {
                xtype: 'textfield',
                fieldLabel:'First Name'
            },
            {
                xtype: 'textfield',
                fieldLabel: 'Last Name'
            },
            {
                xtype: 'panel',
                layout:'vbox',
                items: [
                    {
                        xtype: 'datefield',
                        fieldLabel: 'Date of Birth'
                    },
                    {
                        xtype: 'container',
                        items: [
                            {
                                xtype: 'textfield',
                                fieldLabel: 'email',
                                itemId:'email'
                            }
                        ]
                    }
                ]
            }
                    
        ]

    });

    //搜索所有的datefield在所有container中
    var dateFields = Ext.ComponentQuery.query('container datefield');
    console.log('DateField in Containers: ');
    console.log(dateFields);
    //搜索panel的直接子元素datefield
    var dateFieldsInPanels = Ext.ComponentQuery.query('panel > datefield');
    console.log('Child DateField in Panel: ' );
    console.log(dateFieldsInPanels);
    //根据ID
    var emailInMyContainer = Ext.ComponentQuery.query('#myContainer > #email');
    console.log('#email inside #myContainer');
    console.log(emailInMyContainer);
});
```

# MessageBox

> 和js中alert()不同，message box 不会阻断代码的运行，如果想阻断代码运行，需要指定 callback function
> Ext.Msg is a singleton class,如果同时定义多个message box 只会显示最后的一个

四种类型：

* alert
* confirm
* prompt
* custom

```js
Ext.Msg.alert('Status', 'This is Ext JS message box.'); 
//or
var msg = Ext.create('Ext.window.MessageBox');
msg.alert('Status', 'This is Ext JS message box.');

Ext.Msg.confirm("Confirmation", "Do you want to Save changes?", function(btnText){
                if(btnText === "no"){
                    Ext.Msg.alert("Alert", "You have confirmed 'No'.");
                }
                else if(btnText === "yes"){
                    Ext.Msg.alert("Alert", "You have confirmed 'Yes'.");
                }
            }, this);

Ext.Msg.prompt("Ext JS Tutorials", "Please enter your Sencha Id:", function(btnText, sInput){
                if(btnText === 'ok'){
                    Ext.Msg.alert("Status", "You entered:" + sInput);
                }
            }, this);

Ext.Msg.show({
    title : 'Save',
    msg : 'Do you want to Save the changes? ',
    width : 300,
    closable : false,
    buttons : Ext.Msg.YESNOCANCEL,
    buttonText : 
    {
        yes : 'Yes & Continue',
        no : 'No & Continue',
        cancel : 'Discard'
    },
    multiline : false,
    fn : function(buttonValue, inputText, showConfig){
        Ext.Msg.alert('Status', buttonValue);
    },
    icon : Ext.Msg.QUESTION
});

```

# MVVM架构

从extjs5起开始支持mvvm架构  

* view: 任何类型的组件，将信息输出到浏览器（View is any type of components that output information on to your browser, i.e. Container, Form, Grid, Chart.）
* model：提供数据模型，定义数据字段,一般和store一起用,为Grid等提供数据；这里也是数据校验，转换的好地方（validation, conversion）
* viewController：附加给特定view，处理view的所有事件
* viewModel: 数据中心，管理特定view的数据，其他组件可以与之绑定，与viewModel中的数据同步变化


# Ext.data.Model

## 定义一个Model

从server 保存和接受数据的相关类：

* Model (Ext.data.Model)
* Store (Ext.data.Store)
* Proxy (Ext.data.proxy.Proxy) 注意：Proxy能和store关联也能和Model关联
* Session (Ext.data.Session)

Model包括：fields, validations and methods

```js
Ext.define('Student', {
    extend: 'Ext.data.Model',
    idProperty:'Id',
    fields: [
        { name: 'Id', type: 'int' },//type取值：auto, int, string, float, Boolean and date
        'firstName',//没有type，默认是string
        'lastName'
    ],
    validators: {
        firstName: 'presence',
        lastName: { type: 'length', min: 2 }
    }

});

var studentRecord = Ext.create('Student',{
    firstName:'',
    lastName: 'J'
});

if (!studentRecord.isValid())//验证是否通过校验器
{
    var validation = studentRecord.getValidation();
    console.log('First Name: ' + validation.get('firstName'));
    console.log('Last Name: ' + validation.get('lastName'));
}
```

常见校验器：

![](Extjs学习笔记/validate.png)


## 自定义field type

```js
Ext.define('Gender', {
    extend: 'Ext.data.field.String',
    alias: 'data.field.gender',
    validators: {
            type: 'inclusion',
            list: [ 'female', 'male' ]
    }
}); 

Ext.define('Student', {
    extend: 'Ext.data.Model',
    idProperty:'Id',
    fields: [
        { name: 'Id', type: 'int' },
        'firstName',
        'lastName',
        { name:'gender', type:'gender'} // Uses custom field
    ] 
});
var studentRecord = Ext.create('Student',{
    firstName:'James',
    lastName: 'Bond',
    gender:'TEST'
});

if (!studentRecord.isValid())
{
    var validation = studentRecord.getValidation();

    console.log('Gender: ' + validation.get('gender'));
}
```

## association between Model

### one to one

```js
Ext.define('Address', {
    extend: 'Ext.data.Model',

    fields: [
        { name: 'addressId', type: 'int' },
        'address',
        'city',
        'state'
    ],
    identifier: {
        type: 'negative'
    }

});

Ext.define('Student', {
    extend: 'Ext.data.Model',
    
    idProperty:'Id',
    fields: [
        { name: 'Id', type: 'int' },
        'firstName',
        'lastName',
        { name:'addressId', reference:'Address', unique: true}
    ],
    identifier: {
        type: 'negative'
    }

});

var studentRecord = Ext.create('Student',{
    firstName:'James',
    lastName: 'Bond'
});


var address = Ext.create('Address', {
    address: 'Y Street'
})

studentRecord.address = address;
```

### one to many

hasMany配置项  
还可以通过reference配置项

```js
Ext.define('Student', {
    extend: 'Ext.data.Model',
    
    idProperty:'Id',
    fields: [
        { name: 'Id', type: 'int' },
        'firstName',
        'lastName'
    ],
    identifier: {
        type: 'negative'
    },

    hasMany: {
        name: 'Courses',
        model:'Course'
    }
});

Ext.define('Course', {
    extend: 'Ext.data.Model',
    idProperty: 'Id',
    fields: [
        { name: 'Id', type: 'int' },
        'courseName'
    ],
    identifier: {
        type: 'negative'
    }
});

var course1 = Ext.create('Course', {
    courseName: 'Ext JS 4'
})

var course2 = Ext.create('Course', {
    courseName: 'Ext JS 5'
})
var studentRecord = Ext.create('Student',{
    firstName:'James',
    lastName: 'Bond'
});

var courses = studentRecord.Courses();
courses.add(course1);
courses.add(course2);
```

### many to many

通过manaToMany配置项  

```js
Ext.define('Student', {
    extend: 'Ext.data.Model',
    
    idProperty:'Id',
    fields: [
        { name: 'Id', type: 'int' },
        'firstName',
        'lastName'
    ],
    identifier: {
        type: 'negative'
    },
    manyToMany: 'Course' 
});

Ext.define('Course', {
    extend: 'Ext.data.Model',
    idProperty: 'Id',
    fields: [
        { name: 'Id', type: 'int' },
        'courseName'
    ],
    identifier: {
        type: 'negative'
    }
});



var course1 = Ext.create('Course', {
    courseName: 'ExtJS 4'
});

var course2 = Ext.create('Course', {
    courseName: 'ExtJS 5'
});

var course3 = Ext.create('Course', {
    courseName: 'ExtJS 6'
});

var student1 = Ext.create('Student', {
    firstName: 'Bob',
    lastName: 'Friss'
});

var student2 = Ext.create('Student', {
    firstName: 'James',
    lastName: 'Bond'
});

var student3 = Ext.create('Student', {
    firstName: 'Sachin',
    lastName: 'Tendulkar'
});

var courses = student1.courses();
courses.add(course1);
courses.add(course2);

var students = course3.students();
students.add(student2);
students.add(student3);
```

# Ext.app.ViewModel

* viewModel用来为特定的component管理数据，可以理解为一个数据容器
* 支持与view的双向数据绑定，view和viewmodel中的数据会保持一致
* 和model不同，viewmodel中不能有Proxy，因此无法直接从server获取数据

## 简单示例

```js
Ext.define('MyApp.view.TestViewModel', {
    extend: 'Ext.app.ViewModel',

    alias: 'viewmodel.test',

    data: {
        id: 1,
        name: 'Steve'
    }
});

Ext.create('Ext.Component', {

    viewModel:{
        type:'test'
    },

    bind:{
        html:'{id} - {name}'
    },

    renderTo:Ext.getBody()
});
```

## MVVM架构中

```js
Ext.define('MyExtJSApp.StudentViewModel', {
    extend: 'Ext.app.ViewModel',
    alias:'viewmodel.studentviewmodel',
    
    data: {
        firstName: 'James',
        lastName:'Bond'
    },

    formulas: {
        //定义键值对，值有function返回，和data中定义的键值对地位一样，可以在component中获取
        fullName: function(get){
            return get('firstName') + ' ' + get('lastName');
        }
    }
});

Ext.application({
    name: 'SchoolApp',
    requires: ['SchoolApp.StudentViewModel'],

    launch: function() {//APP在launch时候调用此函数
        Ext.create('Ext.container.Viewport', {

            items: [{
                xtype: 'form',
                layout:'form',
                bodyPadding: '5',
                buttonAlign: 'center',

                viewModel: {
                    type: 'StudentViewModel'
                },
                
                items: [{
                    xtype: 'textfield',
                    fieldLabel: 'First Name',
                    bind: {
                        value: '{firstName}'
                    }
                }, {
                    xtype: 'textfield',
                    fieldLabel: 'Last Name',
                    bind: {
                        value: '{lastName}'
                    }
                }, {
                    xtype: 'displayfield',
                    fieldLabel: 'Full Name',
                    bind: {
                        value: '{fullName}'
                    }
                }],
                buttons: [{
                    text: 'Save'
                }, {
                    text: 'Reset'
                }]

            }]

        })
    }
});
```

## links

* viewmodel中可使用links链接指定的record为我所用

```js
Ext.define('SchoolApp.view.student.StudentViewModel', {
    extend: 'Ext.app.ViewModel',
    alias:'viewmodel.StudentViewModel',
    
    formulas: {
        fullName: function(get){
            return get('theStudent.firstName') + ' ' + get('theStudent.lastName');
        }
    },
    links: {
        theStudent: {//给ID为5的student命名为theStudent
            type: 'Student',//这里是Model，Model中可定义Proxy，从server中获取数据
            id:5
        }
    }
});
```

# Ext.app.ViewController

* 利用CSS-like selectors来处理component的事件
* APP中ViewController和view一般可以放在一个文件夹下

```js
Ext.define('SchoolApp.view.student.StudentMaster', {
    extend: 'Ext.form.Panel',
    alias: 'widget.StudentMaster',
    
    config: {},
    requires: ['SchoolApp.view.student.StudentViewController'],
    title: 'Student Information',

    constructor: function (config) {
        return this.callParent(arguments);
    },

    controller: 'StudentViewController',

    initComponent: function () {

        Ext.apply(this, {

            resizable: false,
            collapsible: true,
            bodyPadding: '5',
            buttonAlign: 'center',
            border: false,
            layout: 'anchor',
            defaults: {
                anchor: '100%'
            },
            items: [{
                xtype: 'textfield',
                fieldLabel: 'First Name',
                reference: 'firstName'// controller中：this.lookupReference('firstName').setValue('Steve');
                //或者这样用：this.getRreferences().firstName
            },
            {
                xtype: 'textfield',
                fieldLabel: 'Last Name',
                reference: 'lastName'
            }
            ],
            buttons: [{
                        text: 'Read', itemID: 'btnRead',
                        listeners: {
                            click: 'onReadClick'//单击事件名
                        }
                },
                {
                    text: 'Save', itemID: 'btnSave',
                    listeners: {
                        click: 'onSaveClick'
                    }
                },
                {
                    text: 'Reset',
                    listeners: {
                        click: 'onResetClick'
                    }
                },
                {
                    text: 'Exit',
                    listeners: {
                        click: 'onExitClick'
                    }
                }

            ]

        });

        this.callParent(arguments);
    }
});

Ext.define('SchoolApp.view.student.StudentViewController', {
    extend: 'Ext.app.ViewController',
    alias: 'controller.StudentViewController',

    init: function (view) {

    },

    onReadClick: function () {
        this.lookupReference('firstName').setValue('Steve');
        this.lookupReference('lastName').setValue('Jobs');

        Ext.Msg.alert('Status', 'Read button clicked.');
    },

    onSaveClick: function () {
        Ext.Msg.alert('Status', 'Save button clicked.');

    },

    onResetClick: function () {
        this.lookupReference('firstName').setValue('');
        this.lookupReference('lastName').setValue('');

        Ext.Msg.alert('Status', 'Reset button clicked.');
    },

    onExitClick: function () {
        Ext.Msg.alert('Status', 'Exit button clicked.');
        this.getView().destroy();//销毁view
    }

});
```

# Ext.Ajax


```js
Ext.Ajax.request({
    url: '/api/students',
    method: 'GET',
    timeout: 60000,
    params:
    {
        id: 1 // loads student whose Id is 1
    },
    headers:
    {
        'Content-Type': 'application/json'
    },
    success: function (response) {
    
    },
    failure: function (response) {
        Ext.Msg.alert('Status', 'Request Failed.');

    }
});
```

#  Ext.data.Session

* session 可以和任意的view，viewmodel，store关联
* 数据加载进session后，可以被他管理，处理crud操作
* 设置view下session : true，创建新的session并与之建立联系，所有子元素都从这个session生成

```js
//两个可编辑的Grid
Ext.application({
    name: 'SchoolApp',
    autoCreateViewport: false,
    views: ['SchoolApp.view.student.StudentList', 'SchoolApp.view.student.StateList'],
    requires: ['SchoolApp.view.student.StudentViewModel'],

    launch: function () {

        Ext.create('Ext.container.Viewport', {
            layout: 'border',
            dock: 'top',
            viewModel: {
                type: 'StudentViewModel'
            },
            session: true,
            items: [
                {
                    region: 'north',
                    xtype: 'toolbar',
                    items: [{
                        text: 'Save Batch',
                        handler: function () {
                            try {
                                var viewport = this.up().up();
                                var ses = viewport.getSession();

                                var batch = ses.getSaveBatch();

                                batch.on({
                                    complete: function () {
                                        Ext.Msg.alert('Status', 'Data Saved Successfully!');
                                    },
                                    exception: function () {
                                        Ext.Msg.alert('Error', 'Error occurred');
                                    }
                                });

                                batch.start();
                            }
                            catch (ex) {
                                Ext.Msg.alert('Error', ex.message);
                            }
                        }
                    },
                    {
                        text: 'Show Changes',
                        handler: function () {
                            var viewport = this.up().up();
                            var ses = viewport.getViewModel().getSession();
                            var changes = ses.getChanges();//a list of record which have changed

                            if (!changes) {
                                Ext.Msg.alert('Status', 'No Changes');
                                return;
                            }
                            //submit changed records to the remote server
                            if (changes.State && changes.State.U) // if state updated
                            {
                                var changedStateNames = "Changed State Names: ";
                                for (var i = 0; i < changes.State.U.length ; i++) {
                                    changedStateNames += changes.State.U[i].name + ", ";
                                }

                                Ext.Msg.alert('Status', changedStateNames);
                            }

                            if (changes.Student && changes.Student.U) { // if student updated
                                var changedStudentNames = "Changed Student Names: ";
                                for (var i = 0; i < changes.Student.U.length ; i++) {
                                    changedStudentNames += changes.Student.U[i].firstName + ", ";
                                }

                                Ext.Msg.alert('Status', changedStudentNames);
                            }
                        }
                    }]
                },
                {
                    region: 'center',
                    xtype: 'StudentList',
                    bind: { store: '{students}' },
                    flex: 1
                },
                {
                    xtype: 'StateList',
                    region: 'south',
                    bind: { store: '{states}' },
                    flex: 1
                }]
        });
    }
});

Ext.define('SchoolApp.view.student.StudentViewModel', {
    extend: 'Ext.app.ViewModel',
    alias: 'viewmodel.StudentViewModel',

    stores: {
        students: {
            model: 'SchoolApp.model.Student',
            session: true,
            autoLoad: true,
            sorters: [{
                property: 'firstName',
                direction:'DESC'
            }]
        },
        states: {
            model: 'SchoolApp.model.State',
            session: true,
            autoLoad: true,
            sorters: [{
                property: 'name',
                direction: 'ASC'
            }]
        }
    }
});
```

# Ext.data.ChainedStore, Ext.data.Store

* chained store链接到store，record也是来自store
* chained store能定义自己的sorters， filters，以不同的规则展示原来的record
*  chained store能定义在单独的js文件中，也能定义在viewmodel中

```js
Ext.define('SchoolApp.store.Student', {
    extend: 'Ext.data.Store',
    model: 'SchoolApp.model.Student',
   
    autoLoad: true,
    sorters: [{
        property: 'firstName',
        direction: 'ASC'
    }]

}); 

Ext.define('SchoolApp.store.StudentChainedStore', {
    extend: 'Ext.data.ChainedStore',
    source:'SchoolApp.store.Student',//source store
    sorters: [{//在这里设置另外的sorters
        property: 'firstName',
        direction: 'desc'
    }]

});
```

# CRUD例子

## 在form中

```js
Ext.define('MyApp.model.Student', {
    
    extend: 'Ext.data.Model',
    idProperty:'Id',//每个model都会自动生成一个‘id’ field，不论是否显式指定；idProperty被指定，将代替id为Id
    schema: {
        namespace: 'School.model'
    },
    fields: [
        { name: 'Id', type: 'int', defaultValue: 0},//此时，Student.getFields()里面没有id了，被Id替换了
        { name: 'firstName', type: 'string' },
        { name: 'middleName', type: 'string' },
        { name: 'lastName', type: 'string' },
        { name: 'birthDate', type: 'date' },
        { name: 'address1', type: 'string' },
        { name: 'address2', type: 'string' },
        { name: 'city', type: 'string' },
        { name: 'state', type: 'string' }
    ],
    
    validations: [{
        type: 'presence',
        field: 'firstName'
    }]

});

Ext.define('MyApp.view.student.Student', {
    extend: 'Ext.form.Panel',

    xtype: 'studentForm',
    title: 'Student Entry Form',
    requires: [
        'MyApp.view.student.StudentController'
    ],

    controller: 'student',
    initComponent: function () {//APP launch时触发
        Ext.apply(this,//将如下属性加入当前对象
        {
            //表示CUD 操作将使用form.Submit()
            jsonSubmit: true,
            url: '/api/student',
            resizable: false,
            collapsible: false,
            bodyPadding: '5',
            buttonAlign: 'center',
            border: false,
            trackResetOnLoad: true,
            layout:
            {
                type: 'vbox'
            },
            fieldDefaults:
            {
                xtype: 'textfield',
                msgTarget: 'side',
                labelAlign: 'top',
                labelStyle: 'font-weight:bold'
            },
            defaultType: 'textfield',
            items: [{
                    xtype: 'fieldcontainer',
                    layout: 'hbox',
                    defaultType: 'textfield',
                    width: '100%',
                    fieldDefaults:
                    {
                        labelAlign: 'top',
                        labelStyle: 'font-weight:bold'
                    },
                    items: [{
                        fieldLabel: 'Id',
                        name: 'Id',
                        readOnly: true,
                        width: 55
                    },
                    {
                        fieldLabel: 'First Name',
                        flex: 1,
                        name: 'firstName',
                        margin: '0 0 0 5',
                        allowBlank: false
                    },
                        {
                            name: 'middleName',
                            width: 150,
                            margin: '0 0 0 5',
                            fieldLabel: 'Middle Name:'
                        },
                        {
                            fieldLabel: 'Last Name',
                            flex: 1,
                            margin: '0 0 0 5',
                            name: 'lastName'
                        }]
                },
                    {
                        xtype: 'datefield',
                        fieldLabel: 'Date of Birth',
                        name: 'birthDate'
                    },
                    {
                        xtype: 'textfield',
                        fieldLabel: 'Address',
                        width: '100%',
                        name: 'address1'
                    },
                    {
                        xtype: 'textfield',
                        hideLabel: true,
                        name: 'address2',
                        width: '100%',
                        fieldLabel: 'address2'

                    },
                    {
                        xtype: 'textfield',
                        fieldLabel: 'City',
                        width: '100%',
                        name: 'city'
                    },
                    {
                        xtype: 'textfield',
                        fieldLabel: 'state',
                        width: '100%',
                        name: 'state'
                    }
            ],
            buttons: [{
                text: 'Create',
                itemId: 'btnCreate',
                formBind: true,
                handler: 'onCreateClick'
            },
            {
                text: 'Read',
                itemId: 'btnLoad',
                handler: 'onReadClick'
            },

            {
                text: 'Update',
                itemId: 'btnUpdate',
                formBind: true,
                handler: 'onUpdateClick'
            },
            {
                text: 'Delete',
                itemId: 'btnDelete',
                formBind: true,
                handler: 'onDeleteClick'
            },
            {
                text: 'Reset',
                itemId: 'btnReset',
                handler: 'onResetClick'
            },
            {
                text: 'Clear',
                itemId: 'btnClear',
                handler: 'onClearClick'
            }]
        });

        this.callParent(arguments);//call父类的constructor

    },
    clearForm: function () {
        this.getForm().getFields().each(function (field) {
            field.validateOnChange = false;
            field.setValue('');
            field.resetOriginalValue();
        });
    }
});

Ext.define('MyApp.view.student.StudentController', {
    extend: 'Ext.app.ViewController',

    alias: 'controller.student',

    onCreateClick: function (sender, record) {
        var studentForm = this.getView().getForm();

        if (!studentForm.isDirty()) {
            Ext.Msg.alert('Status', 'No new data to create.');
            return;
        }
        else if (!studentForm.isValid()) {
            Ext.Msg.alert('Status', 'Invalid data.');
            return;
        }

        //submit()执行CUD，这里是增，POST请求
        // Submit the Ajax request and handle the response
        studentForm.submit({
            url: '/api/student',//=>http://localhost/api/student
            waitMsg: 'Saving..',
            headers:
            {
                'Content-Type': 'application/json'
            },
            clientValidation: true,
            submitEmptyText: true,
            success: function (form, action) {

                var student = Ext.create('School.model.Student');
                var resp = Ext.decode(action.response.responseText);

                if (resp.data[0]) {
                    // addstudent returns student model with Id so we can re-load model into form so form will have isDirty false
                    student.set(resp.data[0]);
                    studentForm.loadRecord(student);
                }

                Ext.Msg.alert('Status', 'Saved successfully.');

            },
            failure: function (form, action) {
                if (action.failureType === Ext.form.action.Action.CLIENT_INVALID) {
                    Ext.Msg.alert('CLIENT_INVALID', 'Something has been missed. Please check and try again.');
                }
                if (action.failureType === Ext.form.action.Action.CONNECT_FAILURE) {
                    Ext.Msg.alert('CONNECT_FAILURE', 'Status: ' + action.response.status + ': ' + action.response.statusText);
                }
                if (action.failureType === Ext.form.action.Action.SERVER_INVALID) {
                    Ext.Msg.alert('SERVER_INVALID', action.result.message);
                }
            }
        });
    },

    onReadClick: function (sender, record) {
        var studentForm = this.getView().getForm();

        //load()发出get请求，读取数据
        //result should contain success=true and data property otherwise it will go to failure even if there is no failure
        studentForm.load({
            waitMsg: 'Loading...',
            method: 'GET',
            params:
            {
                id: 1
            },
            success: function (form, action) {
                try {
                    var resp = Ext.decode(action.response.responseText);

                    if (resp.data.length > 0) {
                        // addstudent returns student model with Id so we can re-load model into form so form will have isDirty false
                        var student = Ext.create('School.model.Student');
                        student.set(resp.data[0]);
                        studentForm.loadRecord(student);
                    }
                }
                catch (ex) {
                    Ext.Msg.alert('Status', 'Exception: ' + ex.Message);

                }
            },
            failure: function (form, action) {
                Ext.Msg.alert("Load failed", action.result.errorMessage);
            }
        });
    },

    onUpdateClick: function (sender, record) {
        var studentForm = this.getView().getForm();

        if (!studentForm.isDirty()) {
            Ext.Msg.alert('Status', 'No pending changes to save.');
            return;
        }
        else if (!studentForm.isValid()) {
            Ext.Msg.alert('Status', 'Invalid data.');
            return;
        }

        studentForm.submit({
            url: '/api/student',
            waitMsg: 'Updating..',
            method: 'PUT',
            headers:
            {
                'Content-Type': 'application/json'
            },
            clientValidation: true,
            success: function (form, action) {
                try {
                    var student = Ext.create('School.model.Student');
                    var resp = Ext.decode(action.response.responseText);

                    if (resp.data.length > 0) {
                        // addstudent returns student model with Id so we can re-load model into form so form will have isDirty false
                        student.set(resp.data[0]);
                        studentForm.loadRecord(student);
                    }

                    Ext.Msg.alert('Status', 'Saved successfully.');
                }
                catch (ex) {
                    Ext.Msg.alert('Status', 'Exception: ' + ex.Message);

                }
            },
            failure: function (form, action) {
                if (action.failureType === Ext.form.action.Action.CLIENT_INVALID) {
                    Ext.Msg.alert('CLIENT_INVALID', 'Something has been missed. Please check and try again.');
                }
                if (action.failureType === Ext.form.action.Action.CONNECT_FAILURE) {
                    Ext.Msg.alert('CONNECT_FAILURE', 'Status: ' + action.response.status + ': ' + action.response.statusText);
                }
                if (action.failureType === Ext.form.action.Action.SERVER_INVALID) {
                    Ext.Msg.alert('SERVER_INVALID', action.result.message);
                }
            }
        });
    },
    onDeleteClick: function (sender, record) {
        var me = this,
            studentForm = me.getView();

        if (!studentForm.getValues(false, false, false, true).Id) {
            Ext.Msg.alert('Status', 'Invalid or No data to delete.');
            return;
        }

        var student = Ext.create('School.model.Student'), data;

        student.set(studentForm.getValues());
        data = student.getData();

        Ext.Msg.show({
            title: 'Delete',
            msg: 'Do you want to delete this record? ',
            width: 300,
            closable: false,
            buttons: Ext.Msg.YESNO,
            icon: Ext.Msg.QUESTION,
            fn: function (buttonValue, inputText, showConfig) {
                if (buttonValue === 'yes') {

                    studentForm.submit({
                        url: '/api/student',
                        method: 'DELETE',
                        clientValidation: true,
                        waitMsg: 'Deleting..',
                        headers:
                        {
                            'Content-Type': 'application/json'
                        },

                        success: function (form, action) {
                            try {
                                var resp = Ext.decode(action.response.responseText);
                                studentForm.clearForm();

                                Ext.Msg.alert('Success', resp.message);
                            }
                            catch (ex) {
                                Ext.Msg.alert('Status', 'Exception: ' + ex.Message);

                            }
                        },
                        failure: function (form, action) {
                            if (action.failureType === Ext.form.action.Action.CLIENT_INVALID) {
                                Ext.Msg.alert('CLIENT_INVALID', 'Something has been missed. Please check and try again.');
                            }
                            if (action.failureType === Ext.form.action.Action.CONNECT_FAILURE) {
                                Ext.Msg.alert('CONNECT_FAILURE', 'Status: ' + action.response.status + ': ' + action.response.statusText);
                            }
                            if (action.failureType === Ext.form.action.Action.SERVER_INVALID) {
                                Ext.Msg.alert('SERVER_INVALID', action.result.message);
                            }
                        }
                    });
                }
            }

        });
    },
    onResetClick: function (sender, record) {
        this.getView().getForm().reset();
    },
    onClearClick: function (sender, record) {
        this.getView().clearForm();
    },

});
```

## 在Grid中

```js
//model用上个例子的，没变

Ext.define('MyApp.viewmodel.StudentViewModel', {
    extend: 'Ext.app.ViewModel',
    alias: 'viewmodel.studentviewmodel',
    stores: {
        StudentListStore: {
            model: 'MyApp.model.Student',
            autoLoad: true,//grid will display data as soon as it load records 
            autoSync: true,//当用户在Grid进行了操作，StudentListStore也会相应的update or delete
            //如果autoSync：false，store不会自动更新变化，需要xxxStore.sync({xxx}),见下节
            proxy:
            {
                type: 'rest',
                reader:
                {
                    rootProperty: 'data',
                    type: 'json'
                },
                url: '/api/student',
                writer: {
                    type: 'json',
                    dateFormat: 'd/m/Y',
                    writeAllFields: true
                }
            }
        }

    }
});

Ext.define('MyApp.view.studentlist.StudentList', {
    extend: 'Ext.grid.Panel',
    xtype: 'studentList',
    requires: [
        'MyApp.view.studentlist.StudentListController',
        'MyApp.viewmodel.StudentViewModel'
    ],
    title: 'Student List',

    controller: 'student-list',
    viewModel: { type: 'studentviewmodel' },//包含此view的store
    reference:'studentlistgrid',
    selType: 'rowmodel',
    selModel:
    {
        mode: 'SINGLE'
    },
    viewConfig:
    {
        stripeRows: true
    },
    listeners: {
        selectionchange: 'onSelectionChange'
    },
    bind: {
        store: '{StudentListStore}'//绑定viewmodel中的StudentListStore
    },
    initComponent: function () {
        Ext.apply(this,
        {
            plugins: [Ext.create('Ext.grid.plugin.RowEditing',//可以进行行编辑
            {
                clicksToEdit: 2
            })],

            columns: [{
                text: "Id",
                dataIndex: 'Id',
                hidden: false,
                width: 35
            },
            {
                text: "First Name",
                flex: 1,
                dataIndex: 'firstName',
                editor:
                {
                    // defaults to textfield if no xtype is supplied
                    allowBlank: false
                }
            },
            {
                text: "Middle Name",
                flex: 1,
                dataIndex: 'middleName',
                editor:
                {
                    allowBlank: true
                }
            },
            {
                text: "Last Name",
                flex: 1,
                dataIndex: 'lastName',
                editor:
                {
                    allowBlank: true
                }
            },
            {
                xtype: 'datecolumn',
                header: "Birth Date",
                width: 135,
                dataIndex: 'birthDate',
                editor:
                {
                    xtype: 'datefield',
                    allowBlank: true
                },
                renderer: Ext.util.Format.dateRenderer('d/m/Y')
            },
            {
                text: "City",
                flex: 1,
                dataIndex: 'city',
                editor:
                {
                    allowBlank: true
                }
            },
            {
                text: "State",
                flex: 1,
                dataIndex: 'state',
                editor:
                {
                    allowBlank: true
                }
            }],
            tbar: [{
                text: 'Add Student',
                iconCls: 'fa-plus',
                handler: 'onAddClick'
            }, {
                itemId: 'removeStudent',
                text: 'Remove Student',
                iconCls: 'fa-times',
                reference: 'btnRemoveStudent',
                handler: 'onRemoveClick',
                disabled: true
            }]
        });

        this.callParent(arguments);
    }
});

Ext.define('MyApp.view.studentlist.StudentListController', {
    extend: 'Ext.app.ViewController',

    alias: 'controller.student-list',

    onAddClick: function (sender, record) {

        
        var studentGrid = this.getView();
        var studentStore = studentGrid.getStore();

        //adding dummy student假数据
        var studentModel = Ext.create('School.model.Student');
        studentModel.set("Id", 0);
        studentModel.set("firstName", "New Student");
        studentModel.set("middleName", "");
        studentModel.set("lastName", "");
        studentModel.set("birthDate", "");
        studentModel.set("city", "");
        studentModel.set("state", "");

        studentStore.insert(0,studentModel);
    },

    onLoadClick: function (sender, record) {
        var studentStore = this.getView().getStore();
        studentStore.load();
    },

    onRemoveClick: function (sender, record) {
        var studentGrid = this.getView();
        var studentStore = studentGrid.getStore();

        //delete selected rows if selModel is checkboxmodel
        var selectedRows = studentGrid.getSelectionModel().getSelection();

        studentStore.remove(selectedRows);
    },

    onSelectionChange: function (sender, record, isSelected) {
        var removeBtn = this.lookupReference('btnRemoveStudent');
        if(record.length)
            removeBtn.setDisabled(false);
        else
            removeBtn.setDisabled(true);
    }
});

```


# sync（）

* sync() 可以对多个record执行CRUD操作

```js
//fires create, update and delete request when calling sync and commit changes in the store when autoSync=false
        studentStore.sync({
            
            success : function(batch, opt){
                Ext.Msg.alert('Status', 'Changes saved successfully.');
            },
            failure : function(batch, opt){
                var msg = '';
                
                if(batch.hasException){
                    
                    for(var i = 0; i < batch.exceptions.length; i ++ ){
                        switch(batch.exceptions[i].action){
                            case "destroy" : 
                                msg = msg + batch.exceptions[i].records.length + " Delete, ";
                                break;
                            case "update" : 
                                msg = msg + batch.exceptions[i].records.length + " Update, ";
                                break;
                            case "create" : 
                                msg = msg + batch.exceptions[i].records.length + " Create, ";
                                break;
                        }
                    }
                    
                    Ext.Msg.alert("Status", msg + " operation failed!");
                }
                else
                    Ext.Msg.alert('Status', 'Changes failed.');
            }
        });
```

# Grid中的分页

```js
Ext.define('School.model.Student', {
    
    extend: 'Ext.data.Model',
    idProperty:'Id',
    schema: {
        namespace: 'School.model'
    },
    fields: [
        { name: 'Id', type: 'int', defaultValue: 0},
        { name: 'firstName', type: 'string' },
        { name: 'middleName', type: 'string' },
        { name: 'lastName', type: 'string' },
        { name: 'birthDate', type: 'date' },
        { name: 'address1', type: 'string' },
        { name: 'address2', type: 'string' },
        { name: 'city', type: 'string' },
        { name: 'state', type: 'string' }
    ]

});

Ext.define('School.view.student.StudentViewModel', {
    extend: 'Ext.app.ViewModel',
    alias: 'viewmodel.studentviewmodel',
    stores: {
        StudentListPagingStore: {
            model: 'School.model.Student',
            autoLoad: true,//load data when view loads
            pageSize: 5,
            proxy:
            {
                type: 'rest',
                url: '/api/student'
                reader:
                {
                    type: 'json',
                    rootProperty: 'Students',
                    totalProperty: 'TotalCount'//有多少条record
                }
            }
        }

    }
});

Ext.define('School.view.student.StudentListPaging', {
    extend: 'Ext.grid.Panel',
    xtype: 'studentListPaging',

    title: 'Student List - Paging Demo',

    viewModel: { type: 'studentviewmodel' },
    selType: 'rowmodel',
    selModel:
    {
        mode: 'SINGLE'
    },
    viewConfig:
    {
        stripeRows: true
    },
    listeners: {
        selectionchange: 'onSelectionChange'
    },
    bind: {
        store: '{StudentListPagingStore}'
    },
    initComponent: function () {
        Ext.apply(this,
        {
            columns: [{
                text: "Id",
                dataIndex: 'Id',
                width: 35
            },
            {
                text: "First Name",
                flex: 1,
                dataIndex: 'firstName'
            },
            {
                text: "Middle Name",
                flex: 1,
                dataIndex: 'middleName'
            },
            {
                text: "Last Name",
                flex: 1,
                dataIndex: 'lastName'
            },
            {
                xtype: 'datecolumn',
                header: "Birth Date",
                width: 135,
                dataIndex: 'birthDate',
                renderer: Ext.util.Format.dateRenderer('d/m/Y')
            },
            {
                text: "City",
                flex: 1,
                dataIndex: 'city'
            },
            {
                text: "State",
                flex: 1,
                dataIndex: 'state'
            }],
            bbar: [{
                xtype: 'pagingtoolbar',
                bind:{
                    store: '{StudentListPagingStore}'
                },
                displayInfo: true,
                displayMsg: 'Displaying {0} to {1} of {2} &nbsp;records ',
                emptyMsg: "No records to display&nbsp;"
            }]

        });

        this.callParent(arguments);
    }
});
```




