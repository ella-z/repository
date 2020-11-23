# el-scrollbar
- el-scrollbar是elementUI的滚动条组件，在官方文档中没有写出，但是在源码中存在，所以我们可以直接使用。
   - 在使用时要设置外层容器高度。并且要设置el-scrollbar 的高度为100% 
```
<template>
  <div style="height:600px;">
    <el-scrollbar style="height:100%">
        <div style="width:700px;height:700px;border:solid;" >
          ....... blabla.....
        </div>
    </el-scrollbar>
  </div>
</template>
```
