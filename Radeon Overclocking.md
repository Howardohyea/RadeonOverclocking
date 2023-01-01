# Table of Contents
- [Preface](#preface)
- [Prequisites](#prequisites)

# Preface
Overclocking is about pushing hardware and silicon out of their comfort zone in pursuit of performance. Overclocking is inheritantly dangerous, it's just the amount you're willing to risk.

It's not as bad as it sounds to be honest, sure, you're risking your hardware, but you're very likely to become thermally limited or even power limited before you're at the point of putting in absurd voltages and frying your hardware. With all the software and hardware safeguards you're probably fine.

With that out of the way, how do you overclock your AMD GPU?

# Prequisites
You need an AMD Radeon GPU, that's obvious. I'm not going to cover overclocking ancient ATI cards, nor the Polaris and Vegas. If you're here for instructions for those cards, you'll have to look elsewhere.

Specifically, I'm going to focus on the RDNA cards, which is the RX 5000 series and above. The instructions for all the RDNA cards is identical, so you don't have to worry about generation specific 

Now, you'll also need some required software, and they're as follows:

1. [AMD Software: Adrenalin Edition](https://www.amd.com/en/support), their software comes with a built in overclocking tool, so we'll be using it.
2. [HWInfo](https://www.hwinfo.com/) for monitoring system temperature, power draw, fans, and just about anything.
3. [3DMark](https://3dmark.com), a synthetic benchmark for both CPU and GPU, it's not very good for comparison between hardware but useful to test overclocking and stability. The full test is paid but the free demo can be found [here](steam://install/231350), this demo link requires Steam to be installed to your PC.
4. A document to write your benchmark score and overclocking settings down. This is for comparing your score improvements and possible overclocking settings. 

# Testing Methodology
Any guide or review lacking testing methodology is not a good guide, so I'll include this. 

Throughout the guide, I'm using my Sapphire RX 6900XT Nitro+ SE for demonstration. The fan curve is default, Resizable BAR is enabled. Do note that everything I say, like score and percentage is relative to my GPU, so yours might be a little different. 

Case: Corsair 4000D Airflow
CPU: i7 12700K stock, Gear 2
RAM: 32GB DDR4 4000 Trident Z Royale

All benchmarks were ran 3 times with the average taken.

# Overclocking
With the software and test system out of the way, it's time to dial in some overclocking.

However, before we do any tuning, we need to get some baseline of how your GPU performs.

## The Software
The tuning page is hidden away pretty well, You need to go to `Performance` --> `Tuning`, and under `Tuning Control`, select `Custom`.
![Performance Tuning](/Assets/Tuning%20Page.jpg)
There's also one Tuning Preset, `Rage Mode`, that ups your power limit and *doesn't void the warranty*. The performance gain isn't much, but for users who want to play it *absolutely safe*, the option is there.

## Getting a Baseline
Open 3DMark and run Time Spy, and to save time, I suggest running a Custom Test without the CPU test. 
![the recommended test and setting](/Assets/3D4K.jpg)
After you ran the test, note that score down. 

For reference, my card scored `9846` at stock. We'll be using this as baseline.

## Upping the power limit
Modern cards have a lot of safeguards, so it's going to be hard to brick the GPU. It's perfectly safe to up the power limit to the max allowed. 
![shown here with default settings](Assets/Power%20Limit.jpg)
At stock, my card is rated for 289 watts. If we put the slider to the max, the TDP jumps to 332 watts instead. So what performance does the extra 43 watts get us?

After some benchmarks, I got 10272. A 4.3% improvement for 15% more power[^1]. Not good enough, let's do some more tuning.

## Tuning the VRAM
VRAM is like the GPU's system RAM, and unlike system RAM, tweaking VRAM is as simple as changing a slider. Beware, however, that upping the memory clock too much will cause performance regression. However, for some GPUs and models, you may run into the slider limit before performance regresses.

I suggest starting from the max, lower the VRAM in 50MHz steps and running a 3DMark test. If performance regresses, you could try to fine tune and get the optimal setting.

It's also safe to enable `Fast Timings` in the setting, for my GPU, it improved score by about 800 to 1000 points.
![Memory overclocking is on the bottom left](/Assets/memory%20tuning.jpg)

## Changing Clocks and Voltage
Now, the GPU have access to more power, but it might not know how to tap into the extra potential, thus creating such inefficiency. It's only until we tweak the Voltage and Clock does the card know what to do. 

Strangely, only upping the Maximum Core Clock doesn't seem to do a whole lot.

[^1]: By "power", I mean the rated TGP, and not the actual comsumed power. However, in practice, those two values are pretty similar, the GPU won't exceed TGP by more than a watt or two.