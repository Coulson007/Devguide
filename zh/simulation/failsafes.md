# 模拟故障保护

[Failsafes](https://docs.px4.io/master/en/config/safety.html) define the safe limits/conditions under which you can safely use PX4, and the action that will be performed if a failsafe is triggered (for example, landing, holding position, or returning to a specified point).

在 SITL 中，默认情况下会禁用某一些故障，以便方便模拟使用。 本主题说明如何在实际世界中尝试 SITL 仿真之前测试安全关键行为。

> **Note** 您还可以使用 [ HITL 模拟](../simulation/hitl.md) 测试故障。 HITL 使用飞行控制器的常规配置参数。

## 数据链路丢失

默认情况下启用 *数据链路丢失* 故障保护（无法通过 MAVLink 获取外部数据）。 这使得模拟仅适用于连接的 GCS，SDK 或其他 MAVLink 应用程序。

将参数 [NAV_DLL_ACT](../advanced/parameter_reference.md#NAV_DLL_ACT) 设置为想要的故障保护操作，以改变行为。 例如，设置为 `0` 禁用它。

> **Note** 当您执行 `make clean` 时，SITL 中的所有参数（包括此参数）都会被重置。

## RC 链接损失

*RC 链接损失* failslafe （来自远程控制的数据不可用） 被默认启用。 这使得模拟仿真只能使用 MAVLink 或远程控制连接。

将参数 [NAV_RCL_ACT](../advanced/parameter_reference.md#NAV_RCL_ACT) 设置为所需的故障保护操作，以更改行为。 例如，设置为 `0` 禁用它。

> **Note** 当您执行 `make clean` 时，SITL 中的所有参数（包括此参数）都会被重置。

## 低电量

模拟仿真的电池永远不会耗尽电量，并且默认情况下仅耗尽其容量的 50％ 会发送电压报告。 这可以在 GCS UI 中测试电池指示，而不会触发可能中断其他测试的低电池反应。

要更改此最小电池百分比值，请更改 [this line](https://github.com/PX4/Firmware/blob/9d67bbc328553bbd0891ffb8e73b8112bca33fcc/src/modules/simulator/simulator_mavlink.cpp#L330)。

要控制电池消耗到最小值的速度，请使用参数 [SIM_BAT_DRAIN](../advanced/parameter_reference.md#SIM_BAT_DRAIN)。

> **Tip** 通过在飞行中更改此配置，您还可以测试恢复能力，以模拟不准确的电池状态估计或空中充电技术。

## GPS 损失

为了模拟丢失和重新获取 GPS 全球定位系统信息，您可以停止/重新启动 GPS 驱动程序。 在pxh终端中通过运行 `param set SIM_GPS_BLOCK 1` 和 `param set SIM_GPS_BLOCK 0` 命令来分别屏蔽和取消屏蔽消息。