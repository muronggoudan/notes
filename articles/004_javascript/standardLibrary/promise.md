# :thinking:Promise

[[toc]]


## 构造函数
`Promise()`
创建一个新的 Promise 对象。该构造函数主要用于包装还没有添加 promise 支持的函数。

## 静态方法

### `Promise.all(iterable)`
这个方法返回一个新的promise对象，该promise对象在iterable参数对象里所有的promise对象都成功的时候才会触发成功，一旦有任何一个iterable里面的promise对象失败则立即触发该promise对象的失败。这个新的promise对象在触发成功状态以后，会把一个包含iterable里所有promise返回值的数组作为成功回调的返回值，顺序跟iterable的顺序保持一致；如果这个新的promise对象触发了失败状态，它会把iterable里第一个触发失败的promise对象的错误信息作为它的失败错误信息。Promise.all方法常被用于处理多个promise对象的状态集合。

### `Promise.allSettled(iterable)`
等到所有promises都已敲定（settled）（每个promise都已兑现（fulfilled）或已拒绝（rejected））。
返回一个promise，该promise在所有promise完成后完成。并带有一个对象数组，每个对象对应每个promise的结果。

### `Promise.any(iterable)`
接收一个Promise对象的集合，当其中的一个promise成功，就返回那个成功的promise的值。

### `Promise.race(iterable)`
当iterable参数里的任意一个子promise被成功或失败后，父promise马上也会用子promise的成功返回值或失败详情作为参数调用父promise绑定的相应句柄，并返回该promise对象。

### `Promise.reject(reason)`
返回一个状态为失败的Promise对象，并将给定的失败信息传递给对应的处理方法

### `Promise.resolve(value)`
返回一个状态由给定value决定的Promise对象。如果该值是thenable(即，带有then方法的对象)，返回的Promise对象的最终状态由then方法执行决定；否则的话(该value为空，基本类型或者不带then方法的对象)，返回的Promise对象状态为fulfilled，并且将该value传递给对应的then方法。通常而言，如果您不知道一个值是否是Promise对象，使用Promise.resolve(value) 来返回一个Promise对象,这样就能将该value以Promise对象形式使用。

## Promise 原型

## 属性

### `Promise.prototype.constructor`
返回被创建的实例函数.  默认为 Promise 函数.

## 方法

### `Promise.prototype.catch(onRejected)`
添加一个拒绝(rejection) 回调到当前 promise, 返回一个新的promise。当这个回调函数被调用，新 promise 将以它的返回值来resolve，否则如果当前promise 进入fulfilled状态，则以当前promise的完成结果作为新promise的完成结果.

### `Promise.prototype.then(onFulfilled, onRejected)`
添加解决(fulfillment)和拒绝(rejection)回调到当前 promise, 返回一个新的 promise, 将以回调的返回值来resolve.

### `Promise.prototype.finally(onFinally)`
添加一个事件处理回调于当前promise对象，并且在原promise对象解析完毕后，返回一个新的promise对象。回调会在当前promise运行完毕后被调用，无论当前promise的状态是完成(fulfilled)还是失败(rejected)

