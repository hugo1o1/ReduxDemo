# ReduxDemo
### 步骤：

1. `create-react-app`新建一个 react +redux demo。

2. 安装 redux 框架库

   ```npm
   npm install redux -save-dev
   ```

3. 在 src 底下新建 store 文件夹 用于管理数据流， 新建index.js 创建一个store 

   ```javascript
   import {createStore } from 'redux';
   import reducer from './reducer';
   
   const store  = createStore(reducer);// 传入 reducer
   
   export default store;
   
   ```

4. 新建 reducer.js 处理 state 和action, 当action 变多之后， 可以新建action.js 保存所有的actions.  reducer 是一个纯函数。（相同的输入保证相同的输出， 不会产生副作用，不会改变被传入的数据或者其他数据）

   

   ```javascript
   // 相当于一个笔记本
   const defaultState = {
      inputValue:'',
      list:[1,2,3] 
   };
   
   export default (state = defaultState, action)=> {
       switch (action.type) {
           case 'change_list':
               {
                   const newState = JSON.parse(JSON.stringify(state));
                   newState.list.push(action.value);
                   return newState;
               }
               case 'delete_list':
               {
                   const newState = JSON.parse(JSON.stringify(state)); 
                   newState.list.splice(newState.list.findIndex(a=>a.value ===action.value), 1)
                   return newState;
               }
       
           default:
               break;
       }
       
       return state;
   
   }
   
   ```

   5.以一个Todo list 为例。

   ```javascript
   import React, {Component} from 'react';
   import { Input } from 'antd';
   import 'antd/dist/antd.css';
   import { List, Typography } from 'antd';
   import store from './store';
   const Search = Input.Search;
   
   export default class Todo extends Component{
       constructor(props){
           super(props);
           
          this.state = store.getState();// 获取 store 中的数据
       
          store.subscribe(this.handleChange); // 发生改变触发 handlechange 函数
          
       }
       handleChange=()=>{
           this.setState(store.getState())
       }
       handleSearch=(value)=>{
           const action ={
               type:'change_list',
               value:value
           }
          store.dispatch(action);
       }
       handledelete=(value)=>{
           const action ={
               type:'delete_list',
               value:value
           }
           store.dispatch(action);
       }
   
       render(){
           return(<div className='TOdolist' >
              <Search
         placeholder="input add text"
         enterButton="添加"
         size="large"
         onSearch={value => this.handleSearch(value)}
       />
        <Search
         placeholder="input delet text"
         enterButton="删除"
         size="large"
         onSearch={value => this.handledelete(value)}
       />
        <List
        style={{ width: 300 }}
         size="small"
         header={<div>Header</div>}
         footer={<div>Footer</div>}
         bordered
         dataSource={this.state.list}
         renderItem={item => (<List.Item>{item}</List.Item>)}
       />
           </div>)
       }
   }
   ```

   











#### 注意

1. 在reducer 中不能直接改变 state ,可以 深拷贝之后， 更新返回。

2. store 会对比reducer 返回的newstate 对比，发生改变就会触发  store.subscribe()

3. store.subscribe() 中的参数为，需要调用的方法。

4. store 只能创建一次

   
