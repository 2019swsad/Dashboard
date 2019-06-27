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
wxml标签可以单独出现的情况，尽量单独出现，如。

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

2.注释规范
除组件外的其他块级元素，均需注释出其功能，并在其上下空出一行与其他代码进行区分。

  
    <view>...</view>
    
    //导航栏
    <view>...</view>
    
    <view>...</view>

### WCSS规范
1.    CSS规范 
在开发过程中rpx和px均可能用到，如通常情况下间距使用rpx，字体大小和边框等使用px，开发者根据实际情况而定。

    width: 100rpx;
    font-size: 14px;

CSS代码需有明显的代码缩进。每一个样式类之间空出一行。

    .v-tag{
      width: 100%;
    }
    
    .v-container{
      width: 100%;
    }
    
    

尽量使用简写属性，并且同一属性放置在一起，避免散乱。

/**使用简写属性**/

    .v-image{
      margin: 0 auto;
    }

/**同一属性放在一块**/

    .v-tag{
      margin-left: 10rpx;
      margin-right: 10rpx
    }


采用flex进行布局，禁止使用float以及vertical-align。

    .container{
      disaplay: flex;
      flex-dirextion: row
    }



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

