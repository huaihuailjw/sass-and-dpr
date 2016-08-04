<h2>CSS预加载处理</h2>

<p>SASS是基于 Node.js的，需要要安装 Node.js</p>
<p>1、为了确保依赖环境正确，我们先执行几个简单的命令检查。</p>
<pre><code>    
	node -v
    Node是一个基于Chrome JavaScript V8引擎建立的一个解释器
    检测Node是否已经安装，如果正确安装的话你会看到所安装的Node的版本号
</code></pre>
<p>2、接下来看看npm，它是 node 的包管理工具，可以利用它安装 gulp 所需的包</p>
<pre><code>    npm -v
    这同样能得到npm的版本号，装 Node 时已经自动安装了npm
</code></pre>

<h2>安装SASS</h2>

<pre><code>
	npm install -g sass
	全局安装

	sudo npm install -g sass
	Mac 使用权限安装(需要输入密码)

	sass -v

	升级sass版本的命令行为
	gem update sass
</code></pre>

<h2>SASS 简单编译 </h2>
<pre><code>
	单文件转换命令
	sass style.scss style.css

	单文件监听命令
	sass --watch style.scss:style.css

	文件夹监听命令
	sass --watch sassFileDirectory:cssFileDirectory

	css文件转成sass/scss文件（在线转换工具css2sass)

	sass-convert style.css style.sass   
	sass-convert style.css style.scss
</code></pre>

<h2>viewport</h2>
<pre><code>
	<meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />

	在移动开发时我们都会有上面这段代码

	viewport :虚拟窗口大小
	width: 控制viewport大小 可以自己设定值如320px(很少用) 一般设置为device-width(设备宽度)
	initial-scale 初始缩放比例 即页面初次加载时的缩放比例默认为1
	maximum-scale：用户可缩放到的最大比例
	dpr(device pixel ratio)

	设备像素比dpr 要了解这一概念还得清楚另外两个概念
</code></pre>
<p>设备物理像素</p>
<p>
	通俗的讲设备屏幕有多少个可以闪烁的点 是一个具体的概念 比如iphone6横向就有750个可以改变颜色的点 类似与电视机 如果家里有10年前买的大头电视，你趴在屏幕前仔细看能看到一个个RGB的点 这就是设备的物理像素
</p>
<p>设备独立像素</p>
<p>
	设备独立像素是一个虚拟的概念，如程序中的css 比如我们将一个div宽度设置为10像素 那么在pc上系统会将这个div显示在屏幕的10个点上
</p>
<pre><code>
	dpr = 设备物理像素/设备独立像素
	程序中的1px占据设备上的几个最小物理点可以这么理解
	iphone3G 设备物理像素320个点 设备独立像素320px 那么dpr就是1
	iphone6 设备物理像素750个点 设备独立像素375px 那么dpr就是2
	也就是我们css中写的1px其实不等于设备实际上的那1px 也可能等于设备上的2px
	根据dpr我们就可以灵活的在移动端缩放页面比例
	可以通过window.devicePixelRatio来获取dpr
</code></pre>

<h2>动态rem</h2>
<p>
	通过上面的rem,viewport,以及dpr我们就可以完成我们的终极适配了，告别死板写法 不再这样写死 我们知道了设备的dpr就可以明确的知道缩放多少，而且这样还解决了很难解决的1px横线的问题
</p>
<p>我们需要这样一段js代码</p>
<pre><code>
	具体可以看
	http://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html
</code></pre>	
<p>
	跟淘宝很像，淘宝判断安卓和IOS的dpr 据说可以解决安卓的不稳定。
	经过试用目前没有发现不稳定状态。
</p>


<h2>接下来就是rem</h2>
<p>rem（font size of the root element）是指相对于根元素（即html元素）的字体大小的单位。</p>
<pre><code>
	sass 配合移动端
	$browser-default-font-size: 64px !default;
	// 这里的64px 是根据设计图的640 如果是750 则需要改成75px
	html {
	    font-size: $browser-default-font-size;
	}
	@function pxTorem($px) {
	    // 获取宽度高度等
	    @return $px / $browser-default-font-size * 1rem;
	}
</code></pre>

<p>由于移动端文字对rem较差比较大(不推荐使用rem)</p>
<pre><code>
	// 这里推荐加上important以免被覆盖.
	@mixin px2px($px){
	    // 获取文字大小函数
	    [data-dpr="1"] & { 
	        font-size: ($px / 2) * 1px !important; 
	    }  
	    [data-dpr="2"] & { 
	        font-size: $px * 1px !important; 
	    }  
	    [data-dpr="3"] & { 
	        font-size: ($px / 2 * 3) * 1px !important;
	    }
	}
</code></pre>

