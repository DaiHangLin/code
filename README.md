# code

```
const toUrlEncoded = obj => Object.keys(obj).map(k => encodeURIComponent(k) + '=' + encodeURIComponent(obj[k])).join('&');
```

```
https://commonsware.com/Android/previews/screenshots-and-screen-recordings
```


### 性能影响
 + 批量处理数据, 比如 调用100个api，和调1个api处理100个处理性能上是相差很多的

### saga
 + 业务逻辑应该和ui抽离，UI层需要做到不处理具体的业务逻辑

