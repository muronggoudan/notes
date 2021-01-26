# :thinking:浏览器中的页面

##  关于PerformanceAPI的时间戳解析

* navigationstart - 表征了从同一个浏览器上下文的上一个文档卸载unload结束时的UNIX时间戳。如果没有上
一个文档，这个值会和fetchStart相同。
* unloadEventStart - 表征了unload事件抛岀时的UNIX时间戳。如果没有上一个文档，这个值会返回0。
* unloadEventEnd - 表征了unload事件处理完成时的UNIX时间戳。如果没有上一个文档，这个值会返回。
* redirectStart - 表征了第一个HTTP重定向开始时的UNIX时间戳。如果没有重定向，或者重定向中的一个不同源，这个值会返回0。
* redirectEnd - 表征了最后一个HTTP重定向完成时也就是说是HTTP响应的最后一个比特直接被收到的时间的UNIX时间戳。如果没有重定向，或者重定向中的一个不同源，这个值会返回0。
* fetchStart - 表征了浏览器准备好使用HTTP请求来获取fetch文档的UNIX时间戳。这个时间点会在检查任何应用缓存之前。
* domainLookupStart - 表征了域名查询开始的UNIX时间戳。如果使用了持续连接persistent connection，或者这个信息存储到了缓存或者本地资源上，这个值将和fetchStart一致。
* domainLookupEnd - 表征了域名查询结束的UNIX时间戳。如果使用了持续连接persistent connection，或者 这个信息存储到了缓存或者本地资源上，这个值将和fetchStart一致。
* connectStart - 返回HTTP请求开始向服务器发送时的Unix毫秒时间戳。如果使用持久连接persistent connection，则返回值等同于fetchStart属性的值。
* connectEnd - 返回浏览器与服务器之间的连接建立时的Unix毫秒时间戳。如果建立的是持久连接，则返回值等同于fetchStart属性的值。连接建立指的是所有握手和认证过程全部结束。
* secureConnectionStart - 返回浏览器与服务器开始安全链接的握手时的Unix毫秒时间戳。如果当前网页不要求安全连接，则返回0。
* requeststart - 返回浏览器向服务器发出HTTP请求时或开始读取本地缓存时的Unix毫秒时间戳。
* responsestart - 返回浏览器从服务器收到或从本地缓存读取第一个字节时的Unix毫秒时间戳。如果传输层在开始请求之后失败并且连接被重开，该属性将会被数制成新的请求的相对应的发起时间。
* responseEnd - 返回浏览器从服务器收到或从本地缓存读取，或从本地资源读取最后一个字节时如果在此之前HTTP连接已经关闭，则返回关闭时的Unix毫秒时间戳。
* domLoading - 返回当前网页DOM结构开始解析时即document.readyState属性变为“loading”、相应的ready.statechange事件触发时的Unix毫秒时间戳。
* domlnteractive - 返回当前网页DOM结构结束解析、开始加载内嵌资源时即document.readyState属性变为 “interactive”、相应的readystatechange事件触发时的Unix毫秒时间戳。
* domContentLoadedEventStart - 返回当解析器发送DOMContentLoaded事件，即所有需要被执行的脚本己经被解析时的Unix毫秒时间戳。
* domContentLoadedEventEnd - 返回当所有需要立即执行的脚本己经被执行不论执行顺序时的Unix毫秒时间戳。
* domComplete - 返回当前文档解析完成，即document.readyState变为'complete'且相对应的 ready stat echange 被触发时的Unix毫秒时间戳。
* loadEventStart - 返回该文档下，load事件被发送时的Unix毫秒时间戳。如果这个事件还未被发送，它的值将会是0。
* loadEventEnd - 返回当load事件结束，即加载事件完成时的Unix毫秒时间戳。如果这个事件还未被发送，或者尚未完成，它的值将会是0。

## DOM树如何生成

HTML解析器并不是等整个文档加载完成之后再解析的，而是网络进程加载了多少数据，HTML解析器便解析多少数据。

1. 网络进程接收到响应头之后，会根据响应头中的`content-type`字段来判断文件的类型，比如content-type的值是“text/html“，那么浏览器就会判断这是一个HTML类型的文件，然后为该请求选择或者创建一个渲染进程。 渲染进程准备好之后，网络进程和渲染进程之间会建立一个共享数据的管道，网络进程接收到数据后就往这个管道里面放，而渲染进程则从管道的另外一端不断地读取数据，并同时将读取的数据“喂“给HTML解析器。你可以把这个管道想象成一个“水管“，网络进程接收到的字节流像水一样倒进这个“水管“，而“水管“的另外一端是渲染进程的HTML解析器，它会动态接收字节流，并将其解析为DOM。
2. 第一个阶段，通过分词器将字节流转换为Token。至于后续的第二个和第三个阶段是同步进行的，需要将Token 解析为DOM节点，并将DOM节点添加到DOM树中。
3. HTML解析器维护了一个Token栈结构，该Token栈主要用来计算节点之间的父子关系，在第一个阶段中生成 的Token会被按照顺序压到这个栈中。具体的处理规则如下所示：
    1. 如果压入到栈中的是StartTag Token， HTML解析器会为该Token创建一个DOM节点，然后将该节点加入到 DOM树中，它的父节点就是栈中相邻的那个元素生成的节点。
    2. 如果分词器解析出来是文本Token，那么会生成一个文本节点，然后将该节点加入到DOM树中，文本Token是 不需要压入到栈中，它的父节点就是当前栈顶Token所对应的DOM节点。
    3. 如果分词器解析出来的是EiidTag.标签，比如是EndTag div， HTML解析器会查看Token栈顶的元素是否是 StarTag div，如果是，就将StartTag；div从栈中弹出，表示该div元素解析完成。

## JavaScript是如何影响DOM生成的
当浏览器在解析dom的过程中解析到`<script>`标签时，渲染引擎判断这是一段脚本，此时HTML解析器就会暂停DOM的解析，因为接下来的JavaScript可能要修改当前已经生成的DOM结构。
这时候HTML解析器暂停工作，JavaScript引擎介入，并执行script标签中的这段脚本，脚本执行完成之后，HTML解析器恢复解析过程，继续解析后续的内容，直至生成最终的DOM。如果是在html中使用外联脚本。则html解析器将会等待外联脚本下载完成之后进行执行，执行完成后再恢复对html的解析。使用`async`和`defer`标识，在下载过程中将不会影响html解析器的工作。使用`async`标志的脚本文件一旦加载完成，会立即执行；而使用了`defer`标记的脚本文件，需要在DOMContentLoaded事件之前执行。不过Chrome浏览器做了很多优化，其中一个主要的优化是预解析操作。当渲染引擎收到字节流之后，会开启一个预解析线程，用来分析HTML文件中包含的JavaScript。CSS等相关文件，解析到相关文件之后，预解析线程会提前下载这些文件。再回到DOM解析上，我们知道引入JavaScript线程会阻塞DOM，不过也有一些相关的策略来规避，比如使用CDN来加速JavaScript文件的加载，压缩JavaScript文件的体积。另外，如果JavaScript文件中没有操作DOM相关代码，就可以将该JavaScript脚本设置为异步加载，通过`async`或`defer`来标记代码， 

# 渲染流水线视角下的CSS

## 那渲染流水线为什么需要CSSOM呢？

第一个是提供给JavaScript操作样式表的能力，第二个是为布局树的合成提供基础的样式信息。

等DOM和CSSOM都构建好之后，渲染引擎就会构造布局树。布局树的结构基本上就是复制DOM树的结构，不同之处在于DOM树中那些不需要显示的元素会被过滤掉，如`display:none`属性的元素、`head`标签、`script`标签等。复制好基本的布局树结构之后，渲染引擎会为对应的DOM元素选择对应的样式信息，这个过程就是样式计算。样式计算完成之后，渲染引擎还需要计算布局树中每个元素对应的几何位置，这个过程就是计算布局。通过样式计算和计算布局就完成了最终布局树的构建。再之后，就该进行后续的绘制操作了。

# 影响页面展示的因素以及优化策略

要想缩短白屏时长，可以有以下策略：
1. 通过内联JavaScript、内联CSS来移除这两种类型的文件下载，这样获取到HTML文件之后就可以直接开始 渲染流程了；
2. 但并不是所有的场合都适合内联，那么还可以尽量减少文件大小，比如通过webpack等工具移除一些不必要的 注释，并压缩JavaScript文件；
3. 还可以将一些不需要在解析HTML阶段使用的JavaScript标记上sync或者defer；
4. 对于大的CSS文件，可以通过媒体查询属性，将其拆分为多个不同用途的CSS文件，这样只有在特定的场景 下才会加载特定的CSS文件。

# 显示器是怎么显示图像的

每个显示器都有固定的刷新频率，通常是60HZ，也就是每秒更新60张图片，更新的图片都来自于显卡中一个叫前缓冲区的地方，显示器所做的任务很简单，就是每秒固定读取60次前缓冲区中的图像，并将读取的图像显示到显示器上。

# 如何生成一帧图像

关于其中任意一帧的生成方式，有重排、重绘和合成三种方式。
这三种方式的渲染路径是不同的，逋常渲染路径越长，生成图像花费的时间就越多。比如重排，它需要重新根据CSSOM和DOM来计算布局树，这样生成一幅图片时，会让整个渲染流水线的每个阶段都执行一遍，如果布局复杂的话，就很难保证渲染的效率了。而重绘因为没有了重新布局的阶段，操作效率稍微高点，但是依然需要重新计算绘制信息，并触发绘制操作之后的一系列操作。

# 分层和合成

在这个过程中，将素材分解为多个图层的操作就称为分层，最后将这些图层合并到一起的操作就称为合成。

理解了为什么要引入合成和分层机制，下面我们再来看看Chrome是怎么实现分层和合成机制的：
在Chrome的渲染流水线中，分层体现在生成布局树之后，渲染引擎会根据布局树的特点将其转换为层树(Layer Tree)，层树是渲染流水线后续流程的基础结构。层树中的每个节点都对应着一个图层，下一步的绘制阶段就依赖于层树中的节点。绘制阶段其实并不是真正地绘出图片，而是将绘制指令组合成一个列表，比如一个图层要设置的背景为黑色，并且还要在中间画一个圆形，那么绘制过程会生成`Paint BackGroundColor:Black`、`Paint Circle`这样的绘制指令列表，绘制过程就完成了。有了绘制列表之后，就需要进入光栅化阶段了，光栅化就是按照绘制列表中的指令生成图片。每一个图层都对应一张图片，合成线程有了这些图片之后，会将这些图片合成为“一张“图片，并最终将生成的图片发送到后缓冲区。这就是一个大致的分层、合成流程。

需要重点关注的是，合成操作是在合成线程上完成的，这也就意味着在执行合成操作时，是不会影响到主线程执行 的。这就是为什么经常主线程卡住了，但是CSS动画依然能执行的原因。

## 分块机制

# 如何利用分层技术优化代码
可以使用`will-change`来告诉渲染引擎你会对该元素做一些特效变换，CSS代码如下
```css
.box (
  will-change: transform opacity;
}
```

这段代码就是提前告诉渲染引擎box元素将要做几何变换和透明度变换操作，这时候渲染引擎会将该元素单独实现一帧，等这些变换发生时，渲染引擎会通过合成线程直接去处理变换，这些变换并没有涉及到主线程，这样就大大提升了渲染的效率。这也是CSS动画比JavaScript动画高效的原因。

## 加载阶段
这些能阻塞网页首次渲染的资源称为关键资源：
1. 第一个是关键资源个数；
2. 第二个是关键资源大小；
3. 第三个是请求关键资源需要多少个RTT (Round Trip Time)。

RTT就是这里的往返时延。它是网络中一个重要的性能指标，表示从发送端发送数据开始，到发送端收到来自接收端的确认，总共经历的时延。
由于渲染引擎有一个预解析的线程，在接收到HTML数据之后，预解析线程会快速扫描HTML数据中的关键资源，一旦扫描到了，会立马发起请求，你可以认为JavaScript和CSS是同时发起请求的，所以它们的请求是重叠的，那么计算它们的RTT时，只需要计算体积最大的那个数据就可以了，总的优化原则就是减少关键资源个数，降低关键资源大小、降低关键资源的RTT次数。

# 交互阶段

如果在讦算样式阶段发现有布局信息的修改，那么就会触发重排操作，然后触发后续渲染流水线的一系列操作，这个代价是非常大的。

同样如果在计算样式阶段没有发现有布局信息的修改，只是修改了颜色一类的信息，那么就不会涉及到布局相关的调整，所以可以跳过布局阶段，直接进入绘制阶段，这个过程叫重绘。不过重绘阶段的代价也是不小的。

还有另外一种情况，通过CSS实现一些变形、渐变、动画等特效，这是由CSS触发的，并且是在合成线程上执行的，这个过程称为合成。因为它不会触发重排或者重绘，而且合成操作本身的速度就非常快，所以执行合成是效率最高的方式。

# 一个大的原则就是让单个帧的生成速度变快

1. 减少JavaScript脚本执行时间
    1. 一种是将一次执行的函数分解为多个任务，使得每次的执行时间不要过久。
    2. 另一种是釆用Web Workers，你可以把Web Workers当作主线程之外的一个线程，在Web Workers中是可以执行JavaScript脚本的，不过Web Workers中没有DOM、CSSOM环境，这意味着在Web Workers中是无法通 过JavaScript来访问DOM的，所以我们可以把一些和DOM操作无关且耗时的任务放到Web Workers中去执行。
2. 避免强制同步布局
所谓强制同步布局，是指JavaScript强制将计算样式和布局操作提前到当前的任务中。

3. 避免布局抖动
4. 合理利用CSS合成动画
合成动画是直接在合成线程上执行的，这和在主线程上执行的布局、绘制等操作不同，如果主线程被JavaScript或者一些布局任务占用，CSS动画依然能继续执行。所以要尽量利用好CSS合成动画，如果能让CSS处理动画，就尽量交给CSS来操作。另外，如果能提前知道对某个元素执行动画操作，那就最好将其标记为`will-change`，这是告诉渲染引擎需要将该元素单独生成一个图层。

5. 避免频繁的垃圾回收

## PWA 

## DOM的缺陷

比如，我们可以调用document， body. appendChild(node)；往body节点上添加一个元素，调用该API之后会引发一系列的连锁反应。首先渲染引擎会将node节点添加到body节点之上，然后触发样式计算、布局、绘制、栅格化、合成等任务，我们把这一过程称为重排。除了重排之外，还有可能引起重绘或者合成操作，形象地理解就是“牵一发而动全身”。另外，对于DOM不当操作还有可能引发强制同步布局和布局抖动的问题，这些操作都会大大降低渲染效率。因此，对于DOM的操作我们时刻都需要非常小心谨慎。

# 什么是虚拟DOM
在谈论什么是虚拟DOM之前，我们先来看看虚拟DOM到底要解决哪些事情。

1. 将页面改变的内容用到虚拟DOM上，而不是直接应用到DOM上。
2. 变化被应用到虚拟DOM上时，虚拟DOM并不急着去渲染页面，而仅仅是调整虚拟DOM的内部状态，这样操作虚拟DOM的代价就麦得非常轻了。
3. 在虚拟DOM收集到足够的改变时，再把这些变化一次性应用到真实的DOM上。

创建阶段。首先依据JSX和基础数据创建出来虚拟DOM，它反映了真实的DOM树的结构。然后由虚拟DOM树创建出真实DOM树，真实的DOM树生成完后，再触发渲染流水线往屏幕输岀页面。

更新阶段。如果数据发生了改变，那么就需要根据新的数据创建一个新的虚拟DOM树；然后React比较两个树，找岀变化的地方，并把变化的地方一次性更新到真实的DOM树上；最后渲染引擎更新渲染流水线，并生成新的页面。

## 受启发的成熟方案
1. 双缓存
2. MVC模式 

## WebComponent