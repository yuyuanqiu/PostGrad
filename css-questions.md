# CSS 相关问题

## at-rules 规则`@identifier`

### 定义

- 指示 css 行为的语句
- 以@开头，后跟一个标识符，包括直到下一个分号/css 块的所有内容
  - `@identifier (rule)`

### 常用的@规则

#### @charset

定义：
- 指定样式表中使用的字符编码
- 必须是样式表的第一个元素，且前面不能有任意字符
- 不能被嵌套，多个该规则只使用第一个
- 不能在html元素或style中使用
- 在某些css属性使用非ASCII字符时非常有用，比如content
- 大小写不敏感
- 语法：`@charset "utf-8";`：必须使用双引号，且charset和"之间只能空一个空格

#### @import

定义：
- 用于导入其他样式表
- 必须先于除@charset规则之外的所有规则之前导入
- 语法：
  - `import url;`
    - url表示一个string或者uri
    - eg:
      - `@import 'common.css';`
      - `@improt url('common.css') print and (orientation: landscape)`
  - `import url list-of-media-queries;`
    - list-of-media-queries表示一个可用逗号分隔的媒体查询列表

#### @namespace

定义：
- 可以将通配、元素、属性选择器限制在指定的命名空间里的元素
- 只有在包含多个namespace的文档时才有用（html中的svg、mathml等）
- 必须在@charset和@import之后，在样式表中位于其他样式声明之前
- 可用于定义默认命名空间
- 可用于定义命名空间前缀

#### @media（可嵌套）

> 可嵌套@规则：表示可作为样式表的语句，也可用在条件规则组中

条件规则组：

- 可嵌套语句、规则、@规则
- 所指向的条件值为 Boolean 值，当为 true 时内部的语句生效

媒体查询：

- 用处：
  - 用于根据设备的大致类型和特征参数（屏幕分辨率/视窗宽度）修改网站/app
  - 通过@media/@import 规则装饰样式
  - 用`media=`属性为 style、link、source 等元素指定特定的媒体类型
- 语法：
  - 每个媒体查询语句由一个可选的`媒体类型`和任意数量的`媒体特征`表达式构成
  - 可使用多个逻辑操作符合并多条媒体查询语句
  - 媒体查询语句不区分大小写
- 注意：
  - 附加到 link 元素的媒体查询条件即使为 false，仍将下载该样式表（但不使用）

##### 媒体类型：

- 定义：
  - 描述设备的类别，可省略（除非使用 not/only 逻辑操作符修饰）
  - 隐式应用 all 类型
- 分类：
  - `all`：适用所有设备
  - `print`：适用于在打印预览模式下在屏幕上查看的分页材料和文档（打印机）
  - `screen`：适用于屏幕
  - `speech`：用于语音合成器

##### 媒体特性：

- 定义：
  - 描述了用户代理、输出设备、浏览环境的具体特征
  - 可选，负责测试特性是否存在、特性的值，
  - 每条媒体特性表达式必须用括号括起来
  - `某些查询特性是可以使用max/min前缀的，某些查询特性的值必须带有单位`
    - 为了更好的访问性，当使用长度时，应使用em（更好的调整文本大小）
  - 形式：`特性：特性值`，`特性`
  - 语法改进（第四版本）：
    - 对现代浏览器的支持，改进了max/min的语法
      - 除使用max/min前缀，也可直接使用 `<=`（max前缀），`>=`（min前缀）
      - 合并两个范围查询：`(30em <= width <= 50em)`：`(min-width: 30em) and (max-width: 50em)`
    - 使用not否定某个媒体特性：`(not(hover))`
    - 使用or（类似逗号）
    - 
- 分类：
  - `aspect-ratio`：视窗的宽高比
    - eg: media (aspect-ratio: 2/1)：视窗宽高比为 2:1
  - `height`：视窗的高
    - eg：media (max-height: 1000px)：当视窗的高在 1000 以内时
  - `device-height`：设备的高
    - eg: media (max-device-height: 1000px)：设备的高在 1000px 内
  - `grid`：网格屏幕（1）还是点阵屏幕（0）
    - eg：media (gird: 1)：当设备为网格屏幕时
  - `orientation`：屏幕的方向，纵向（portrait，高度>宽度），横向（landscape，宽度 > 高度）
  - `resolution`：像素密度（分辨率，dpi）
    - eg：media (max-resolution: 1000dpi)：设备的分辨率为 1000 以内时

逻辑操作符：

- 定义：
  - 使用not、and、only用于联合构造复杂的媒体查询，或使用逗号分隔多个媒体查询并组合成一个规则
- 分类：
  - `and`：每个查询都为真才执行，还用于将媒体特性和媒体类型结合在一起
  - `not`：
    - 用于否定媒体查询，不满足查询条件返回true
    - 在逗号分隔的查询列表中将否定应用了该查询的特定查询
    - 必须指定媒体类型
  - `only`：
    - 仅在整个查询匹配时才应用整个样式，对较早防止浏览器应用所选样式很有用，对现代浏览器无影响
    - 不使用only时，旧浏览器会直接忽略后面的查询
    - 必须指定媒体类型
  - `逗号`：
    - 将多个媒体查询合并为一个规则
    - 各个查询互不干扰（某查询为true，某查询为false时，为true的查询将应用样式）
    - 类似于or（或css选择器中的逗号）

@meida 定义：

- 基于一个/多个`媒体查询`的结果来应用样式表的一部分
- 可以指定一个媒体查询和一个 css 块
- 仅当媒体查询匹配时，该 css 块才会应用在文档中

##### 语法

#### @document（可嵌套）