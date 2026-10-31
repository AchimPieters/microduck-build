# Building a MicroDuck, Part 4: Too Many Ducks

In [Part 1](https://www.studiopieters.nl/microduck-build-part-1-the-plan/) I picked the XGO-Duck and said I'd order the parts. I haven't. The more I research, the more MicroDuck builds I find, and the more of them I find, the less sure I am that I picked the right one. This part is not a build step. It's me sorting out the doubt, in public, before I spend money.

## How I got lost

I started with one robot: Pollen Robotics' Microduck, about 25 cm tall, under 800 g, 15 servos, walking with a policy trained in simulation. Then I started searching GitHub, and it turned out the Microduck is less an open design than an open *idea*.

Pollen published the software ([microduck](https://github.com/pollen-robotics/microduck)) and the training code ([microduck_rl](https://github.com/pollen-robotics/microduck_rl)). Going by what I could find, the mechanical design, the full bill of materials and the IMU board were not published. The software repo has no BOM and no CAD, and it doesn't even name the servo model.

So every "replica" is somebody's reconstruction. Some rebuild the look. Some rebuild the electronics. Some swap the servos. Some only exist as a parts list. Which one is "closest to the original" depends on what you count, and nobody can measure it against drawings that don't exist.

At one point I pasted in a summary of the field that sounded very sure of itself. It named projects I couldn't find, and it ranked builds I couldn't verify. That didn't help my doubt either. I went back and checked everything myself, repo by repo. This post is what survived.

## The candidates

| Build | Servos | Brain | What's actually shown |
| --- | --- | --- | --- |
| **Pollen Microduck** (the original) | 15, model not named in the repo | Rockchip RK3566 | It's a product, announced at $399 |
| **[XGO-Duck](https://github.com/LuwuDynamics/xgoduck_hardware)** (my plan) | 15× Feetech 1910 | Arduino UNO Q | Hardware, runtime and training code all published; ships its own ONNX policies |
| **[fanhao375](https://github.com/fanhao375/microduck-replica)** | 15× Feetech HD-1910 (an XL330 variant is listed but not validated) | Radxa Zero 3W + Pollen's HAT | Standing, walking, forward roll and getting up, reported on real hardware; system image, CAD, BOM |
| **[AI-FanGe](https://github.com/AI-FanGe/Microduck-build-tutorial)** | 14× Dynamixel XL330 (+ optional mouth) | Pi Zero 2 W + OpenRB-150 | A clip of the robot walking; BOM, print files and an SD image; MIT license |
| **[BoBoRedesign](https://github.com/AiBoBoMaker/MicroDuck-BoBoRedesign)** | XL330 or HD-1910 | Radxa ZERO 3W | Print files and a purchase list; I found no sign of a walking robot |
| **Reconstructions** ([pablo-mano](https://github.com/pablo-mano/microduck-replica), [virtualrobotix](https://github.com/virtualrobotix/microduck-replica), [prodou](https://github.com/prodou/microduck-replica)) | various | various | Studies and parts lists, not finished builds |

One thing I did myself in the previous weeks: I compared the XGO-Duck's training code against Pollen's. Most of it is a close fork. The runtime is a separate implementation for different hardware. So "XGO-Duck" is a Microduck in layout and training approach, but not in parts.

## Pros and cons, as far as I can tell

**XGO-Duck.** Everything fits together: hardware, runtime and policies are made for each other, and there's a browser-based setup page. The downsides are that it's the furthest from Pollen's electronics, the UNO Q is a new board with a young ecosystem, and the design files don't carry a final license yet. I also still don't know the total price.

**fanhao375.** The best proof of a working robot on cheap servos, and the most complete documentation. But the license is personal and non-commercial, much of it is in Chinese, and the reference BOM needs a custom IMU board that nobody has published a design for.

**AI-FanGe.** The most "off the shelf" route: a Pi, a controller, XL330 servos and standard modules. Most readable if you want to understand what's going on. The price is the servos (XL330s are the expensive part), and the mechanical design is inherited from earlier projects rather than from Pollen's drawings.

**BoBoRedesign.** On paper the cheapest, and it supports both servo families. But I couldn't find evidence that anyone has made it walk, and its own notes call parts of the electronics untested.

**Original Pollen Microduck.** No sourcing, no printing, no calibration guesswork. The only real "con" is that it's not a build.

## Is building one worth it, money-wise?

The original is announced at **$399, roughly €350**, before tax and shipping. Here is what the replicas come to, using prices I could find. All of these are rough, not quotes:

| Build | Rough total | Notes |
| --- | --- | --- |
| Pollen original | ≈ €350 | Introductory price, excl. tax and shipping |
| fanhao375 BOM, XL330 servos | ≈ €800–960 | 15 servos alone are about €600 in Europe |
| fanhao375 BOM, HD-1910, bought in Europe | ≈ €640–805 | The servo listing I found was sold out |
| fanhao375 BOM, HD-1910, bought in China | ≈ €375–540 | Before shipping and import costs |
| AI-FanGe | ≈ €690–780 | My own estimate; the repo lists no prices |
| BoBoRedesign, HD-1910 | ≈ €250 | From their Taobao list; no shipping, import, screws or wire; the HD-1910 is a 15-day presale |
| BoBoRedesign, XL330 | ≈ €580 | The XL330 shows as out of stock |
| XGO-Duck | unknown | I still have no total, and neither did Part 2 |

So: **no, it isn't cheaper.** Only the China-sourced Feetech routes come in under the original, and only if shipping, import and my own time stay small. And that is before counting a 3D printer, print failures, a spare servo, a soldering iron and the weekends.

The honest reason to build it is not saving money. It's to learn how a small RL-trained biped is put together, from the servo IDs up to the policy. If I want a Microduck that walks next week, I should buy the original.

## What I still don't know

- What the full XGO-Duck costs, with and without Luwu Dynamics' electronics kit.
- Whether the Feetech 1910 is easy to get at the moment, and for how much.
- Whether the XGO-Duck's walking policy works well on a real robot, rather than just in the repo.
- Where the XGO-Duck's mechanical files come from. Their own notes say the origin and rights of inherited models aren't recorded yet.
- Whether I care more about being close to the original or about having something that works.

## What I'm going to do first

I think the right next step is to study before buying. My list:

1. Get a real XGO-Duck price: the kit, the UNO Q, and 15 servos, in euros, with shipping.
2. Read the XGO-Duck runtime and the fanhao375 build notes side by side, and find out which one has fewer unknowns.
3. Look for evidence of the XGO-Duck walking on real hardware, not just in the demo.
4. Check the Pollen shop: price, availability, and delivery to the Netherlands.
5. Decide, on paper, what "success" means: a robot that walks, or a robot that matches the original.

Then I decide: stay with the XGO-Duck, switch to one of the others, or just order the original.

## Credits and disclaimers

This is a personal, non-commercial project. Microduck and microduck_rl are by Pollen Robotics; XGO-Duck is by Luwu Dynamics. I'm not affiliated with or endorsed by Pollen Robotics, Hugging Face, Luwu Dynamics, or any of the other projects listed. Prices are rough, change often, and some servos are sold out. Check before you buy.

## What's next

Part 5 will be the decision, and the shopping list that goes with it.

---

<!-- NOTES FOR YOU, remove before publishing

- Language: the existing parts are in English, so this one is too. Say if you want it in Dutch.
- Numbering: Part 1 said Part 4 would be servo setup, and Part 2 said "Part 4" for the servo IDs. This post replaces that slot, so the plan in Part 1 (nine parts) no longer lines up. Either renumber, or add a line to Part 1.
- Part 3 was not in the folder, so I haven't linked to it or referred to its content. Adjust the intro if Part 3 already says something about this. I also only linked Part 1, because I know its URL from Part 2.
- "Current lean": I wrote that you are still on the XGO-Duck but unsure. Change it if that's not how you feel.
- Prices: I took them from earlier research in this chat. The fanhao375 totals are my sums from its BOM; the AI-FanGe line is my own estimate; the BoBoRedesign numbers are converted from yuan at roughly €0.125 per yuan; $399 is converted at roughly €0.88 per dollar. None are live. Please re-check anything you want to quote.
- Not verified: whether the XL330 is the servo in Pollen's original. fanhao375's BOM says the model number is inferred. I wrote "model not named in the repo" for that reason.
- The paragraph about the unreliable summary refers to the text you pasted at the start of the session. Remove it if you'd rather not mention it.
- Claim to double-check before publishing: "the license is personal and non-commercial" (fanhao375 image) and "my own notes say XGO-Duck origin isn't recorded" (from UPSTREAM.md, via vergelijking.md).
-->
