单行文本截断

```css
p {
	overflow: hidden;
	white-space: nowrap;  //多行状态不换行
	text-overflow: ellipsis;
}
```

多文本截断

```css
p {   //详情文本截断
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 2;
	overflow: hidden;
	text-overflow: ellipsis;
}
```

```
display: flex;	//t
flex-direction: column; //纵向排布
justify-content: space-between; //两端对齐
```

