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
 
 +  单一数据来源 =>  不在store中存储两份一样的数据，同样的数据只保留一份，其他应该保留对这份数据的索引


### 编程风格
 
 +  悲观性编程 => 考虑失败的场景，边界值，特殊输入，null等
 
 +  公用的组件或者逻辑，应该一开始就设计好，后期refactor的代价会更大


### redux

 + redux 中store里存的数据应该和服务端db保持一致， 应该是打散的数据，而非组织好的数据

 + 组织数据应该在selector中处理，ui负责selector 相应需要的数据
    
 

