# Regex

> 字符类别

符号 | 含义 | 举例  |
----|----|----|
. | 匹配一个字符 | .n.wl.d.. 匹配 acknowledge
\d | 表示数字, 等价于[0-9]
\D | 表示非数字, 等价于[^0-9]
\w | 匹配任意来自基本拉丁字母表中的字母数字字符，还包括下划线, 等价于[a-zA-Z0-9_];/\w/ 匹配 "apple" 中的 'a'，"$5.28" 中的 '5' 和 "3D" 中的 '3'
\W | 匹配任意不是基本拉丁字母表中单词（字母数字下划线）字符的字符。等价于 [^A-Za-z0-9_]。匹配 "50%"中的%
\s | 表示空格, 等价于[\ \t\r\n\f]
\S | 对空格取反, 等价于[^\ \t\r\n\f]

> 字符集合

符号 | 含义 | 举例  |
----|----|----|
[aeiou]|它匹配任意在括号内的字符, 可以使用-来指定范围。在[]中^是取反不包括的意思;在[]除了^和-之外其他的特殊字符比如$不需要转义，用字符的字面意思| 相当于a\|e\|i\|o\|u; /[Kk]nowledge/; [^0-9]不包含数字

> 边界

符号 | 含义 | 举例  |
----|----|----|
^  | 匹配输入开始 | /^A/ 不匹配 "an A" 中的 "A"，但匹配 "An A" 中的 "A"
$  | 匹配输入结束 | /t$/ 不匹配 "eater" 中的 "t"，但匹配 "eat" 中的 "t"

> 分组和反向引用

符号 | 含义 | 举例  |
----|----|----|
(x)|将匹配分组，可以在匹配的结果中按顺序访问分组结果
\n|通过制定\1 \2来指向正则表达式中的第一个第二个括号

> 数量词

符号 | 含义 | 举例  |
----|----|----|
\| | 或者（优先级低| a\|b\|c 匹配a或者b或者c
\*  |0个或者多个字符| 
\+  |一个或者多个字符
？ | 0个或者一个字符
{n} | n个前面的字符
{n,m}| n到m个前面的字符
{n,} |至少n个前面的字符
{,m} |最多m个前面的字符

### 关键点

- 以下字符在正则表达式中有特殊含义，需要转义: { } [ ] ( ) ^ $ . | * + ? $ \ 从而使他们使用自己的字面量意思
e.g. /\\$/匹配字符$;  / \\\ /匹配字符\

- 在[]中上述的特殊字符是自己的字面量意思，不需要转义
e.g. /[\$]/ 匹配\或者$

### JS中涉及到regex的函数

> Regex对象自带的方法
- exec: 返回null或者数组; result[0]表示捕获到的字符串, result[1...n]表示括号中的分组捕获
```js
var re = /quick\s(brown).+?(jumps)/ig;
var result = re.exec('The Quick Brown Fox Jumps Over The Lazy Dog');
// ["Quick Brown Fox Jumps", "Brown", "Jumps", index: 4, input: "The Quick Brown Fox Jumps Over The Lazy Dog", groups: undefined]
```
- test: 判断某个字符串是否匹配这个正则表达式

```js
/abc/.test('test') // false
```
> String.prototype.replace
- 第一个参数是一个正则表达式，或者一个要被替换的字符串；第二个参数可以是替换的字符串或者函数;
- 第二个参数是函数的情况，参数依次是match(匹配到的字符串), P1...Pn表示匹配到的分组， offset匹配到的子串在原始位置的偏移量, str原始字符串;参见第6题
- 第二个参数是字符串的时候，也可以是用$n来指定分组匹配的结果

举例: 使用replace来交换两个字符串的位置
```js
foo bar".replace(/(\w+)\s+(\w+)/, "$2 $1")
```

### 练习

- 正整数: ^[1-9]\d*$
- 负整数: ^-[1-9]\d*$
- 整数: ^-?([1-9]\d*|0)$
- 浮点数: ^-?(0|[1-9]\d*)(\.)?(\d)*


#### 1. 将”<tr><td>{id}</td><td>{name}</td></tr>”中的{id}替换成10，{name}替换成Tony

```js
var str = "<tr><td>{id}</td><td>{name}</td></tr>";
str.replace(/\{id\}/g, 10).replace(/\{name\}/, "Tony");
```

#### 2. 为了保证页面输出安全，我们经常需要对一些特殊的字符进行转义，请写一个函数escapeHtml，将<, >, &, “进行转义

符号|字符实体|
----|----
<   | \&lt;
\>    | \&gt;
&    | \&amp;
"     | \&quot;
'   | \ &apos;

```js
function escapeHTML(str) {
    return str.replace(/[<>&"]/g, function(match) {
        switch (match) {
            case '<':
                return '&lt;';
            case '>':
                return '&gt;';
            case '&':
                return '&amp;';
            case '"':
                return '&quot;'
        }
    });
}

console.log(escapeHTML('<test>"hah&you'));
// &lt;test&gt;&quot;hah&amp;you
```

#### 3. 正则表达式的字面量和构造函数的异同

- new Regex() 接受两个参数，第一个是一个字符串表示的正则，第二个制定匹配的模式。由于第一个参数是字符串，因此可以动态生成，适用于提前不能确定正则表达式的情况
- 字面量提供正则表达式的编译状态，构造函数是运行时编译。在循环中使用字面量构造一个表达式的时候，正则表达式不会在每次迭代中重新编译
- 构造函数创造正则对象时，需要常规的字符转义规则（在前面加反斜杠 \)

new RegExp("\\\w+") 等价于 /\w+/
new RegExp("\w+") 等价于 /w+/
new RegExp("\w") 等价于 /w/

### 4. 清除字符串首尾的空格

```js
function trim(str) {
    return str.replace(/^\s+/, '').replace(/\s+$/, '');
}

console.log(trim('   this is a test  '));
```
### 5.编写一个JavaScript函数，输入指定类型的选择器(仅需支持id，class，tagName三种简单CSS选择器，无需兼容组合选择器)可以返回匹配的DOM节点

关键点写一个正则表达式判断传入的选择器类型
- /^#\w+$/ 匹配id
- /^\.\w+$/ 匹配类名
- /^\w+$/ 匹配标签
- /^(#)?(\.)?(\w+)$/ : 利用分组，这样可以从result[1] result[2] result[3]中拿到匹配结果 


### 6. 以下函数的作用？空白处填写?

```js
(function(window) {
    function fn(str) {
        this.str = str;
    }
 
    fn.prototype.format = function() {
        var arg = ______;
        return this.str.replace(_____, function(a, b) {
            return arg[b] || "";
        });
    }
    window.fn = fn;
})(window);
 
//use
(function() {
    var t = new fn('<p><a href="{0}">{1}</a><span>{2}</span></p>');
    console.log(t.format('http://www.alibaba.com', 'Alibaba', 'Welcome'));
})();
```
- var arg = arguments; 获取输入的所有参数
- replace的第一个参数填写一个正则表达式 /\{(\d)\}/g: \d需要用括号括起来表示分组，这样在replace第二个函数中的第二个参数才表示匹配到的分组结果
- 作用是将字符串中的类似{0}的字符串替换成输入的内容，返回格式化后的字符串

### 7. 匹配邮箱的正则表达式

以网易注册邮箱的时候填写地址的规则为例"6~18个字符，可使用字母、数字、下划线，需以字母开头"
/^[a-zA-Z](\w){5,18}@[a-z0-9.]{3,6}$/
