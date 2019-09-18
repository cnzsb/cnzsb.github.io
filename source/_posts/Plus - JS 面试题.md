title: 'Plus - JS 面试题'
date: 2017-02-06 16:16:59
tags: [JavaScript, 面试题]

---

> 设计一个 plus 方法，达到以下测试结果。
> 
> ```javascript
'use strict'
const assert = require('assert')
describe('闭包应用', function(){
  it('plus(0) === 0', function(){
    assert.equal(0, plus(0).toString())
  })
  it('plus(1)(1)(2)(3)(5) === 12', function(){
    assert.equal(12, plus(1)(1)(2)(3)(5).toString())
  })
  it('plus(1)(4)(2)(3) === 10', function(){
    assert.equal(10,plus(1)(4)(2)(3).toString())
  })
  it('方法引用', function(){
    var plus2 = plus(1)(1)
    assert.equal(12, plus2(1)(4)(2)(3).toString())
  })
})
```

<!-- more -->

看到本题，首先就会发现正常的函数操作只是暂时存储了所有参数，而 toString 方法才是用来真正的执行方法，因此在默认的行为中只需要利用闭包来存储数据即可，并不难理解。

```javascript
'use strict'

// 方法 1
function plusA(...num) {
  var args = []
  args.push(...num)
  var sum = function (...n) {
    args.push(...n)
    return sum
  }
  
  sum.toString = () => args.reduce((a, b) => a + b)
  return sum
}

console.log('方法一测试结果：')
test(plusA)

// 方法 2，内部通过柯里化来实现
function plusB(...num) {
  var sum = function () {
    var args = []
    var save = function (...n) {
      args.push(...n)
      return save
    }
    save.toString = () => args.reduce((a, b) => a + b)
    return save
  }
  return sum()(...num)
}

console.log('方法二测试结果：')
test(plusB)


function test(plus) {
  console.log(plus(0, 1).toString())
  console.log(12 === plus(1)(1)(2)(3)(5).toString())
  console.log(10 === plus(1)(4)(2)(3).toString())
  var plus2 = plus(1)(1)
  console.log(12 === plus2(1)(4)(2)(3).toString())
  
  console.log('多参数执行, plus(1, 2)(3, 4): ', plus(1, 2)(3, 4).toString())
}
```
