# Building a MicroDuck, Part 2: The Hardware

In [Part 1](https://www.studiopieters.nl/microduck-build-part-1-the-plan/) I decided to build the **XGO-Duck** from Luwu Dynamics: [LuwuDynamics/xgoduck_hardware](https://github.com/LuwuDynamics/xgoduck_hardware). This part is the shopping list. I haven't ordered anything yet, so there are no prices here, only what to buy and what to check.

> **Updated.** The first version of this post listed the OpenELAB parts (Raspberry Pi Zero 2 W, OpenRB-150, BNO08x IMU, 14 Dynamixel XL330 servos). The list below follows the XGO-Duck bill of materials ([`BOM.md`](https://github.com/LuwuDynamics/xgoduck_hardware/blob/main/BOM.md)) instead.

## Decisions before ordering

- **Kit or single parts?** Luwu Dynamics sells an [electronics kit](https://shop.xgorobot.com/products/xgo-duck-biped-robot-electronics-kit-arduino-uno-q?variant=68379124138235) with everything in the BOM except the Arduino UNO Q. I'll compare it with ordering the parts myself.
- **One spare servo.** As insurance against a failed motor.
- **Roller skates, yes or no?** The BOM includes quantities for the optional roller skates (4 extra M2 × 6 screws and 8 extra 10 × 15 × 3 mm bearings). The numbers below include them.
- **UNO Q variant.** Choose the 2 GB or the 4 GB version.
- **Print files.** They are in the `structure/` folder of the repository, so that open question from Part 1 is answered.

## Parts list

From the XGO-Duck BOM, for one robot:

| Part | Quantity | Notes |
| --- | ---: | --- |
| Arduino UNO Q (2 GB or 4 GB) | 1 | Choose one RAM variant. |
| Robot expansion board (Luwu Dynamics) | 1 | Select "Only Robot Driver Board for Arduino UNO Q". Includes the IMU and servo connections. |
| Feetech 1910 servo | 15 | Legs, neck, head and mouth. Plus one spare. |
| AMP 3-pin cable | 15 | One per servo. |
| 18650 battery pack, 8.4 V | 1 | XH2.54 connector, more than 3,500 mAh, more than 3C discharge. |
| 8.4 V Li-ion charger | 1 | 5.5 × 2.1 mm DC barrel plug. |
| M2 × 6 countersunk, flat-end self-tapping screw | 204 | 200 + 4 for the roller skates. |
| M2.3 × 8 flat-top round-head, flat-end self-tapping screw | 6 | |
| M3 × 16 round-head machine screw | 4 | |
| Bearing, 10 × 15 × 3 mm | 10 | 2 + 8 for the roller skates. |
| Bearing, 16 × 22 × 4 mm | 11 | |

The BOM has no purchase links for the servos, cables, battery or charger. The two bearing sizes link to a Tmall listing.

## 3D printing

The 30 STL files are in `structure/`. The repository recommends:

- **4 walls and 13% Gyroid infill**.
- The **print orientation shown in the reference image**, not the slicer's auto-arrange.
- **TPU** for the soft parts (foot soles and the mouth pieces).
- A **test print** of a servo mount and a bearing seat first, to check the fit before printing everything.

File names carry the quantity and side, for example `2x_shank.stl` (two copies) and `left_foot.stl`.

## Tools

- 3D printer, soldering iron, multimeter
- A computer for the UNO Q setup (the runtime uses Arduino App Lab)
- A screwdriver set. The repository tells you to identify the three screw types carefully, since some look alike.

## Servo IDs

The runtime expects these IDs, which I'll set one servo at a time in Part 4:

| IDs | Joints |
| --- | --- |
| 10–14 | Left leg |
| 20–24 | Right leg |
| 30–33 | Neck and head |
| 34 | Mouth |

I'll label each servo as I configure it.

## Buying checks

- Confirm the servo is the **Feetech 1910 (HLS1910)**, not another Feetech model.
- Check the battery rating (more than 3,500 mAh and more than 3C) and the XH2.54 connector.
- Check the charger plug (5.5 × 2.1 mm) and 8.4 V output before connecting anything.
- Check the three screw types against the shape guide in `BOM.md`.

## On arrival

Count the servos and cables, check the labels, inspect the battery, and confirm every item on the list is there before I start printing.

## Licensing and credits

This is a personal, non-commercial build. XGO-Duck by Luwu Dynamics, based on Microduck and microduck_rl by Pollen Robotics. I'm not affiliated with or endorsed by Luwu Dynamics, Pollen Robotics or Arduino. The XGO-Duck repository doesn't carry a final license for its design files yet, so I'm not redistributing them. If you plan to sell anything built from it, ask Luwu Dynamics first.

## What's next

In Part 3 I print the parts and run the test print first.

---

<!-- NOTES FOR YOU, remove before publishing

What I changed compared with the version I could see (a summary only, not the verbatim text):
- The parts list now follows the XGO-Duck BOM, not OpenELAB. Removed: Raspberry Pi Zero 2 W, OpenRB-150, BNO08x IMU, microSD card, U2D2 and U2D2 Power Hub, bus splitter, 5 V regulator, 6 V battery. Those are replaced by the UNO Q, the expansion board (which has the IMU and servo connectors), and an 8.4 V pack with a charger.
- 14 servos -> 15 servos. The "skip the optional mouth servo" advice is gone, because the XGO-Duck uses ID 34 for the mouth.
- Servo ID scheme changed from 1-15 to 10-14 / 20-24 / 30-33 / 34, as the runtime README says.
- The "2.4 GHz Wi-Fi only" remark was dropped, since it was about the Pi Zero 2 W. Check the UNO Q's Wi-Fi yourself if you want to keep a Wi-Fi note.
- Print section: 4 walls and 13% Gyroid, TPU parts and a test print, from the repository's printing notes.
- Check: prices, shop links, the exact tools you listed (crimping tool, heat-shrink, JST connectors) and whether you still need them. Also whether the UNO Q needs a USB-C cable and power supply for setup, which I didn't verify.
-->
