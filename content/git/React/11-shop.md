---
title:小案例（购物车）
---

## 小案例（购物车）

### 在state中写入一个数组data的商品信息，组件化

```
import React from 'react'

class Shop extends React.Component{
  constructor(){
    super()
    this.state = {
      data:[
        {name:'烟',num:10,price:50},
        {name:'酒',num:10,price:100},
        {name:'糖',num:10,price:20},
        {name:'茶',num:10,price:500}
      ]
    };
  }
  render(){
    return(
      <div>
        Shop
      </div>    
    )
  }
}
export default Shop
```

### 数量的加减操作

```
  handleNum(index,num){
//传入两个参数index方便找到修改的值，num传入修改值
    let data = this.state.data
    data[index].num = (data[index].num + num)<0 ? 0 : data[index].num + num
//因为只能修改state对象下的属性，不能直接修改它属性下的东西，
//所以只能声明一个变量来放入修改的属性
    this.setState({
      data:data
    })
  }

  <input type='button' value='+1' onClick={this.handleNum.bind(this,index,+1)} />
  &nbsp;&nbsp;&nbsp;{item.num}&nbsp;&nbsp;&nbsp;
  <input type='button' value='-1' onClick={this.handleNum.bind(this,index,-1)} />
```

小贴士：`&nbsp;`（Html内空格会被解析为字符串，用来代替空格）

### 添加删除按钮

```
利用splice方法删除index对应条数
handleDel(index){
  let data = this.state.data
  data.splice(index,1)
  this.setState({
    data:data
  })
}
//删除按钮
<input type='button' value='删除' 
  onClick={this.handleDel.bind(this,index)} />
```

利用`filter`来删除

```
handleDel(i){
  this.setState({
    data:this.state.data.filter((item,index) => index!=i)
  })
}
```

`filter`方法有三个参数[元素的值,元素的索引,被遍历的数组]，函数的返回值是一个条件，符合条件的值会被放在一个新数组内。

### 合计计算

```
//写在render内，利用forEach计算出所有商品价值
let sum = 0
    this.state.data.forEach(item => sum +=item.num*item.price)
//写在return内
<span>总计：{sum}</span>
```

### 将数据map到render里

```
<table style={{width:'100%',textAlign:'center'}}>
  <thead>
    <tr>
      <th>名称</th>
      <th>单价</th>
      <th>数量</th>
      <th>小计</th>
      <th>删除</th>
    </tr>
  </thead>
  <tbody>
    {this.state.data.map((item,index) => 
      <tr key={Math.random()}>
        <td>{item.name} </td>
        <td><input type='button' value='+1' onClick={this.handleNum.bind(this,index,+1)} />
        &nbsp;&nbsp;&nbsp;{item.num}&nbsp;&nbsp;&nbsp;
        <input type='button' value='-1' onClick={this.handleNum.bind(this,index,-1)} /></td>
        <td>{item.price} </td>
        <td>{item.num*item.price} </td>
        <td><input type='button' value='删除' onClick={this.handleDel.bind(this,index)} /></td>
      </tr>
      )}
  </tbody>
</table>
```

### 完成后的购物车

```
import React from 'react'

class Shop extends React.Component{
  constructor(){
    super()
    this.state = {
      data:[
        {name:'烟',num:10,price:50},
        {name:'酒',num:10,price:100},
        {name:'糖',num:10,price:20},
        {name:'茶',num:10,price:500}
      ]
    };
  }
  handleNum(index,num){
    let data = this.state.data
    data[index].num = (data[index].num + num)<0 ? 0 : data[index].num + num
    this.setState({
      data:data
    })
  }
  handleDel(index){
    let i = index
    // let data = this.state.data
    // data.splice(index,1)
    this.setState({
      data:this.state.data.filter((item,index) => index!==i)
    })
    // console.log(this.state.data[index])
  }
  render(){
    let sum = 0
    this.state.data.forEach(item => sum +=item.num*item.price)
    return(
      <div>
        {
          this.state.data.length===0 ? '购物车已经空空的' :
          <div>
            <table style={{width:'100%',textAlign:'center'}}>
            <thead>
              <tr>
                <th>名称</th>
                <th>单价</th>
                <th>数量</th>
                <th>小计</th>
                <th>删除</th>
              </tr>
            </thead>
            <tbody>
            {this.state.data.map((item,index) => 
              <tr key={Math.random()}>
                <td>{item.name} </td>
                <td><input type='button' value='+1' onClick={this.handleNum.bind(this,index,+1)} />&nbsp;&nbsp;&nbsp;{item.num}&nbsp;&nbsp;&nbsp;<input type='button' value='-1' onClick={this.handleNum.bind(this,index,-1)} /></td>
                <td>{item.price} </td>
                <td>{item.num*item.price} </td>
                <td><input type='button' value='删除' onClick={this.handleDel.bind(this,index)} /></td>
              </tr>
              )}
            </tbody>
            </table>
            <span>总计：{sum}</span>
          </div>    
        } 
      </div>         
    )
  }
}
export default Shop
```
