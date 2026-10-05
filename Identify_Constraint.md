Task 1 — Identify Constraints

Below are 10 safety and operational constraints required for the Automated Railway Level-Crossing Control System (ARLCCS).

C1: The barriers must not open while a train is present inside the crossing zone.

Reason: Opening the barriers during train occupancy exposes vehicular and pedestrian traffic to immediate collisions.

C2: Warning lights and audible alarms must be active whenever the barriers are closing or fully closed.

Reason: Road users approaching the crossing need continuous visual and audible alerts to halt before reaching the barrier.

C3: Road traffic signals must display RED whenever a train approaches, enters, or occupies the crossing.

Reason: Vehicles must be legally and visually barred from entering the intersection area ahead of barrier deployment.

C4: The railway signal must display STOP (RED) if the barriers fail to reach the fully closed and locked state.

Reason: A train must never receive clearance to traverse the intersection if physical barricades fail to secure the track.

C5: The barriers must not begin opening until the exit sensors confirm the train has completely cleared the crossing.

Reason: Premature barrier ascent while the rear wagons are still clearing the roadway risks high-speed impact with early-accelerating vehicles.

C6: If an obstacle is detected on the tracks inside the crossing zone, the incoming train signal must be set to STOP.

Reason: Trapped vehicles or pedestrians will cause a catastrophic collision unless the oncoming train is forced into emergency braking.

C7: If a train detection sensor fails or produces inconsistent readings, the system must trigger a failsafe state (barriers closed, train signaled to STOP).

Reason: In safety-critical systems, loss of sensing capability must default to the most restrictive safe state rather than assuming track clearance.

C8: Warning signals (lights and alarms) must activate for a mandated minimum warning time before the barriers begin downward motion.

Reason: Trapped vehicles already on the crossing need time to exit safely without being pinned down by sudden barrier closure.

C9: If communication is lost between the safety-monitoring unit and the crossing controller, train signals must revert to STOP.

Reason: The control system cannot verify barrier integrity or track state without live telemetry, requiring trains to halt before entering the block.

C10: Road traffic signals must never display GREEN simultaneously with the railway signal displaying CLEAR (GREEN).

Reason: Conflicting green signals simultaneously grant right-of-way to both trains and road traffic, causing a right-angle collision.
