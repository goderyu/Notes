| 快捷键          | 说明                                             |
| --------------- | ------------------------------------------------ |
| Ctrl-Shift-T    | 显示/隐藏视口工具栏                              |
| G               | 视口游戏模式(显示/隐藏坐标轴、光源标记等)        |
| 1~5             | 选中关键帧后设置插值模式                         |
| Ctrl-O          | 选中Track对象后展开/折叠子属性                   |
| Shift-O         | 选中Track对象后展/折所有子属性                   |
| Ctrl-L/R Arrow  | 选中关键帧后左右跳帧                             |
| Home            | Sequence编辑器视图重置状态                       |
| ,和.            | 前后跳转至关键帧                                 |
| Ctrl/Shift-Drag | Actor放置在视口后添加到Sequence中(Possess/Spawn) |


## UE4.18 LevelSequence 事件调用

+Track，选择Event Track，之后为Events添加关键帧。4.18版本只支持Trigger(瞬时事件)。关键帧添加后，选中该关键帧，右键找到Properties，找到Event Name，输入触发事件的名称。

![image-20200102140140093](illustration/UE-Sequence%E5%AD%A6%E4%B9%A0/image-20200102140140093.png)



选中Events，右键选择Properties，找到Event Receivers，点击+创建一个元素，指定事件接收者。设置好之后，需要打开事件接收者对象的蓝图，创建自定义事件，命名要求与Event Name一致，编写好事件逻辑后，播放动画即可看到事件被触发。

![image-20200102140300338](illustration/UE-Sequence%E5%AD%A6%E4%B9%A0/image-20200102140300338.png)

![image-20200102140518310](illustration/UE-Sequence%E5%AD%A6%E4%B9%A0/image-20200102140518310.png)

![image-20200102140540492](illustration/UE-Sequence%E5%AD%A6%E4%B9%A0/image-20200102140540492.png)

运行查看效果

![image-20200102140615006](illustration/UE-Sequence%E5%AD%A6%E4%B9%A0/image-20200102140615006.png)