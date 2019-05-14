# Regex
正则表达式

符号 | 含义 | 举例  |
----|----|----|
. | 匹配一个字符 | .n.wl.d.. 匹配 acknowledge
\| | 或者（优先级低| a\|b\|c 匹配a或者b或者c
\*  |0个或者多个字符| 
\+  |一个或者多个字符
？ | 0个或者一个字符
^  | 字符串的开始位置
$  | 字符串的结束位置
{n} | n个前面的字符
{n,m}| n到m个前面的字符
{n,} |至少n个前面的字符
{,m} |最多m个前面的字符
[aeiou]|任意字符中的一个, 在[]中^是取反不包括的意思;在[]除了^和-之外其他的特殊字符比如$不需要转义，用字符的字面意思| 相当于a\|e\|i\|o\|u; /[Kk]nowledge/; [^0-9]不包含数字
\d | 表示数字, 等价于[0-9]
\D | 表示非数字, 等价于[^0-9]
\w | 表示单词, 等价于[a-zA-Z0-9_]
\W | 对\w取反, 等价于[^a-zA-Z0-9]
\s | 表示空格, 等价于[\ \t\r\n\f]
\S | 对空格取反, 等价于[^\ \t\r\n\f]

### 练习

- 正整数: ^[1-9]\d*$
- 负整数: ^-[1-9]\d*$
- 整数: ^-?([1-9]\d*|0)$
- 浮点数: ^-?(0|[1-9]\d*)(\.)?(\d)*


1. 将”<tr><td>{id}</td><td>{name}</td></tr>”中的{id}替换成10，{name}替换成Tony

```js
var str = "<tr><td>{id}</td><td>{name}</td></tr>";
str.replace(/\{id\}/g, 10).replace(/\{name\}/, "Tony");
```

2.为了保证页面输出安全，我们经常需要对一些特殊的字符进行转义，请写一个函数escapeHtml，将<, >, &, “进行转义

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

3. 正则表达式的字面量和构造函数的异同
4. 清除字符串首尾的空格

```js
function trim(str) {
    return str.replace(/^\s+/, '').replace(/\s+$/, '');
}

console.log(trim('   this is a test  '));
```
5.编写一个JavaScript函数，输入指定类型的选择器(仅需支持id，class，tagName三种简单CSS选择器，无需兼容组合选择器)可以返回匹配的DOM节点

关键点写一个正则表达式判断传入的选择器类型

6. 以下函数的作用？空白处填写?

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

7. 匹配邮箱的正则表达式
