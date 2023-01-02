# Table of Contents
- [Preface](#preface)
- [Prequisites](#prequisites)
- [Testing Methodology](#testing-methodology)
- [Overclocking](#overclocking)
    1. [The Software](#the-software)
    2. [Getting a Baseline](#getting-a-baseline)
    3. [Upping the power limit](#upping-the-power-limit)
    4. [Tuning the VRAM](#tuning-the-vram)
    5. [Changing Clocks and Voltage](#changing-clocks-and-voltage)
        - [RDNA1 specific instructions](#rdna1-specific-instructions)
- [Credits](#credits)

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
4. [Unigine Heaven](https://benchmark.unigine.com/heaven) is absolutely ancient. First released in 2009 for DX11 it's also a tech demo and benchmark rolled into one. It's not indicative of performance but it can load a GPU and runs in the background.
4. A document to write your benchmark score and overclocking settings down. This is for comparing your score improvements and possible overclocking settings. 

# Testing Methodology
Any guide or review lacking testing methodology is not a good guide, so I'll include this. 

Throughout the guide, I'm using my Sapphire RX 6900XT Nitro+ SE for demonstration. The fan curve is default, Resizable BAR is enabled. Remember, everything I say, like benchmark score and percentage is all relative to my GPU, so yours, even when following the exact instructions, might be a little different. 

Case: Corsair 4000D Airflow
CPU: i7 12700K stock, Gear 2
RAM: 32GB DDR4 4000 Trident Z Royale
Motherboard: ROG Strix Z690-A

All benchmarks were ran 3 times with the average taken.

# Overclocking
With the software and test system out of the way, it's time to dial in some overclocking.

However, before we do any tuning, we need to get some baseline of how your GPU performs.

## The Software
The tuning page is hidden away pretty well, You need to go to `Performance` --> `Tuning`, and under `Tuning Control`, select `Custom`.
![Performance Tuning](/Assets/Tuning%20Page.jpg)
There's also one Tuning Preset, `Rage Mode`, that ups your power limit and *doesn't void the warranty*. The performance gain isn't much, but for users who want to play it *absolutely safe*, the option is there.

There's a strange feature in AMD software where `Advanced Control` just changes the slider from percentage based into the actual value. I suggest enabling this for every setting.

## Getting a Baseline
Open 3DMark and run Time Spy, and to save time, I suggest running a Custom Test without the CPU test. 
![the recommended test and setting](/Assets/3D4K.jpg)
After you ran the test, note that score down. 

For reference, my card scored `9846` at stock. We'll be using this as baseline.

## Upping the power limit
Modern cards have a lot of safeguards, so it's going to be hard to brick the GPU. It's perfectly safe to up the power limit to the max allowed. RDNA cards are all fairly power starved, so giving the card more juice is crucial[^1]. 
![shown here with default settings](Assets/Power%20Limit.jpg)
At stock, my card is rated for 289 watts. If we put the slider to the max, the TDP jumps to 332 watts instead. So what performance does the extra 43 watts get us?

After some benchmarks, I got 10272. A 4.3% improvement for 15% more power[^2]. Not good enough, let's do some more tuning.

## Tuning the VRAM
VRAM is like the GPU's system RAM, and unlike system RAM, tweaking VRAM is as simple as changing a slider. Beware, however, that upping the memory clock too much will cause performance regression[^3]. However, for some GPUs and models, you may run into the slider limit before performance regresses.

I suggest starting from the max, lower the VRAM in 50MHz steps and running a 3DMark test. If performance regresses, you could try to fine tune and get the optimal setting. It's also safe to enable `Fast Timings` in the setting, which improved my score by about 800 to 1000 points.
![Memory overclocking is on the bottom left](/Assets/memory%20tuning.jpg)

## Changing Clocks and Voltage
Now, the GPU have access to more power, but it might not know how to tap into the extra potential, thus creating such inefficiency. It's only until we tweak the Voltage and Clock does the card know what to do. For starters, leave the minimum clock alone, changing that would only result in very bad idle power consumption and no perceivable performance gain[^4]. 

Strangely, only upping the Clock Limit slider doesn't seem to do a whole lot performance wise, and may even cause stability and deteriorating performance. That's where the voltage slider comes into play. 

For RDNA cards, Clock and Voltage is inversely related, lowering voltage will increase your clock. This seems counterintunitive, especially compared to how CPU overclocking works, but with lower voltages more power will become available to the GPU, thus clocking higher.

First off, don't go and crank the GPU core frequency dial all the way to the max allowed, we need to tweak the voltage first. Start by dropping voltages in small steps, run a benchmark to check the core clock, until you reach "the wall".

For RDNA2 and 3, there's a "wall" that doesn't allow the card to clock higher despite being below the max frequency allowed. This wall is around 50MHz below the Clock Limit specified by the software. In essence, "Maximum Frequency" seems to be "Maximum minus 50". 

When you reach the wall, now's the time to dial an increase for the Clock Limit slider. Add 100MHz and start decrease voltage again until you hit the new wall. At this point, tweaking the card can be simplified as follow:

1. Decrease voltage until the frequency reach the Clock Limit slider.
2. Increase Clock Limit by 100MHz
3. Repeat step 1-2 until the GPU exhibits instability and crashes. 

That's it! You're overclocking!

<!-- Insert overclocked screenshot and OC score here
-->

### RDNA1 specific instructions
There was a lot of memes about how bad the RX 5700XT drivers were. Well, it was *abysmal* at launch, but gotten vastly better in the coming months. However, overclocking is relatively simple for that generation. Upping the core and memory clocks directly yields an improvement in performance. 

However, I need to do more research regarding the relation between voltage and frequency for RDNA1. If anyone is kind enough to try the above instructions on their card please let me know the result.

# Credits
Did you think I did all the testing myself for this guide? Of course not! I'd like to thank the following sources and/or people for providing me with information.

TechPowerUp and their [Asus RX 7900 XTX OC review](https://www.techpowerup.com/review/asus-radeon-rx-7900-xtx-tuf-oc/39.html), helping me provide insight to how RDNA3 overclocks.

TPU and their [Asus Strix RX 5700 XT review](https://www.techpowerup.com/review/asus-radeon-rx-5700-xt-strix-oc/32.html), which provided simple overclocking instructions.

Tom's Hardware for reviewing the [RX 6900 XT](https://www.tomshardware.com/reviews/amd-radeon-rx-6900-xt-review) for confirming how RDNA2 overclocks and providing a comparison for my card.

[^1]: This is most obvious comparing a Reference 6900XT and the Strix 6800XT. The Strix have much more power available and it can beat the overclocked Reference 6900XT when the Strix is pushed to the limits.
[^2]: By "power", I mean the rated TGP, and not the actual comsumed power. However, in practice, those two values are pretty similar, the GPU won't exceed TGP by more than a watt or two.
[^3]: Unlike Nvidia cards, where overclocking the VRAM will cause visual artifacts and corrupt textures, the only indication the VRAM is at its limit for AMD is regressing performance. 
[^4]: When going from idle to full power, all devices, graphics card or CPU, will take a few milisecond to ramp up, depending on the device. Dialing up the minimum clocks effectively cancels this ramp up at the cost of running the card at full speed perpetually, which is not worth the absurd idle power consumption this comes with.