---
layout : page
title: Robotics
date : 2025-09-13
---



* Let's assume a **moving robot** has **two sensors**: IMU and GPS.

The IMU (Inertial Measurement Unit) provides data at a high sampling rate—for example, 100 Hz or even 1000 Hz. This means it gives you new acceleration (and sometimes orientation) readings many times per second. However small errors in acceleration measurements accumulate over time, leading to position **drift**. So although it's good short-term but integrates noise long-term.

 GPS provides global, drift-free position (relative to Earth) updates at a low sampling rate—typically 1 Hz to 10 Hz. It doesn't drift over time and thus **corrects** the long-term drift of the IMU.

Sensor Fusion (e.g., Kalman Filter/Particle Filter) is used to combine the data from these sensors **probabilistically** to estimate the robot's true position more accurately than either sensor alone.

---

## 3. Kalman Filter (KF) in Plain Terms

The Kalman Filter is a recursive algorithm that estimates the true state of a system from noisy measurements, using a **predict-update** cycle.


### **1. Notation and Variables**
| Symbol | Meaning                                                                 |
|--------|-------------------------------------------------------------------------|
| \( x \)      | State vector (e.g., position, velocity of the robot)                   |
| \( x' \)     | Predicted state (a priori estimate)                                    |
| \( F \)      | State transition matrix (how the state evolves over time)              |
| \( B \)      | Control input matrix (how control inputs affect the state)             |
| \( u \)      | Control input (e.g., acceleration command)                             |
| \( P \)      | Estimate covariance (uncertainty in the current state estimate)                |
| \( P' \)     | Predicted covariance (a priori uncertainty)                            |
| \( Q \)      | Process noise covariance (uncertainty in the model/prediction)         |
| \( z \)      | Measurement vector (e.g., GPS position)                                |
| \( H \)      | Measurement matrix (how the state maps to measurements)                |
| \( R \)      | Measurement noise covariance (uncertainty in the measurements)         |
| \( K \)      | Kalman Gain (weights the prediction vs. measurement)                   |

---

### **2. Predict Step**
The **predict step** uses the system model to estimate the next state and its uncertainty.


Let's revise some basic maths first.

Assume \( x \) is a \( n \times 1 \) vector, and  \( A \) is a \( m \times n \) matrix to transform \( x \) to a \( m \times 1 \) vector \( y \)
$$
y = Ax
$$
If \( x \) is a random vector, then its covariance matrix \( \Sigma_{n \times n} \) (which denotes uncertainty) will also be transformed as follows:
$$
\Sigma_y = A \Sigma A^T
$$
where \( \Sigma_y \) is of shape \( m \times m \).

Now let's proceed with our robot example.

#### **State Prediction:**
\[ x' = Fx + Bu \]
- \( x' \): Predicted state at the next time step.
- \( Fx \): Predicts how the current state \( x \) evolves over time.
- \( Bu \): Accounts for any control input (e.g., acceleration from motors).
- **Intuition:** This is like saying, "Based on the current state and what I know about the system, here’s where I think the robot will be next."

#### **Covariance Prediction:**
\[ P' = FPF^T + Q \]
- \( FPF^T \): Propagates the current uncertainty \( P \) through the state transition.
- \( Q \): Adds uncertainty due to **process noise** (e.g., IMU drift, unmodeled dynamics).
- **Intuition:** "My prediction isn’t perfect—here’s how uncertain I am about it."

---

### **3. Update Step**
The **update step** corrects the prediction using a noisy measurement.

#### **Kalman Gain:**

\[ K = P'H^T(HP'H^T + R)^{-1} \]
- \( P'H^T \): Cross-covariance between the state and measurement.
- \( HP'H^T + R \): Total uncertainty in the measurement space (prediction uncertainty + measurement noise).
- **Intuition:** \( K \) determines how much to trust the measurement vs. the prediction. If \( R \) (measurement noise) is large, \( K \) becomes small, and the filter trusts the prediction more. If \( P' \) (prediction uncertainty) is large, \( K \) becomes large, and the filter trusts the measurement more.

#### **State Update:**
\[ x = x' + K(z - Hx') \]
- \( z - Hx' \): **Innovation** or **residual** (difference between the actual measurement and the predicted measurement).
- \( K(z - Hx') \): Weighted correction term.
- **Intuition:** "My measurement says I’m off by this much—let’s adjust the state estimate accordingly."

#### **Covariance Update:**
\[ P = (I - KH)P' \]
- \( (I - KH) \): Reduces the uncertainty after incorporating the measurement.
- **Intuition:** "Now that I’ve seen the measurement, I’m more confident in my estimate."

---

### **4. Why It Works**
- **Recursive:** The Kalman Filter doesn’t need to store all past data—it only needs the current state and covariance.
- **Optimal:** It minimizes the mean squared error of the estimate, making it the best linear estimator for Gaussian noise.
- **Balances Trust:** The Kalman Gain \( K \) dynamically balances trust between the prediction and the measurement based on their uncertainties.

---

### **5. Example: 1D Robot Localization**
Suppose a robot moves in 1D, and you have:
- **State:** \( x = [\text{position}, \text{velocity}]^T \)
- **Process Model:** \( F = \begin{bmatrix} 1 & \Delta t \\ 0 & 1 \end{bmatrix} \), \( B = \begin{bmatrix} \Delta t^2/2 \\ \Delta t \end{bmatrix} \)
- **Measurement Model:** \( H = \begin{bmatrix} 1 & 0 \end{bmatrix} \) (you only measure position, not velocity).

#### **Predict:**
- Predict the robot’s position and velocity using its dynamics.
- Increase uncertainty due to process noise (e.g., IMU noise).

#### **Update:**
- When GPS gives a position measurement, use \( K \) to correct the state and reduce uncertainty.

---

### **6. Key Insights**
- **\( Q \) and \( R \):** These matrices are tuned based on how noisy the system and measurements are. Poor tuning leads to either ignoring measurements or overreacting to noise.
- **Covariance \( P \):** Tracks how uncertain the filter is. Small \( P \) means high confidence; large \( P \) means low confidence.
- **Kalman Gain \( K \):** The "magic" of the filter. It automatically adjusts based on the relative uncertainties of the prediction and measurement.

---
**Question:** Would you like a concrete numerical example to see how these equations play out in practice?
* **State vector**:

  $$
  x =
  \begin{bmatrix}
  position \\
  velocity
  \end{bmatrix}
  $$
* **Predict (using IMU)**:
  Update position and velocity from acceleration. Uncertainty grows.
* **Update (using GPS)**:
  Correct prediction with measured GPS position. Uncertainty shrinks.
* **Mathematics**:

  * Predict:
    $x' = F x + B u$
    $P' = F P F^T + Q$
  * Update:
    $K = P' H^T (H P' H^T + R)^{-1}$
    $x = x' + K(z - Hx')$
    $P = (I - KH) P'$
    where $Q$ = process noise, $R$ = measurement noise, $P$ = covariance.

---

## 4. ROS Concepts Mapping

ROS isn’t math — it’s how we **structure software** to move data between algorithms. Here’s how theory plugs into ROS:

| Robotics Concept           | ROS Concept                                                                            |
| -------------------------- | -------------------------------------------------------------------------------------- |
| IMU, GPS sensors           | **Nodes** publishing messages                                                          |
| Sensor data streams        | **Topics** (`/imu/data`, `/gps/position`)                                              |
| Messages (accel, position) | **Message types** (`sensor_msgs/Imu`, `std_msgs/Float64`)                              |
| Fusion algorithm (KF)      | **Node** subscribing to `/imu/data` & `/gps/position`, publishing `/estimate/position` |
| Running everything         | `ros2 run` starts nodes; topics connect automatically                                  |
| Entry points (`setup.py`)  | How ROS knows which Python function = which executable node                            |

So in this demo:

* **`sensor_simulator` node**: fakes sensor data (IMU + GPS) → publishes to topics.
* **`simple_kf` node**: subscribes to those topics → runs Kalman filter → publishes fused estimate.
* **ROS topics** are the glue: pub/sub connects nodes without hardcoding.
* **Entry points** let you launch each node with `ros2 run my_first_package node_name`.

---

## 5. Big Picture Flow

1. **Simulator** generates noisy data:

   * Publishes IMU acceleration at 50 Hz.
   * Publishes GPS position at 1 Hz.
2. **ROS transports data** through topics.
3. **KF Node**:

   * Every IMU msg → KF prediction step.
   * Every GPS msg → KF update step.
   * Publishes fused estimate (`/estimate/position`).
4. **User** can visualize with `ros2 topic echo /estimate/position`.

---

## 6. Why This Matters

* This toy example = **miniature version** of what happens in real robots (cars, drones, rovers).
* In real systems, KF/UKF fuse:

  * IMU, wheel odometry, GPS, LiDAR, cameras, radar…
* ROS lets you **modularize**: each sensor driver publishes; fusion nodes subscribe; mapping/localization uses fused data.

---

✅ Eagle-eye view:

* **Simulator Node** = fake world → noisy sensors.
* **KF Node** = brain → estimates state probabilistically.
* **ROS Pub/Sub** = nervous system → moves sensor signals around.
* **Entry points** = ROS launcher → makes nodes executable.

---

Would you like me to now draw a **block diagram** (sensor nodes → topics → KF node → estimate) so you see the architecture visually before you code?
