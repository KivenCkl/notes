# vue

Q: 绑定键盘事件不生效

A: 原因在于事件绑定在元素上面，如果焦点不在按钮上，就无法响应这个事件。加上 `.native` 覆盖原有封装的键盘事件即可，例如

```vue
<el-input v-model="searchParmas.gameName" placeholder="游戏名称" class="w120" @keyup.native="getGameList(searchParmas.gameName)"></el-input>
```