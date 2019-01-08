# 新建自定义代码片段

Tools-&gt;Developer-&gt;New Snippet…

```text
<snippet>
  <content><![CDATA[
// 这里写需要输出的内容
console.log(${1:this});
]]></content>
  <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
  <!-- <tabTrigger>这里输入你需要的快捷命令【例子：clg】</tabTrigger> -->
  <!-- Optional: Set a scope to limit where the snippet will trigger -->
  <!-- <scope>source.python</scope> -->
</snippet>
```

编辑完之后，保存到 /Packages/User 目录下面，直接按保存

注意：文件扩展名需要改成 .sublime-snippet ，否则 Sublime 无法识别这个文件是一个代码片段

