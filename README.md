# vue-quill-editor-upload
A plug-in for uploading images to your server when you use vue-quill-editor.

富文本编辑器vue-quill-editor的辅助插件，用于上传图片到你的服务器

## 更新记录
当前版本：1.1

- 增加设置请求header的接口 headers()
- 增加input点击事件的接口  change()


## install
- npm
```
npm install vue-quill-editor-upload --save  
```
## use
You have to install vue-quill-editor first.

请确保您已安装了 vue-quill-editor
- import

```
import {quillRedefine} from 'vue-quill-editor-upload'
```
- demo

### first
  
you must to do: ```:options="editorOption"``` to bound Parameters

你必须绑定option ```:options="editorOption"``` 
```vue
<template>
  <!-- bidirectional data binding（双向数据绑定） -->
  <quill-editor 
                :options="editorOption">
  </quill-editor>
</template>

```

### second
 
 return editorOption  
 
 必须在return 中书写editorOPtion 并且设置默认为空对象

```vue
 data () {
      return {
        content: '',
        editorOption: {}  // 必须初始化为对象 init  to Object
      }
    }
```
### three 
  
  init  in  created
 
 在created生命周期中生成实际数据
 ```
 created () {
       this.editorOption = quillRedefine(
         {
           // 图片上传的设置
           uplpadConfig: {
             action:  '',  // 必填参数 图片上传地址
             // 必选参数  res是一个函数，函数接收的response为上传成功时服务器返回的数据
             // 你必须把返回的数据中所包含的图片地址 return 回去
             res: (respnse) => {
               return respnse.info  // 这里切记要return回你的图片地址
             }
           }
         }
       )
      // console.log(this.editorOption)
     }
 ```
### 注意事项 （matters need attention）
由于不同的用户的服务器返回的数据格式不尽相同

因此
在uploadConfig中，你必须如下操作
```vue
 // 你必须把返回的数据中所包含的图片地址 return 回去
 res: (respnse) => {
    return respnse.info  // 这里切记要return回你的图片地址
 }
```
比如你的服务器返回的成功数据为
```vue
{
code: 200,
starus: true,
result: {
    img: 'http://placehold.it/100x100' // 服务器返回的数据中的图片的地址
 }
}
```
那么你应该在参数中写为：
```vue
 // 你必须把返回的数据中所包含的图片地址 return 回去
 res: (respnse) => {
    return respnse.result.img  // 这里切记要return回你的图片地址
 }
```


example
完整用例
```vue
<template>
  <!-- bidirectional data binding（双向数据绑定） -->
  <quill-editor v-model="content"
                ref="myQuillEditor"
                :options="editorOption">
  </quill-editor>
</template>

<script>
  import {quillRedefine} from 'vue-quill-editor-upload'
  import {quillEditor} from 'vue-quill-editor'
  export default {
    components: {quillEditor, quillRedefine},
    data () {
      return {
        content: '',
        editorOption: {}  // 必须初始化为对象 init  to Object
      }
    },
    created () {
      this.editorOption = quillRedefine(
        {
          // 图片上传的设置
          uplpadConfig: {
            action: '',  // 必填参数 图片上传地址
            // 必选参数  res是一个函数，函数接收的response为上传成功时服务器返回的数据
            // 你必须把返回的数据中所包含的图片地址 return 回去
            res: (respnse) => {
              return respnse.info
            },
            methods: 'POST',  // 可选参数 图片上传方式  默认为post
            token: sessionStorage.token,  // 可选参数 如果需要token验证，假设你的token有存放在sessionStorage
            name: 'img',  // 可选参数 文件的参数名 默认为img
            size: 500,  // 可选参数   图片限制大小，单位为Kb, 1M = 1024Kb
            accept: 'image/png, image/gif, image/jpeg, image/bmp, image/x-icon',  // 可选参数 可上传的图片格式
            // input点击事件  formData是提交的表单实体
            change: (formData) => { 
            },
            // 设置请求头 xhr: 异步请求， formData: 表单对象
            headers: (xhr, formData) => {
                // xhr.setRequestHeader('myHeader','myValue');
                // formData.append('token', '1234')
            },
            // start: function (){}
            start: () => {
            },  // 可选参数 接收一个函数 开始上传数据时会触发
            end: () => {
            },  // 可选参数 接收一个函数 上传数据完成（成功或者失败）时会触发
            success: () => {
            },  // 可选参数 接收一个函数 上传数据成功时会触发
            error: () => {
            }  // 可选参数 接收一个函数 上传数据中断时会触发
          },
          // 以下所有设置都和vue-quill-editor本身所对应
          placeholder: '',  // 可选参数 富文本框内的提示语
          theme: '',  // 可选参数 富文本编辑器的风格
          toolOptions: [],  // 可选参数  选择工具栏的需要哪些功能  默认是全部
          handlers: {}  // 可选参数 重定义的事件，比如link等事件
        }
      )
      console.log(this.editorOption)
    }
  }
</script>

```
## quillRedefine 可接收的所有参数(all params)
```vue
{
          // 图片上传的设置
          uplpadConfig: {
            action: '',  // 必填参数 图片上传地址
            // 必选参数  res是一个函数，函数接收的response为上传成功时服务器返回的数据
            // 你必须把返回的数据中所包含的图片地址 return 回去
            res: (respnse) => {
              return respnse.info
            },
            methods: 'POST',  // 可选参数 图片上传方式  默认为post
            token: sessionStorage.token,  // 可选参数 如果需要token验证，假设你的token有存放在sessionStorage
            name: 'img',  // 可选参数 文件的参数名 默认为img
            size: 500,  // 可选参数   图片限制大小，单位为Kb, 1M = 1024Kb
            accept: 'image/png, image/gif, image/jpeg, image/bmp, image/x-icon',  // 可选参数 可上传的图片格式
            // input点击事件  formData是提交的表单实体
            change: (formData) => { 
            },
            // 设置请求头 xhr: 异步请求， formData: 表单对象
            headers: (xhr, formData) => {
                // xhr.setRequestHeader('myHeader','myValue');
                // formData.append('token', '1234')
            },
            start: () => {
            },  // 可选参数 接收一个函数 开始上传数据时会触发
            end: () => {
            },  // 可选参数 接收一个函数 上传数据完成（成功或者失败）时会触发
            success: () => {
            },  // 可选参数 接收一个函数 上传数据成功时会触发
            error: () => {
            }  // 可选参数 接收一个函数 上传数据中断时会触发
          },
          // 以下所有设置都和vue-quill-editor本身所对应
          placeholder: '',  // 可选参数 富文本框内的提示语
          theme: '',  // 可选参数 富文本编辑器的风格
          toolOptions: [],  // 可选参数  选择工具栏的需要哪些功能  默认是全部
          handlers: {}  // 可选参数 重定义的事件，比如link等事件
}
```















