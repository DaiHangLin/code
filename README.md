# code

```
const toUrlEncoded = obj => Object.keys(obj)
     .map(k => encodeURIComponent(k) + '=' + encodeURIComponent(obj[k])).join('&');
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
 
 +  业务 和 ui剥离
  
     举例：  当进入一个绘画的component后，我们需要在离开component后上传绘画的数据
     
        1. yield call(startPaintSaga) 

             =====  等待startPanitSaga 被 cancel 掉 ===== 原因可能是离开component或者点击某个按钮主动上传
        
        2. 等待 cancel 掉的action，
        
            ===== race { take 1. 离开component  take 2. 主动点击上传}
            
        3. yield call(uploadBitmap)
         
        
        4. swict (action) {
             case '离开component'
                yield call(....)
                break
              case '自动上传'
                 yield call(....)
                 break
        }
      分析
         
         这个例子中，saga是和业务逻辑相关的，而不是和UI绑定的，
         应该是saga根据接收的action，决定
         下一步需要处理的逻辑，或者下一步UI呈现的状态
         
    
+  put  vs  call
    
      put 
            
         put一个action，立马完成，执行下一步

      call 
           
            启动一个saga，会block住ui，后续代码会等待这个saga结束
            
+  race
      
      race代表同时等待多个action，哪个先到就先处理哪个action,
           
           比如同时等待 actionA, actionB
           if (captureActionA) {
             yield call(doActionAFunc)
             yield put(actionB) =>  这里一般都是不work的，因为此时的race并没有结束，
               这里dispatch的action会因为当前的saga还未结束，而无法被处理
           }
           
            



### 编程风格
 
 +  悲观性编程 => 考虑失败的场景，边界值，特殊输入，null等
 
 +  公用的组件或者逻辑，应该一开始就设计好，后期refactor的代价会更大


### redux

 + redux 中store里存的数据应该和服务端db保持一致， 应该是打散的数据，而非组织好的数据

 + 组织数据应该在selector中处理，ui负责selector 相应需要的数据
    
 

