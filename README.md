# REAPER 社区机器人应答匹配

*reabot.csv* 文件是用于社区（QQ 群：243473647）自助应答机器人 **Reabot** 的关键词匹配字典。

- 建议使用文本编辑工具（例如 Atom，Notepad++ 等）来编辑字典。
- 不建议使用 WPS Office 编辑，因为 Ryusa 发现打开之后全是乱码。



## 用法

其实下载 *.csv* 文件看一下格式就会知道如何编辑啦。

如果自动回答的是多行内容，需要使用双引号`"`将多行内容包括起来，类似于编程语言里的`{}`。

目前关键字不支持正则表达式，多个关键字匹配同一条结果需分行。



### 触发规则

#### 完全匹配（full）

match_type 列的值，表示与 keyword 完全匹配时触发。

#### 相似匹配（part）

- match_1

  当完全匹配没有结果时，首先遍历 match_1 这一列的值，用来判断消息里是否包含 match_1 的词。

  每行可以有多个值，用英文逗号来分割。

  如用户的消息为：购买，match_1 值为 剁手,**购买**,打折，匹配到的是“购买”，故最后匹配到 keyword 为“剁手”的这一行。

  注意 match_1 每个值，包括每行逗号分割的值，必须独一无二。

- match_1_weight

  数字越大代表权重越高，用来应对消息里同时存在多个 match_1 词的情况。

  若无 match_1_weight 值，则直接返回 reply_content。

  如用户的消息为：视频教程，同时匹配到两列，但由于 教程 的权重较高，则优先匹配 教程 这一行。

- match_2

  也是用来判断是否包含 match_2 。
  
- corpus

  语料库，每个英文逗号分割各同一个意思的句子，用来做相似度比较。这里的语料越多，才有余量提高判断值，提高精确度。

  由于默认主体为 reaper，源消息已去除 reaper 字眼，此处不必再写。

  限定：简体中文，没有标点符号。

##### 流程总结

1. 判断消息是否与 keyword 完全相等。
2. 判断消息是否包含 match_1。
3. 根据 match_1_weight 选择对应的行。
4. 若有 match_2，再判断消息是否包含 match_2，用作更复杂的选择。
5. 根据匹配到某行 corpus 的值，来遍历判断相似度。若出现相似度大于 0.3 的句子则直接返回 reply_content。



