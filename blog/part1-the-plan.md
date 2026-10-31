# Building a MicroDuck, Part 1: The Plan

I'm building a small two-legged duck robot, and this series follows the build from first idea to first steps. This first part is the plan: what I'm building, which design I'm following, what I still need to find out, and how the series is laid out.

## What I'm building

The robot is a 25 cm duck-shaped biped. It started as **Microduck** by Pollen Robotics, which has 15 servos in its legs, neck, head and beak, and walks with a policy trained by reinforcement learning in simulation.

I'm not building Pollen's own robot. I'm building the **XGO-Duck** by Luwu Dynamics, an adaptation of Microduck whose hardware, runtime and training tools are all published on GitHub:

- Hardware: [LuwuDynamics/xgoduck_hardware](https://github.com/LuwuDynamics/xgoduck_hardware)
- Onboard runtime: [LuwuDynamics/xgoduck_runtime_arduino](https://github.com/LuwuDynamics/xgoduck_runtime_arduino)
- Training: [LuwuDynamics/xgoduck_rl](https://github.com/LuwuDynamics/xgoduck_rl)

> **A change of plan.** An earlier version of this post followed the OpenELAB build guide and the AI-FanGe tutorial (Raspberry Pi Zero 2 W, Dynamixel XL330 servos, OpenRB-150 controller). I've switched to the Luwu Dynamics XGO-Duck repository. It is complete, it answers most of the open questions I had, and the hardware, runtime and training code are made for each other.

## Why the XGO-Duck

Here are the questions I had in the first version, and what the Luwu repository says about them:

| Open question | Answer in the XGO-Duck repo |
| --- | --- |
| Where do the print files come from? | The `structure/` folder holds 30 printable STL files. |
| Which battery should I use? | An 8.4 V 18650 pack, more than 3,500 mAh, more than 3C discharge, XH2.54 connector, plus a matching 8.4 V charger. |
| How do I set up servos without a U2D2? | The runtime has a browser-based servo setup page that scans servos, changes IDs and sets the center position. |
| Will the walking model work on a slightly different robot? | The repo ships its own ONNX policies (walk, roller, get-up, pick, roulade), trained for this robot's geometry. |

## What's different from Pollen's Microduck

The XGO-Duck keeps the Microduck layout (fifteen servos, same joint order) but changes the electronics and software:

| | Pollen Microduck | XGO-Duck (my build) |
| --- | --- | --- |
| Computer | Rockchip RK3566 | Arduino UNO Q |
| Servos | Dynamixel XL330 | Feetech 1910 |
| IMU | two IMUs | QMI8658A on the expansion board |
| Software | Rust runtime (`robotctl`) | Python runtime with a web interface |
| Policies | Pollen's `microduck_rl` | Luwu's `xgoduck_rl` |

That matters because Pollen's policies and software are **not** interchangeable with this hardware. The servos, geometry and calibration are different, so I'll use Luwu's own runtime and policies.

## Credits

XGO-Duck by Luwu Dynamics, based on Microduck and microduck_rl by Pollen Robotics. Training uses [mjlab](https://github.com/mujocolab/mjlab) and [BAM](https://github.com/Rhoban/bam) by Rhoban. This is a personal, non-commercial build, and it is not endorsed by Pollen Robotics, Hugging Face, Luwu Dynamics, Rhoban or Arduino.

## Goals

1. Make the robot stand and walk using the ready-made walking policy.
2. Control it from a browser, without a laptop cable attached.
3. If that works, add character and explore training my own motions.

## Costs

I don't have a total yet. The 15 servos and the UNO Q are the biggest items. Luwu Dynamics also sells an electronics kit with everything in the bill of materials except the Arduino UNO Q. I'll compare buying the kit with buying the parts one by one in Part 2, and fill in real prices there.

Besides the robot itself I need a 3D printer, a soldering iron, and a computer for setup. I also plan one spare servo.

## The plan, in nine parts

1. **The plan** (this post)
2. **The hardware**: the parts list and ordering
3. **Printing the parts**: orientation, settings, and TPU parts
4. **Servo setup**: IDs and center positions
5. **Building the legs and body**
6. **Wiring and power**
7. **Software**: installing the runtime on the UNO Q
8. **First steps**: calibration and the first walk
9. **Next steps**: updates, optional roller skates and my own policies

## Safety

I'll be working with a soldering iron, a 3D printer and a lithium battery pack. I'll hold the robot for the first run, keep it away from table edges, and avoid stalling servos while testing, because the repository warns about this.

## What's next

In Part 2 I go through the hardware list and order the parts.

---

<!-- NOTES FOR YOU, remove before publishing

What I changed compared with the version I could see (a summary only, not the verbatim text):
- Replaced the OpenELAB / AI-FanGe basis with Luwu Dynamics' xgoduck_hardware, with links to its three repos.
- 14 servos -> 15 servos. Raspberry Pi Zero 2 W -> Arduino UNO Q. XL330 -> Feetech 1910. 6 V battery -> 8.4 V 18650 pack.
- The four open questions (print files, battery, U2D2, walk model) are answered in a table.
- The "nine-part plan" keeps the same nine parts, with changed titles for parts 7 and 9.
- Gamepad goal changed to browser control, because the runtime README describes a web interface. If the runtime supports a gamepad, say so.
- Added the credit line and non-endorsement statement that Luwu's UPSTREAM.md suggests.
- Check: the exact costs, any personal intro, and your own wording from the original.
-->
