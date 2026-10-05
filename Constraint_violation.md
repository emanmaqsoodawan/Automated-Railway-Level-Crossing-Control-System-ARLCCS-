# Task 3 - Identify Constraint Violations

A formal violation occurs whenever the antecedent (condition) is TRUE while the consequent (required state) is FALSE. In this situation, the implication P -> Q evaluates to FALSE.

## Violation 1 - Relates to C1

### Variable Assignment

Train_In_Crossing = TRUE
Barrier_Open = TRUE

### Failure Analysis

A hydraulic valve failure or manual override mechanism releases the barrier latch while an 8-car passenger train traverses the intersection. This violation is detected because the track circuit registers track occupancy, while the upper limit switch of the barrier arm reports a raised position.

## Violation 2 - Relates to C2

### Variable Assignment

Barrier_Closed = TRUE
Warnings_Active = FALSE

### Failure Analysis

The flasher relay blows a fuse or the audio enunciators burn out while the barrier rests in the downward horizontal position at night. The safety-monitoring current-sensing circuit registers 0 A draw across the warning circuits while the barrier limit switch reports that the barrier is fully lowered.

## Violation 3 - Relates to C3

### Variable Assignment

Train_Approaching = TRUE
Road_Signal_Red = FALSE

### Failure Analysis

The digital output module controlling the red signal fails to trigger because of a stuck relay contact or software scheduling freeze. This leaves yellow or green lights visible to motorists as a train enters the approach block. The system verifies this fault because the train proximity detector registers a positive signal, while the current monitor on the road signal shows no illumination on the red lamp circuit.

## Violation 4 - Relates to C4

### Variable Assignment

Train_Signal_Green = TRUE
Barrier_Closed = FALSE

### Failure Analysis

An obstruction, such as fallen debris, blocks the barrier at 45 degrees and prevents it from lowering. However, an unvalidated logic path grants the oncoming train a green signal. The railway aspect telemetry reads clear while the barrier proximity switches indicate that the lowered and locked interlock is not engaged.

## Violation 5 - Relates to C5

### Variable Assignment

Barrier_Open = TRUE
Train_Cleared = FALSE

### Failure Analysis

A slow-moving freight train triggers a premature timeout timer, initiating the barrier-raising sequence while the long rear freight cars are still traversing the roadway. The barrier angle sensors report upward movement even though downstream track axle counters show an imbalance, meaning the number of axles entered is greater than the number of axles exited.

## Violation 6 - Relates to C6

### Variable Assignment

Obstacle_Detected = TRUE
Train_Signal_Green = TRUE

### Failure Analysis

An optical LiDAR or loop detector detects a stalled vehicle across the crossing tracks, but the controller fails to drop the interlocking relay and cancel the train's approach signal. A safety audit verifies this breach because the obstacle monitor input is HIGH while the interlocking output contact for the railway green signal remains energized.

## Violation 7 - Relates to C7

### Variable Assignment

Sensor_Failure = TRUE
Barrier_Closed = FALSE
Train_Signal_Green = TRUE

### Failure Analysis

A short circuit occurs in the approach track inductive loop, causing the system to receive null data. Instead of entering a fail-safe restrictive mode, the system leaves the barriers upright and maintains normal train signals. The system watchdog detects self-test error flags on the sensor bus, which are active while the barrier actuator line remains inactive.

## Violation 8 - Relates to C8

### Variable Assignment

Comm_Loss = TRUE
Train_Signal_Green = TRUE

### Failure Analysis

A fiber-optic disconnect or high packet jitter disrupts communication between the Safety Monitoring Unit and the signal mast controller. Instead of defaulting to safe dark or red signaling, the track signal maintains its last-known clear state. A heartbeat timeout is recorded in the system error log while current continues flowing to the green signal bulb.

# Verification Summary

C1 - Barrier remains down with train present

Rule:
Train_In_Crossing -> NOT Barrier_Open

Diagnostic Method:
Track circuit state vs. barrier angle limit switch


C2 - Warnings active when barrier engaged

Rule:
(Barrier_Closing OR Barrier_Closed) -> Warnings_Active

Diagnostic Method:
Current-sensing transducer on warning circuit


C3 - Road red on approach or occupancy

Rule:
(Train_Approaching OR Train_In_Crossing) -> Road_Signal_Red

Diagnostic Method:
Digital signal loopback monitoring


C4 - Train stopped if barrier not closed

Rule:
Train_Signal_Green -> Barrier_Closed

Diagnostic Method:
Physical interlock switch verification before signal clear


C5 - No opening until train clears

Rule:
Barrier_Open -> (Train_Cleared AND NOT Train_Approaching)

Diagnostic Method:
Axle counter differential balance check


C6 - Stop train on track obstruction

Rule:
Obstacle_Detected -> NOT Train_Signal_Green

Diagnostic Method:
LiDAR/RADAR zone occupancy interrupt check


C7 - Default to safe mode on sensor fault

Rule:
Sensor_Failure -> (Barrier_Closed AND NOT Train_Signal_Green)

Diagnostic Method:
Built-in self-test (BIST) heartbeat and loop diagnostics


C8 - Restrictive signal on communication link drop

Rule:
Comm_Loss -> NOT Train_Signal_Green

Diagnostic Method:
Telemetry watchdog timer expiration drop
