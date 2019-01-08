# 计数器

参考网址:

[http://www.w3cplus.com/css3/css-counters.html](http://www.w3cplus.com/css3/css-counters.html)  
[http://www.webhek.com/css-counters](http://www.webhek.com/css-counters)  
[http://www.jiawin.com/css-counter-increment-counter-reset](http://www.jiawin.com/css-counter-increment-counter-reset)

```text
/**
 *   ul上设置的属性：
 *   counter-reset:[ <identifier> <integer>? ]+ | none | inherit;//标识计数器作用域(必需且必须用于选择器，值可以自定义）
 *      identifier:值必需，定义计数器的名称;
 *      integer:设置或重置调用计算数器时起始值，可以是正数、零或负数。
 *      none：默认。不能对选择器的计数器进行重置。
 *      inherit：规定应该从父元素继承 counter-reset 属性的值。
 *      //display: none无法重置计数器；visibility: hidden，则可重置计数器
 *      //counter-reset只是定义了一个计数器标识符，但没有指定用到哪。换句话说，这个计数器标识符，可以用到任何标签元素上。
 *      //如果要在某个标签上使用counter-reset声明好的计数器，需要使用counter-increment来显式设置
 *
 * ul>li上设置的属性：
 *   counter-increment:[ <identifier> <integer>? ]+ | none | inherit;//标识计数器与实际相关联元素范围。
 *      identifier:值必需，获取counter-reset定义的标识符；
 *      integer：可选，指定计数起始值，可以是正数、零或负数；如果未指定任何值，则该值为1（前提是counter-reset未显式设置计数的起始值）。其值递增是按倍数值递增，如果设置了值为2,后面元素递增值为4、6、8，依此类推
 *      none:默认。选择器无计数器增量。
 *      inherit: 规定应该从父元素继承 counter-increment 属性的值。
 *      //每遇到一个符合条件li元素，counter-increment就会被调用一次，计数就是增加1。
 *      //display: none无法增加计数；visibility: hidden，则可增加计数
 *
 * ul>li:before上设置的属性：
 *   content:string|url|counter(name)|counter(name, list-style-type)|counters(name, string)|counters(name, string, list-style-type)|attr(X)|open-quote|close-quote|no-open-quote|no-close-quote;
 *    //调用定义好的计数器标识符
 *   //counter()命令还可以接受第二个参数，两参数之间使用逗号(,)来分隔,当作同时计算多个元素时数据的分隔符
 *   //第一个参数是counter-increment定义的属性值<identifier>,用来告诉该文档插入的计数器标识符名称是什么。
 *   //第二个是用来设置计数器的风格，有点类似于list-style-type。默认情况之下取值为十制，但你也可以重置这个样式风格，比如upper-roman或者upper-alpha等。
 *
 * list-style-type可设置的值：（参考网址：http://www.w3school.com.cn/cssref/pr_list-style-type.asp）
 * list-style-type: disc | circle | square | decimal | decimal-leading-zero | lower-roman | upper-roman | lower-greek | lower-latin | upper-latin | armenian | georgian | none | inherit;
 *    disc:默认，实心圆;
 *    circle:空心圆;
 *    square:实心方块;
 *    decimal:数字;
 *    decimal-leading-zero:0开头的数字,如01、02、03...;
 *    lower-roman：小写罗马数字(i, ii, iii, iv, v, 等。
 *    upper-roman：大写罗马数字(I, II, III, IV, V, 等。
 *    lower-greek：小写希腊字母(alpha, beta, gamma, 等。
 *    lower-latin：小写拉丁字母(a, b, c, d, e, 等。
 *    upper-latin：大写拉丁字母(A, B, C, D, E, 等。
 *    lower-alpha：小写英文字母The marker is lower-alpha (a, b, c, d, e, 等。
 *    upper-alpha：大写英文字母The marker is upper-alpha (A, B, C, D, E, 等。
 *    armenian：传统的亚美尼亚编号方式
 *    georgian：传统的乔治亚编号方式(an, ban, gan, 等。
 *    none：无标记
 *    inherit;
 */

body{
  counter-reset: TopicalTitle;
}
section{
  width:100%;
  color:#aca277;
  font-size:4.5rem;
}
section h1{
  display:inline-block;
  border-bottom: 2px solid #7f632e;
  position:relative;
  counter-increment: TopicalTitle;
}
section h1:before{
  content: counter(TopicalTitle, upper-alpha);
  font-size:8.125rem;
  color:#8d6c3b;
  margin-right:2rem;
}
```

