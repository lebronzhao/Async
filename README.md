# Async



- each: 如果想对同一个集合中的所有元素都执行同一个异步操作。
- map: 对集合中的每一个元素，执行某个异步操作，得到结果。所有的结果将汇总到最终的callback里。与each的区别是，each只关心操作不管最后的值，而map关心的最后产生的值。



#### map （coll，iteratee，callback opt）

`coll`通过`iteratee`函数映射每个值来生成新的值集合,在`iteratee`被称为从一个项目`coll` ，当它已经完成处理回调。这些回调中的每一个都有两个参数：an `error`和已转换的项`coll`。如果 `iteratee`将错误传递给其回调，则主`callback`（对于 `map`函数）将立即被调用，并显示错误。

##### 参数：

| 名称         | 类型              | 描述                                       |
| ---------- | --------------- | ---------------------------------------- |
| `coll`     | 数组 \| 可替代 \| 目的 | 要迭代的集合。                                  |
| `iteratee` | 异步功能]           | 应用于每个项目的异步功能 `coll`。迭代应该用转换的项目完成。用（项目，回调）调用。 |
| `callback` | 功能              | 所有`iteratee` 函数完成后调用的回调，或发生错误。结果是一个转换项目的数组`coll`。用（err，results）调用。 |

##### 例

```
async.map(['file1','file2','file3'], fs.stat, function(err, results) {
    // results is now an array of stats for each file
});
```

#### mapLimit （coll，limit，iteratee，callback opt）

```
import mapLimit from 'async/mapLimit';
```

一样，[`map`](http://caolan.github.io/async/docs.html#map)但一次运行最多的`limit`异步操作。

#### mapSeries （coll，iteratee，callback opt）

```
import mapSeries from 'async/mapSeries';
```

与[`map`](http://caolan.github.io/async/docs.html#map)一次只运行一次异步操作相同。

#### mapValues （obj，iteratee，callback opt）

```
import mapValues from 'async/mapValues';
```

[`map`](http://caolan.github.io/async/docs.html#map)设计用于对象的亲戚。

##### 参数：

| 名称         | 类型                                       | 描述                                       |
| ---------- | ---------------------------------------- | ---------------------------------------- |
| `obj`      | 目的                                       | 要迭代的集合。                                  |
| `iteratee` | [异步功能](http://caolan.github.io/async/global.html) | 一个应用于每个值和键入的功能`coll`。迭代应该以转换的值作为结果来完成。用（值，键，回调）调用。 |
| `callback` | 功能 <可选>                                  | 所有`iteratee` 函数完成后调用的回调，或发生错误。`result`是一个新对象，由每个键组成`obj`，每个变换值位于右侧。用（err，result）调用。 |

#### mapValuesLimit （obj，limit，iteratee，callback opt）

```
import mapValuesLimit from 'async/mapValuesLimit';
```

一样，[`mapValues`](http://caolan.github.io/async/docs.html#mapValues)但一次运行最多的`limit`异步操作。

#### mapValuesSeries （obj，iteratee，callback opt）

```
import mapValuesSeries from 'async/mapValuesSeries';
```

与[`mapValues`](http://caolan.github.io/async/docs.html#mapValues)一次只运行一次异步操作相同。







```
/**
 * Created by dllo on 17/5/2.
 */
var async = require('async');

//async 提供了很多函数
//each 并行执行
//eachSeries 逐个执行

var class1217 = [{name:'文冠龙',age:999,delay:500},
                 {name:'李青',age:998,delay:200},
                 {name:'刁刁',age:997,delay:1000},
                 {name:'班长',age:996,delay:100},
                 {name:'于谦益',age:995,delay:1000}];

//each 并行

async.each(class1217,function (item,callback) {
        setImmediate(function () {
            var date = new Date();
            var time = date.getTime();
            console.log('each 1 :' + item.name + time);
            callback(null,item)
        },item.delay)

},function done(error) {
        console.log(error)
    }
);

//eachSeries 逐个执行,串行
async.eachSeries(class1217,function (item,callback) {
        setImmediate(function () {
            var date = new Date();
            var time = date.getTime();
            if(item.age === 996){
                callback('此人年龄是假的');
            }else{
                console.log('eachSeries 1 :' + item.name + time);
                callback(null,item)
            }

        },item.delay)

    },function done(error) {
        console.log(error)
    }
);
async.map(class1217,function (item,callback) {
        setImmediate(function () {
            var date = new Date();
            var time = date.getTime();
            if(item.age === 996){
                callback('此人是假的');
            }else{
                console.log('map 1 :' + item.name + time);
                callback(null,item)
            }

        },item.delay)

    },function done(error) {
        console.log(error)
    }
);
async.mapSeries(class1217,function (item,callback) {
        setImmediate(function () {
            var date = new Date();
            var time = date.getTime();
            if(item.age === 996){
                callback('此人是假的');
            }else{
                console.log('mapSeries :' + item.name + time);
                callback(null,item)
            }

        },item.delay)

    },function done(error) {
        console.log(error)
    }
);
```

each 1 :文冠龙1493725534450
each 1 :李青1493725534467
each 1 :刁刁1493725534468
each 1 :班长1493725534468
each 1 :于谦益1493725534468
null
eachSeries 1 :文冠龙1493725534469
map 1 :文冠龙1493725534469
map 1 :李青1493725534469
map 1 :刁刁1493725534469
此人是假的
map 1 :于谦益1493725534470
mapSeries :文冠龙1493725534470
eachSeries 1 :李青1493725534470
mapSeries :李青1493725534470
eachSeries 1 :刁刁1493725534471
mapSeries :刁刁1493725534471
此人年龄是假的
此人是假的