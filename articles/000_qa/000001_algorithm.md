# :thinking:数据结构与算法

[[toc]]

## 找出整型数组中乘积最大的三个数（leetcode#628 三个数的最大乘积）
>给定一个包含整数的无序数组，要求找出乘积最大的三个数。
解法：
* 暴力n3遍历（n3）
* 排序后求乘积 +++ 或 --+ （nlogn）
* hashmap

## 寻找连续数组中的缺失数（剑指 Offer 53 - II. 0～n-1中缺失的数字）
>给定某无序数组，其包含了 n 个连续数字中的 n - 1 个，已知上下边界，要求以O(n)的复杂度找出缺失的数字。
解法：
* 通过等差数列求和公式`(max + min) * n / 2`求总和减数组元素总和即缺失数字

## 数组去重
* hashmap
* Set
* 双循环
* Array.prototype.indexOf & Array.prototype.includes
* 排序后对比

## 数组中元素最大差值计算（leetcode#121 买卖股票的最佳时机）
>给定某无序数组，求取任意两个元素之间的最大差值，注意，这里要求差值计算中较小的元素下标必须小于较大元素的下标。譬如[7, 8, 4, 9, 9, 15, 3, 1, 10]这个数组的计算值是 11( 15 - 4 ) 而不是 14(15 - 1)，因为 15 的下标小于 1。
* 双循环求最小值
* 单次循环获取最小值并和后面的的n个数值求最小值

## 数组中元素乘积（leetcode#238 除自身以外数组的乘积）
>给定某无序数组，要求返回新数组 output ，其中 output[i] 为原数组中除了下标为 i 的元素之外的元素乘积，要求以 O(n) 复杂度实现
* 使用left和right数组分别存储顺序乘积和倒序乘积
* 利用输出数组和中间变量优化空间复杂度

## 数组交集（leetcode#349 两个数组的交集）
>给定两个数组，要求求出两个数组的交集，注意，交集中的元素应该是唯一的。
* 双集合
* 排序加双指针

## 颠倒字符串
>给定某个字符串，要求将其中单词倒转之后然后输出，譬如"Welcome to this Javascript Guide!" 应该输出为 "emocleW ot siht tpircsavaJ !ediuG"。
* split + reverse + join
* 双端队列

## 乱序同字母字符串
>给定两个字符串，判断是否颠倒字母而成的字符串，譬如Mary与Army就是同字母而顺序颠倒
* split + toLowerCase + sort

## 用栈实现队列（leetcode#232 用栈实现队列）
* 双栈

## 用队列实现栈（leetcode#225 用队列实现栈）
* 双队列
* 单队列

## 判断合法括号（leetcode#20 有效的括号）
* 栈 + 哈希表

## 二进制转换（leetcode#190 颠倒二进制位）

## 二分查找（leetcode#704 二分查找）
```js
// 急需优化
// 随便写的
function lookup(nums, target) {
  let len = nums.length;
  let start = 0;
  let end = len;

  while (start <= end) {
    if (nums[start] === target) {
      return start;
    } else if (nums[end] === target) {
      return end;
    }

    const mid = Math.ceil((end + start) / 2);

    if (nums[mid] > target) {
      if (end === mid) return -1;
      end = mid; 
    } else if (nums[mid] < target) {
      if (start === mid) return -1;
      start = mid
    } else if (nums[mid] === target) {
      return mid;
    } else {
      return -1;
    }
  }
}
```

## 判断是否为2的指数值
```js
(i & (i - 1)) === 0
```

## 介绍下深度优先遍历和广度优先遍历，如何实现？

## 请分别用深度优先思想和广度优先思想实现一个拷贝函数？

## 算法手写题
>已知如下数组： var arr = [ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10]; 编写一个程序将数组扁平化去并除其中重复部分数据，最终得到一个升序且不重复的数组

```js
function normalize(array) {
  return array.reduce((acc, cur) => {
    return Array.isArray(cur) ? [...acc, ...normalize(cur)] : [...acc, cur];
  }, []);
}
```

## 两个数组合并成一个数组
>请把两个数组 ['A1', 'A2', 'B1', 'B2', 'C1', 'C2', 'D1', 'D2'] 和 ['A', 'B', 'C', 'D']，合并为 ['A1', 'A2', 'A', 'B1', 'B2', 'B', 'C1', 'C2', 'C', 'D1', 'D2', 'D']。

## 实现 (5).add(3).minus(2) 输出 6 功能
```js
Number.prototype.add = function (num) {
  return this + num;
}
Number.prototype.minus = function (num) {
  return this - num;
}
```

## 冒泡排序如何实现，时间复杂度是多少，还可以如何改进？
* 时间复杂度O(n2)
* 循环一遍没有元素交换说明排序完成

## 某公司 1 到 12 月份的销售额存在一个对象里面
>如下：{1:222, 2:123, 5:888}，请把数据处理为如下结构：[222, 123, null, null, 888, null, null, null, null, null, null, null]
* 哈希表
* 自己折腾的方法
```js
function to(data, months) {
  const result = Array.prototype.slice.call({...Array(months + 1).fill(null), ...data, length: months + 1});
  result.shift();
  return result;
}
```

## 数组编程题
>随机生成一个长度为 10 的整数类型的数组，例如 [2, 10, 3, 4, 5, 11, 10, 11, 20]，将其排列成一个新数组，要求新数组形式如下，例如 [[2, 3, 4, 5], [10, 11], [20]]。

## 如何把一个字符串的大小写取反（大写变小写小写变大写），例如 ’AbC' 变成 'aBc' 。
* 利用正则替换每个字母转换成大写并和转换前比对是否一致，一致则转换成小写。

## 实现一个字符串匹配算法，从长度为 n 的字符串 S 中，查找是否存在字符串 T，T 的长度是 m，若存在返回所在位置。

## 旋转数组
```js
function ReverseArray(array) {
  let i = 0;
  while (2 * i <= array.length) {
    const temp = array[i];
    array[i] = array[array.length - 1 - i];
    array[array.length - 1 - i] = temp;

    i++
  }
  return array;
}
```

## 打印出 1~10000 之间的所有对称数

## 移动零
```js
function MoveZeros(array) {
  let j = 0;
  for (let i = 0; i < array.length; i ++) {
    if (array[i] !== 0) {
      const temp = array[j];
      array[j] = array[i];
      array[i] = temp;

      j++;
    }
  }
  return array;
}
```

## 两数之和
```js
function TwoSum(nums, target) {
  const map = new Map();
  for (let i = 0; i < nums.length; i ++) {
    if (map.has(target - nums[i])) {
      return [map.get(target - nums[i]), i];
    } else {
      map.set(nums[i], i);
    }
  }
  return -1;
}
```

## 在输入框中如何判断输入的是一个正确的网址

## 实现 `convert` 方法，把原始 list 转换成树形结构，要求尽可能降低时间复杂度

## 实现模糊搜索结果的关键词高亮显示
```js
const reg = new RegExp(search, 'ig');
const splitIconTypes = iconType.split(reg);
const matchIconTypes = iconType.match(reg); 
```

## 已知数据格式，实现一个函数 fn 找出链条中所有的父级 id

## 给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。请找出这两个有序数组的中位数。要求算法的时间复杂度为 O(log(m+n))

## 编程算法题
>用 JavaScript 写一个函数，输入 int 型，返回整数逆序后的字符串。如：输入整型 1234，返回字符串“4321”。要求必须使用递归函数调用，不能用全局变量，输入函数必须只有一个参数传入，必须返回字符串。

```js
function reverse(s) {
  s = '' + s;
  return s.length === 1 ? s : s[s.length - 1] + reverse(s.substr(0, s.length - 1));
}
```

## 修改以下 print 函数，使之输出 0 到 99，或者 99 到 0
>要求： 
>1. 只能修改 setTimeout 到 Math.floor(Math.random() * 1000 的代码 
>2. 不能修改 Math.floor(Math.random() * 1000 
>3. 不能使用全局变量
```js
function print(n){
  setTimeout((() => {
    console.log(n);
  })(), Math.floor(Math.random() * 1000));
}

for(var i = 0; i < 100; i++){
  print(i);
}
```

## 不用加减乘除运算符，求整数的7倍
```js
function bitAdd(m, n) {
  while (m) {
    [m, n] = [(m & n) << 1, m ^ n];
  }
  return n;
}

let multiply7 = (num) => bitAdd(num << 3, -num);
```

## 模拟实现一个 localStorage

## 模拟 localStorage 时如何实现过期时间功能

## 编程题
>url有三种情况
>https://www.xx.cn/api?keyword=&level1=&local_batch_id=&elective=&local_province_id=33
>https://www.xx.cn/api?keyword=&level1=&local_batch_id=&elective=800&local_province_id=33
>https://www.xx.cn/api?keyword=&level1=&local_batch_id=&elective=800,700&local_province_id=33
>匹配elective后的数字输出（写出你认为的最优解法）:
>[] || ['800'] || ['800','700']

## 考虑到性能问题，如何快速从一个巨大的数组中随机获取部分元素

## 编程题，请写一个函数，完成以下功能
>输入 '1, 2, 3, 5, 7, 8, 10' 输出 '1~3, 5, 7~8, 10'

## 编程题，写个程序把 entry 转换成如下对象
```js
var entry = {
  a: {
    b: {
      c: {
        dd: 'abcdd'
      }
    },
    d: {
      xx: 'adxx'
    },
    e: 'ae'
  }
}

// 要求转换成如下对象
var output = {
  'a.b.c.dd': 'abcdd',
  'a.d.xx': 'adxx',
  'a.e': 'ae'
}
```

```js
const result = {};
function convert(entry, path = '') {
  for (let [key, value] of Object.entries(entry)) {
    if (typeof value === 'object') {
      convert(value, !path ? path : path + '.' + key);
    } else {
      result[path + '.' + key] = value;
    }
  }
}
```

## 编程题，写个程序把 entry 转换成如下对象（跟昨日题目相反）
```js
var entry = {
  'a.b.c.dd': 'abcdd',
  'a.d.xx': 'adxx',
  'a.e': 'ae'
}

// 要求转换成如下对象
var output = {
  a: {
    b: {
      c: {
        dd: 'abcdd'
      }
    },
    d: {
      xx: 'adxx'
    },
    e: 'ae'
  }
}
```

```js
function convert(entry) {
  const result = {};
  for (let [key, value] of Object.entries(entry)) {
    const keys = key.split('.');
    let o = result;
    let len = keys.length;
    keys.forEach((key, index) => {
      if (!o(key)) {
        if (index !== len - 1) {
          o[key] = {};
        } else {
          o[key] = value;
        }
      }
      o = o[key];
    })
  }
  return result;
}
```

## 根据以下要求，写一个数组去重函数
>如传入的数组元素为[123, "meili", "123", "mogu", 123]，则输出：[123, "meili", "123", "mogu"]
>如传入的数组元素为[123, [1, 2, 3], [1, "2", 3], [1, 2, 3], "meili"]，则输出：[123, [1, 2, 3], [1, "2", 3], "meili"] 
>如传入的数组元素为[123, {a: 1}, {a: {b: 1}}, {a: "1"}, {a: {b: 1}}, "meili"]，则输出：[123, {a: 1}, {a: {b: 1}}, {a: "1"}, "meili"]

## 找出字符串中连续出现最多的字符和个数
>'abcaakjbb' => {'a':2,'b':2}
>'abbkejsbcccwqaa' => {'c':3}

## 写一个单向链数据结构的 js 实现并标注复杂度

## 统计 1~n 整数中出现 1 的次数

## 算法题
>如何将[{id: 1}, {id: 2, pId: 1}, ...] 的重复数组（有重复数据）转成树形结构的数组 [{id: 1, child: [{id: 2, pId: 1}]}, ...] （需要去重）

## 扑克牌问题
>有一堆扑克牌，将牌堆第一张放到桌子上，再将接下来的牌堆的第一张放到牌底，如此往复； 最后桌子上的牌顺序为： (牌底) 1,2,3,4,5,6,7,8,9,10,11,12,13 (牌顶)； 问：原来那堆牌的顺序，用函数实现。

## 实现一个 Dialog 类，Dialog可以创建 dialog 对话框，对话框支持可拖拽（腾讯）

## 用 setTimeout 实现 setInterval，阐述实现的效果与 setInterval 的差异

## 求两个日期中间的有效日期
>如 2015-2-8 到 2015-3-3，返回【2015-2-8 2015-2-9...】

## 算法题
>在一个字符串数组中有红、黄、蓝三种颜色的球，且个数不相等、顺序不一致，请为该数组排序。使得排序后数组中球的顺序为:黄、红、蓝。例如：红蓝蓝黄红黄蓝红红黄红，排序后为：黄黄黄红红红红红蓝蓝蓝。

## 反转链表，每 k 个节点反转一次，不足 k 就保持原有顺序

## 将 '10000000000' 形式的字符串，以每 3 位进行分隔展示 '10.000.000.000'

## 手写二进制转Base64

## 二分查找如何定位左边界和右边界

## 用最简洁代码实现 indexOf 方法

## 实现一个 normalize 函数，能将输入的特定的字符串转化为特定的结构化数据
