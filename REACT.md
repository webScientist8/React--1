###REACT
`安装和使用react`
```
$ npm install -g create-react-app //全局安装
$ create-react-app my-app //创建react项目
$ cd my-app //进入项目
$ npm start //启动服务
```

对象不是react的合法子元素
```
let ele = <h1>{greeting(user)}</h1>;
// ｛｝中的值不能是对象
```
当给React元素添加属性的时候，属性名跟原生的不一样，需要使用驼峰命名法，有些的属性比较特殊如class=> className，for =>htmlFor
style属性期待一个样式对象的映射，而不是字符串
```
let ele = <div>
    <button tabIndex={1}>1</button>
    <button tabIndex={3}>2</button>
    <button tabIndex={2}>3</button>
    <img src={str} alt="" style={{width: '150px', border: '10px solid rgba(0,0,0,0.3)'}}/>
</div>;
//ele是虚拟DOM，它是一个普通的JS对象，但是可以用来描述真实DOM，属性包含type,props(children)
```

react元素是不可变的，如果要更新的话需要创建新的元素
> 通过函数式定义组件
```
function Welcome(props) {
    return <h1>Hello {props.name},Your age is {props.age}!</h1>
}

ReactDOM.render(<Welcome name="abc" age="8"/>,document.querySelector('#root'));
// 渲染函数式组件的时候，会先把传入组件的属性包装成一个对象，传入组件函数，返回一个React元素
```
> 通过类定义组件
```
class Welcome extends React.Component {
    render() {
    return <h1>{this.props.name}</h1>
    }
}
// 类通过继承React.Component就变成了组件，至少需要一个render方法用来返回react元素。
// 类的名称首字母必须大写，因为ReactDom通过首字母来区分是React组件还是React元素
// 如何渲染类级组件
// 1、把所有传递给组件的属性封装成一个对象
// 2、把属性对象传递给组件的构造函数，从而得到组件的实例
// 3、调用实例的render方法得到React元素。
// 4、由React DOM把React元素转换成真实的DOM对象并插入到页面中

ReactDOM.render(<Welcome name="abc" age="8"/>, document.querySelector('#root'));
```
> 复合组件：必须且只能返回一个顶级元素。必须返回一个实际内容或Null，否则报错
```
function Welcome(props) {
    return <h1>{props.name}</h1>;
}
function App() {
    return (
        <div>
            <Welcome name="ll"/>
            <Welcome name="mm"/>
            <Welcome name="nn"/>
            <Welcome name="yy"/>
        </div>
    );
}
```
> **纯函数**
1、永远不会试图修改输入的参数
2、相同的输入参数一定会产生相同的输出

如果一个值或属性是不需要在render的时候使用，就不能放在state里，可以放在实例上

只能用setState来修改state里面的属性，因为一旦调用 了setState方法，不但可以修改状态对象，还会自动重新调用render方法修改界面的显示效果。
setState可以新增state字段，但是不能删除字段

单向数据流：数据从上往下传递，父组件可以把父组件状态、属性或手工输入的值作为子组件的属性值

在类组件里，除了生命周期函数和构造函数之外，其它函数里的this都指向undefined，解决this问题，可以在render里使用bind或constructor使用bind

**与运算符&&**
通过用花括号包裹代码在 JSX 中嵌入任何表达式 ，也包括 JavaScript 的逻辑与 &&，它可以方便地条件渲染一个元素。
```
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}
```
**三目运算符**
条件渲染的另一种方法是使用 JavaScript 的条件运算符 condition ? true : false。
```
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```
**防止组件渲染**
只需让render方法返回null即可实现

componentDidMount
componentWillUnmount
`ReactDOM.unmountComponentAtNode( 卸载在某个节点的组件) `：从DOM元素中移除已挂载的React组件

> **dataset：**
> 可以获取元素上的自定义属性
> 如：在元素上自定义一个data-field属性，可以通过[元素].dataset.field属性获取它的值
```
class Regform extends Component {
    constructor() {
        super();
        this.state = {userName: 's', password: 's',desc:'ab',value:''}
    }

    save = (event) => {
        this.setState({[event.target.dataset.field]: event.target.value})
    }

    render() {
        return (<form onSubmit={this.output}>
            <label htmlFor="">用户名<input required type="text" value={this.state.userName} onChange={this.save} data-field="userName"/></label>
           

        </form>)
    }
}

```

**children**：子元素
```
class Panel extends Component {
    render() {
        return (<div style={{border: '1px solid black',textAlign:'center'}}>
            {this.props.children} //使用时，可以给它传入子元素
        </div>)
    }
}

ReactDOM.render(<Panel>
    <ul>
        <li>a</li>
        <li>b</li>
    </ul>
</Panel>, document.querySelector('#root'));
```

> React.Children.map()可以迭代数组。
```
class Panel extends Component {
    render() {
        return (<div style={{border: '1px solid black', textAlign: 'center'}}>
            <ul>{React.Children.map(this.props.children, (item, index) => {
                return <li key={index}>{item}</li>
            })}</ul>
        </div>)
    }
}

ReactDOM.render(<Panel>
    <ul>
        <li>a</li>
        <li>b</li>
    </ul>
</Panel>, document.querySelector('#root'));
```

setState方法提供了回调函数

PropTypes参数验证，需要先安装
```
npm i prop-types -S
```
> https://reactjs.org/docs/typechecking-with-proptypes.html

react-router

> npm install xxx 安装了一个模块的话
那么启动脚本就丢失了
所以永远 不要通过 npm install来安装模块
用yarn add xxx
yarn add react-scripts

安装react动画插件
```
yarn add react-transition-group

```

```
// 使用exact，表示完全匹配这个路径
<Route exact path="/" component={Home}/>
```
开发时使用HashRouter，上线时使用BrowerRouter

props有三个属性：history、match、location
- history
	- push
	- goBack
	- goForward
- match：匹配的是当前URL中的路径和当前路由的path属性的匹配结果
	- 
- location
	- pathname
	- state

```
// 传递state对象
<Link to={{pathname:'/detail',state:{user:item}}}>{item.name}</Link>
```
路由的渲染方式有三种：
1、component
2、使用render的方法，会把render的返回值渲染出来

withRouter向指定的组件传入三个属性history、match、location

render路径匹配则渲染，路径不匹配则不渲染；children不管路径是否匹配，都会渲染

**NavLink路由库提供的**
```
  <NavLink to="/home" activeClassName="selected">首页</NavLink>
```

```
// exact 表示绝对匹配
 <Route exact path="/" component={Home}/>
```
> withRouter的用法
```
//使用withRouter会给函数传递一个由history/math/location组成的一个对象
let Signout = withRouter(function (props) {
    return (
        <div>
            <button onClick={()=>{
                loginState.isLogin=false;
                props.history.push('/');
            }}>退出登录</button>
        </div>
    )
});
```
> 自定义路由组件，需要返回一个`Route`，然后可以通过render或children方法进行渲染。render只会把匹配到的路径进行渲染；而children会把所有的进行渲染，如果Link的路径与当前的路径不匹配，那么match没有任何东西。
```
function PrivateRoute({component: Component, ...rest}) {

    return (
        <Route {...rest} render={props => (
            loginState.isLogin ? <Component/> :
                <Redirect to={{pathname: '/login', mm: props.location.pathname}}/>
        )}/>
    )
}
```

react中的setState是异步方法

**cloneElement**：可以传入3个参数，第一是被克隆的对象，第二个是新的属性对象，第三个是新的子元素，第二和第三个参数会替换原有数据
```
class Wrapper extends React.Component {
    render() {
        let children = this.props.children;
        let newEle = React.cloneElement(children,{style:{color:'red'}},'world'); //克隆的时候可以传入新的属性，新的子元素
        return newEle
    }
}
```

React.PureComponent：如果新的状态对象和旧的状态对象相等，则返回false

```
//shouldComponentUpdate接收两个参数，一个是最新的props，另一个是最新的state，这个函数返回布尔值，如果返回的是true则执行componentWillUpdate、render和componentDidUpdate。
```

####如何提高react组件的渲染效率
+ 给列表中的组件添加key属性
+ 子组件执行shouldComponentUpdate方法，自行决定是否更新
> react 15.3.0新增了一个PureComponent类，以es2015的方式方便地定义纯组件，用于取代之前的PureRenderMixin。
> **注意：**`PureComponent和PureRenderMixin都是浅比较，如果对象包含了复杂的数据结构，深层次的差异可能会产生误判`

#### redux
**安装redux**
```
$ yarn add redux
```
**引入redux**
```
$ import {createStore} from 'redux'
```
**创建Store时需要给createStore传递一个reducer函数，因此先创建reducer函数**
```
// reducer需要2个参数，第一个是Store传给它的状态（使用时需要初始化state，例：state=0）。第二个是执行的动作action，action是一个对象，里面有type属性，用来指定动作，action可以添加其它属性。
//一定要写default，用来初始化仓库的
let reducer=( state=0, action )=>{
	switch(action.type){
		case 'add':
			return state+1;
		default:
			return state;
	}
}
```
**创建仓库**
创建一个仓库
```
let store = new createStore(reducer);
```
**获取仓库里的state**
```
store.getState();//使用这个可以获取仓库里的state
```
**执行动作**
```
//给dispatch传入一个action对象，包含type，用来指定执行的动作，可以action对象添加自定义属性，让reducer进行处理
store.dispatch({type:'add',payload:1})
```
**订阅仓库**
```
//通过subscribe来订阅
store.subscribe(()=>{
	this.setState({num:store.getState()})
})
```

**REACT路由传参**
http://reacttraining.cn/web/example/url-params
> 给react路由传递参数，路由会将路由对象传递给组件，传递的参数放在了路由对象中的match.params上了。

```
const ParamsExample = () => (
  <Router>
    <div>
      <h2>账号</h2>
      <ul>
        <li><Link to="/reacttraining">React Training</Link></li>
      </ul>
      <Route path="/:id" component={Child}/>
    </div>
  </Router>
)

const Child = ({ match }) => (
  <div>
    <h3>ID: {match.params.id}</h3>
  </div>
)
```

**onSubmit事件要写在表单上**

##REDUX中间件
```
$ npm install redux-logger redux-thunk redux-promise -S
```

使用中间件时，logger要放在最后
```
import logger from 'redux-logger';
import thunk from 'redux-thunk';
import promise from 'redux-promise';
let store = createStore(reducer, applyMiddleware(thunk, promise, logger));
```

**限流和防抖**

**react-router-redux**
https://github.com/ReactTraining/react-router/tree/master/packages/react-router-redux


mongodb

win10:powershell下 ./mongod --dbpath=E:\bbb
cmd下mongod --dbpath=e:\bbb

启动mongo，cmd下输入 mongo，Powershell下输入./mongo


git bash 

```
ssh root@ip
```

查看占用端口号
netstat -auto | findstr '8080'
### mac linux
```
ps -ef|grep 8080
kill -9 9080
```