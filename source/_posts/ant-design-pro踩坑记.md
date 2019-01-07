---
title: ant-design-pro踩坑记
date: 2018-12-17 11:31:25
tags: 
    - antd
    - antd-pro
    - dva
    - roadhog
category:
    - antd
    - antd-pro
    - dva
    - roadhog
---


# 1. 注册model
在引入新页面时，新建model需要注册，这个时候需要在commen中的router页面进行注册，否则无法使用dispatch进行后续操作。

# 2.在ant-design-pro中解决跨域办法
需要在配置文件中(.webpackrc)加入如下代码
```
"proxy": {
      "/api": {
        "target": "http://xxx:xx/",
        "changeOrigin": true,
        "pathRewrite": { "^/api" : "" }
      }
},
```

需要注意的是此处不是将/api/*代理到正式请求/api/*中，（例如请求/api/users则会代理到http://xxx:xx/users）
如果需要多次代理且需要代理到不同的服务器则可以在配置文件中进行如下配置

```
"proxy": {
      "/test": {
        "target": "http://xxx:xx/",
        "changeOrigin": true,
        "pathRewrite": { "^/test" : "" }
      },
      "/cross": {
        "target": "http://jsonplaceholder.typicode.com",
        "changeOrigin": true,
        "pathRewrite": {"^/cross": ""}
      } // 此处有一点需要注意，不能在最后一个代理对象后面加逗号，否则会报错！！！
  },
```

之所以不在pro中使用```/api```进行代理是因为pro中的其他接口是基于此的，所以如果对其进行代理会导致页面报错从而有可能误导开发人员。

# 3.权限控制
在pro里面权限控制是通过Authorized来进行控制的，通过对本地存储的antd-pro-authority的值进行判断，在登陆时进行权限的刷新，所以要将权限控制精细到页面元素级别的话需要通过对获取到的值进行判断，在实际项目开发过程中，有可能该值不会存储在本地而是通过服务获取到，以此来提高安全性。

##### 在pro中控制权限是通过角色来进行控制的，不同的角色拥有不同的权限，在```src/router```中来对角色进行权限的判定。例如登陆页面是guest，而内容页面则是admin和user。新增角色也是在此处进行添加。
##### 将权限控制精细到按钮级别，示例代码如下

```
import RenderAuthorized from 'ant-design-pro/lib/Authorized';
import { Alert } from 'antd';

const Authorized = RenderAuthorized('user');
const noMatch = <Alert message="No permission." type="error" showIcon />;

ReactDOM.render(
  <Authorized authority={['user', 'admin']} noMatch={noMatch}>
    <Alert message="Use Array as a parameter passed!" type="success" showIcon />
  </Authorized>,
  mountNode,
);
```

需注意的是引入的是组件而不是工具类里的Authorized，两者用法不同。
# 4.  在model中怎么同时发起多次请求，因为yield将异步请求转为同步请求了，所以请求会按照同步顺序依次执行，使请求时间延长
错误写法

```
// effects将按顺序执行
const response = yield call(fetch, '/users');
const res = yield call(fetch, '/roles');
```

正确写法

```
// effects将会同步执行
const [response, res] = yield [
  call(fetch, '/users'),
  call(fetch, '/roles'),
]
```

##### 在有一些敏感数据不适合放在浏览器缓存中时，要怎么做
##### 还有一点值得考虑的是如何对权限进行分配。
# 5. 在项目中如果需要通过类名来选择某些元素，最好直接在其属性上添加，不要使用```styles.class```这种方式，因为其在渲染后类名会发生一定的变化，导致不发通过类名来定位元素