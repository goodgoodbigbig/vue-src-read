# VUE中的Cache实现

- 缓存实现

```js
export function cached<F: Function> (fn: F): F {
  const cache = Object.create(null) // 存储一类缓存数据的对象
  return (function cachedFn (str: string) {
    const hit = cache[str] // 优先从缓存中获取数据
    return hit || (cache[str] = fn(str)) // 缓存中没有则调用load函数取数据，并存储在cache对象中
  }: any)
}
```

- 创建缓存，并定义数据获取的方式
```js
const idToTemplate = cached(id => {
  const el = query(id)
  return el && el.innerHTML
})
```

- 调用的地方：
```js
template = idToTemplate(id)
```

---
举个例子，我们要缓存根据学生id查询出的学生名字。

```js

const allNames = {
    1: "小明",
    2: "小强"
}

// 缓存的实现
function cached(load) {
  const cache = Object.create(null)
  return (function cachedFn (key) {
    const value = cache[key]
    if(value) {
        console.log(`命中缓存key=${key}, value=${value}`)
    } else {
        cache[key] = load(key)
        console.log(`直接获取key=${key}, value=${cache[key]}`)
    }
    return cache[key]
  })
}

// 创建缓存
findByIdCache = cached(id => {
    return allNames[id]
})

findByIdCache(2)
findByIdCache(2)

// 结果：
// 直接获取key=2, value=小强
// 命中缓存key=2, value=小强

```




