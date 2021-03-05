# LinuxCNC - PrintNC config files using a Mesa 7i76e

## Overview
These are my settings for the LinuxCNC for the machine PrintNC. They are targeted to the Mesa 7i76e FPGA board but might be helpful for other Meas or parallel BOB boards. 

## file Status
- Sofar I've only configured the steppers in the right configuration for a gantry style machine (with joints XYYZ).

## INI file notes

**[KINS]** we need to add an extra joint from 3 to 4, coordinates has another joint: **XYYZ** and `kinstype=BOTH` to coordinat the movement of the two **"Y"** joints together

```
[KINS]
JOINTS = 4
KINEMATICS = trivkins coordinates=XYYZ kinstype=BOTH
...
```

**[TRAJ]** It is not necessary but mayebe common practice to add the extra **Y** axis here:
```
[TRAJ]
COORDINATES =  XYYZ
...
```

## HAL file notes

We need to added `pid.y2` and renamed `pid.y` to `pid.y1`
```
loadrt pid names=pid.x,pid.y1,pid.y2,pid.z,pid.s
```

For both Y joints, we `setp` to the correct pid corrasponding to the joint set in the [.ini](Howard_PrintNC.ini) file
```python
#  AXIS Y JOINT 1/2
setp   pid.y1.Pgain     [JOINT_1]P

...

#  AXIS Y JOINT 2/2
setp   pid.y2.Pgain     [JOINT_2]P
```


If you are wiring your motors to the mesa board in the order of XYYZ than the Z axis index gets bumped one joint over (set in the [.ini](Howard_PrintNC.ini) file):
```python
#  AXIS Z JOINT 3

setp   pid.z.Pgain     [JOINT_3]P
```
