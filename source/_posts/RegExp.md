---
title: JavaScript 正则表达式
date: 2017-12-04 11:41:36
tags: [RegExp, JavaScript]
categories: '编程'
---

{% cq %}
七言秋雨似情长,宫影空明人不往。  
智绝情深不由伤,音泣相思泪以裳。

**中二病也要谈恋爱**
{% endcq %}

<!-- more -->



什么是正则表达式
---

正则表达式是用于匹配字符串中字符组合的模式。正则表达式使用单个字符串来描述、匹配一系列匹配某个句法规则的字符串，简而言之，正则表达式就是对字符串执行模式匹配的强大工具。在 JavaScript 中，正则表达式也是对象。这些模式被用于 RegExp 的 exec 和 test 方法, 以及 String 的 match、replace、search 和 split 方法。



如何创建正则表达式
---

你可以使用以下两种方法之一构建一个正则表达式：

第一种，使用一个正则表达式字面量，其由包含在斜杠之间的模式组成，如下所示：
``` javascript
/*
   /pattern/flags 
*/

const regex = /ab+c/;

const regex = /^[a-zA-Z]+[0-9]*\W?_$/gi;
```
在加载脚本后，正则表达式字面值提供正则表达式的编译。当正则表达式保持不变时，使用此方法可获得更好的性能。

第二种，调用RegExp对象的构造函数，如下所示：
``` javascript
/* 
    new RegExp(pattern [, flags])
*/

let regex = new RegExp("ab+c");

let regex = new RegExp(/^[a-zA-Z]+[0-9]*\W?_$/, "gi");

let regex = new RegExp("^[a-zA-Z]+[0-9]*\\W?_$", "gi");
```
使用构造函数提供正则表达式的运行时编译。使用构造函数，当你知道正则表达式模式将会改变，或者你不知道模式，并从另一个来源，如用户输入。

**参数说明：**
- 参数 pattern 是一个字符串，指定了正则表达式的模式或其他正则表达式。
- 参数 attributes 是一个可选的字符串，包含属性 "g"、"i" 和 "m"，分别用于指定全局匹配、区分大小写的匹配和多行匹配。ECMAScript 标准化之前，不支持 m 属性。如果 pattern 是正则表达式，而不是字符串，则必须省略该参数。

<span style="color: red;">PS: 单看概念容易晕，最好结合具体的例子。</span>



特殊字符(规则)
---

字符类别（Character Classes）
<table>
  <tr>
    <th width="20%" style="text-align:center;">字符</th>
    <th style="text-align:center;">含义</th>
  </tr>
  <tr>
    <td style="text-align:center;">.</td>
    <td>(点号，小数点) 匹配任意单个字符，但是行结束符除外：\n \r \u2028 或 \u2029。在字符集中，点( . )失去其特殊含义，并匹配一个字面点( . )。需要注意的是，m 多行（multiline）标志不会改变点号的表现。因此为了匹配多行中的字符集，可使用[^] （当然你不是打算用在旧版本 IE 中），它将会匹配任意字符，包括换行符。<br>例如，/.y/ 匹配 "yes make my day" 中的 "my" 和 "ay"，但是不匹配 "yes"。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\d</td>
    <td>匹配任意阿拉伯数字。等价于[0-9]。<br>例如，/\d/ 或 /[0-9]/ 匹配 "B2 is the suite number." 中的 '2'。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\D</td>
    <td>匹配任意一个不是阿拉伯数字的字符。等价于[^0-9]。<br>例如，/\D/ 或 /[^0-9]/ 匹配 "B2 is the suite number." 中的 'B'。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\w</td>
    <td>匹配任意来自基本拉丁字母表中的字母数字字符，还包括下划线。等价于 [A-Za-z0-9\_]。<br>例如，/\w/ 匹配 "apple" 中的 'a'，"$5.28" 中的 '5' 和 "3D" 中的 '3'。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\W</td>
    <td>匹配任意不是基本拉丁字母表中单词（字母数字下划线）字符的字符。等价于 [^A-Za-z0-9\_]。<br>例如，/\W/ 或 /[^A-Za-z0-9\_]/ 匹配 "50%" 中的 '%'。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\s</td>
    <td>匹配一个空白符，包括空格、制表符、换页符、换行符和其他 Unicode 空格。等价于 [ \f\n\r\t\v​\u00a0\u1680​\u180e\u2000​\u2001\u2002​\u2003\u2004​ \u2005\u2006​\u2007\u2008​\u2009\u200a​\u2028\u2029​​\u202f\u205f​ \u3000]。<br>例如 /\s\w\*/ 匹配 "foo bar" 中的 ' bar'。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\S</td>
    <td>匹配一个非空白符。等价于 [^ \f\n\r\t\v​\u00a0\u1680​\u180e\u2000​\u2001\u2002​\u2003\u2004​ \u2005\u2006​\u2007\u2008​\u2009\u200a​\u2028\u2029​\u202f\u205f​\u3000]。<br>例如，/\S\w\*/ 匹配 "foo bar" 中的 'foo'。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\t</td>
    <td>匹配一个水平制表符（tab）</td>
  </tr>
  <tr>
    <td style="text-align:center;">\r</td>
    <td>匹配一个回车符（carriage return）</td>
  </tr>
  <tr>
    <td style="text-align:center;">\n</td>
    <td>匹配一个换行符（linefeed）</td>
  </tr>
  <tr>
    <td style="text-align:center;">\v</td>
    <td>匹配一个垂直制表符（vertical tab）</td>
  </tr>
  <tr>
    <td style="text-align:center;">\f</td>
    <td>匹配一个换页符（form-feed）</td>
  </tr>
  <tr>
    <td style="text-align:center;">[\b]</td>
    <td>匹配一个退格符（backspace）（不要与 \b 混淆）</td>
  </tr>
  <tr>
    <td style="text-align:center;">\0</td>
    <td>匹配一个 NUL 字符。不要在此后面跟小数点。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\cX</td>
    <td>X 是 A - Z 的一个字母。匹配字符串中的一个控制字符。<br>例如，/\cM/ 匹配字符串中的 control-M。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\xhh</td>
    <td>匹配编码为 hh （两个十六进制数字）的字符。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\uhhhh</td>
    <td>匹配 Unicode 值为 hhhh （四个十六进制数字）的字符。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\</td>
    <td>对于那些通常被认为字面意义的字符来说，表示下一个字符具有特殊用处，并且不会被按照字面意义解释。<br>例如 /b/ 匹配字符 'b'。在 b 前面加上一个反斜杠，即使用 /\b/，则该字符变得特殊，以为这匹配一个单词边界。<br>或对于那些通常特殊对待的字符，表示下一个字符不具有特殊用途，会被按照字面意义解释。<br>例如，\* 是一个特殊字符，表示匹配某个字符 0 或多次，如 /a\*/ 意味着 0 或多个 "a"。 为了匹配字面意义上的 \* ，在它前面加上一个反斜杠，例如，/a\\\*/匹配 'a\*'。</td>
  </tr>
</table>

字符集合（Character Sets）
<table>
  <tr>
    <th width="20%" style="text-align:center;">字符</th>
    <th style="text-align:center;">含义</th>
  </tr>
  <tr>
    <td style="text-align:center;">[xyz]</td>
    <td>一个字符集合，也叫字符组。匹配集合中的任意一个字符。你可以使用连字符'-'指定一个范围。<br>例如，[abcd] 等价于 [a-d]，匹配"brisket"中的'b'和"chop"中的'c'。</td>
  </tr>
  <tr>
    <td style="text-align:center;">[^xyz]</td>
    <td>一个反义或补充字符集，也叫反义字符组。也就是说，它匹配任意不在括号内的字符。你也可以通过使用连字符 '-' 指定一个范围内的字符。<br>例如，[^abc] 等价于 [^a-c]。 第一个匹配的是 "bacon" 中的'o' 和 "chop" 中的 'h'。</td>
  </tr>
</table>

边界（Boundaries）
<table>
  <tr>
    <th width="20%" style="text-align:center;">字符</th>
    <th style="text-align:center;">含义</th>
  </tr>
  <tr>
    <td style="text-align:center;">^</td>
    <td>匹配输入开始。如果多行（multiline）标志被设为 true，该字符也会匹配一个断行（line break）符后的开始处。<br>例如，/^A/ 不匹配 "an A" 中的 "A"，但匹配 "An A" 中的 "A"。</td>
  </tr>
  <tr>
    <td style="text-align:center;">$</td>
    <td>匹配输入结尾。如果多行（multiline）标志被设为 true，该字符也会匹配一个断行（line break）符的前的结尾处。<br>例如，/t$/ 不匹配 "eater" 中的 "t"，但匹配 "eat" 中的 "t"。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\b</td>
    <td>匹配一个零宽单词边界（zero-width word boundary），如一个字母与一个空格之间。 （不要和 [\b] 混淆）<br>例如，/\bno/ 匹配 "at noon" 中的 "no"，/ly\b/ 匹配 "possibly yesterday." 中的 "ly"。</td>
  </tr>
  <tr>
    <td style="text-align:center;">\B</td>
    <td>匹配一个零宽非单词边界（zero-width non-word boundary），如两个字母之间或两个空格之间。<br>例如，/\Bon/ 匹配 "at noon" 中的 "on"，/ye\B/ 匹配 "possibly yesterday." 中的 "y</td>
  </tr>
</table>

分组（Grouping）与反向引用（back references）
<table>
  <tr>
    <th width="20%" style="text-align:center;">字符</th>
    <th style="text-align:center;">含义</th>
  </tr>
  <tr>
    <td style="text-align:center;">(x)</td>
    <td>匹配 x 并且捕获匹配项。 这被称为捕获括号（capturing parentheses）。<br>例如，/(foo)/ 匹配且捕获 "foo bar." 中的 "foo"。被匹配的子字符串可以在结果数组的元素 [1], ..., [n] 中找到，或在被定义的 RegExp 对象的属性 $1, ..., $9 中找到。捕获组（Capturing groups）有性能惩罚。如果不需再次访问被匹配的子字符串，最好使用非捕获括号（non-capturing parentheses）</td>
  </tr>
  <tr>
    <td style="text-align:center;">\n</td>
    <td>n 是一个正整数。一个反向引用（back reference），指向正则表达式中第 n 个括号（从左开始数）中匹配的子字符串。<br>例如，/apple(,)\sorange\1/ 匹配 "apple, orange, cherry, peach." 中的 "apple,orange,"。</td>
  </tr>
  <tr>
    <td style="text-align:center;">(?:x)</td>
    <td>匹配 x 不会捕获匹配项。这被称为非捕获括号（non-capturing parentheses）。匹配项不能够从结果数组的元素 [1], ..., [n] 或已被定义的 RegExp 对象的属性 $1, ..., $9 再次访问到。</td>
  </tr>
</table>

数量词（Quantifiers）
<table>
  <tr>
    <th width="20%" style="text-align:center;">字符</th>
    <th style="text-align:center;">含义</th>
  </tr>
  <tr>
    <td style="text-align:center;">x\*</td>
    <td>匹配前面的模式 x 0或多次。<br>例如，/bo\*/ 匹配 "A ghost booooed" 中的 "boooo"，"A bird warbled" 中的 "b"，但是不匹配 "A goat grunted"。</td>
  </tr>
  <tr>
    <td style="text-align:center;">x+</td>
    <td>匹配前面的模式 x 1或多次。等价于 {1,}。<br>例如，/a+/ 匹配 "candy" 中的 "a"，"caaaaaaandy" 中所有的 "a"。</td>
  </tr>
  <tr>
    <td style="text-align:center;">x\*?、x+?</td>
    <td>像上面的 \* 和 + 一样匹配前面的模式 x，然而匹配是最小可能匹配<span style="color: red;">（即非贪婪模式，不加?就是贪婪模式）</span>。<br>例如，/".\*?"/ 匹配 '"foo" "bar"' 中的 '"foo"'，而 \* 后面没有 ? 时匹配 '"foo" "bar"'。</td>
  </tr>
  <tr>
    <td style="text-align:center;">x?</td>
    <td>匹配前面的模式 x 0或1次。<br>例如，/e?le?/ 匹配 "angel" 中的 "el"，"angle" 中的 "le"。如果在数量词 \*、+、? 或 {}, 任意一个后面紧跟该符号（?），会使数量词变为非贪婪（ non-greedy） ，即匹配次数最小化。反之，默认情况下，是贪婪的（greedy），即匹配次数最大化。</td>
  </tr>
  <tr>
    <td style="text-align:center;">x?</td>
    <td>匹配前面的模式 x 0或1次。<br>例如，/e?le?/ 匹配 "angel" 中的 "el"，"angle" 中的 "le"。如果在数量词 \*、+、? 或 {}, 任意一个后面紧跟该符号（?），会使数量词变为非贪婪（ non-greedy） ，即匹配次数最小化。反之，默认情况下，是贪婪的（greedy），即匹配次数最大化。</td>
  </tr>
  <tr>
    <td style="text-align:center;">x(?=y)、x(?!y)</td>
    <td>这两个放在断言（Assertions）里面讲</td>
  </tr>
  <tr>
    <td style="text-align:center;">x|y</td>
    <td>匹配 x 或 y <br>例如，/green|red/ 匹配 "green apple" 中的 ‘green'，"red apple." 中的 'red'。</td>
  </tr>
  <tr>
    <td style="text-align:center;">x{n}</td>
    <td>n 是一个正整数。前面的模式 x 连续出现 n 次时匹配。<br>例如，/a{2}/ 不匹配 "candy," 中的 "a"，但是匹配 "caandy," 中的两个 "a"，且匹配 "caaandy." 中的前两个 "a"。</td>
  </tr>
  <tr>
    <td style="text-align:center;">x{n,}</td>
    <td>n 是一个正整数。前面的模式 x 连续出现至少 n 次时匹配。<br>例如，/a{2,}/ 不匹配 "candy" 中的 "a"，但是匹配 "caandy" 和 "caaaaaaandy." 中所有的 "a"。</td>
  </tr>
  <tr>
    <td style="text-align:center;">x{n,m}</td>
    <td>n 和 m 为正整数。前面的模式 x 连续出现至少 n 次，至多 m 次时匹配。<br>例如，/a{1,3}/ 不匹配 "cndy"，匹配 "candy," 中的 "a"，"caandy," 中的两个 "a"，匹配 "caaaaaaandy" 中的前面三个 "a"。注意，当匹配 "caaaaaaandy" 时，即使原始字符串拥有更多的 "a"，匹配项也是 "aaa"。</td>
  </tr>
</table>

断言（Assertions）
<table>
  <tr>
    <th width="20%" style="text-align:center;">字符</th>
    <th style="text-align:center;">含义</th>
  </tr>
  <tr>
    <td style="text-align:center;">x(?=y)</td>
    <td>仅匹配被y跟随的x。<br>举个例子，/Jack(?=Sprat)/，如果"Jack"后面跟着sprat，则匹配之。/Jack(?=Sprat|Frost)/ ，如果"Jack"后面跟着"Sprat"或者"Frost"，则匹配之。但是，"Sprat" 和"Frost" 都不会在匹配结果中出现。</td>
  </tr>
  <tr>
    <td style="text-align:center;">x(?!y)</td>
    <td>仅匹配不被y跟随的x。<br>举个例子，/\d+(?!\\\.)/ 只会匹配不被点（.）跟随的数字。/\d+(?!\\\.)/.exec('3.141') 匹配"141"，而不是"3.141"</td>
  </tr>
</table>



对象方法(使用)
---

正则表达式可以被用于 RegExp 的exec和test方法以及 String 的match、replace、search和split方法。
<table>
  <tr>
    <th width="20%" style="text-align:center;">方法</th>
    <th style="text-align:center;">描述</th>
  </tr>
  <tr>
    <td style="text-align:center;">exec</td>
    <td>一个在字符串中执行查找匹配的RegExp方法，它返回一个数组（未匹配到则返回null）。[传送门](http://www.w3school.com.cn/jsref/jsref_exec_regexp.asp)</td>
  </tr>
  <tr>
    <td style="text-align:center;">test</td>
    <td>一个在字符串中测试是否匹配的RegExp方法，它返回true或false。[传送门](http://www.w3school.com.cn/jsref/jsref_test_regexp.asp)</td>
  </tr>
  <tr>
    <td style="text-align:center;">match</td>
    <td>一个在字符串中执行查找匹配的String方法，它返回一个数组或者在未匹配到时返回null。[传送门](http://www.w3school.com.cn/jsref/jsref_match.asp)</td>
  </tr>
  <tr>
    <td style="text-align:center;">search</td>
    <td>一个在字符串中测试匹配的String方法，它返回匹配到的位置索引，或者在失败时返回-1。[传送门](http://www.w3school.com.cn/jsref/jsref_search.asp)</td>
  </tr>
  <tr>
    <td style="text-align:center;">replace</td>
    <td>一个在字符串中执行查找匹配的String方法，并且使用替换字符串替换掉匹配到的子字符串。[传送门](http://www.w3school.com.cn/jsref/jsref_replace.asp)</td>
  </tr>
  <tr>
    <td style="text-align:center;">split</td>
    <td>一个使用正则表达式或者一个固定字符串分隔一个字符串，并将分隔后的子字符串存储到数组中的String方法。[传送门](http://www.w3school.com.cn/jsref/jsref_split.asp)</td>
  </tr>
</table>

这些方法详细的说明和使用方式建议点击表格中的传送门，到 W3C 中查看。



参考文献
---

- [MDN文档——RegExp](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
- [MDN文档——正则表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)
- [W3C——JavaScript RegExp 对象](http://www.w3school.com.cn/jsref/jsref_obj_regexp.asp)
- [慕课网——JavaScript正则表达式](http://www.imooc.com/learn/706)