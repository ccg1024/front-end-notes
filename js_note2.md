### Window

> window端记录，接linux端。

### 正则表达式后续

利用表达式过滤敏感词(**替换**)，获取特定的部分(**提取**)等。

```js
// normal pasword match, more than 6 char, less then 12 char
/^[a-z0-9_-]{6,12}.test('some_password')

// meta char: like '[a-z]'
```

元字符提高了灵活性与匹配能力

- 边界符：开头与结尾必须是什么字符
- 量词：重复的次数
- 字符类（'\d',表示0-9）

**量词**

除了.,*,?,+。还有下面的几个更常用的。

- {n}: repeat n times
- {n,}: repeat n or more times
- {n,m}: repeat between n and m times. **do not have space after ','**

> 在中括号中的'^'表示取反，即除了什么，例如除了小写字母'[^a-z]'

**字符类**

| 预定类 | 说明 |
| --- | --- |
| \d | like [0-9] |
| \D | like [^0-9] |
| \w | like [A-Za-z0-9_] |
| \W | like [^A-Za-z0-9_] |
| \s | match any space(space, tab...etc) |
| \S | match any not space char |

#### 修饰符

- i: ignore, 忽略大小写
- g: global, 全局匹配(类似vim中的替换操作的g)

#### 通过正则替换

配合字符串函数实现

```js
str.replace(/RegExp/, '')

// 自己写的下划线与驼峰命名法转换
function toUpperCase(str) {
  const newStr = str.replace(/.+?(_.)+/g, ($1, $2) => {
    console.log('first: ', $1)  // _name_y, xj_c
    console.log('seconde: ', $2)  // _y, _c
    // the return value will replace $1 value in str
    return $1.replace($2, $2[1].toUpperCase())
  })
  console.log(newStr)
}
console.log('_name_yxj_com_')
toUpperCase('_name_yxj_com_')
// out: _nameYxjCom_
```

### 用户登录

登录成功后，用户名等数据存储在`localStorage`中。
