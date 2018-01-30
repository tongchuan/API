# 第七章 mixin
第六章提到过，mixin允许我们定义可以在多个组件中公用的方法。让我们更详细地深究这一点。

## 什么是mixin
在React的主页上有这样一段定时器组件的例子：
~~~
var Timer = React.createClass({
    getInitialState: function(){
        return {secondsElapsed: 0};
    },
    tick: function(){
        this.setState({secondsElapsed: this.state.secondsElapsed+1});
    },
    componentDidMonut: function(){
        this.interval = setInterval(this.tick,100);
    },
    componentWillUnmount: function(){
        clearInterval(this.interval);
    },
    render: function(){
        return (
            <div>Seconds Elapsed:{this.state.secondsElapsed}</div>
        );
    }
});
~~~
这些代码看起来不错，不过我们有很多组件都要使用定时器，执行的都是面这段代码。这时候就轮到mixin大显神威了。我们想最终实现一个像下面这样的定时器组件：
~~~
var Timer = React.createClass({
    mixins: [IntervalMixin(1000)],
    getInitialState: function(){
        return {secondsElapsed: 0};
    },
    onTick: function(){
        this.setState({secondsElapsed: this.state.secondsElapsed + 1});
    },
    render: function(){
        return (
            <div>Seconds Elapsed: {this.state.secondsElapsed}</div>
        );
    }
});
~~~
mixin相当简单，他们就是混合进组件类中的对象而已。React在这方面实现得更加深入，他们能防止静默函数覆盖，同时还支持多个mixin混合。这些功能在别的系统中可能引起冲突。
举个例子：
~~~
React.createClass({
    mixins: [{
        getInitialState: function(){ rturn {b: 1}  }
    }],
    getInitialState: function(){ rturn {b: 2}  }
});
~~~
我们在mixin和组件类中同时定义了getInitialState方法，得到的初始化state是{a:1,b:2}。如果mixin中的方法和组件类中的方法返回的对象中存在重复的键，React会抛出一个错误来警示这个问题。

以component开头的生命周期方法，如componentDidMount，会按照mixin数值中定义的顺序被调用，并最终调用组件类中定义的componentDidMount,如果他存在的话。

回到我们之前的例子，我们需要实现一个IntervalMixin。有时候单独一个对象足以完成mixin工作，然而其他情况下我们需要用一个函数来返回这个对象。在这个例子中，我们希望能够指定时间间隔。
~~~
var IntervalMixin = function(interval){
    rturn {
        componentDidMount: function(){
            this._interval = setInterval(this.onTick, interval);
        },
        componentWillUnmount: function(){
            clearInterval(this._interval);
        }
    };
};
~~~
这样实现很不错，但是也有一定的局限性。我们不能拥有多个定时器，用户也不能选择处理定时器函数，而且我们也不能手动终止这个定时器，除非使用内部属性_interval。要解决这些问题，我们的mixin需要一个公共API。

下面这个例子简单地展示了自2014年1月1日到目前为止的总秒数。这段代码相对长了一些，但却更加灵活、强大。
~~~
var IntervalMixin = {
    setInterval: function(callback,interval){
        var token = setInerval(callback,intervaal);
        this._intervals.push(token);
        return token;
    },
    componentDidMount: function(){
        this._intervals = [];
    },
    componentWillUnmount: function(){
        this._interval.map(clearInterval);
    }
};

Var Since2014 = React.createClass({
    mixins: [IntervalMixin],
    componentDidMount: function(){
        this.setInterval(this.forceUpdate.bind(this),1000);
    },
    render: function(){
        var from = Number(new Date(2014,0,1));
        var to = Date.now();
        return (
            <div>{Math.round((to-from)/1000)}</div>
        );
    }
});
~~~
关于mixin的例子还有很多，我们无法再这里全部展示，然而下面几种用法值得参考：
 * 一个监听事件并修改state的mixin（如flux store mixin）.
 * 一个上传mixin，并负责处理XHR上传请求，同时将状态以及上传的进度同步到state。
 * 渲染层mixin，简化在< /body>之前渲染子元素的过程（如渲染模态对话框）。

## 总结
mixin是解决代码段重复的最强大工具之一，它同时还能让组件保持专注于自身的业务逻辑。mixin允许我们使用强大的抽象功能，甚至有些问题如果没有mixin就无法被优雅地解决。

即使我们只打算在单个组件中使用一个mixin，它还是为我们提供了描述一个特定行为或角色并提供给该组件的能力。mixin减少了我们在了解整个组件之前需要阅读的代码量，同时允许我们在不污染组件本身的情况下做一些丑陋的处理（比如管理内部属性_interval）。当阅读了一章关于DOM的内容是，思考一下你能从组件中抽离出来的行为或者角色，最好亲手实践一下。 