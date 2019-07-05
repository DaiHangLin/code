# code

```
const toUrlEncoded = obj => Object.keys(obj)
     .map(k => encodeURIComponent(k) + '=' + encodeURIComponent(obj[k])).join('&');
```

### 性能影响
 + 批量处理数据, 比如 调用100个api，和调1个api处理100个处理性能上是相差很多的
 
 + 在angular/react都是基于数据是单向数据流模式
 
   数据检测变化是有周期性的，会在一个周期结束后去检测数据是否发送改变
   所以修改数据和检测数据其实是异步的，这里使用里第三方的库比如ngZone等，
   数据一直在改变，但是检测周期是固定的

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
           
+ 启动和关闭saga

      因为saga如果挂了，会导致整个app无响应，所以启动saga 和 关闭saga应该成对出现，并且按虚启动，在用户退出登录或者某些操作时应该能够关闭不必要的saga

+  saga中对于call的理解

      saga中的call会block住流程，只有call中的saga完成了才会继续执行下一步，
      所以对于call可以用来做很多事情，
      
      比如 可以再tryCatchFinally处理正常结束和被cancel后应该做的事情

      ```
      finally {
         if (yield cancelled) {
            // 被cancel掉了，说明有新的业务分支产生
         } else {
            // 业务正常结束
         }
      }
      ```



### 编程风格
 
 +  悲观性编程 => 考虑失败的场景，边界值，特殊输入，null等
 
 +  公用的组件或者逻辑，应该一开始就设计好，后期refactor的代价会更大
 
 +  每个function或者说函数，都应该遵循单一指责原则，在项目发展阶段
     不论是复用还是保持项目中细节上的逻辑保持一致都有好处，而且减少reflactor的机会，减少出错的概率

 +  注册和销毁应该成对出现，最好在注册前就销毁一次，防止多次注册

 +  公用的组件有时候会有一些方法在不同的组件中行为不一致

         这种情况不应该在这个公用组件中处理这些行为，应该由调用的组件各自维护

 +  老生常谈 状态的一致性，
         
         修改某个状态后，一般都需要在某个时间点恢复至默认状态，比如添加一个flag为true
         后，一定是需要在某个场景下重置为false，给某个数组添加数据后，肯定需要清空

### redux

 + redux 中store里存的数据应该和服务端db保持一致， 应该是打散的数据，而非组织好的数据

 + 组织数据应该在selector中处理，ui负责selector 相应需要的数据

 + redux 中存储的数据不应该是DTO级别的数据，应该是各自待组合数据的mapping关系，DTO应   该由selector来完成



### react

  + 复杂的UI设计，尽量使用组合，而且嵌套
  
  + 每个组件应该尽量的独立，绑定各自需要的selector，react会根据检测selector中的数据是否改变做出更新
   
  +  对于不和store关联的纯函数
     可以从父组件传递数据

  +  react hook

      + useState
            react里面所有的值应该都是不可变，比如
            ```
            const [count, setCount] = useState(0)
            这里的count是不可变的值，只能通过setCount来修改
            ```
      + useEffect

            比class component更加灵活的“监听”

            1. 比如注册和取消注册，在class component里需要在两个相应的生命周期函数里执行，
            使用useEffect则更加灵活
            ```
             useEffect(() => {
               addListener()
               return () => {
                  removeListener
               }
             }, [])
            ```
            2. 上个例子中的 [] => 表示这个effect只执行一次

            3. useEffect => 会更新当前的所有对象（应该生成新的对象），
            所以在useEffect的listener里面调用useState可能是不工作的，
            因为这个时候的useState是更新前旧的引用，需要将useState作为
            参数传递listenr
            ```
             useEffect(() => {
               addListener()
               return () => {
                  removeListener
               }
             }, [setCount])
            ```

     
     
### git

   + 分布式版本管理系统，什么是分布式？项目版本不论在任何机器上都有完整的一份数据
   
   + git biscet
   
         二分查找的方式定位出问题的commit
         1. git biscet start 开始二分查找
         2. git biscet good commit-id 指定一个行为正确的commit
         3. git biscet bad commit-id 指定一个行为错误的commit 默认为最后一个分支
         4. git biscet good / bad 指定当前commit行为是否正确
         5. 重复第4步直到找到出问题的commit
         6. git biscet reset 结束二分查找
         
   + git rebase -i HEAD~5
   
         交互式处理前5条commit
         1. git会列出前5条commit和可操作的命令，
           比如 edit，square，
         2. 选择 edit，保持退出后之后的修改都会基于这个editting 的 commit
         3. 编辑结束后 git rebase --continue 结束交互

   + git fetch remote 
          
         有些时候远端比如gerrit，创建分支后，客观端 git pull --rebase 却没有拉取新的分支，需要手动fetch下数据

    
### 基础概念

   + 客户端cookie
      
         cookie是在客户端保存登录状态的，cookie和请求发送的域名相关，域名需要一样才会使用相同的cookie，即使是同一个网站及其对应的ip，并不能使用同一个cookie

