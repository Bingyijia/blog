## CSS案例练习总结与踩坑

1. display: inline-block;

```css
display: inline-block;
*display: inline;	/* 兼容ie块级元素显示问题 */
*zoom: 1;	/* 兼容ie块级元素显示问题 */
```

2. 关于清除浮动的方法

3. 关于表格
   有的时候用来分离行还是很好用的

   ```css
   table {
   	border-spacing: 0 10px;
   }
   tr {
   	background: #F00;
   }
   td {
   	padding: 10px;
   }
   tr :first-child {
   	border-radius: 5px 0 0 5px;
   }
   tr :last-child {
   	border-radius: 0 5px 5px 0;
   }
   ```

4. display的属性会破坏transition的动画效果，建议采用visiblity或者将宽高设置为0

5. input创建的按钮，需要主要，在mac下的chrome高度不起作用，需要添加语句

   -webkit-appearance: button;
   Firefox在私有属性里面额外设置了边框和留白，去掉即可。

   ```css
   button::-moz-focus-inner,
   input[type="button"]::-moz-focus-inner { border:none; padding:0; }
   ```

   css对按钮初始化：

   ```css
   .btn {
     	line-height:normal;
     	/* 解决ff下的padding */
   	-moz-padding-start:npx;
     	-moz-padding-end:npx;
   }
   ```

   ​



