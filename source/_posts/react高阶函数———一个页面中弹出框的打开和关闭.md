---
title: react高阶函数——页面中弹出框的打开和关闭
date: 2018-12-27 18:21:44
tags: 
- 高阶组件HOC
categories:
- react
blogexcerpt: react的高阶组件就是一个函数，接收组件作为参数，然后再返回一个新的组件。它是一个纯函数，注入组件并返回组件，对这个高阶组件页面中的内容无任何副作用，只是为这个页面增加了某些状态或功能，说白了就是在这个页面外面包了一层。
---

>前言：在最近的项目中，多个页面都有弹出框的功能，所以我写了一个高阶组件，减少了代码量，顺便学习了一下高阶组件。

1. 在src文件夹下建立名为hoc的子文件夹，以后的高阶组件都写在这个文件夹里面
```javascript
import React, { PureComponent } from 'react';
import { connect } from 'dva';
import { notification } from 'antd';

const ModalSubmitHoc = WrappedComponent => {
  return connect(({ }) => ({

  }))(
    class extends PureComponent {
      state={
        modalParams:{
          visible: false,
          visibleObject: {}
        },
        modalMethods:{

        },
      }

      constructor(){
        super()
        this.state={
          modalParams:{
            visible: false,
            visibleObject: {}
          },
          modalMethods: {
            openModal:this.openModal,
            closeModal:this.closeModal,
            openOneModal: this.openOneModal,
            closeOneModal: this.closeOneModal,
            submitModal:this.submitModal,
            getModalVisibleObject: this.getModalVisibleObject
          }
        }
      }

      // 生成多少个弹窗的visible
      getModalVisibleObject = (visibleArr) => {
        let c = {}
        visibleArr.map((_)=>{
          c[_] = false
        })
        this.setState({
          visibleObject: c
        })
      }

      // 页面内只有一个弹窗
      openModal = (visible=true) => {
        this.setState({modalParams:{visible}})
      }
      closeModal = (visible=false) => {
        this.setState({modalParams:{visible}})
      }

      // 页面内有多个弹窗
      openOneModal = (visibleName) => {
        this.setState({
          modalParams: {
            visibleObject: {
              [visibleName]: true
            }
          }
        })
      }
      closeOneModal = (visibleName) => {
        this.setState({
          modalParams: {
            visibleObject: {
              [visibleName]: false
            }
          }
        })
      }

      resetFields = ()  => {
        const { form } = this.props;
        form.resetFields()
      }

      /**
       * 提交modal
       * @param {String} dispatchType dispatch的type
       * @param {Object} values 提交的参数
       * @param {string} visibleName 提交的弹窗名字
       */
      submitModal = (dispatchType, values, visibleName) => {
        const { dispatch } = this.props;
        dispatch({
          type:`${dispatchType}`,
          payload: {
            ...values,
          },
          callback: (code, message, description) => {
            if(code){
              notification.success({message})
              this.resetFields()
              if(visibleName){
               this.closeOneModal(visibleName)
              }else {
               this.closeModal()
              }
            }else {
              notification.error({message, description})
            }
          }
        })
      }

      render() {
        const newProps = {...this.props,...this.state};

        return <WrappedComponent {...newProps} />;
      }
    }
  );
};
export default ModalSubmitHoc;

```

2. 在页面中使用
页面通过获取props来使用高阶组件的参数和方法
```javascript
import ModalSubmitHoc from '@/hoc/ModalSubmitHoc'

@ModalSubmitHoc
export default class 组件 extends PureComponent{

}
```


