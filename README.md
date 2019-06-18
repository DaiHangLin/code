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
 + 多条业务线 => 同一个业务 => 处理不同业务 (避免进行传值的if else 操作)
   
    yield call(a) => yield call(common) => yield call(aNext)
    
    yield call(b) => yield call(common) => yield call(bNext)
    
    yield call(c) => yield call(common) => yield call(cNext)
    
 +  使用函数来处理UI，使用新特性hook，但是hook应该只和ui的状态有关，不应该处理过多的业务逻辑
    
 

