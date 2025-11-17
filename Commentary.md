 

## Introduction

I implemented an NPC AI-driven vehicle control system inside Unreal Engine 5 using the Vehicle Template. The system makes steering decisions every frame and functions as a lightweight, real-time AI driver.  

I then captured and analysed performance telemetry using Unreal Insights, measuring inference time, execution counts, and hardware performance during gameplay.


--- 

 
## AI System Overview

The AI car drives autonomously by following a spline placed around the track. Each frame, the system:
- Locates the closest point on the spline,
- Calculates a look-ahead position using the spline tangent,
- Computes the rotation required to steer toward the next spline point,
- Converts this into a steering value (–1 to 1),
- Applies a constant throttle for forward movement.

This logic runs in real time through **Event Tick**, enabling continuous, adaptive steering decisions and smooth autonomous driving behaviour.
   

 

--- 

 

## Telemetry Collection Using Unreal Insights 

 

The system was profiled using **Unreal Insights**. The trace was recorded in **Simulate** mode so that only the AI execution influenced the timing results. Once the countdown finished and the vehicle began driving, a 20–30 second trace session was captured. 

 

Unreal Insights automatically tracked execution counts, timing for each Blueprint node, and total cost. The **Timers** panel recorded aggregated totals, while **Callers/Callees** offered node-level breakdowns. 

 

### Data Collected 

 

From the Timers panel: 

 

- **AI execution count:** 6,242   

- **Total inclusive AI time:** 458.3 ms   

- **Exclusive AI time:** 45.9 ms   

- **Average inference time per tick:** ~0.073 ms (≈ 73 µs) 

## Calculating the Average AI Inference Time 

 

In this project, the main AI Blueprint (`BP_Car_AI_C`) executed **6,242 times**, with a total inclusive time of **458.3 ms**. The average inference time was calculated using the following formula: 

 


Average inference time per tick =  
**458.3 ms ÷ 6,242 ≈ 0.073 ms**

 

This means the AI system takes approximately **0.073 milliseconds (≈73 microseconds)** to process each decision.  

The entire AI logic consumes under 0.1 milliseconds per frame, making it extremely lightweight. 



 ![Insights3](https://github.com/user-attachments/assets/b6b5a7ce-6e89-425e-bb3d-7c100208397c)

![Insights2](https://github.com/user-attachments/assets/5575cd19-c15c-4536-9768-1f1dfd454051)
![Insights](https://github.com/user-attachments/assets/f5be8954-24ca-40e1-85fb-7e596b2ee28c)

### Blueprint Node-Level Costs 

 

The Callers/Callees panel revealed detailed per-node execution times: 

 

- **Find Tangent Closest to World Location:** ~60 µs   

- **Find Location Closest to World Location:** ~56 µs   

- **Find Look At Rotation:** ~3.9 µs   

- **Normalize / Vector math nodes:** 1–10 µs   

- **MapRangeClamped:** ~1.4 µs   

- **Set Steering Input:** ~3.1 µs   

 

This confirms that most of the AI’s computational cost comes from spline queries and rotation calculations, which is expected for a steering-based control algorithm. 

 

The Timeline view also showed a stable, repeating AI workload on every frame with no performance spikes. 

 

--- 

 

## Platform and Hardware 

 

Unreal Insights recorded platform and build information automatically: 

 

- **Platform:** Win64   

- **Engine:** Unreal Engine 5.6 (Development Editor)   

- **Build Config:** Development   

 

The machine used for testing had the following hardware: 

 

- **CPU:** Intel Core i9-11900F @ 2.50GHz   

- **GPU:** NVIDIA GeForce RTX 3080   

- **RAM:** 32 GB   

- **Operating System:** Windows 10 (64-bit) 

 

 
