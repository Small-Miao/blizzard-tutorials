# 摄像机动作

编辑器的摄像机系统为控制玩家如何观察游戏提供了一整套强大工具。而真正让它成为创意手段的，是摄像机动作的加入。借助这些动作，你可以为摄像机制作动画、在不同镜头间切换，并施加各种效果与滤镜。这种动态摄像机用法，能让你的项目在叙事或玩法中融入电影化表现。

![](./resources/046_Camera_Actions1.png)
*joecab 的 Optical Illusion 作品中的创意摄像机角度*

## 摄像机动作列表

你可以在创建动作时进入 'Camera' 标签来访问摄像机动作，这会显示如下界面。

[![摄像机动作](./resources/046_Camera_Actions2.png)](./resources/046_Camera_Actions2.png)
*摄像机动作*

下表对其中一部分摄像机动作进行了说明。

| 动作 | 作用 |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Apply Camera Object | 在一段 Duration 时间内，将某个 Camera 设为某个 Player 的主摄像机，并可设置过渡的 Initial Velocity 与 Decelerate。 |
| Apply Camera Property | 在一段 Duration 时间内把某个 Property 应用于摄像机，并可设置 Initial Velocity 与 Decelerate。它不会改变该玩家当前使用的摄像机对象。 |
| Follow Unit Group with Camera | 将某个 Player 对其当前选中单位组的跟随状态切换为 Do/Do Not Do。处于跟随状态的摄像机会平滑、持续地追随目标。 |
| Lock Camera Input | 切换某个 Player 的锁定状态。被锁定后，玩家将无法对摄像机进行任何手动调整。这个动作常用于过场。 |
| Lock Camera Mouse Relative Mode On/Off | 为某个 Player 打开或关闭摄像机鼠标相对模式。该模式会把鼠标移动变成类似按住中键拖拽滚屏的行为，同时也会隐藏鼠标光标。它通常会与 Turn Camera Mouse Rotation On/Off 搭配使用，以创建类似第一人称游戏中的视角控制。 |
| Make Camera Look At | 在一段 Duration 时间内，让某个 Player 的摄像机朝向某个 Region 中的目标 Point，并可设置 Initial Velocity 与 Decelerate。它本质上会在不干扰其他属性的前提下，在 XY 平面上追踪摄像机目标。 |
| Make Camera Look At And Follow Actor | 类似于 Make Camera Look At 和 Follow Unit Group with Camera 的组合，但它还接受一个 Actor 输入。 |
| Make Camera Look At And Follow Unit | 类似于 Make Camera Look At 和 Follow Unit Group with Camera 的组合，但它还接受一个 Unit 输入。 |
| Pan Camera | 在一段 Duration 时间内，让某个 Player 的摄像机朝向某个 Region 中的目标 Point，并可设置 Initial Velocity 与 Decelerate。它本质上会在等距平面上移动视角，而不影响其他摄像机特性。若启用 Smart panning，且摄像机已经朝向目标 Point，则不会再次平移。 |
| Restore Camera | 在一段 Duration 时间内，将某个 Player 的摄像机恢复到此前保存的配置，并可设置 Initial Velocity 与 Decelerate。 |
| Save Camera | 保存某个 Player 当前的摄像机配置。 |
| Set Camera Bounds | 将一组 Players 的摄像机边界设置为某个 Region，从而把视角移动限制在该区域内。若 Minimap 参数选择 Do，小地图也会按摄像机边界调整大小。这在战役中经常用于把一张地图拆分成可分别探索的若干区域。 |
| Set Camera Mouse Rotation Speed | 为某个 Player 设置摄像机的 Yaw 或 Pitch 灵敏度。该设置会在开启 camera mouse rotation 时生效。 |
| Set Camera Object Property | 将某个 Camera 的某个 Property 设为指定 Value。属性包括 Distance、Rotation、Roll 等。 |
| Set Camera Object Target | 将某个 Camera 对象的目标设置为某个 Region 中的 Target。 |
| Shake Camera | 让某个 Player 的摄像机发生震动。震动沿某个 Direction，以基础 Frequency 为基础并叠加 Random 扰动，持续一个 Duration。若 Duration 参数设为 0，则会无限期震动，直到使用 Stop Shaking Camera 动作为止。 |
| Shake Camera Using Preset | 通过从预设列表中选择 Amplitude 和 Frequency 配置，让某个 Player 的摄像机震动。该震动支持在 Duration 内进行 Blend In 和 Blend Out。 |
| Stop Shaking Camera | 停止某个 Player 的摄像机震动。 |
| Turn Camera Height Displacement On/Off | 为某个 Player 打开或关闭摄像机高度位移功能。该功能会对当前被跟随的飞行单位进行高度修正，以保证它们在摄像机缩放时仍清晰可见。 |
| Turn Camera Height Smoothing On/Off | 为某个 Player 打开或关闭摄像机高度平滑功能。该功能会在摄像机跨越不同地形高度时提供平滑过渡。 |
| Turn Camera Mouse Rotation On/Off | 为某个 Player 打开或关闭摄像机鼠标旋转。开启后，玩家的拖拽滚屏操作会变成自由视角旋转，而不是平移摄像机。它可与 Lock Camera Mouse Relative Mode On/Off 搭配使用，以实现典型第一人称游戏中的摄像机控制。 |
| Turn Camera Vertical Field of View On/Off | 为某个 Player 打开或关闭垂直视野。默认情况下，不同宽高比之间会保留水平视野，而垂直空间会被裁切或扩展。开启垂直视野后，则会保留垂直空间，而水平空间根据需要裁切或扩展。 |
| Zoom Camera | 让某个 Player 的摄像机对某个位置执行缩放，在一段 Duration 内从 DistanceFrom 过渡到 DistanceTo。 |

## 使用摄像机动作

来看下面这一组摄像机动作序列。

[![摄像机动作序列](./resources/046_Camera_Actions3.png)](./resources/046_Camera_Actions3.png)
*摄像机动作序列*

这段序列先移除了游戏 UI，并创建了一组陆战队员。之后开始执行一系列摄像机动作。首先，Pan Camera 动作把摄像机移到这对陆战队员的中心。这个过渡通过极小的 Duration 值以及不使用 Deceleration 实现得几乎瞬间完成。接着，一个 Apply Camera Object 动作通过设置较小的 Distance 值来拉近摄像机。然后，另一个 Apply Camera Object 通过把 Rotation 设为两名陆战队员之间的角度来改变摄像机轴向。这些属性都以与前一个动作相同的近乎瞬时 Duration 应用。最终效果如下图所示。

[![通过动作设置的自定义摄像机效果](./resources/046_Camera_Actions4.png)](./resources/046_Camera_Actions4.png)
*通过动作设置的自定义摄像机效果*

## 附件

 * [046_Camera_Actions.SC2Map](./maps/046_Camera_Actions.SC2Map)
