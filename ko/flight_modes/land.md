# 착륙 모드

[<img src="../../assets/site/position_fixed.svg" title="필요한 위치 추정치(예: GPS) " width="30px" />](../getting_started/flight_modes.md#key_position_fixed)

*착륙* 모드는 기체의 모드가 활성화된 위치에 착륙합니다. 착륙후 기체는 가급적 짧은 시간내에 시동이 해제됩니다.

:::note
* This mode requires a valid global position estimate (from GPS or inferred from a [local position](../ros/external_position_estimation.md#enabling-auto-modes-with-a-local-position)).
* In a failsafe the mode only requires altitude (typically a barometer is built into the flight controller).
* 이 모드는 자동입니다. 기체를 제어시 사용자 개입이 *필요하지* 않습니다.
* RC 제어 스위치는 기체의 비행 모드를 변경할 수 있습니다.
* RC stick movement in a multicopter (or VTOL in multicopter mode) will [by default](#COM_RC_OVERRIDE) change the vehicle to [Position mode](../flight_modes_mc/position.md) unless handling a critical battery failsafe.
* The mode can be triggered using the [MAV_CMD_DO_LAND_START](https://mavlink.io/en/messages/common.html#MAV_CMD_DO_LAND_START) MAVLink command, or by explicitly switching to Land mode. :::

각 기체 유형에 대한 구체적인 동작은 아래에 설명되어 있습니다.


## 멀티콥터 (MC)

The vehicle will land at the location at which the mode was engaged. 기체는 [MPC_LAND_SPEED](#MPC_LAND_SPEED)에 지정된 속도로 하강하고 착륙후 시동 해제됩니다 ([기본값](#COM_DISARM_LAND)).

RC stick movement will change the vehicle to [Position mode](../flight_modes_mc/position.md) (by [default](#COM_RC_OVERRIDE)).

착륙은 다음 매개변수의 영향을 받습니다.

| 매개 변수                                                                                                   | 설명                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <a id="MPC_LAND_SPEED"></a>[MPC_LAND_SPEED](../advanced_config/parameter_reference.md#MPC_LAND_SPEED)   | 착륙 하강 속도. 이는 접지 상태를 알 수 없으므로, 낮은 속도를 유지하여야 합니다                                                                                                                                                          |
| <a id="COM_DISARM_LAND"></a>[COM_DISARM_LAND](../advanced_config/parameter_reference.md#COM_DISARM_LAND) | 착륙후 자동 시동 해제 대기 시간 (단위 초). -1로 설정하면 착륙시 시동 해제되지 않습니다.                                                                                                                                                   |
| <a id="COM_RC_OVERRIDE"></a>[COM_RC_OVERRIDE](../advanced_config/parameter_reference.md#COM_RC_OVERRIDE) | Controls whether stick movement on a multicopter (or VTOL in MC mode) causes a mode change to [Position mode](../flight_modes_mc/position.md). 자동 모드와 오프보드 모드에 대해 별도로 활성화할 수 있으며, 기본적으로 자동 모드에서 활성화됩니다. |
| <a id="COM_RC_STICK_OV"></a>[COM_RC_STICK_OV](../advanced_config/parameter_reference.md#COM_RC_STICK_OV) | The amount of stick movement that causes a transition to [Position mode](../flight_modes_mc/position.md) (if [COM_RC_OVERRIDE](#COM_RC_OVERRIDE) is enabled).                                         |


## Fixed-wing (FW)

:::warning
Fixed-wing _Land mode_ is currently broken: [PX4-Autopilot/pull/21036](https://github.com/PX4/PX4-Autopilot/pull/21036). (Specifically, switching to Land mode causes a fly-away.)

Automated landing in missions is supported: [Mission mode > Fixed-wing mission landing](../flight_modes/mission.md#fw-mission-landing). :::


## 수직이착륙기

A VTOL follows the LAND behavior and parameters of [Fixed-wing](#fixed-wing-fw) when in FW mode, and of [Multicopter](#multi-copter-mc) when in MC mode. [NAV_FORCE_VT](../advanced_config/parameter_reference.md#NAV_FORCE_VT)이 설정되면(기본값: 켜짐) 고정익 모드의 VTOL이 착륙 직전에 멀티콥터로 되돌아갑니다.