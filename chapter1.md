



# react 基础知识



---



### 第一节：

 1、props:是react实例上属性值得集合



 2、class：是es6种的保留的关键字了，在jsx中，应该用className



 3、在react中，行内样式不能用字符串的形式来表示，需要用样式对象来表示，样式的类名 要用驼峰命名的方式来表示







例子：



 var Hello = React.createClass({

 render:function(){

 var styleObj = {

 color:'red',

 fontSize:"44px"

 };

 return <div style={styleObj}>Hello {this.props.title} {this.props.name}</div> }

 })

 React.render(<Hello name="world" title="Mr"></Hello>,document.getElementById('contair));



![](/assets/react01.png)


