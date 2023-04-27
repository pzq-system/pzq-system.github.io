## 创建Vue 项目

1. npm create vite@latest
2. npm install
3. npm run dev

## 安装 npm install sass --save

## 安装 npm install element-plus --save

```tsx
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

createApp(App)
.use(ElementPlus)
.mount('#app')

```

## 安装路由 npm install vue-router@next --save

1. src目录下新建文件夹router,文件夹在新建路由文件index.ts

```tsx
import { createRouter, createWebHistory } from "vue-router";
import LoginPage from '../view/index/LoginPage.vue'

const router = createRouter({
    history :createWebHistory(),
    routes:[
        {path:"/login",component:LoginPage}
    ]
})
export default router;
```

1. 在项目中导入

```tsx
import router from './router/index'
app.use(router)
```

1. 在APP.vue里面加上路由标签

```
  <router-view></router-view>
```

## 安装状态管理 npm install vuex@next --save

```
import { createStore } from 'vuex'
const store = createStore({
    //状态变量
    state(){
        return{
            Token:localStorage["token"],
            NickName:localStorage["nickname"]
        }
    },
    //方法
    mutations:{
        SettingNicName(state:any,NickName:string){
            state.NickName = NickName
        },
        SettingToken(state:any,Token:string){
            state.Token = Token
        }
    }
})
export default store;
```

```tsx
import stota from 'vuex'
app.use(stota)
```

## 使用echarts 图表 使用

官网：https://echarts.apache.org/

```

```

