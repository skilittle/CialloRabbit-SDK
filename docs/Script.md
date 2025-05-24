# JSON主结构
```json
{
    "uuid": "...",
    "name": "...",
    "version": "yy-mm-dd",
    "describe": "...",
    "story": {
        ...
    }
}
```

| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `uuid` | `uuid字符串` | 脚本的唯一uuid |  |
| `name` | `名称字符串` | 脚本名 |  |
| `version` | `i.i.i 格式的版本号` | 版本号 |  |
| `describe` | `描述字符串` | 描述 |  |
| `date` | `yy-mm-dd格式的日期` | 发布日期 |  |
| `story` | `{" [storyId] ": {...}}` | <b><a href="#main:story">剧情主内容</a></b> |  |

<br />


## story <span id="main:story">剧情主内容节点</span>

| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `type` | `字符串` | <b><a href="#main:story:type">场景类型</a></b> |  |
| `label` | `"label":[...]` | <b><a href="#main:story:label">场景标签对象</a></b> | 非必须 |
| `action` | `"action":{...}` | <b><a href="#main:story:action">执行程序对象</a></b> | 非必须 |
| `next` | `"next": {...}` | <b><a href="#main:story:next">下一个场景对象</a></b> |  | 
| `dialog` | `"dialog": {...}` | <b><a href="#main:story:dialog">对话框对象</a></b> | 非必须 |
| `audio` | `"audio": {...}` | <b><a href="#main:story:audio">音频对象</a></b> | 非必须 |
| `scene` | `"scene": {...}` | <b><a href="#main:story:scene">画面场景对象</a></b> | 非必须 |

<br />

### type <span id="main:story:type">场景类型</span>
场景可以是以下类型
| 值 | 说明 |
| --- | --- |
| `renderer` | 此场景是渲染画面场景 |
| `video` | 此场景是视频画面场景 |
| `command` | 此场景仅执行命令,不渲染场景 |

<br />

### label <span id="main:story:label">场景标签对象</span>
以数组的形式存储着场景标签参考以下的标签
| 值 | 说明 |
| --- | --- |
| `log` | 此场景的对话内容会记录在记录对象里 |


<br />

### action <span id="main:story:action">执行程序对象</span>
执行一个js的内部程序,并传入参数或条件
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `method` | `方法名` | 在js对象里定义的方法名 | 不包含括号 |
| `val` | `传入参数` | 调用方法后传入的参数 | 非必须 |

<br />

### next <span id="main:story:next">下一个场景对象</span>
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `title` | `标题字符串` | 此参数仅用于备注分支的标题 | 非必须 |
| `type` | `scene` | <b><a href="#main:story:next:type">下一个场景处理类型</a></b> |  |
| `mode` | `default` `select` | <b><a href="#main:story:next:mode">处理模式</a></b> | <font color="red">废弃</font> |
| `console` | `true` `false` | <b><a href="#main:story:next:console">是否允许控制</a></b> | 非必须 |
| `sleep` | `毫秒` | <b><a href="#main:story:next:console">延迟允许操作时间</a></b> | 非必须 |
| `duration` | `毫秒` | <b><a href="#main:story:next:duration">本场景持续时间</a></b> | 非必须 |
| `to` | `"@[脚本名]*[场景id]"` | <b><a href="#main:story:next:to">下一个场景目标</a></b> |  |
| `select` | `"select":[{...}]` | <b><a href="#main:story:next:select">分支选择对象</a></b> |  |
| `condition` | `"condition":[{...}]` | <b><a href="#main:story:next:condition">条件分支对象</a></b> |  |

<br />

#### type <span id="main:story:next:type">下一个场景处理类型</span>
下一个场景目标的执行类型 
| 值 | 说明 |
| --- | --- |
| `scene` | 继续播放剧情场景 |
| `game` | 调用小游戏 |
| `function` | 调用程序方法 |

<br />

#### mode <span id="main:story:next:mode">处理模式</span>
跳转到下个场景的处理模式,此参数<font color='red'>废弃</font>,后续将通过判断是否存在`select`对象或者`console`或者`condition`参数决定模式
| 值 | 说明 |
| --- | --- |
| `default` | 直接过渡到下一个场景 |
| `select` | 显示剧情分支选择菜单 |
| `condition` | 使用条件判断 |

<br />

#### console <span id="main:story:next:console">是否允许控制</span>
允许用户操作或者直接跳过本次场景,当`mode`为`select`时此参数决定是否要用户操作后才显示选择菜单 
如果为真,当存在`select`对象时用户需要操作一次才会显示选择菜单否则此场景播放结束后会立即显示选择菜单

默认值:`true`

<br />

#### sleep <span id="main:story:next:sleep">延迟允许用户操作</span>
在这个时间后用户才能操作,`console`为`true`有效

默认值:`0`

<br />

#### duration <span id="main:story:next:duration">本场景持续时间</span>
本场景在此时间后,自动进入下一个场景,不论用户有没有操作以及过渡是否播放完

默认值:`0`

<br />

#### to <span id="main:story:next:to">下一个场景目标</span>
以场景描述格式`"@[脚本名]*[场景id]"`表示下一个场景的目标
例如`"@main*test"`表示脚本名为`main`,场景id为`test`
此参数和`select`对象只能存在一个,当同时存在时`select`不生效

<br />

#### select <span id="main:story:next:select">分支选择对象</span> 
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `title` | `选项标题字符串` | 显示在选择菜单的标题 |  |
| `to` | `"@[脚本名]*[场景id]"` | 选项对应的场景目标 | <b><a href="#main:story:next:to">格式与to相同</a></b> |

<br />

#### condition <span id="main:story:next:condition">条件分支对象</span> 
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `eval` | `判断语句` | 使用js的判断语句格式 |  |
| `to` | `"@[脚本名]*[场景id]"` | 选项对应的场景目标 | <b><a href="#main:story:next:to">格式与to相同</a></b> |

<br />

### dialog <span id="main:story:dialog">对话框对象</span>
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `title` | `标题字符串` `@[语言文件]*[行ID]` | <b><a href="#main:story:dialog:title">对话框的标题文本</a></b> | 非必须 |
| `text` | `对话字符串` `@[语言文件]*[行ID]` `[行ID]` | <b><a href="#main:story:dialog:text">对话的内容</a></b> |  |
| `sleep` | `毫秒` | <b><a href="#main:story:dialog:sleep">延迟显示时间</a></b> | 非必须 |
| `display` | `true` `false` | 是否显示对话框 | <font color='red'>废弃</font> |
| `endHide` | `true` `false` | <b><a href="#main:story:dialog:endHide">结束隐藏对话框</a></b> | 非必须 |

<br />

#### title <span id="main:story:dialog:title">对话框的标题文本</span>
可以直接输入显示的文本或语言文件描述格式`@[语言文件]*[行ID]`来决定显示对话框的标题
例如`@common*Dy88165533`表示对应语言下`common`文件的`Dy88165533`行
为空时显示空标题,如此对象不存在,则不显示标题

默认值:无

<br />

#### text <span id="main:story:dialog:text">对话字符串</span>
可以直接输入显示的文本或语言文件描述格式`@[语言文件]*[行ID]`或直接输入`[行ID]`者来决定显示对话框的字幕内容
与`title`不同的是,如果仅输入行ID,默认使用`story`语言文件
例如`@common*Dy88165533`表示对应语言下`common`文件的`Dy88165533`行
例如`Dy88165533`表示对应语言下`story`文件的`Dy88165533`行

<br />

#### sleep <span id="main:story:dialog:sleep">延迟显示时间</span>
设置对话框在此时间后才显示

默认值:`0`

<br />

#### endHide <span id="main:story:dialog:endHide">结束隐藏对话框</span>
在本场景结束后是否隐藏对话框

默认值:`false`

<br />

### audio <span id="main:story:audio">音频对象</span>
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `type` | `bgm` `voice` `effect` `stop` | <b><a href="#main:story:audio:type">分支选择对象</a></b> | |
| `channel` | `"[音频通道名称]"` | <b><a href="#main:story:audio:channel">音频通道</a></b>  |  |
| `path` | `路径` |  <b><a href="#main:story:audio:path">音频路径</a></b>  |  |
| `res` | `文件名` | <b><a href="#main:story:audio:res">音频文件名</a></b> |  |
| `sleep` | `毫秒` | <b><a href="#main:story:audio:sleep">延迟播放时间</a></b> | 非必须 |
| `loop` | `true` `false` | <b><a href="#main:story:audio:loop">循环播放</a></b> | 非必须 |


<br />

#### type <span id="main:story:audio:type">音频类型</span>
决定着音频的处理类型
| 值 | 说明 |
| --- | --- |
| `bgm` | 背景音乐 |
| `voice` | 语音 |
| `effect` | 音效 |
| `stop` | 停止此通道的音频 |

<br />

#### channel <span id="main:story:audio:channel">音频通道</span>
通道名称在项目内设置,每个通道同一时间只能播放一个音频
对于不同的人物语音可以通过通道来标识

<br />


#### path <span id="main:story:audio:path">音频路径</span>
音频所在相对于`sounds/[码率]/`目录内的文件夹名
例如填写`bgm`就是相当于文件位于`sounds/bgm/`目录内

<br />

#### res <span id="main:story:audio:res">音频文件名</span>
音频的文件名,不包括后缀名

<br />

#### sleep <span id="main:story:audio:sleep">延迟播放时间</span>
以整个场景开始播放为基准时间,延迟一段时间后开始播放音频

默认值:`0`

<br />

#### loop <span id="main:story:audio:loop">循环播放</span>
决定音频是否循环播放
如果一个通道的音频开始循环播放,并且在后面的场景没有为此通道更新或停止音频,那么将会一直播放到游戏结束

默认值:`false`

<br />

### scene <span id="main:story:scene">画面场景对象</span>
画面场景对象是多个图层的集合,当<b><a href="#main:story:type">场景类型</a></b>为`renderer`时有效
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `name` | `场景名字符串` | <b><a href="#main:story:audio:name">场景名称</a></b> |  |
| `keep` | `true` `false` | <b><a href="#main:story:audio:keep">保持画面</a></b> | 非必须 |
| `inherit` | `true` `false` | <b><a href="#main:story:audio:inherit">继承画面</a></b> | <font color='red'>废弃</font> |
| `layer` | `"layer": [{...}]` | <b><a href="#main:story:scene:layer">图层对象</a></b> | 非必须 |

<br />

#### name <span id="main:story:scene:name">场景名称</span>
该场景在渲染器中的唯一名称,对于保持画面的场景可以通过此名称来更新

<br />

#### keep <span id="main:story:scene:keep">保持画面</span>
此参数告诉渲染器在入场过渡播放完或整个场景结束后不清除此场景的图层,也不播放退场过渡动画
<b>注意:当脚本的最后一个场景播放结束后,渲染器无论如何都会清屏,请确保要保留的场景都在一个脚本里使用</b>

默认值:当<b><a href="#main:story:scene:layer">layer</a></b>对象不存在时为`true`,否则为`false`

<br />

### video <span id="main:story:video">媒体画面对象</span>
画面场景对象是多个图层的集合,当<b><a href="#main:story:type">场景类型</a></b>为`video`时有效
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `path` | `路径` | <b><a href="#main:story:video:path">资源路径</a></b> |  |
| `res` | `文件名` | <b><a href="#main:story:video:res">资源名</a></b> |  |
| `loop` | `true` `false` | <b><a href="#main:story:video:loop">循环播放</a></b> | 非必须 |

<br />

#### path <span id="main:story:video:path">资源路径</span>
资源所在相对于`assets/`目录内的文件夹名,可以是目录的子目录
例如media表示资源在`assets/media/`内
当脚本位于`creative_workshop`中的项目时,`assets`也是相对于项目内部的文件夹而言

<br />

#### res <span id="#main:story:video:res">资源名</span>
视频的文件名,不带后缀名,只支持mp4格式

<br />

#### loop <span id="#main:story:video:loop">循环播放</span>
决定视频是否循环播放
如果用户使用自动播放模式,视频仍然只会播放一次就进入下一个场景

默认值:`false`

<br />

# layer <span id="main:story:scene:layer">图层对象</span> 
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `type` | `block` `gradientBlock` `text` `image` `sprite` `frame` `template` | <b><a href="#main:story:scene:layer:type">图层类型</a></b> |  |
| `align` | `0-8整数` | <b><a href="#main:story:scene:layer:align">对齐方式</a></b> | 非必须 |
| `sleep` | `毫秒` | <b><a href="#main:story:scene:layer:sleep">延迟渲染时间</a></b> | 非必须 |
| `path` | `路径` | <b><a href="#main:story:scene:layer:path">资源路径</a></b> | 非必须 |
| `res` | `文件名` `十六进制颜色` `"res":{...}` | <b><a href="#main:story:scene:layer:res">资源名</a></b> |  |
| `alpha` | `0-1浮点数` | <b><a href="#main:story:scene:layer:alpha">透明度</a></b> | 非必须 |
| `rotate` | `0-360整数` | <b><a href="#main:story:scene:layer:rotate">旋转角度</a></b> | 非必须 |
| `position` | `"position":[w, h, x, y]` | <b><a href="#main:story:scene:layer:position">图层位置</a></b> |  |
| `cover` | `globalCompositeOperation值` | <b><a href="#main:story:scene:layer:cover">覆盖方式</a></b> | 非必须 |
| `childrenCover` | `globalCompositeOperation值` | <b><a href="#main:story:scene:layer:childrenCover">子级覆盖方式</a></b> | 非必须 |
| `parent` | `字符串` | <b><a href="#main:story:scene:layer:parent">继承父级名</a></b> | 非必须 |
| `transition` | `"transition":{"start":{...},"end":{...}}` | <b><a href="#main:story:scene:layer:transition">过渡对象</a></b> | 非必须 |
| `animation` | `"animation":{...}` | <b><a href="#main:story:scene:layer:animation">动画对象</a></b> | 非必须 |
| `children` | `"children":{...}` | <b><a href="#main:story:scene:layer:children">子级图层对象</a></b> | 非必须 |
|  |  |  |  |

<br />

## type <span id="main:story:scene:layer:type">图层类型</span>
决定着图层的类型
当类型为sprite时,不论json内定义了多少个部件,此类型的图层都只使用character部件,且支持被其他类作为<a href="#main:story:scene:layer:parent">父级继承</a>
| 值 | 说明 |
| --- | --- |
| `block` | 纯色块 |
| `gradientBlock` | 渐变块 |
| `text` | 文字 |
| `image` | 图片 |
| `sprite` | 精灵 |
| `frame` | 帧动画 |
| `template` | 模板 |

<br />

## align <span id="main:story:scene:layer:align">对齐方式</span>
图层位于画面九宫格的位置排列,`0`是左上角,`4`是居中,`8`右下角,依此类推

默认值:`0`

<br />

## sleep <span id="main:story:scene:layer:sleep">延迟渲染时间</span>
以整个场景开始播放为基准时间,延迟一段时间后开始渲染场景
<b>注意:`transition`的计时会在渲染后开始</b>

默认值:0

<br />

## path <span id="main:story:scene:layer:path">资源路径</span>
资源所在相对于`assets/`目录内的文件夹名,可以是目录的子目录
例如`textures/backgrounds`表示资源在`assets/textures/backgrounds/`内
仅当图层为`image`,`sprite`,`template`,`frame`时有效,所以当`type`为`block`,`gradientBlock`,`text`时可以省略
当脚本位于`creative_workshop`中的项目时,`assets`也是相对于项目内部的文件夹而言

默认值:无

<br />

## res <span id="main:story:scene:layer:res">资源名</span>
对于不同的图层类型有不同的处理方式,参考以下
| 值 | 说明 |
| --- | --- |
| `block` | 十六进制RGB颜色值,例如`#ff0000` |
| `gradientBlock` | 渐变块样式对象 `"res":{...}` |
| `text` | 文字样式对象 `"res":{...}` |
| `image` | png图像名,不包含后缀名,例如`bg100001` |
| `sprite` | 精灵的json文件名,不包含后缀名,例如`chr001` |
| `frame` | 精灵的json文件名,不包含后缀名,例如`chr001` |
| `template` | 模板的json文件名,不包含后缀名,例如`tp001` |

<br />

### gradientBlock <span id="main:story:scene:layer:res:gradientBlock">渐变块样式对象</span>
如果<b><a href="#main:story:scene:layer:name">图层类型</a></b>为`gradientBlock`则参考以下渐变快对象

| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `console` | `"console":[x1, y1, x2, y2]` | <b><a href="#main:story:scene:layer:res:gradientBlock:console">渐变线位置</a></b> |  |
| `colors` | `"color":[[...]]` | <b><a href="#main:story:scene:layer:res:gradientBlock:colors">渐变色列表</a></b> |  |

<br />

#### console <span id="main:story:scene:layer:res:gradientBlock:console">渐变线位置</span>
此对象是一个数组,决定了渐变线径的方向,`x1` `y1`为渐变线的起点,`x2` `y2`为渐变线的终点
例如`"console":[0, 0, 1280, 720]`表示绘制一个渐变块,其渐变径向从坐标0,0开始到1280,720,看上去是一个斜向的渐变块

<br />

#### colors <span id="main:story:scene:layer:res:gradientBlock:colors">渐变色列表</span>
此对象是一个数组,内部必须包含至少两个颜色数组,每个颜色数组包含一个该色条在渐变快中的占比以及argb颜色
每个色条均会以均匀的渐变方式过渡到下一个色条直到渐变结束
例如`"color": [[0.6, "rgba(255, 255, 255, 0)"],[0.4, "rgba(255, 255, 255, 1)"]]`表示绘制一个黑渐变到白的渐变块,其中黑占%60,白占40%


### text <span id="main:story:scene:layer:res:text">文字样式对象</span>
如果<b><a href="#main:story:scene:layer:name">图层类型</a></b>为`text`则参考以下文字样式对象

| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `text` | `字符串` | 要显示的文本字符串 |  |
| `font` | `"font":[...]` | <b><a href="#main:story:scene:layer:res:text:font">字体样式</a></b> |  |
| `stroke` | `"stroke":[...]` | <b><a href="#main:story:scene:layer:res:text:stroke">字体描边</a></b> | 非必须 |
| `shadow` | `"shadow":[...]` | <b><a href="#main:story:scene:layer:res:text:shadow">字体阴影</a></b> | 非必须 |

<br />

#### font <span id="main:story:scene:layer:res:text:font">字体样式</span>
此对象是一个数组,包含着字体的主要样式
第一位是字号,决定着字体的大小,单位为`px`
第二位是字体粗细允许的值:`normal` `bold` `bolder` `lighter` `100` `200` `300` `400` `500` `600` `700` `800` `900`
第三位是字体格式,正常为`normal`,斜体为`italic`,倾斜体为`oblique`
第四位是字体颜色,只支持十六进制颜色,如`#ffff00`
第五位是字体名称,该字体必须是在css中声明的
<b>注意:所有样式必须声明,如果需要保持默认请在对应的值里写上false</b>
例如`[24, false, false, "#000000", "Arial"]`表示字体大小`24px`,字粗和格式均为`默认`,`颜色黑`,字体为`Arial`
例如`[45, "bold", "italic", "#000000", "宋体"]`表示字体大小`48px`,`粗体`+`斜体字`,`颜色黑`,字体为`宋体`

<br />

#### stroke <span id="main:story:scene:layer:res:text:stroke">字体描边</span>
此对象是一个数组,决定着字体描边的大小和颜色
例如`[3, "#700000"]`表示描边`3px`,颜色`#700000`

默认值:`null`

<br />

#### shadow <span id="main:story:scene:layer:res:text:shadow">字体阴影</span>
此对象是一个数组,决定着字体阴影的样式
例如`[20, 8, 12, "#700000"]`表示阴影模糊级别`20`,向坐标`X`偏移`8`,向坐标`Y`偏移`12`,颜色`#700000`

默认值:`null`

<br />

## alpha <span id="main:story:scene:layer:alpha">透明度</span>
图层开始或者关键帧渲染的透明度

默认值:`1.0`

<br />

## rotate <span id="main:story:scene:layer:rotate">旋转角度</span>
图层开始或者关键帧渲染的旋转角度

默认值:`0`

<br />

## position <span id="main:story:scene:layer:position">图层位置</span>
图层开始或者关键帧渲染的宽,高,横轴位置,纵轴位置,可以只写位置而不写宽高
单位可以是`px`或`%`,没有单位时默认当`px`处理,当宽高省略时默认为`100%`,且位置相对于`align`开始
<b>注意:宽高百分比是相对于资源本身的宽高来计算的,而位置的百分比是相对于整个画面来计算的</b>
例如`[100,200]`表示图像大小`100%`,位置`x` = `100px`,`y` = `200px`
例如`[50%,50%,100,200]`表示图像大小宽高为`50%`,位置`x` = `100px`,`y` = `200px`

<br />

## cover <span id="main:story:scene:layer:cover">覆盖方式</span>
图层的覆盖关系,支持canvas的`globalCompositeOperation值`,用来决定该图层如何覆盖在底层的图层上,完整参考[https://www.cnblogs.com/fangsmile/p/10132920.html](https://www.cnblogs.com/fangsmile/p/10132920.html),常用值参考如下
| 值 | 说明 |
| --- | --- |
| `source-over` | 在底层图层上显示本图层 |
| `source-atop` | 在底层图层上显示本图层,但本图层只显示在底层图层的范围内 |
| `source-in` | 在底层图层上显示本图层,但只显示本图层和底层图层重叠后的部分 | 
| `source-out` | 在底层图层上显示本图层,但只显示本图层和底层图层不重叠的部分 |
| `destination-over` | 在底层图层下显示本图层 |
| `destination-atop` | 在底层图层下显示本图层,但本图层只显示在底层图层的范围内 |
| `destination-in` | 在底层图层下显示本图层,但只显示本图层和底层图层重叠后的部分 |
| `destination-out` | 在底层图层下显示本图层,但只显示本图层和底层图层不重叠的部分 |

默认值:`source-over`

<br />

## childrenCover <span id="main:story:scene:layer:childrenCover">覆盖方式</span>
子级图层与上级图层的覆盖关系,内容与<b><a href="#main:story:scene:layer:cover">覆盖方式</a></b>一样

默认值:`source-over`

<br />

## parent <span id="main:story:scene:layer:parent">继承父级名</span>
让本子级图层继承父级`sprite`子类的位置和大小,如父级`sprite`存在`eyes`子类时,将此值设置为`eyes`可以直接让图层直接覆盖在父级对应的子类的位置中且可将<a href="#main:story:scene:layer:position">图层位置</a>省略
而且在此参数生效的情况下设置<a href="#main:story:scene:layer:position">图层位置</a>,图像的位置也是相对于该设置的子类的位置进行偏移
仅当父级图层的<a href="#main:story:scene:layer:type">图层类型</a>为`sprite`时有效

默认值:`null`

<br />

## transition <span id="main:story:scene:layer:transition">过渡对象</span>
包含一个`in`对象和`out`对象,用来设定入场和离场的动画关键帧,关键帧对象包含参数如下
<b>注意:当`out`里的动画对象中没有任何关键帧参数时,程序仍然会等待`in`关键帧的持续时间和延迟时间结束后才会继续播放后面的动画</b>

| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `bezier` | `"bezier":[p1x, p1y, p2x, p2y]` | <b><a href="#main:story:scene:layer:transition:bezier">三次贝塞尔曲线过渡</a></b> | 非必须 |
| `sleep` | `毫秒` | <b><a href="#main:story:scene:layer:transition:sleep">延迟</a></b> | 非必须 |
| `duration` | `毫秒` | <b><a href="#main:story:scene:layer:transition:name">持续时间</a></b> | 非必须 |
| `rotate` | `0-360整数` | <b><a href="#main:story:scene:layer:rotate">旋转角度</a></b> | 非必须 |
| `alpha` | `0-1浮点数` | <b><a href="#main:story:scene:layer:alpha">透明度</a></b> | 非必须 |
| `position` | `"position":[w, h, x, y]` | <b><a href="#main:story:scene:layer:position">图层位置</a></b> | 非必须 |

<br />

### bezier <span id="main:story:scene:layer:transition:bezier">三次贝塞尔曲线过渡</span>
根据三次贝塞尔曲线的进度控制动画,效果与CSS中的三次贝塞尔曲线相似
<b>注意:如果不设置,程序将使用线性过渡来控制动画.这样可以节省大量的计算量,所以如果需要使用线性过渡请不要设置`[0, 0, 1, 1]`这样的值</b>

默认值:`[0, 0, 1, 1]`

<br />

### sleep <span id="main:story:scene:layer:transition:sleep">延迟</span>
以当前图层`sleep`开始播放为基准时间,延迟一段时间后开始渲染关键帧

默认值:`0`

<br />

### duration <span id="main:story:scene:layer:transition:duration">持续时间</span>
当前关键帧过渡的持续时间

默认值:`0`

<br />

## animation <span id="main:story:scene:layer:animation">动画对象</span>
帧动画对象,用于播放整组的精灵图像动画,仅当图层type为frame时有效

| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `sleep` | `毫秒` | <b><a href="#main:story:scene:layer:animation:sleep">延迟</a></b> | 非必须 |
| `plays` | `"plays":[...]` | <b><a href="#main:story:scene:layer:animation:plays">播放次数</a></b> |  |
| `cycle` | `"cycle":[...]` | <b><a href="#main:story:scene:layer:animation:cycle">循环间隔</a></b> | 非必须 |
| `frame` | `"frame":[...]` | 帧对象 | <font color='red'>废弃</font> |
| `class` | `[部件名]:[分类名]` | <b><a href="#main:story:scene:layer:animation:class">动画类名</a></b> |  |
| `bind` | `"bind":{...}` | <b><a href="#main:story:scene:layer:animation:bind">动画绑定</a></b> | 非必须 |

<br />

### sleep <span id="main:story:scene:layer:animation:sleep">延迟</span>
控制延迟播放,当图层被渲染第一帧后,一直到设置的延迟时间结束才开始播放

默认值:`0`

<br />

### plays <span id="main:story:scene:layer:animation:plays">播放次数</span>
控制一组动画播放的次数,包含一个数组,其中可以是整数的次数,也可以是`loop` `only`这种字符串
比如`[6]`表示一组动画播放6次后停止在最后一帧,又比如`["loop"]`表示如同GIF一样一直循环一组动画,还要`["only"]`表示一组动画播放一次后停止在最开始的第一帧
注意:`[1]`和`["only"]`的区别再于停止时的位置

<br />

### cycle <span id="main:story:scene:layer:animation:cycle">循环间隔</span>
一组动画在播放到最后一帧后,间隔多久会播放下一次,包含一个数组,可以是一个整数的毫秒时间,也可以是两个整数的毫秒时间任何由程序决定这个区间内随机的数值
比如`[1000]`表示循环间隔固定`1000`毫秒,又比如`[1000,3000]`表示循环间隔在最小为`1000`毫秒,最大为`3000`毫秒之间随机决定一个值(一般用于眨眼的动画)

默认值:`[0]`

<br />

### class <span id="main:story:scene:layer:animation:class">动画类名</span>
res中使用的json文件包含动画的map名称,格式为`[部件名]:[分类名]`
比如使用`eyes:微笑`则代表使用json文件中声明的`eyes`部件和`微笑`的动画

<br />

### bind <span id="main:story:scene:layer:animation:bind">动画绑定</span>
设置动画的联动绑定,包含一个对象但具体的参数会因触发器的不同而不同,参考如下
| key | 值 | 说明 | 备注 |
| --- | --- | --- | --- |
| `trigger` | `字符串` | 触发器名称 |  |
| `type` | `字符串` | 触发类型 |  |
| `object` | `字符串` | 绑定对象 |  |
| `range` | `"range":[...]` | 绑定区间 |  |

目前支持的触发器有`audio`,该触发器提供一个`volume`类型的触发事件,当绑定这个触发器类型的时候,动画的进度将会根据声音的高度而变化,比如用于人物口型

使用时必须将`object`设置为要绑定的<a href="#main:story:audio:channel">音频通道</a>

对于不同人物的音高范围可以不同,此时可以使用`range`分别设置对应帧和音高范围的绑定,比如[50,70,90]表示把三帧分别绑定到音高的`50%`,`70%`,`90%`上。此参数可以省略,如果不设置,则按照帧的数量均匀分配到每个百分比上

完整例子:`{"trigger": "audio", "type": "volume", "channel": "Elaina", "range": [50, 70, 90]}`

注意:当场景播放完毕且图层没有被保持时,绑定会自动解除



<br />
