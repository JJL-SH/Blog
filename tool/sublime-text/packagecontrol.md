# 扩展安装源 packagecontrol.io 无法访问

- 打开命令面板 Ctrl (Command)+Shift+p
- 找到并打开：Preferences: Package Control sublime Settings - User
- 添加一行"channels"字段：

```
"channels":
[
  "https://raw.githubusercontent.com/HBLong/channel_v3_daily/master/channel_v3.json"
],
```

加载更快的方法

如果还是太慢您可以指定本地channel_v3.json地址，方法如下：

- 下载json文件：packagecontrol.io下载 或 GitHub下载地址
- 将上面"channels"字段改为：

```
"channels":
[
  "channel_v3.json所在目录/channel_v3.json"
],
```