# ant-design-table-drag
**基于antDesign实现vue2、vue3的表格、表头拖拽功能**

近期项目开发后台，由于表头数据比较多，需要实现表格表头拖拽功能，项目应用的UI是antDesign，查找文档目前vue版本的antDesign好像并不支持这一功能，所以自己写了一个小demo，希望对大家有所帮助。

**项目中使用**

~~~javascript
customDragCol({
    columns: props.columns, // <a-table :columns="columns">
    dragAreaWidth: 20 // 拖拽区域的范围 默认20个像素（数字类型）
    minCellWidth: 30  // 每个表头单元格的最小宽度 默认30个像素（数字类型）
    mode: 'line'      // 拖拽模式 'default' or 'line',不传就是默认default
    lineStyle：{      // 参考线样式，只有在mode:'line'的模式下才有效
     	background: '#ccc',
        width: '1px'
	}     
})
~~~

**“default”模式下的样式预览**

![1b191549ea6c4d0a9bc8276a970dd1d.png](https://i.loli.net/2021/09/18/N9Y8WILaSf2Hxvi.png)

**“line”模式下的样式预览**

![7623f8f763af09a8f9663e207336d94.png](https://i.loli.net/2021/09/18/X5ZS8zAhF6i4Rrg.png)



**customDragCol函数的默认值**

```javascript
dragAreaWidth: 20  // 注意：这里是数字类型
minCellWidth: 30   // 注意：这里是数字类型
mode: 'default'
lineStyle：{
    background: '#ccc',
    width: '1px'
}  
```

**注意点：**

​		属性“columns”是必传项目，不传会报错。属性“columns”也就是<a-table :columns="columns">中的“columns”属性，columns中必须有“width”属性，width属性必须是***数字***类型。

​		这里的原理就是监听鼠标移动事件判断距离，进而修改<a-table :columns="columns">中“columns”中的“width”属性。

**下面附上简单的使用教程：**

~~~javascript
<template>
    <a-table
      :columns="tableHead"
      :bordered="customDragCol"
	... 省去了其他属性，'dataSource'、'row-selectio'之类的
    >
    	... 省去一些插槽
    </a-table>
</template>
<script>
import { defineComponent,  onUpdated } from 'vue';
import customDragCol from './hooks'; // 这里就是我们的函数
import { Table } from 'ant-design-vue';
export default defineComponent({
  name: 'standardTable',
  props: {
    tableHead: Array,
    customDragCol: { // 上层组件传递过来的是否要进行表头拖拽
      type: Boolean,
      default: false
    }
  },
  setup(props) {
    onUpdated(_ => {
      // 只有columns是必传的，其他的属性不传就用默认值了
      props.customDragCol && customDragCol({ columns: props.tableHead });
    });
  }
});
~~~

以上代码，我的目录结构：

![image.png](https://i.loli.net/2021/09/18/TJCF63VPD2yurXH.png)
