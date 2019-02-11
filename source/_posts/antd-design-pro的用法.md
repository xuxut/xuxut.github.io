---
title: antd design pro的用法
date: 2019-02-11 9:43:21
tags: 
- react
categories:
- 前端框架 
---

## antd design pro的用法
namespace:
state:
effects: 
reducer:

<!-- more -->

拿回数据需要做的事情：
model中写namespace（全局的state的名字）/state（全局state中的属性）/effects（发起请求拿回数据，触发reducer改变state）/reducer（改变数据）/subscriptions（监控路由的变化,键盘输入，地理位置，返回一个函数）
mock中写这个请求对应的数据。在mock中对接收的数据，及返回的数据进行处理，再将最终的结果进行返回。
service中写用request发起这个请求，返回数据

```javascript
// 监控各种变化
subscriptions{
    setup({ dispatch,history }) {// 方法名可以随便定义，当监听有变化的时候就会依次执行这的变化
        
        return history.listen(({ pathname, search,query }) => {// 表示路由变化时，执行以下语句
            //console.log(`query = ${JSON.stringify(query)}`);
            
            if(pathname.indexOf('/list/table-list')>=0){
            
            const {page=1 , name,category:type}  = query;
            console.log('page = ',page, ' , name = ',name,',type =',type,);
            dispatch({type:'fetch',payload:{page,name,type,from:'unchecks','status':'EXAMINE'}});
            }else if(pathname.indexOf('/list/basic-list')>=0){
            const {page=1 , name,category:type}  = query;
            dispatch({type:'fetch',payload:{page,name,type,from:'tracks',}});
            }else if(pathname.indexOf('/list/card-list')>=0){
            const {page=1 , name,category:type}  = query;
            dispatch({type:'fetch',payload:{page,name,type,from:'rolleds','status':'FINISH'}});
            }
        });
    }

    setup({ dispatch, history }) {  // 这里的方法名可以随便命名，当监听有变化的时候就会依次执行这的变化
        window.onresize = () => {   //这里表示的当浏览器的页面的大小变化时就会触发里面的dispatch方法
            dispatch (type:"save")        
        }
},
```

1. `state`定义初始状态，
`reducer`定义相关函数，来修改状态。
```javascript
    export default {
        namespace: '名字',
        state: '初始状态' & {resultCode,resultMsg},
        reducer: {
            updateState(state, {payload})
        }
    }
```

2. `effects`描述当前发生的事情，是改变state的方法。若需要发起请求后，再改变状态，需要在effects定义的函数中发起请求。请求后返回的参数传递到put函数中，触发reducer，进而修改状态。
```javascript
    effects: {
        *fetchRes(_, { call, put}){
            const res = yield call(请求函数,[请求参数]);
            yield put({
                type:'updateState',
                payload: res
            })
        }
    }
```
> 请求函数放在`service`文件夹中，通过封装的request的函数向mock发起请求。
```javascript
    // mock/~.js
    const getNotice = (req, res) => {
        res.json([{
            uname:'haha',
            des:'你好'
        }])
    }
    export default {
        'GET /api/···': getNotices,
    };

    // service/api 
    export async function query(params){
        // 带参数的get请求
        return request(`/api/···${strinify(params)}`)
        // post请求
        return request('/api/···', {
            method: 'POST',
            body: params
        })
    }

    // utils/request
    export default function request(url, options){
        ~~~
    }
```

3. effects只是描述这件事情发生后的情况，并没有触发。所以在视图组件中，需要通过`dispatch`({
    type: ''
})
的方法，触发当前发生的事情，然后执行2中的顺序，待状态修改完成，会自动渲染到视图中形成新的数据状态。想要调用`dispatch`需要加上connect
```javascript
    componentDidMount(){
        const { dispatch } = this.props;
        dispatch({
            type:namespace+'/'+effects中的描述事件的方法
        })
    }
```

4. 重要的一点是，若想在视图中使用state，并更改state，也就是把model和组件串起来，需要用到`@connect`，是connect的装饰器，在外面包裹了一层state。
```javascript
    @connect(( profile, loading )=>({
        profile,
        loading: loading.effects[namespace+'/'+effects中的描述事件的方法]
    }))
```
connect里面的参数，是全局里面的state，将全局state注入到当前的组件中，当前组件用this.props取出。
loading属性是加载时调用此请求

> 总结：model的流程大致是 store(View -> effects -> reducer -> state -> view)  
关于发送请求的流程是 view -> sevice发起请求 -> mock返回数据 -> model更改状态 -> view

## 菜单栏 导航信息
左侧菜单栏文字信息修改：zh-CN.js（中文配置）
右上头部信息更改： componennts - GlobalHeader - RightContent.js
右上铃铛通知：ZComponent - MyNotifyIcon.js
面包屑导航：components - PageHeaderWrapper - index.js
            修改breadcrumbList的参数 [{title:'', name: '', href: ''}]

## 点击跳转 传参
```javascript
import {routerRedux} from 'dva/router'
    const {dispatch} = this.props;
    dispatch(routerRedux.push({pathname:'/profile/advanced', params/query/search: `参数`}));
```
有两种跳转方式：
第一种是 import { routerRedux } from 'dva/router';
第二种是 import  router  from 'umi/router';
`router.push({pathname:路径,params/query/search:参数) = dispatch(routerRedux.push({pathname:路径,params/query/search:参数}))`

```javascript
router.go(n) 
//这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 window.history.go(n)
router.push(location) 
//想要导航到不同的 URL，则使用 router.push 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。
router.replace(location) 
//跟 router.push 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。
```

`跳转前确认：有一个生命周期钩子：routerWillLeave(){}`

Browser History：地址形式https://xxx.com/ccc/cc/,  访问指定url
Hash History: 地址形式https://xxx.com/#/ccc/cc, 以 # 后面的路径进行处理

>react-router官方推荐使用browserHistory
首先 browserHistory 其实使用的是 HTML5 的 History API，浏览器提供相应的接口来修改浏览器的历史记录；而 hashHistory 是通过改变地址后面的 hash 来改变浏览器的历史记录；

History API 提供了 pushState() 和 replaceState() 方法来增加或替换历史记录。而 hash 没有相应的方法，所以并没有替换历史记录的功能。但 react-router 通过 polyfill 实现了此功能，具体实现没有看，好像是使用 sessionStorage。

另一个原因是 hash 部分并不会被浏览器发送到服务端，也就是说不管是请求 http://domain.com/index.html#foo 还是 http://domain.com/index.html#bar ，服务只知道请求了 index.html 并不知道 hash 部分的细节。而 History API 需要服务端支持，这样服务端能获取请求细节。
>

还有一个原因是因为有些应该会忽略 URL 中的 hash 部分，记得之前将 URL 使用微信分享时会丢失 hash 部分。

## 表格组件
进行筛选功能时，用onFilter过滤掉不需要的数据，不需要的数据返回false。对时间的顺序进行sorter排序，用两个参数，对需要排序的列进行大小加减。

## webpack打包`yarn run analyze`
```
yarn run analyze
```
查看项目打包各个模块的体积

## pureComponent和Component
pureComponent的相当于把Component中的shouldComponentUpdate()钩子函数进行了二次封装，进行了浅比较，当数据相同时就不会重新渲染数据了。
用原生的shouldComponentUpdate()通过返回return false阻止render渲染。一般与与第三方的immutable.js库连用，保证数据的唯一性。

## 高阶组件
高阶组件就是没有副作用的纯函数，且该函数接受一个组件作为参数，并返回一个新的组件。最主要的作用是重用组件逻辑。最好是能用父组件就用父组件，因为高阶组件是一种更hack的方法，更具灵活性。
区别：父组件可以在元素树上任意使用，高阶组件只应用于一个组件。
> 高阶组件的用途两点：属性代理和反向继承。通过调用不断将需要的参数传递进去，内部是函数式编程规范进行处理的。

## 关于&&
1 && 4：返回4
0 && 4：返回0
这两个执行语句并不直接返回true或false，如果两边的值为boolean类型，那就返回bool类型的值。

## 设置代理
config.js中设置代理
```javascript
    publicPath: "http://static.wanxiangzc.com/web/shinva/",
    base: "/shinva",
    history: "hash",//改为hash history的方式（默认browserHistory）
    hash: true,
    proxy: {
        "/api": {
        "target": "http://192.168.2.37:8080/api/",
        "changeOrigin": true,//（必须）实现跨域
        "pathRewrite": { "^/api" : "" }
    }
  }
```

疑难：父元素如何禁止子元素的事件？
事件捕获： addEventListener('事件名',funciton(e){e.stop..;e,prevent..}, true)

### react组件：父元素嵌套子元素，子元素路由不一样。
> 将子路由写在父路由的routes中，在父组件中用props获取children，组件中直接使用{children}即可。

## 404路由组件必须放在最下面
