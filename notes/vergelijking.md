# Vergelijking Pollen Microduck ↔ Luwu XGO-Duck

Gegenereerd op 2026-10-27 uit ondiepe klonen (`--depth 1`) van:

| Map | Repo |
|---|---|
| `microduck_src/repo` | pollen-robotics/microduck (runtime, Rust) |
| `microduck_rl_src/repo` | pollen-robotics/microduck_rl |
| `xgoduck_hardware_src/repo` | LuwuDynamics/xgoduck_hardware |
| `xgoduck_runtime_src/repo` | LuwuDynamics/xgoduck_runtime_arduino |
| `xgoduck_rl_src/repo` | LuwuDynamics/xgoduck_rl |

Gemeten met `diff`, `cmp` en een Python-script. Waar ik iets niet heb nagegaan, staat dat erbij.

## 1. Architectuur

| Onderdeel | Pollen | Luwu |
|---|---|---|
| Computer | Rockchip RK3566 (Radxa), 1 GB RAM | Arduino UNO Q (Linux-host + STM32) |
| Servo's | Dynamixel XL330 (`rustypot`, `Xl330Controller`) | Feetech 1910 (HLS1910), SCS-protocol (`sketch/scs_bus.h`) |
| IMU | 2 stuks, via `imu_to_dxl`-bord | QMI8658A op uitbreidingskaart |
| Runtime | Rust-daemons (`robotd`, `duck-control`, `robotctl`) | Python-host (`controller.py`, `policy.py`, `rl_core.py`) + STM32-sketch |
| Bediening | `robotctl`, gamepad | webinterface op poort 9527 |
| Actuatormodel (RL) | XL330 | HLS1910 via BAM (`params/1910_m6.json`) |
| Accu | NP-F550 2600 mAh | 8,4 V 18650-pack, > 3500 mAh |

Gedeeld: 15 servo's, policy-volgorde en servo-ID's (10–14 links, 20–24 rechts, 30–33 nek/kop, 34 snavel), ONNX-policies, PPO op mjlab.

## 2. RL-repo: `xgoduck_rl` t.o.v. `microduck_rl`

Zelfde package-naam (`mjlab_microduck`) en structuur: Luwu heeft het project geforkt en aangepast.

Vergeleken: `src/mjlab_microduck/tasks/*.py`.

| Bestand | Status | Verschil (regels, zonder witruimte) |
|---|---|---|
| `microduck_ball_kick_env_cfg.py` | identiek | 0 |
| `microduck_roller_crouch_env_cfg.py` | identiek | 0 |
| `microduck_roller_slope_env_cfg.py` | identiek | 0 |
| `microduck_roller_standup_env_cfg.py` | identiek | 0 |
| `microduck_spin_env_cfg.py` | identiek | 0 |
| `microduck_velocity_swizzle_env_cfg.py` | identiek | 0 |
| `slope_terrain.py` | identiek | 0 |
| `symmetry.py` | identiek | 0 |
| `testbench_env_cfg.py` | identiek | 0 |
| `backlash.py` | gewijzigd | 2 |
| `microduck_roulade_env_cfg.py` | gewijzigd | 2 |
| `microduck_velocity_rollers_env_cfg.py` | gewijzigd | 2 |
| `microduck_velocity_env_cfg.py` | gewijzigd | 4 |
| `microduck_standup_env_cfg.py` | gewijzigd | 10 |
| `microduck_ground_pick_env_cfg.py` | gewijzigd | 14 |
| `microduck_sitstand_env_cfg.py` | gewijzigd | 87 |
| `__init__.py` | gewijzigd | 449 |
| `microduck_velstand_env_cfg.py` | gewijzigd | 629 |
| `mdp.py` | gewijzigd | 1038 van ca. 7700 regels |
| `xgoduck_physics.py` | nieuw bij Luwu | – |
| `xgoduck_tricks_env_cfg.py` | nieuw bij Luwu | – |
| `xgoduck_velocity_swizzle_env_cfg.py` | nieuw bij Luwu | – |
| `distill.py` | alleen bij Pollen | – |

Opmerkingen:
- `mdp.py` bij Luwu heeft CRLF-regeleinden, het origineel LF. Het ruwe diff (15.298 regels) is daardoor misleidend; zonder witruimte blijven 1038 regels over. Ik heb die niet inhoudelijk gelezen.
- Nieuwe robotmappen bij Luwu: `robot/xgoduck`, `robot/xgoduck_roller`, met eigen MJCF, URDF en 15 meshes, plus `vendor/bam` (Rhoban).
- `robot/microduck/` is ook nog aanwezig bij Luwu (met `add_backlash.py` gewijzigd).

## 3. Meshes en licentie

Pollen: `LICENSE-MODELS.md` zet `robot/microduck/assets/*.stl` en `robot/xl330_test_bench/assets/*.stl` onder **CC BY-NC-SA 4.0**. De rest (MJCF, configs, code) is Apache-2.0.

Gemeten:
- Luwu's `robot/microduck/assets/` bevat 43 STL's met dezelfde bestandsnaam als bij Pollen (plus 4 extra). **Geen enkele** is bytegelijk. Alle 43 hebben ook andere vertex-bytes, ook als je de normalen negeert. Bij de steekproef `ankle_left.stl` zijn de bestandsgrootte (248.984 bytes), het aantal driehoeken (4978) en de bounding box gelijk.
- Dat wijst op heruitgevoerde of bewerkte export van dezelfde geometrie. Ik heb **niet** vastgesteld of het afgeleide werken zijn, en ook niet of de geometrie vertex voor vertex overeenkomt.
- `robot/xgoduck/assets/` (15 meshes): 13 namen komen niet bij Pollen voor, 2 (`neck_pitch.stl`, `trunk_base.stl`) wel, en die zijn niet gelijk aan Pollens bestanden.
- De 30 STL's in `xgoduck_hardware/structure/` heb ik niet met Pollens bestanden vergeleken.
- `xgoduck_rl` heeft een Apache-2.0 `LICENSE`, maar geen `LICENSE-MODELS.md` en geen vermelding van CC BY-NC-SA.
- `xgoduck_hardware` heeft geen top-level licentie. `LICENSING.md` noemt de bedoeling: persoonlijk en educatief gebruik toegestaan, commercieel via `hello@xgorobot.com`. `UPSTREAM.md` zegt dat herkomst en rechten van geërfde modellen nog niet zijn vastgelegd.
- `xgoduck_runtime_arduino` heeft geen `LICENSE`-bestand.

## 4. Runtime

- Servo-ID-volgorde `MOTOR_IDS = [10–14, 20–24, 30–34]` in `rl_core.py`. De policy-volgorde (`POLICY_IDS`) laat de snavel (34) weg: 14 policy-gewrichten, 15 servo's.
- Luwu levert 5 ONNX-policies mee: walk, roller, getup, pick, roulade.
- Pollens Rust-code (`duck-control/src/bus.rs`) is Dynamixel-specifiek (XL330-registers). Er is geen gedeelde code met Luwu's Python/C++-runtime vastgesteld.

## 5. Wat niet is nagegaan

- Inhoudelijke diff van `mdp.py`, `__init__.py`, `velstand` en `sitstand`.
- Vergelijking van policy-gewichten (`.onnx`) of observatieruimte.
- Git-geschiedenis (alleen `--depth 1`), dus geen upstream-basisrevisie.
- STL's in `xgoduck_hardware/structure/` t.o.v. Pollen of t.o.v. de RL-meshes.

## 6. Conclusie

- Luwu's RL-code is een dichte fork van Pollens: 9 taken ongewijzigd, 9 licht tot sterk aangepast, 3 nieuw.
- De runtime is een eigen implementatie voor andere hardware.
- Mesh-herkomst en licentie van de geërfde modellen zijn niet vastgelegd; dat is het belangrijkste open punt voor herverdeling of commercieel gebruik.
