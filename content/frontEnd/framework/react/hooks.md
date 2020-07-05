## react hook

### 1、react hook父组件获取子组件的数据/函数
在react中，常用props实现子组件数据到父组件的传递，但是父组件调用子组件的功能却不常用。文档上说ref其实不是最佳的选择，在这里可以用useImperativeHandle这个hooks的api，其函数形式是：
```js
useImperativeHandle(ref,createHandle,[deps])
```
其实这个api也是ref的一种形式，但是相当于做了一定的优化，可以选择让子组件只暴露一定的api给父组件，根据在文档和其他资料上给出的方法，一共有两大步骤： 
1. 将ref传递到子组件
2. 需要使用forwardRef对子组件进行包装  

子组件childMap:
```js
//ChildMap
import React,{forwardRef，useRef,useImperativeHandle} from 'react'

function ChildMap(props,ref){
  const mapRef = useRef(null)
  useImperativeHandle(ref,()=>{
    return {
      //clickSwitch是子组件暴露出取的函数
      clickSwitch(){
        if(type==1){
          // initChinaMap()
          // setType(2)
        }else{
          // initWordMap()
          // setType(1)
        }
      }
    }
  })

  //重点ref
  return (
    <div>
    <div id="myMap" ref={mapRef}>test</div>
    </div>
  )
}

//暴露出取配合forwardRef
export default forwardRef(ChildMap)
```
注意：ref是子组件声明的时候传入进来的，也就是：
```js
function ChildMap(props,ref){
  //代码块
}
```
其实官方给出的例子是：
```js
import React,{useRef,useImperativeHandle,forwardRef} from 'react'
function FancyInput(props,ref){
  const inputRef = useRef()
  useImperativeHandle(ref,()=>({
    focus:()=>{
      inputRef.current.focus()
    }
  }))

  return <input ref={inputRef}/>
}

export default forwardRef(FancyInput)
```
父组件FatherMap:
```js
import React, {useRef} from 'react'

function FatherMap(){
  const myChildRef = useRef()

  handleMapClick(){
    //代码块
  }
  clickAll(){
    //代码块
  }

  //注意重点是ref
  return (
  
  <div>
    <ChildMap propData = {myMapData} handleMapClick={this.handleMapClick.bind(this)} ref={myChildRef} />
    <div className="mapbutton-wrap">
      <ButtonGroup>
        <Button onClick={()=>myChildRef.current.clickSwitch()}>Switch</Button>
        <Button onClick={()=>this.clickAll.bind(this)}>All</Button>
      </ButtonGroup>
    </div>
  </div>
  )
}
```
现在我们就可以在父组件里面通过myChildRef.current.clickSwitch()调用子组件的函数了。

### 2、useCallback:把匿名回调存起来
避免在component render时候声明匿名方法，因为这些匿名方法会被反复重新声明而而发被多次利用，然后容易造成component反复不必要的渲染。 
在class component当中我们通常将回调函数声明为类成员：
```js
import React from 'react'

class MyComponent extends React.Component {
  constructor(props){
    super(props)
    this.clickCallback = this.clickCallback.bind(this)
  }

  clickCallback(){
    //代码块
  }

  render(){
    return (
      <button onClick={this.clickCallback}>click me</button>
    )
  }
}
export default MyComponent
```
在react hook中使用**useCallback**就可以避免bind操作：
```js
import React,{useCallback} from 'react'

function MyComponent(props){
  //缓存函数
  const clickCallback = useCallback(()=>{
    //代码块
  },[])

  return <button onClick={this.clickCallback}>click me</button>
}
export default MyComponent
```
**useCallback**缓存函数
```js
const fnA = useCallback(fn,[...args])
```
上面的useCallback会将我们传递给它的函数fn返回，并且将这个结果缓存，当依赖args参数变更时，会返回新的函数，既然返回的是函数，我们无法很好的判断返回的函数是否变更，所以我们可以借助ES6新增的数据类型Set来判断，具体如下：
```js
import React,{useState,useCallback} from 'react'
const set = new Set()

function MyCallback(){
  const [count,setCount] = useState(1)
  const [val,setVal] = useState('')

  const callback = useCallback(()=>{
    console.log(count)
  },[count])

  set.add(callback)

  return (
    <div>
      <h3>{count}</h3>
      <h4>{set.size}</h4>
      <div>
        <buton onClick={()=>setCount(count++)}>+</buton>
        <input value={val} onChange={(event)=>setVal(event.target.value)}/>
      </div>
    </div>
  )
}

export default MyCallback
```
可以看到，每次修改count，set.size就会+1，这说明useCallback依赖变量count，count变化时会返回新的函数，而val变化时，set.size不会变，说明返回的是缓存的函数。  
> 使用场景：  
> 有一个父组件，其中包含子组件，子组件接收一个函数作为props,通常而言，如果父组件更新了，子组件也会执行更新，但是大多数场景下，更新是没有必要的，我们可以借助useCallback来返回函数，然后把中给函数作为props传递给子组件，这样子组件就能避免不必要的更新。  

```js
import React,{useState,useCallback,useEffect} from 'react'
//父组件
function Parent(){
  const [count,setCount] = useState(1)
  const [val,setVal] = useState('')

  //缓存函数
  const callback = useCallback(()=>{
    return count
  },[count])

  return(
    <div>
      <h3>{count}</h3>
      <h4>{set.size}</h4>
      <Child callback={callback}/>
      <div>
        <buton onClick={()=>setCount(count++)}>+</buton>
        <input value={val} onChange={(event)=>setVal(event.target.value)}/>
      </div>
    </div>
  )
}

//子组件
function Child({callback}){
  const [count,setCount] = useState(()=>callback())

  useEffect(()=>{
    setCount(callback())
  },[callback])

  return (
    <div>{count}</div>
  )
}
```
上面的例子，所有依赖本地状态或props来创建函数，需要使用到缓存函数的地方，都可以使用useCallback来做缓存。  
useEffect,useDemo,useCallback都是自带闭包的，也就是说，每一次组件的渲染，其都会捕获当前组件函数上下文中的状态（state,props），所以每一次这三种hooks的执行，反映的也都是当前的状态，你无法使用它们来捕获上一次的状态，对于这种清空，我们应该使用ref来访问。
