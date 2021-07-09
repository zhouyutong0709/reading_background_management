# Element-UI 入门

## 基础用法

1.初始化项目
```
  vue create element-test
```

2.安装
```
  npm i element-ui -S
```

3.Vue 插件
```
// 在main.js引入
  import ElementUI from 'element-ui'

  Vue.use(ElementUI)
```

4.引用样式
```
  import 'element-ui/lib/theme-chalk/index.css'
```

5.element-ui 案例
```
<template>
  <div id="app">
    <el-button @click="show">点我</el-button>
  </div>
</template>

<script>
export default {
  name: 'app',
  methods: {
    show() {
      this.$message.success('Toast from element-ui')
    }
  }
}
</script>
```


## 按需加载

对项目进行打包：
```
npm run build
```

发现 vendors 库高达 789KB
```
-rw-r--r--  1 sam  staff   789518 10 27 20:40 chunk-vendors.cd583f5b.js
```

这是由于我们未使用按需加载，所以对 element-ui 进行全量打包的结果，按需加载的用法如下：


1.安装 babel-plugin-component
```
npm install babel-plugin-component -D
```

2.修改 babel.config.js：
```
{
  "presets": [["es2015", { "modules": false }]],
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
```

3.按需引入 Button 和 Message
```
import { Button, Message } from 'element-ui'

Vue.component(Button.name, Button)
Vue.prototype.$message = Message
```

重新构建，vendors 体积减小到 113KB

```
-rw-r--r--  1 sam  staff  113282 10 27 20:50 chunk-vendors.3d7d02b2.js
```


## 插件引用

可以通过 element 插件快速集成 element-ui
```
vue create element-test1
vue add element
```

## 表单的基本用法

1.el-form 容器，通过 model 绑定数据
2.el-form-item 容器，通过 label 绑定标签
3.表单组件通过 v-model 绑定 model 中的数据

```
<template>
  <div id="app">
    <el-form inline :model="data">
      <el-form-item label="审批人">
        <el-input v-model="data.user" placeholder="审批人"></el-input>
      </el-form-item>
      <el-form-item label="活动区域">
        <el-select v-model="data.region" placeholder="活动区域">
          <el-option label="区域一" value="shanghai"></el-option>
          <el-option label="区域二" value="beijing"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      data: {
        user: 'sam',
        region: '区域二'
      }
    }
  },
  methods: {
    onSubmit() {
      console.log(this.data)
    }
  }
}
</script>
```