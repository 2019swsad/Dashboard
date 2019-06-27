# Code rules
## 前端
参考微信小程序开发规范文档

### 目录规范
1、组件文件
所有组件相关文件统一放在components目录下。

2、图片文件
项目图片文件放置于根目录的images文件夹下，组件独有的图片放在当前组件images目录下

3、模型文件
模型文件主要用于编写各类业务模型。项目模型文件放置于根目录的models文件夹下，组件相关模型放置于components目录下的models文件夹中。



4、行为文件
行为文件放在所引用的组件目录下。

### WXML规范
1.   WXML规范
wxml标签可以单独出现的情况，尽量单独出现，如<input />。

    <input />

控制每行HTML的代码数量在50个字符以内，方便阅读浏览，多余的代码进行换行处理，标签所带属性每个属性间进行换行。

    <v-music
      wx:if="{{classic.type===200}}"
      img="{{classic.img}}"
      content="{{classic.content}}"
    >
    </v-music>

合理展现分离内容，不要使用内联样式。

    //推荐使用
    <image class="tag"></image>

2.     注释规范
除组件外的其他块级元素，均需注释出其功能，并在其上下空出一行与其他代码进行区分。

  
    <view>...</view>
    
    //导航栏
    <view>...</view>
    
    <view>...</view>



## 后端
本次后端采用自定ESlint规范,基本要求看齐Google Style  
### Variable Name
如变量命名使用驼峰法,如```objectOfUser```

### Space and operator
运算符前后有空格

### Indent
两个空格

### End of code
没有分号,除非一行多句  
左花括号放第一行结尾  
右花括号新建一行  

### Object 
属性无括号  
字符串用单引号

### Long expression
采用```.```操作符后换行

### Load modules
采用```require```

