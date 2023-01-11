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
- [Extra](#extra)
    1. [Enabling Resizable BAR on Intel CPUs](#enabling-resizable-bar-on-intel-cpus)
    2. [Pushing past the frequency limit](#pushing-past-the-frequency-limit)
    3. [The RDNA3 Hotspot issue](#the-rdna3-hotspot-issue)
    4. [Difference between different AIB models](#difference-between-different-aib-models)
- [Credits](#credits)

# Preface
Overclocking is about pushing hardware and silicon out of their comfort zone in pursuit of performance. Overclocking is inheritantly dangerous, it's just the amount you're willing to risk.

It's not as bad as it sounds to be honest. Sure, you're risking your hardware, but you're very likely to become thermally limited or even power limited before you're at the point of putting in absurd voltages and frying your hardware. With all the software and hardware safeguards you're probably fine.

With that out of the way, how do you overclock your AMD GPU?

# Prequisites
You need an AMD Radeon GPU, that's obvious. I'm not going to cover overclocking ancient ATI cards, nor the Polaris and Vegas. If you're here for instructions for those cards, you'll have to look elsewhere.

Specifically, I'm going to focus on the RDNA cards, which is the RX 5000 series and above. The instructions for all the RDNA cards is identical, so you don't have to worry about generation specific instructions.

Now, you'll also need some required software, and they're as follows:

1. [AMD Software: Adrenalin Edition](https://www.amd.com/en/support), their software comes with a built in overclocking tool, so we'll be using it.
2. [HWInfo](https://www.hwinfo.com/) for monitoring system temperature, power draw, fans, and just about anything.
3. [3DMark](https://3dmark.com), a synthetic benchmark for both CPU and GPU, it's not very good for comparison between hardware but useful to test overclocking and stability. The full test is paid but the free demo can be found [here](steam://install/231350), this demo link requires Steam to be installed to your PC.
4. **Optional** [Unigine Heaven](https://benchmark.unigine.com/heaven) is absolutely ancient. First released in 2009 for the then-bleeding edge DX11, it's a tech demo and benchmark rolled into one. It's not recommended these days due to it's age but it can be looped in the background unlike 3DMark.
4. A document to write your benchmark score and overclocking settings down. This is for comparing your score improvements and possible overclocking settings. 

# Testing Methodology
Any guide or review lacking testing methodology is not a good guide, so I'll include this. 

Throughout the guide, I'm using my Sapphire RX 6900XT Nitro+ SE[^1] for demonstration. The fan curve is default, Resizable BAR is enabled, and everything about the PC is set as stock. Remember, everything I say, like benchmark score and percentage is all relative to my GPU, so yours, even when following the exact instructions, might be a little different. 

| Hardware      | Model         |
| ------------- | ------------- |
| CPU  | i7 12700K  |
| GPU  | Sapphire RX 6900XT Nitro+ SE  |
| RAM | 32GB DDR4 4000 Trident Z Royale Gear 2|
| Motherboard | Strix Z690-A |
| Case | Corsair 4000D Airflow |

All benchmarks were ran 3 times with the average taken. <!-- in case you're wondering, no, you can't find my benchmark runs I listed in the 3DMark database, since I'm running a Custom Test, the results isn't uploaded, hope you're alright taking my word for it.-->

# Overclocking
With the software and test system out of the way, it's time to dial in some overclocking.

However, before we do any tuning, we need to get some baseline of how your GPU performs.

## The Software
The tuning page is hidden away pretty well, You need to go to `Performance` --> `Tuning`, and under `Tuning Control`, select `Custom`.
![Performance Tuning](/Assets/Tuning%20Page.jpg)
There's also one Tuning Preset that's worth noting, `Rage Mode`, that ups your power limit and *doesn't void the warranty*. The performance gain is pretty laughable, but for users who want to play it *absolutely safe*, the option is there.

There's a strange feature in AMD software where `Advanced Control` just changes the slider from percentage based into the actual value. I suggest enabling this for every setting. You also need to `Enable` the specific tuning category in order to make any changes

## Getting a Baseline
Open 3DMark and run Time Spy Extreme, and to save time, I suggest running a Custom Test without the CPU test. 
![the recommended test and setting](/Assets/3D4K.jpg)
After you ran the test, note that score down. 

For reference, my card scored `9846` at stock. We'll be using this as baseline.

## Upping the power limit
Modern cards have a lot of safeguards, so it's going to be hard to brick the GPU. It's perfectly safe to up the power limit to the max allowed. RDNA cards are all fairly power starved, so giving the card more juice is crucial[^2]. 
![shown here with default settings](Assets/Power%20Limit.jpg)
At stock, my card is rated for 289 watts. If we put the slider to the max, the TDP jumps to 332 watts instead. So what performance does the extra 43 watts get us?

After some benchmarks, I got `10272`. A 4.3% improvement for 15% more power[^3]. Not good enough, let's do some more tuning.

## Tuning the VRAM
VRAM is like the GPU's system RAM, and unlike system RAM, tweaking VRAM is as simple as changing a slider. Beware, however, that upping the memory clock too much will cause performance regression[^4]. However, for some GPUs and models, you may be limited by the slider first before running into memory limits. RDNA2 cards have very low bandwidth overall, with the high-end 6900 XT not having more than 512GB/s of bandwidth. If we max out the slider and added 150MHz to the memory clock, we'll get an extra 38GB/s of bandwidth, or 550GB/s total. This is closing in on 6950 XT performance!

I suggest starting from the max, lower the VRAM in 50MHz steps and running a 3DMark test. If performance regresses, you could try to fine tune and get the optimal setting. It's also safe to enable `Fast Timings` in the setting, which improved my score by about 800 to 1000 points.
![Memory overclocking is on the bottom left](/Assets/memory%20tuning.jpg)

With VRAM maxed out and Fast Timings enabled, `10565` is the end result, 7% improvement over stock, we're making good progress! 

## Changing Clocks and Voltage
Now, the GPU have access to more power, but it might not know how to tap into the extra potential, thus creating such inefficiency. It's only until we tweak the Voltage and Clock does the card know what to do. For starters, leave the minimum clock alone, changing that would only result in very bad idle power consumption and no perceivable performance gain[^5]. 

Strangely, only upping the Clock Limit slider doesn't seem to do a whole lot performance wise, and may even cause stability and deteriorating performance. This is especially the case for RDNA3. That's where the voltage slider comes into play. For RDNA cards, Clock and Voltage is inversely related, lowering voltage will increase your clock. This seems counterintunitive, especially compared to how CPU overclocking works, but with lower voltages more power will become available to the GPU, thus clocking higher.

First off, don't go and crank the GPU core frequency dial all the way to the max allowed, we need to tweak the voltage first. Start by dropping voltages in small steps, run a benchmark to check the core clock, until you reach "the wall". For RDNA2 and 3, there's a "wall" that doesn't allow the card to clock higher despite being below the max frequency allowed. This wall is around 50MHz below the Clock Limit specified by the software. In essence, "Maximum Frequency" seems to be "Maximum minus 50". 

When you reach the wall, now's the time to dial an increase for the Clock Limit slider. Add 100MHz and start decrease voltage again until you hit the new wall. At this point, tweaking the card can be simplified as follow:

1. Decrease voltage until the frequency reach the Clock Limit slider.
2. Increase Clock Limit by 100MHz
3. Repeat step 1-2 until the GPU exhibits instability and/or your PC crashes. 

That's it! You're overclocking!

<!-- Insert overclocked screenshot and OC score here
-->

### RDNA1 specific instructions
There was a lot of memes about how bad the RX 5700XT drivers were. Well, it was *abysmal* at launch, but gotten vastly better in the coming months. However, overclocking is relatively simple for that generation, unlike RDNA2 and 3. Upping the core and memory clocks directly yields an improvement in performance. Despite that, tweaking voltage as above still allows the user to dial in more clocks. 

# Extra
There is some other information about those cards that I want to point out, or just additional information.

## Enabling Resizable BAR on Intel CPUs
When you hover your mouse over the ReBAR toggle, the software claims "This feature is only available on AMD CPU or APU paired with an AMD GPU"[^6]. I'm not sure what AMD is trying to achieve here, spreading false information, but it's totally possible to turn this on with an Intel CPU.

You'll have to go in to your motherboard's BIOS, typically by pressing `Delete` on startup. Once you're in, look for the Resizable BAR option or toggle, which is up top, in this picture.
![The BIOS layout of a Strix Z690-A](/Assets/AsusBIOS.jpg)
Once ReBAR is enabled, you can save, exit, and enable it through AMD Software's Tuning page. 

## Pushing past the frequency limit
You might've noticed there's a limit to the frequency slider I mentioned previously. Well, a lot of people might wonder about going past that. The thing is, without modding and alterations to the hardware and/or software, you can't go past it. There is some select GPUs that doesn't have the limit, but as of this writing, I'm only aware of 6900XT with the XTXH die that doesn't have the frequency cap. Feel free to correct me if I'm wrong[^7].

Thing is, I doubt this limit would do much to most people anyways. The limit for the RX 6900 XT is 3GHz, and I imagine it is going to take some insane effort and cooling to hit it anyways, so I won't worry too much about the said limit.

## The RDNA3 Hotspot issue
After the launch of RDNA3, buyers of AMD's Reference cards complained their card's hotspot temperature could shoot up to 110C after putting any load on the card. After some journalist and overclockers investigated, the issue appears to stem from a vapor chamber design flaw, causing the liquid to unable to circulate back to the GPU after condensation.

This appears to be specific to the Reference cards, so I suggest you buying a card from an AIB, like Asus or Sapphire, just to name a few.

## Difference Between different AIB models
Not all cards are created equal. The variance in silicon quality aside, differences between different AIB cards of the same model will cause variance in performance as well. Let's take my Sapphire RX 6900 XT and the AMD Reference Card as an example. The most notable difference is the Sapphire have dual VBIOSes and an extra 6 pin power connector, which gives it an extra 75 watts compared to the Reference Card. It's those additional features (which may or may not be on my card), like extra power connectors, better cooler, advanced power delivery, which benefits overclocking the most. 

The difference between different cards starts to shrink as we start looking below the flagship. At the mid-range, where advanced power and extra power connectors is no longer a concern, the delta between the cards starts to narrow down. At the low range, like 75 watts or so, it doesn't matter at all at this point. Any cards would perform identically to each other. 

With that in mind, the difference between low and mid-range AIB cards is minimal, it's only when we go higher into flagship territory does extra features matter.

# Credits
Did you think I did all the testing myself for this guide? Of course not! I'd like to thank the following sources and/or people that made this guide possible.

TechPowerUp and their [Asus RX 7900 XTX OC review](https://www.techpowerup.com/review/asus-radeon-rx-7900-xtx-tuf-oc/39.html), helping me provide insight to how RDNA3 overclocks.

TPU and their [Asus Strix RX 5700 XT review](https://www.techpowerup.com/review/asus-radeon-rx-5700-xt-strix-oc/32.html), which provided simple overclocking instructions for RDNA1.

Tom's Hardware for reviewing the [RX 6900 XT](https://www.tomshardware.com/reviews/amd-radeon-rx-6900-xt-review) for confirming how RDNA2 overclocks and providing a comparison for my card. I also used the screenshot for the BIOS page from their [Z690 A review](https://www.tomshardware.com/reviews/asus-rog-strix-z690a-gaming-review/2).

Extreme overclocker Der8auer for dissecting and doing testing on his Reference RX 7900 XTX, leading to the conclusion of a defective vapor chamber. Information via a [Tom's Hardware's article](https://www.tomshardware.com/news/defective-vapor-chamber-may-be-causing-rx-7900-xtx-overheating-issue).

[^1]: It's only when I finished benchmarking and halfway through writing did I notice I'm running on the `Performance` VBIOS with a higher power limit. However, it's hard pushing higher past 332 watts as it's limited by the driver.
[^2]: This is most obvious comparing a Reference 6900XT and the Strix 6800XT. The Strix have much more power available and it can beat the overclocked Reference 6900XT when the Strix is pushed to the limits.
[^3]: By "power", I mean the rated TGP, and not the actual comsumed power. However, in practice, those two values are pretty similar, the GPU won't exceed TGP by more than a watt or two.
[^4]: Unlike Nvidia cards, where overclocking the VRAM will cause visual artifacts and corrupt textures, the only indication the VRAM is at its limit for AMD is regressing performance. 
[^5]: When going from idle to full power, all devices, graphics card or CPU, will take a few milisecond to ramp up, depending on the device. Dialing up the minimum clocks effectively cancels this ramp up at the cost of running the card at full speed perpetually, which is not worth the absurd idle power consumption this comes with. A good analogy would be running your car's engine at 3000 RPM waiting for the red light for the *tiny* advantage during launch, at the cost of absurd amounts of fuel.
[^6]: As of Driver version 22.5.1
[^7]: Back in April 2021, famous overclocker Der8auer set a record for the RX 6900 XT at that time, a whopping 3.225GHz on LN2. His [video](https://www.youtube.com/watch?v=fhP46XWMkdY) on the subject have some fairly interesting insights about AMD's clock limits and extreme overclocking.