# NVIDIA Support in OBS

Some GPUs from the Kepler generation and most GPUs from the Maxwell generation onwards support NVENC. This covers most GPUs manufactured 2012 onwards, starting with select entries GeForce 600 series and the majority of the GeForce 700 series.

If your GPU's name starts with GTX or RTX, it more than likely supports NEVNC.

In each generation, there were many exceptions that do *not* support NVENC. These are mostly mobile and low-end GPUs. Please check the listing below. If your card does not support NVENC, you may need to use another hardware encoder (such as Intel Quick Sync) or use a software encoder, normally x264.

## NVIDIA GPUs without NVENC Support

### TU117 (Turing)

- Consumer
  - GeForce MX450
- Professional
  - NVIDIA T500 Mobile

### GP108 (Pascal)

- Consumer
  - GeForce GT 1030
  - GeForce MX150
  - GeForce MX230
  - GeForce MX250
- Professional
  - Quadro P500 Mobile
  - Quadro P520

### GP108B (Pascal)

- GeForce MX250

### GM108 (Maxwell)

- Consumer
  - GeForce 830M
  - GeForce 840M
  - GeForce 845M (NVENC depends on hardware revision - GM107 vs GM108)
  - GeForce 920MX
  - GeForce 930M
  - GeForce 930MX
  - GeForce 940M
  - GeForce 940MX
  - GeForce 945M
  - GeForce MX110
  - GeForce MX130
- Professional
  - Quadro K620M
  - Quadro M500M
  - Quadro M520 Mobile

### GF116 (Fermi 2.0)

- GeForce GTX 550 Ti

### Other (Older generation)

- GeForce GT 705 (Fermi)
- GeForce GT 730 (NVENC depends on hardware revision - Fermi GF108 vs Kepler GK208)

----

### TechPowerUp GPU Database

- [https://www.techpowerup.com/gpu-specs/-gm108.g761](https://www.techpowerup.com/gpu-specs/-gm108.g761)
- [https://www.techpowerup.com/gpu-specs/-gp108.g808](https://www.techpowerup.com/gpu-specs/-gp108.g808)
- [https://www.techpowerup.com/gpu-specs/-gp108b.g887](https://www.techpowerup.com/gpu-specs/-gp108b.g887)