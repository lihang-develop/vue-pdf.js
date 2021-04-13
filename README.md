# vue+pdf.js
#### vue+pdf.js实现chrome之外浏览器的pdf预览与打印

项目兼容IE并且需要pdf预览和打印功能，调研后选择了pdf.js
初次引入chrome浏览器正常使用，但ie会有报错，查看源码发现是箭头函数，
cdnboot寻找老版本，最后锁定了2版本的第一个版本，引入 IE正常使用

##### 使用方法

1. 公用方法

```javascript
// result为pdf文件流
export const print = (result) => {
  if (window.navigator && window.navigator.msSaveOrOpenBlob) {
    const src = window.URL.createObjectURL(new Blob([result], { type: 'application/pdf' }))
    window.open(`/pdf/web/viewer.html?file=${encodeURIComponent(src)}`)
  } else {
    const src = window.URL.createObjectURL(new Blob([result], { type: 'application/pdf' }))
    window.open(src)
    window.URL.revokeObjectURL(src)
  }
}
```

2. pdf.js是不支持跨域文件加载的 直接加载是不会成功的。会报 “file origin doesnot match viewer”错误。需要修改一下源码

   ```javascript
   // 讲下面代码注释掉	
   if (origin !== viewerOrigin && protocol !== 'blob:') {
     throw new Error('file origin does not match viewer\'s');
   }
   ```

   

