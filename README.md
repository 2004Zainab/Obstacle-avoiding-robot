# Obstacle-Avoiding Robot with Motion Detection

An Arduino-based two-wheel-drive robot that autonomously navigates around obstacles using dual ultrasonic sensors, and pauses movement when a PIR motion sensor detects a person or object nearby.

## Features

- **Autonomous obstacle avoidance** — two ultrasonic sensors (front-left and front-right) detect obstacles within 3 inches and steer the robot away from them.
- **Motion-based safety stop** — a PIR motion sensor halts the robot for 3 seconds whenever motion is detected nearby, then resumes normal operation.
- **PWM speed control** — motor speed is controlled via `analogWrite` on dedicated speed pins for smooth, adjustable movement.
- **Serial debug output** — current robot state (`fwd`, `turnleft`, `turnright`, `stop`) is printed over Serial at 115200 baud for easy debugging.

## Hardware Required

- Arduino (Uno/Nano or similar)
- L298N (or similar) dual H-bridge motor driver
- 2x DC gear motors + wheels
- 2x Ultrasonic distance sensors (HC-SR04 or similar, trigger + echo)
- 1x PIR motion sensor
- Robot chassis + power supply (battery pack)
- Jumper wires

## Pin Configuration

| Component                  | Arduino Pin |
|-----------------------------|:-----------:|
| Right motor input 1 (RM1)   | 7           |
| Right motor input 2 (RM2)   | 6           |
| Left motor input 1 (LM1)    | 5           |
| Left motor input 2 (LM2)    | 4           |
| Right motor speed (PWM)     | 10          |
| Left motor speed (PWM)      | 11          |
| Ultrasonic sensor 1 trigger | 9           |
| Ultrasonic sensor 1 echo    | 8           |
| Ultrasonic sensor 2 trigger | 3           |
| Ultrasonic sensor 2 echo    | 2           |
| PIR motion sensor           | 12          |

> Note: Sensor 1 is treated as the left-side sensor and sensor 2 as the right-side sensor in the avoidance logic below — wire them to match your chassis layout, or swap the pin numbers if your build is mirrored.

## How It Works

1. On each loop, the robot checks the PIR motion sensor.
   - If motion is detected → the robot stops for 3 seconds.
   - Otherwise → the robot moves forward.
2. Both ultrasonic sensors are pulsed to measure distance to the nearest obstacle (converted to inches).
3. Based on the readings:
   - If sensor 1 detects an obstacle within 3 inches → turn left.
   - Else if sensor 2 detects an obstacle within 3 inches → turn right.
   - Otherwise → continue moving forward.

## Installation

1. Install the [Arduino IDE](https://www.arduino.cc/en/software).
2. Wire the components according to the pin table above.
3. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   ```
4. Open the `.ino` file in the Arduino IDE.
5. Select your board and port under **Tools**.
6. Upload the sketch.
7. Open the Serial Monitor (115200 baud) to view live status output.
