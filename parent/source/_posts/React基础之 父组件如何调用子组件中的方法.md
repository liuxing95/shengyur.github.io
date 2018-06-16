title: React基础之 父组件调用子组件的方法
date: 2018/06/02
categories: 库/框架
tags:
  - React
---
直接上代码~~
```
export default class Parent extends Component {
     render() {
         return(
             <div>
                 <Child alias={this.doSth} /><br/>
                 <button onClick={this.click} >父组件click</button>
             </div>
         )
     }

     doSth = (ref) => {    //获取子组件的作用域
         this.anything = ref
     }

     click = (e) => {
         this.anything.myName()
     }

 }

 class Child extends Component {
     componentDidMount(){
         this.props.alias(this)  //子组件中的this作为参数传入
     }

     myName = () => alert('click me ')

     render() {
         return ('我是子组件')
     }
 }
```

简化下代码：
```

export default class Parent extends Component {
  render() {
      return(
          <div>
              <Child alias={(ref)=>{this.anything = ref}}/><br/>
              <button onClick={()=>{this.anything.myName()}} >父组件click</button>
          </div>
      )
  }
}

class Child extends Component {
  componentDidMount(){
      this.props.alias(this)
  }

  myName = () => alert('click me ')

  render() {
      return ('我是子组件')
  }
}
```


上面点击按钮,会弹出子组件的输出

原文：
https://blog.csdn.net/hesonggg/article/details/79373565
