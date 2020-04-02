# Udemy: Component Lifecycle

## Lifecycle of Class-based Components

![Lifecycle of Class-based Components](/mdImg/2020-03-26-09-25-06.png)
![Overview](/mdImg/lifecycleClass.png)
![Update](/mdImg/lifecycleUpdate.png)

1. 组件初始化  
   原因  
   组件第一次建立  
   触发  
   componentWillMount -> render -> componentDidMount
2. 组件更新 — props change  
   原因  
   props 发生改变  
   触发  
   componentWillReceiveProps -> shouldComponentUpdate -> componentWillUpdate -> componentDidUpdate
3. 组件更新 — state change  
   原因  
   this.setState()使 state 发生改变  
   触发  
   shoudlComponentUpdate -> componentWillUpdate -> componentDidUpdate
4. 组件卸载或销毁  
   原因  
   组件卸载或销毁  
   触发  
   componentWillUnmount
