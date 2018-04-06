react作为前端框架之一，有着较好的性能，它主要有如下特性：
1.声明式设计 −React采用声明范式，可以轻松描述应用。

2.高效 −React通过对DOM的模拟，最大限度地减少与DOM的交互。

3.灵活 −React可以与已知的库或框架很好地配合。

4.JSX − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。

5.组件 − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。

6.单向响应的数据流 − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。

下面讲一下入门的demo实例。

一：Hello World

head标签中先要引入三个js文件，react.js (是核心库)，react-dom.js （是提供与dom相关的功能），最后是要把JSX语法转化为javascript语法的 browser.js。



<body>
    <div id="example"></div>
	<script type="text/babel">
	      ReactDOM.render(
	        <h1>Hello, world!</h1>,
	        document.getElementById('example')
	      );//把h1标签内容添加到id为example的div中。
	</script>
</body>
ReactDOM.render是react的基本语法，可以将JAX语法转换为html语言，插入相应的DOM节点。

JSX与普通的js有一点不同在于<script>标签的type属性，js是text/javascript，这个是text/babel 。

相同的是可以把这段代码写入独立的一个js文件，然后在html引入即可（type值需要为text/babel ）。

<div id="example"></div>
<script type="text/babel" src="helloworld_react.js"></script>
（demo1）运行效果：


我们也可以在浏览器调试页面可以看到。

此段代码就是h1标签的内容插入到了id为example的div中。

二：JSX语法介绍

JSX是javascript和XML结合的一种格式，它很灵活，可以将html与javascript混合写，遇到<，就当作HTML解析，遇到{，就当作javascript解析（demo2）。

    <script type="text/babel">
	var names = ['China', 'English', 'American'];//定义一个数组
	ReactDOM.render(
	    <div>
		{		    
                        names.map(function (name) {//map() 把每个元素通过函数传递到当前匹配集合中.
		        return <h1>Hello, {name}!</h1>
		    })
		}
	    </div>,
	    document.getElementById('example')
	);
     </script>    
首先我们定义了一个数组，map方法遍历该数组取值传给<h1>标签，然后插入到div中，效果如下：




JSX可以支持直接将javascript表达式，变量等插入模板，示例（demo3）：

<div id="example"></div>
<script type="text/babel">
    ReactDOM.render(
	<div>
	    <h1>{1+1}</h1>
	</div>,
	document.getElementById('example')
    );
</script>    
{1+1}是一个js表达式，被h1标签括起来插入到div中，运行结果为2.



同样可以插入JavaScript变量（demo4）。

            <div id="example"></div>
	    <script type="text/babel">
		var arr = [
		    <h1>Hello,China</h1>,
		    <h2>Hello,World</h2>,
		];
		ReactDOM.render(
		    <div>{arr}</div>,
		    document.getElementById('example')
		);
	    </script> 
定义了一个两行h标签的数组arr，JSX将其展开都插入到了div中。





三：React组件

<div id="example"></div>
<script type="text/babel">
    var HelloMessage = React.createClass({//定义了一个名字叫做HelloMessage的组件，createClass为创建组件方法，
        render: function(){
        return <h1>Hello World！</h1>;
        }
    });
    ReactDOM.render(
        <HelloMessage />,
        //将HelloMessage插入到div中
        document.getElementById('example')
    );
</script>      
React可以创建组件，将其插入到DOM节点中，需要注意的是组件名字命名，首字母必须为大写，否则会无法运行（demo5）。


另外，组件中只能包含一个顶级的html标签，不能同时有多个同级标签。例如下面代码中写了两个<h1>标签，运行就会报错。

render: function(){
    return <h1>Hello World！</h1>
    <h1>Hello World！</h1>;
}


组件其实就如同html的容器元素，它可以有属性，比方说name，class，id，及自定义属性，然后利用this.props.对属性进行取值。

html中的class属性在React中为className，for属性为htmlFor， 因为class和for在js语法中为保留字。（demo6）
<div id="example"></div>
<script type="text/babel">
    var HelloMessage = React.createClass({
        render: function() {
            return <h1>Hello {this.props.name},className为{this.props.className},data-on为{this.props.dataOn}</h1>;
        }
    });
    ReactDOM.render(
        <HelloMessage name="world"  className="class" dataOn="on"/>,
        document.getElementById('example')
    );
运行效果如下：



另外，组建有一个this.props.children，它表示组件的所有子节点，（demo7）
	    <div id="example"></div>
	    <script type="text/babel">
		var NotesList = React.createClass({
		    render: function() {
		        return (
		            <ol>
		                {
		                    React.Children.map(this.props.children, function (child) {
		                        return <li>{child}</li>;
		                    })
		                }
		            </ol>
		        );
		    }
		});
		ReactDOM.render(
		    <HelloMessage>
		        <span>hello</span>
		        <span>world</span>
		    </HelloMessage>,
		    document.getElementById('example')
		);
	    </script>    
可以看出，HelloMessage有两个span子节点，利用React.children方法进行处理节点，用this.props.children来获取节点，运行效果：


四：Props验证

Props验证使用propTypes，它可以保证组件能够被正确使用，React.PropTypes提供很多验证器validator验证传入数据是否正确，当props传入无效数据时，js控制台会报错。

optionalArray: React.PropTypes.array,    //声明为传入数组必须为数组

React.PropTypes.bool,    //声明为传入数组必须为布尔型

React.PropTypes.func,    //声明为传入数组必须为函式

React.PropTypes.number,   //声明为传入数组必须为数字

React.PropTypes.object,   //声明为传入数组必须为对象类型

React.PropTypes.string,   //声明为传入数组必须为字符串

React.PropTypes.isRequired,//不可为空，必填（demo14）


实例中设置了一个数值为123的data对象，然后利用React.PropTypes.string.isRequired规定了组件的title值必须为字符串且不为空，这段代码执行后会显示123，但是控制台会报错，因为123是number型不是string（demo8）。

            <div id="example"></div>
	    <script type="text/babel">
		   var MyTitle = React.createClass({
			    propTypes: {
			    	title: React.PropTypes.string.isRequired,
			    },			
			    render: function() {
			        return <h1> {this.props.title} </h1>;
			    }
			});
			var data = 123;
			ReactDOM.render(
			    <MyTitle title={data} />,
			    document.getElementById('example')
			);
	    </script>    


另外还可以通过getDefalutProps()方法诶props设置默认值。

示例为我们为MyTitle组件的title设默认值为myTitle（demo9）。

            <div id="example"></div>
	    <script type="text/babel">
		var MyTitle = React.createClass({
		    getDefaultProps: function() {
			 return {
			      name: 'myTitle'
			 };
		    },
		    render: function() {
			    return <h1>Hello {this.props.name}</h1>;
			 }
		    });
	 
		    ReactDOM.render(
			<MyTitle />,
			document.getElementById('example')
		    );
	    </script>    





五：DOM节点

React的组件可以说是虚拟的DOM节点，只有在插入到文档后才变为真实的DOM节点 ，如果想要获取真实DOM，则需要用到this.refs来获取（demo10）。
<script type="text/babel">
    var MyComponent = React.createClass({
        handleClick: function() {
            this.refs.myTextInput.focus();
        },
        render: function() {
            return (
                <div>
                    <input type="text" ref="myTextInput" />
                    <input type="button" value="Focus the text input" onClick={this.handleClick} />
                </div>
            );
        }
    });			
    ReactDOM.render(
        <MyComponent />,
        document.getElementById('example')
    );
</script>    
在这里定义了一个组件叫做MyComponent，然后一段html代码，顶层div标签及子元素两个input，一个是text文本框，一个是button按钮，且在按钮上绑定了onclick方法，this.handClick。

在组件方法中，设置了handleClick方法，this.refs.MytextInput.focus()来返回真实DOM节点获取文本框输入内容。

六：React State（状态）

React组件是一个状态机，通过与用户的交互，实现不同状态，然后根据新的state重新渲染界面，不需要操作DOM（demo11）。

<div id="example"></div>
<script type="text/babel">
    var LikeButton = React.createClass({
        getInitialState: function() {
            return {liked: false};
        },
        handleClick: function(event) {
            this.setState({liked: !this.state.liked});
        },
        render: function() {
            var text = this.state.liked ? '喜欢' : '不喜欢';
            return (
                <p onClick={this.handleClick}>
                    You {text} this. Click to toggle.
                </p>
            );
        }
    });

    ReactDOM.render(
        <LikeButton />,
        document.getElementById('example')
    );
</script> 
上述示例中我们创建了一个组件LikeButton，getInitialState（）方法定义初始化状态，是false，通过this.state属性可以读取state状态，我们给<p>绑定了一个chick时间，点击组件，利用this.setState修改状态值，每次修改后自动调用this.renderf（），重新渲染页面。







七：表单事件

表单属于用户互动组件，text文本框，textarea文本域，select下拉框，radio，checkbox等元素都不能单纯的使用this.props.value来读取值，需要定义onChange回调函数，通过event.target.value来读取用户数组的值（demo12）。

<div id="example"></div>
<script type="text/babel">
    var HelloMessage = React.createClass({
        getInitialState: function() {
            return {value: 'Hello World!'};
        },
        handleChange: function(event) {
            this.setState({value: event.target.value});
        },
        render: function() {
            var value = this.state.value;
            return <div>
                  <input type="text" value={value} onChange={this.handleChange} /> 
                  <h4>{value}</h4>
               </div>;
        }
    });
    ReactDOM.render(
      <HelloMessage />,
      document.getElementById('example')
    );
 </script>    
在这里我们定义了一个HelloMessage组件，div包括了一个input，一个<h4>，input上绑定了一个onChange事件用来监听input的state变化，来获取输入的值。




八：组件的生命周期

组件的生命周期分为三个状态，

Mounting：已插入真实DOM节点
Updating：正在被重新渲染
Unmounting：已移除真实DOM           

然后这三个状态都有两个处理函数，在进入状态之前调用will函数，进入状态后调用did状态。

componentWillMount 在渲染前调用
componentDidMount 在第一次渲染后调用
componentWillUpdate 在组件接收到新的props或state但还没有render时调用
componentDidUpdate 在组件完成更新后立即调用
componentWillUnmount 在组件从DOM中移除时立即调用


<div id="example"></div>
<script type="text/babel">
    var HelloMessage = React.createClass({
        getInitialState: function () {
            return {
              opacity: 1.0
            };
        }, 
        componentDidMount: function () {
            this.timer = setInterval(function () {
                var opacity = this.state.opacity;
                opacity -= .05;
                if (opacity < 0.1) {
                    opacity = 1.0;
                }
                this.setState({
                    opacity: opacity
               });
            }.bind(this), 100);
       },
 
       render: function () {
          return (
              <div style={{opacity: this.state.opacity}}>
                Hello {this.props.name}
              </div>
            );
        }
    });
    ReactDOM.render(
        <HelloMessage name="world" />,
	document.getElementById('example')
    );
 </script> 
定义HelloMessage组件，属性name值为world，render里读取组件name值渲染到div，div的样式透明度，根据state修改而修改，默认初始化值为不透明。

通过componentDidMount方法 设置定时器setInterval，每隔100毫秒修改组件透明度重新渲染组件（demo13）。





九：React AJAX

组件的数据一般是通过AJAX请求服务器来获取，在这里利用componentDidMount方法来设置AJAX方法，将服务器端获取到的数据存入state，根据state的修改来实时重新渲染页面（demo14）。

	<div id="example"></div>
        <script type="text/babel">
            var UserGist = React.createClass({
	        getInitialState: function() {
	            return {
	            	username: '',
	            	lastGistUrl: ''
	            };
	        },

	        componentDidMount: function() {
	            this.serverRequest = $.get(this.props.source, function (result) {
	            	var lastGist = result[0];
		            this.setState({
		                username: lastGist.owner.login,
		                lastGistUrl: lastGist.html_url
		            });
	            }.bind(this));
	        },
	        componentWillUnmount: function() {
	            this.serverRequest.abort();
	        },

        	render: function() {
	            return (
		            <div>
		                {this.state.username} 用户最新的 Gist 共享地址：
		                <a href={this.state.lastGistUrl}>{this.state.lastGistUrl}</a>
		            </div>
	            );
        	}
      	    });

            ReactDOM.render(
        	<UserGist source="https://api.github.com/users/octocat/gists" />,
        	document.getElementById('example')
            );
        </script> 
定义UserGist组件，设置source属性（这写入了一个接口地址，会用来传入componentDidMount方法用来获取数据）





参考资料：

http://www.ruanyifeng.com/blog/2015/03/react.html
http://www.runoob.com/react/react-tutorial.html

https://blog.csdn.net/yf275908654/article/category/6157115

