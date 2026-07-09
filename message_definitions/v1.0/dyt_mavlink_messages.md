# DYT 自定义 MAVLink 消息定义说明



## DYT_TARGET (12932)

Decoded DYT seeker target telemetry, including tracking state, line-of-sight angles, gimbal attitude, target box, and link health.

中文说明：`DYT_TARGET` 用于把 DYT 吊舱驱动解析出的目标状态发给地面站。它对应 `msg/DytTarget.msg`，主要用于观察吊舱是否锁定、LOS 偏差是否正常、云台角度是否正常、目标框是否有效，以及串口解析状态。

### 枚举值

| 字段 | 值 | 名称 | 说明 |
| --- | ---: | --- | --- |
| `tracking_state` | `0` | `TRACKING_STATE_SEARCH` | 搜索中，未锁定。 |
| `tracking_state` | `1` | `TRACKING_STATE_LOCKED` | 已锁定目标。 |
| `tracking_state` | `2` | `TRACKING_STATE_TIMEOUT` | 吊舱遥测超时。 |
| `tracking_state` | `3` | `TRACKING_STATE_ERROR` | 吊舱遥测或解析错误。 |
| `video_source` | `0` | `VIDEO_SOURCE_VIS_1` | 可见光 1。 |
| `video_source` | `1` | `VIDEO_SOURCE_VIS_2` | 可见光 2。 |
| `video_source` | `2` | `VIDEO_SOURCE_IR_1` | 红外 1。 |
| `video_source` | `3` | `VIDEO_SOURCE_IR_2` | 红外 2。 |
| `tracking_algorithm` | `0` | `TRACK_ALGO_ADAPTIVE` | 自适应跟踪。 |
| `tracking_algorithm` | `1` | `TRACK_ALGO_PERSON` | 人员目标算法。 |
| `tracking_algorithm` | `2` | `TRACK_ALGO_VEHICLE` | 车辆目标算法。 |
| `tracking_algorithm` | `3` | `TRACK_ALGO_BUILDING` | 建筑目标算法。 |

### 字段表

| Field Name | Type | Units | Values | Description |
| --- | --- | --- | --- | --- |
| `timestamp` | `uint64_t` | `us` |  | PX4 system boot 以来的消息发布时间。 |
| `timestamp_sample` | `uint64_t` | `us` |  | DYT 遥测帧接收时间。 |
| `frame_counter` | `uint32_t` |  |  | DYT 遥测帧计数。 |
| `tracking_state` | `uint8_t` |  | 见枚举表 | 目标跟踪状态。 |
| `video_source` | `uint8_t` |  | 见枚举表 | 当前视频源。 |
| `tracking_algorithm` | `uint8_t` |  | 见枚举表 | 当前跟踪算法。 |
| `target_valid` | `uint8_t` |  | `0/1` | 吊舱是否报告有效锁定目标。 |
| `auto_hint` | `uint8_t` |  | `0/1` | 是否存在自动锁定候选目标或目标提示。 |
| `image_enhance` | `uint8_t` |  | `0/1` | 图像增强是否开启。 |
| `recording` | `uint8_t` |  | `0/1` | 吊舱是否正在录像。 |
| `motor_on` | `uint8_t` |  | `0/1` | 吊舱电机是否开启。 |
| `follow_mode` | `uint8_t` |  | `0/1` | 吊舱是否处于跟随模式。 |
| `laser_on` | `uint8_t` |  | `0/1` | 激光或测距输出是否开启。 |
| `selftest_done` | `uint8_t` |  | `0/1` | DYT 自检是否完成。 |
| `gyro_calib_failed` | `uint8_t` |  | `0/1` | 陀螺仪校准是否失败。 |
| `servo_fault` | `uint8_t` |  | `0/1` | 是否存在舵机故障。 |
| `image_board_fault` | `uint8_t` |  | `0/1` | 是否存在图像板故障。 |
| `los_x_rad` | `float` | `rad` |  | 图像横向 LOS 误差。 |
| `los_y_rad` | `float` | `rad` |  | 图像纵向 LOS 误差。 |
| `gimbal_roll_rad` | `float` | `rad` |  | 云台 roll 角。 |
| `gimbal_pitch_rad` | `float` | `rad` |  | 云台 pitch 角。 |
| `gimbal_yaw_rad` | `float` | `rad` |  | 云台 yaw 角。 |
| `gimbal_roll_rate_rad_s` | `float` | `rad/s` |  | 云台 roll 角速度。 |
| `gimbal_pitch_rate_rad_s` | `float` | `rad/s` |  | 云台 pitch 角速度。 |
| `gimbal_yaw_rate_rad_s` | `float` | `rad/s` |  | 云台 yaw 角速度。 |
| `bbox_width_px` | `float` |  |  | 目标框宽度，单位为像素。 |
| `bbox_height_px` | `float` |  |  | 目标框高度，单位为像素。 |
| `range_m` | `float` | `m` |  | 目标距离；不可用时可为 `NaN`。 |
| `zoom_ratio` | `float` |  |  | 吊舱变焦倍率。 |
| `frame_dt_s` | `float` | `s` |  | 两帧 DYT 遥测之间的时间间隔。 |
| `last_rx_age_s` | `float` | `s` |  | 最近一次 DYT 遥测样本年龄。 |
| `parse_error_count` | `uint16_t` |  |  | DYT 遥测解析错误累计计数。 |
| `status1` | `uint8_t` |  |  | 解码出的 DYT 原始状态字节 1。 |
| `status2` | `uint8_t` |  |  | 解码出的 DYT 原始状态字节 2。 |
| `status3` | `uint8_t` |  |  | 解码出的 DYT 原始状态字节 3。 |
| `self_test_raw` | `uint8_t` |  |  | DYT 原始自检/状态字节。 |

## DYT_COMMAND (12933)

DYT seeker command topic mirrored over MAVLink for monitoring or ground-control integration.

中文说明：`DYT_COMMAND` 用于把飞控内部的 `dyt_command` 话题镜像到 MAVLink。它可以先作为“飞控 -> 地面站”的监控包使用，显示当前飞控给吊舱发了什么命令。若后续要做“地面站 -> 飞控”的吊舱控制，也可以复用同一个消息 ID，但飞控接收端必须做权限和安全限制。

### 命令值

| 值 | 名称 | 说明 |
| ---: | --- | --- |
| `0` | `CMD_NONE` | 无命令。 |
| `1` | `CMD_AUTO_LOCK` | 自动锁定。 |
| `2` | `CMD_STOP_TRACK` | 停止跟踪。 |
| `3` | `CMD_RETRIGGER` | 重触发跟踪流程。 |
| `4` | `CMD_NOFOLLOW` | 不跟随/指定吊舱协议命令。 |
| `5` | `CMD_CENTER` | 居中序列。 |
| `6` | `CMD_YAW_FOLLOW` | yaw 跟随。 |
| `7` | `CMD_ELECT_LOCK` | 电子锁定。 |
| `8` | `CMD_ELECT_UNLOCK` | 电子解锁。 |
| `9` | `CMD_LOCK_VIEW` | 锁定视场。 |
| `10` | `CMD_CENTER_GIMBAL` | 停止跟踪后发送指定云台角。 |
| `11` | `CMD_SET_FRAME_ANGLE` | 设置云台框架角。 |
| `12` | `CMD_SEARCH_RATE` | 设置搜索速率。 |
| `13` | `CMD_SEND_OWNSHIP_STATE` | 发送本机状态，供地理跟踪使用。 |
| `14` | `CMD_GEO_TRACK` | 发送地理目标，供中制导地理指向使用。 |
| `15` | `CMD_GEO_TRACK_EXIT` | 退出地理跟踪。 |

### 字段表

| Field Name | Type | Units | Values | Description |
| --- | --- | --- | --- | --- |
| `timestamp` | `uint64_t` | `us` |  | PX4 system boot 以来的消息发布时间。 |
| `command` | `uint8_t` |  | 见命令值表 | DYT 命令编号。 |
| `param_x` | `int16_t` |  |  | 通用 X 参数。对框架角命令而言，通常是 yaw，单位为 centidegree。 |
| `param_y` | `int16_t` |  |  | 通用 Y 参数。对框架角命令而言，通常是 pitch，单位为 centidegree。 |
| `param3` | `uint8_t` |  |  | 通用参数 3。 |
| `zoom_rate` | `int8_t` |  |  | 变焦速率命令。 |
| `lat` | `double` | `deg` |  | 本机状态或地理目标命令使用的纬度。 |
| `lon` | `double` | `deg` |  | 本机状态或地理目标命令使用的经度。 |
| `alt` | `float` | `m` |  | 本机状态或地理目标命令使用的高度。 |
| `rel_alt` | `float` | `m` |  | 本机状态或地理目标命令使用的相对高度。 |
| `roll_rad` | `float` | `rad` |  | 地理跟踪使用的本机 roll。 |
| `pitch_rad` | `float` | `rad` |  | 地理跟踪使用的本机 pitch。 |
| `yaw_rad` | `float` | `rad` |  | 地理跟踪使用的本机 yaw。 |
| `airspeed_m_s` | `float` | `m/s` |  | 地理跟踪使用的本机空速。 |
| `groundspeed_m_s` | `float` | `m/s` |  | 地理跟踪使用的本机地速。 |

## DYT_GUIDANCE_STATUS (12934)

DYT guidance controller state and terminal-guidance setpoints mirrored from the `dyt_guidance_status` uORB topic.

中文说明：`DYT_GUIDANCE_STATUS` 用于把 DYT 制导控制器的状态发给地面站。它对应 `msg/DytGuidanceStatus.msg`，适合在 QGC 中显示：当前是否处于末制导、是否接管飞机、目标是否锁定/新鲜、Follow/Intercept 子模式、LOS 向量、LOS 角速度、速度/加速度/yaw setpoint。

### 状态值

| 字段 | 值 | 名称 | 说明 |
| --- | ---: | --- | --- |
| `state` | `0` | `STATE_IDLE` | DYT 制导空闲。 |
| `state` | `1` | `STATE_SEARCH_WAIT_LOCK` | 末制导已授权，等待吊舱锁定。 |
| `state` | `2` | `STATE_TRACK_FOLLOW` | 已进入视觉跟踪 Follow 子模式。 |
| `state` | `3` | `STATE_TRACK_INTERCEPT` | 已进入视觉跟踪 Intercept 子模式。 |
| `state` | `4` | `STATE_LOST_HOLD` | 丢锁后保持/重捕。 |
| `state` | `5` | `STATE_ABORT` | 中止状态，下周期回空闲。 |
| `requested_submode` / `active_submode` | `0` | `SUBMODE_FOLLOW` | Follow 子模式。 |
| `requested_submode` / `active_submode` | `1` | `SUBMODE_INTERCEPT` | Intercept 子模式。 |
| `lost_reason` | `0` | `LOST_REASON_NONE` | 无丢失原因。 |
| `lost_reason` | `1` | `LOST_REASON_TRACKING` | 目标跟踪丢失。 |
| `lost_reason` | `2` | `LOST_REASON_TIMEOUT` | 等待或丢锁超时。 |
| `lost_reason` | `3` | `LOST_REASON_STALE` | 目标数据陈旧。 |
| `lost_reason` | `4` | `LOST_REASON_MANUAL` | 人工摇杆接管。 |
| `lost_reason` | `5` | `LOST_REASON_PRECONDITION` | 前置条件失败。 |
| `lost_reason` | `6` | `LOST_REASON_JITTER` | 抖动或链路间隔异常。 |

### 字段表

| Field Name | Type | Units | Values | Description |
| --- | --- | --- | --- | --- |
| `timestamp` | `uint64_t` | `us` |  | PX4 system boot 以来的消息发布时间。 |
| `state` | `uint8_t` |  | 见状态值表 | DYT 制导状态机当前状态。 |
| `requested_submode` | `uint8_t` |  | `0=FOLLOW`, `1=INTERCEPT` | 当前请求的子模式，通常由拦截拨杆决定。 |
| `active_submode` | `uint8_t` |  | `0=FOLLOW`, `1=INTERCEPT` | 当前实际生效的子模式。 |
| `lost_reason` | `uint8_t` |  | 见状态值表 | 最近一次丢锁、中止或退出原因。 |
| `active` | `uint8_t` |  | `0/1` | DYT 制导是否处于活动状态。 |
| `target_locked` | `uint8_t` |  | `0/1` | 吊舱目标是否锁定。 |
| `controlling_vehicle` | `uint8_t` |  | `0/1` | DYT 制导是否正在接管飞机轨迹/offboard setpoint。 |
| `target_fresh` | `uint8_t` |  | `0/1` | 最新目标样本是否新鲜。 |
| `intercept_allowed` | `uint8_t` |  | `0/1` | 当前是否允许进入 Intercept 子模式。 |
| `los_age_s` | `float` | `s` |  | 当前 LOS 样本年龄。 |
| `frame_dt_s` | `float` | `s` |  | 最新 DYT 目标遥测帧间隔。 |
| `delay_s` | `float` | `s` |  | 末制导固定管线延迟参数。 |
| `los_ned_x` | `float` |  |  | 滤波后的 LOS 向量 NED X 分量。 |
| `los_ned_y` | `float` |  |  | 滤波后的 LOS 向量 NED Y 分量。 |
| `los_ned_z` | `float` |  |  | 滤波后的 LOS 向量 NED Z 分量。 |
| `omega_los_ned_x` | `float` | `rad/s` |  | LOS 角速度估计 NED X 分量。 |
| `omega_los_ned_y` | `float` | `rad/s` |  | LOS 角速度估计 NED Y 分量。 |
| `omega_los_ned_z` | `float` | `rad/s` |  | LOS 角速度估计 NED Z 分量。 |
| `velocity_sp_x` | `float` | `m/s` |  | 末制导速度 setpoint NED X 分量。 |
| `velocity_sp_y` | `float` | `m/s` |  | 末制导速度 setpoint NED Y 分量。 |
| `velocity_sp_z` | `float` | `m/s` |  | 末制导速度 setpoint NED Z 分量。 |
| `acceleration_sp_x` | `float` | `m/s/s` |  | 末制导加速度前馈 NED X 分量。 |
| `acceleration_sp_y` | `float` | `m/s/s` |  | 末制导加速度前馈 NED Y 分量。 |
| `acceleration_sp_z` | `float` | `m/s/s` |  | 末制导加速度前馈 NED Z 分量。 |
| `yaw_sp` | `float` | `rad` |  | 末制导 yaw setpoint。 |
| `yaw_rate_sp` | `float` | `rad/s` |  | 末制导 yaw-rate setpoint。 |

