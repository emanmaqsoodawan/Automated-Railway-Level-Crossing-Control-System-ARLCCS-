#  Task 2: Formalize Constraints

##  Propositional Variables

| Identifier | Domain Class | Description |
| :--- | :--- | :--- |
| `Train_In_Crossing` | Sensor Input | A train is currently occupying the level crossing zone. |
| `Train_Approaching` | Sensor Input | An inbound train has triggered the approach block detector. |
| `Train_Cleared` | Sensor Input | Downstream exit sensors confirm the train has fully cleared the crossing. |
| `Barrier_Open` | Actuator State | The road barrier arms are raised (open to road traffic). |
| `Barrier_Closed` | Actuator State | The road barrier arms are fully lowered and locked. |
| `Barrier_Closing` | Actuator State | The road barrier arms are in active downward motion. |
| `Warnings_Active` | Signaling Output | Acoustic alarms and flashing red road warning lights are engaged. |
| `Road_Signal_Red` | Signaling Output | Road traffic lights display a STOP (RED) aspect. |
| `Train_Signal_Green` | Signaling Output | The railway trackside signal displays a CLEAR / PROCEED (GREEN) aspect. |
| `Obstacle_Detected` | Safety Monitor | Optical/LiDAR surveillance detects an obstruction on the track grid. |
| `Sensor_Failure` | Diagnostic Flag | Built-in self-tests detect an electrical, parity, or reporting sensor fault. |
| `Comm_Loss` | System Health | Heartbeat/telemetry link between the controller and safety unit drops. |

---

##  Formal Logical Expressions

### `C1` Barrier Interlock with Train Presence
> The physical road barriers must never be raised while a train occupies the crossing perimeter.

$$Train\_In\_Crossing \rightarrow \neg Barrier\_Open$$

* **Equivalent Negation:**
  $$\neg (Train\_In\_Crossing \land Barrier\_Open)$$

---

### `C2` Warning Signals During Barrier Engagement
> Warning lights and acoustic alarms must remain active during downward descent and throughout the duration barriers remain closed.

$$(Barrier\_Closing \lor Barrier\_Closed) \rightarrow Warnings\_Active$$

---

### `C3` Road Signal Prohibition
> Road traffic signals must display RED whenever an incoming train approaches or occupies the level crossing.

$$(Train\_Approaching \lor Train\_In\_Crossing) \rightarrow Road\_Signal\_Red$$

---

### `C4` Train Signal Interlock with Barrier State
> The railway trackside signal may grant clear passage (GREEN) only if the barriers are verified to be fully lowered and locked.

$$Train\_Signal\_Green \rightarrow Barrier\_Closed$$

* **Contrapositive Formulation:**
  $$\neg Barrier\_Closed \rightarrow \neg Train\_Signal\_Green$$

---

### `C5` Barrier Opening Release Condition
> The barrier arms can only transition to the open position after the train has fully exited the block and no new train is approaching.

$$Barrier\_Open \rightarrow (Train\_Cleared \land \neg Train\_Approaching)$$

---

### `C6` Track Obstruction Interlock
> The trackside railway signal must immediately revert from GREEN if an obstacle or trapped vehicle is detected inside the crossing.

$$Obstacle\_Detected \rightarrow \neg Train\_Signal\_Green$$

---

### `C7` Sensor Fault Fail-Safe Enforcement
> If any train detection sensor reports a fault or invalid reading, the system must force a fail-safe state (barriers locked down and trains halted).

$$Sensor\_Failure \rightarrow (Barrier\_Closed \land \neg Train\_Signal\_Green)$$

---

### `C8` Communication Loss Fail-Safe
> If the telemetry heartbeat between the safety-monitoring unit and trackside controller drops, clear railway signals must immediately be revoked.

$$Comm\_Loss \rightarrow \neg Train\_Signal\_Green$$
