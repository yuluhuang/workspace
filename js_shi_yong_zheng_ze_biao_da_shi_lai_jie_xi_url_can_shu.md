# JS 使用正则表达式来解析 URL 参数

http://happycoder.net/parse-querystring-using-regexp/


###将 url 中的参数解析成一个对象的需求在日常的开发中比较常见，如，将 `window.location.search` 解析成一个对象。有很多方法来实现这个功能，比较常见也比较容易理解的方法是先用 `String.prototype.split()` 按 `&` 来拆分成参数对，然后再按`=` 拆分每个参数对，组装成对象返回。

下面来分享一个用正则表达式来解析 url 请求参数的方法：

##首先是要构造出正则。

以 `window.location.search` 为例，它的值通常为 `?foo=1&bar=2&baz=3`，根据具体情况会有所不同。观察其组成不难发现，它以 `?` 开头，后跟多个参数对`name=value`，多个参数对之间用 `&` 连接。现在关键是要构造出参数对的正则，而这部分又由 `name`, `=` 和 `value` 组成。

OK，那接下来就是要一步一步构造出这个正则：

`name` 和 `value` 的正则 
为了简单起见，我们假设 `name` 和 `value` 可以为除了 `?`, `=`, `&` 之外的任意字符。那么它们的正则就可以表示为：`/[^?&=]+/`

`=` 它的正则就是 `=` 自身

将这三部分的正则组装成参数对`name=value`的正则

根据实际情况，参数对可能存在以下多种形式：

`param=value`
`param=`
`param`
这部分的正则构造出来就是这个样子：`/[^?&=]+(?:=[^?&=]*)*/`，为了方便后面对 `param` 和 `value` 进行提取，我们将其进行分组: `/([^?&=]+)(?:=([^?&=]*))*/`

构造出 `window.location.search` 的正则 
实际情况是，我们没有必要完全匹配出整个 search 我们只要匹配出其中的参数对就可以了，因此，可以写成这样：`/(([^?&=]+)(?:=([^?&=]*))*)/g`


``` javascript
function parseQueryString(str) {
    var reg = /(([^?&=]+)(?:=([^?&=]*))*)/g;
    var result = {};
    var match;
    var key;
    var value;
    while (match = reg.exec(str)) {
        key = match[2];
        value = match[3] || '';
        result[key] = decodeURIComponent(value);
    }
    return result;
}
```
测试代码
``` javascript

function runTest() {
    var fixtures = [
        {
            input: '?foo',
            expected: {foo: ''}
        }, {
            input: '?foo=',
            expected: {foo: ''}
        }, {
            input: '?foo=123',
            expected: {foo: '123'}
        }, {
            input: '?foo=123&',
            expected: {foo: '123'}
        }, {
            input: '?foo=123&bar',
            expected: {foo: '123', bar: ''}
        }, {
            input: '?foo=123&bar=',
            expected: {foo: '123', bar: ''}
        }, {
            input: '?foo=123&bar=456',
            expected: {foo: '123', bar: '456'}
        }, {
            input: '?foo&bar=456',
            expected: {foo: '', bar: '456'}
        }, {
            input: '?foo=&bar=456',
            expected: {foo: '', bar: '456'}
        }, {
            input: '?foo&bar',
            expected: {foo: '', bar: ''}
        }, {
            input: '?foo=&bar=',
            expected: {foo: '', bar: ''}
        }, {
            input: '?foo=123&bar=456&baz=789',
            expected: {foo: '123', bar: '456', baz: '789'}
        }
    ];
    fixtures.forEach(function (item) {
        var result = parseQueryString(item.input);
        var output = JSON.stringify(result);
        var expected = JSON.stringify(item.expected);
        console.log(
            output === expected,
            'The input string is:', item.input,
            'The actual is:', result, 
            'The expected is:', item.expected
        );
    }); 
}
// Run the test case
runTest();
```