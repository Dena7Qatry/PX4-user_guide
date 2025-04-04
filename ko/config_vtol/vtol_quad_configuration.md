# QuadPlane VTOL 설정 및 튜닝

이것은 QuadPlane VTOL(Quadcopter와 결합된 비행기)에 대한 설정 문서입니다. 기체별 문서와 조립 방법은 [VTOL 프레임 조립](../frames_vtol/README.md)를 참고하십시오.

## 펌웨어 및 기본 설정

1. *QGroundControl*을 실행합니다
2. 마스터 펌웨어 플래시
3. 설정 탭에서 적절한 VTOL 기체를 선택하고, 기체가 목록에 없으면 Fun Cub VTOL 기체를 선택합니다.


### 비행 / 전환 모드 스위치

You should assign a switch on your RC controller for switching between the multicopter- and fixed wing modes.

:::note
While PX4 allows flight without an RC controller, you must have one when tuning/configuring up a new airframe.
:::

This is done in [Flight Mode](../config/flight_mode.md) configuration, where you [assign flight modes and other functions](../config/flight_mode.md#what-flight-modes-and-switches-should-i-set) to switches on your RC controller. The switch can also be assigned using the parameter [RC_MAP_TRANS_SW](../advanced_config/parameter_reference.md#RC_MAP_TRANS_SW).

The switch in the off-position means that you are flying in multicopter mode.


### 멀티콥터 / 고정익 튜닝

Before you attempt your first transition to fixed wing flight you need to make absolutely sure that your VTOL is well tuned in multirotor mode. One reason is this is the mode you will return to if something goes wrong with a transition and it could be it will be moving fairly quickly already. If it isn’t well tuned bad things might happen.

If you have a runway available and the total weight isn’t too high you will also want to tune fixed wing flight as well. If not then you will be attempting this when it switches to fixed wing mode. If something goes wrong you need to be ready (and able) to switch back to multirotor mode.

Follow the respective tuning guides on how to tune multirotors and fixed wings.


### 전환 튜닝

While it might seem that you are dealing with a vehicle that can fly in two modes (multirotor for vertical takeoffs and landings and fixed wing for forwards flight) there is an additional state you also need to tune: transition.

Getting your transition tuning right is important for obtaining a safe entry into fixed wing mode, for example, if your airspeed is too slow when it transitions it might stall.

#### 전환 스로틀

Parameter: [VT_F_TRANS_THR](../advanced_config/parameter_reference.md#VT_F_TRANS_THR)

Front transition throttle defines the target throttle for the pusher/puller motor during the front transition.

This must be set high enough to ensure that the transition airspeed is reached. If your vehicle is equipped with an airspeed sensor then you can increase this parameter to make the front transition complete faster. For your first transition you are better off setting the value higher than lower.

Parameter: [VT_B_TRANS_THR](../advanced_config/parameter_reference.md#VT_B_TRANS_THR)

Generally back-transition throttle can be set to 0 since forward thrust is not (in most cases) desirable. If the motor controller supports reverse thrust however, you can achieve this by setting a negative value.

#### Forward Transition Pusher/Puller Slew Rate

Parameter: [VT_PSHER_SLEW](../advanced_config/parameter_reference.md#VT_PSHER_SLEW)

A forward transition refers to the transition from multirotor to fixed-wing mode. The forward transition pusher/puller slew rate is the amount of time in seconds that should be spent ramping up the throttle to the target value (defined by `VT_F_TRANS_THR`).

A value of 0 will result in commanding the transition throttle value being set immediately. By default the slew rate is set to 0.33, meaning that it will take 3s to ramp up to 100% throttle. If you wish to make throttling-up smoother you can reduce this value.


Note that once the ramp up period ends throttle will be at its target setting and will remain there until (hopefully) the transition speed is reached.


#### 블렌딩 속도

Parameter: [VT_ARSP_BLEND](../advanced_config/parameter_reference.md#VT_ARSP_BLEND)

By default, as the airspeed gets close to the transition speed, multirotor attitude control will be reduced and fixed wing control will start increasing continuously until the transition occurs.

Disable blending by setting this parameter to 0 which will keep full multirotor control and zero fixed wing control until the transition occurs.


#### 전환 대기속도

Parameter: [VT_ARSP_TRANS](../advanced_config/parameter_reference.md#VT_ARSP_TRANS)

This is the airspeed which, when reached, will trigger the transition out of multirotor mode into fixed wing mode. It is critical that you have properly calibrated your airspeed sensor. It is also important that you pick an airspeed that is comfortably above your airframes stall speed (check `FW_AIRSPD_MIN`) as this is currently not checked.

### 전환 팁

As already mentioned make sure you have a well tuned multirotor mode. If during a transition something goes wrong you will switch back to this mode and it should be quite smooth.

Before you fly have a plan for what you will do in each of the three phases (multirotor, transition, fixed wing) when you are in any of them and something goes wrong.

Battery levels: leave enough margin for a multirotor transition for landing at the end of your flight. Don’t run your batteries too low as you will need more power in multirotor mode to land. Be conservative.

#### 전환: 준비하기

Make sure you are at least 20 meters above ground and have enough room to complete a transition. It could be that your VTOL will lose height when it switches to fixed wing mode, especially if the airspeed isn’t high enough.

Transition into the wind, whenever possible otherwise it will travel further from you before it transitions.

Make sure the VTOL is in a stable hover before you start the transition.


#### 전환 : 멀티콥터에서 고정익 (전방 전환)

Start your transition. It should transition within 50 – 100 meters. If it doesn’t or it isn’t flying in a stable fashion abort the transition (see below) and land or hover back to the start position and land. Try increasing the [transition throttle](#transition-throttle) (`VT_F_TRANS_THR`) value. Also consider reducing the transition duration (`VT_F_TRANS_DUR`) if you are not using an airspeed sensor. If you are using an airspeed sensor consider lowering the transition airspeed but stay well above the stall speed.

As soon as you notice the transition happen be ready to handle height loss which may include throttling up quickly.

:::warning
The following feature has been discussed but not implemented yet:
- Once the transition happens the multirotor motors will stop and the pusher/puller throttle will remain at the `VT_F_TRANS_THR` level until you move the throttle stick, assuming you are in manual mode. :::

#### 전환 : 고정익에서 멀티콥터로 (역 전환)

When you transition back to multirotor mode bring your aircraft in on a straight level approach and reduce its speed, flip the transition switch and it will start the multirotor motors and stop the pusher/puller prop immediately and should result in a fairly smooth gliding transition.

Consider that the throttle value you have when you transition will command the amount of thrust your multirotor has at the moment of the switch. Because the wing will still be flying you’ll find you have plenty of time to adjust your throttle to achieve/hold a hover.

For advanced tuning of the back-transition please refer to the [Back-transition Tuning Guide](vtol_back_transition_tuning.md)

#### 전환 중지

It’s important to know what to expect when you revert a transition command *during* a transition.

When transitioning from **multirotor to fixed wing** (transition switch is on/fixed wing) then reverting the switch back (off/multirotor position) *before* the transition happens it will immediately return to multirotor mode.

When transitioning from **fixed wing to multirotor** for this type of VTOL the switch is immediate so there isn’t really a backing out option here, unlike for tilt rotor VTOLs. If you want it to go back into fixed wing you will need to go through the full transition. If it’s still travelling fast this should happen quickly.


### 지원

If you have any questions regarding your VTOL conversion or configuration please see [https://discuss.px4.io/c/px4/vtol](https://discuss.px4.io/c/px4/vtol).
