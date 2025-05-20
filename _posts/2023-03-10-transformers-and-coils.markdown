---
layout: post
title:  Transformers and coils
date: 2023-03-10 17:51:01 -0700
---

There are many electronicians, both hobby and professional, who are at war with electromagnetism. Whenever they need to design a coil or a transformer, an abyss of desperation opens in front of these poor people. The worst thing is that usually these poor victims are not really at fault, since the authors of electronic textbooks seem to have struck a plot to explain these things in such a messy way that nobody can really understand them! Or maybe these authors themselves didn't have a clue about the matter?
Well, internet to the rescue! I will explain the basics, in simple, understandable terms, taking some freedom in regard to standard physics notation. Here you will find most information required to design electromagnetic parts for electronic use.
 

The units
There is one request I have: When you enter this page, you have to leave out all obsolete, absurd units of which most textbooks and catalogs are full: Most notably, inches, gauss and oersted. Delete these three words from your vocabulary! They have no place here. They are the principal culprits in confusing people attempting magnetic design, to the point of driving them crazy. Now that we have gotten rid of them, we can start.
The first unit we will use is the weber, written as Wb. This is the official unit for magnetic flux. If you take a loop of superconducting wire, and apply 1V to this wire during 1s, then the magnetic flux inside this loop will have changed by 1Wb. Note that this is true regardless of size or shape of the loop, and regardless of the matter that's inside the loop! The definition of the weber can be written as:

Wb = V × s

but I prefer a modification of this equation that is more practical, taking into account the number of turns of a multiturn coil. So, this is one of our basic truths:

(1)    Wb = V × s ÷ t

meaning that the change of magnetic flux (weber) is the tension (volt) multiplied by time (second) divided by the amount of turns. This is one of the most powerful and useful formulas we have. In practice it holds true enough even when the wire is not superconductive, as long as its resistance is low enough to cause only a negligible voltage drop at the resulting current. 

If we squeeze a certain amount of magnetic flux through a certain area, then we can speak of magnetic flux density. The unit for this is the tesla, written as T. Its definition is simple and obvious:

(2)    T = Wb ÷ m2

Note that talking about square meters in electronics may look a little removed from practice, as we use parts that have cross-sections more in the square centimeter range. But please, believe me that accepting such "impractical" things is very much more practical than using dozens of different conversion constants! Using the basic units has the large advantage that absolutely no conversion constants are needed.
 

The basic characteristic of any coil is inductance. It is measured in henry, written as H, and its definition is:

(3)    H = V × s ÷ A

or, in words, one henry is the amount of inductance that will allow the current to rise by one ampere when applying one volt during one second. This equation is also very useful for our purposes. And now we can start to play! We can join equations 1 and 3 to make the following:

H = Wb × t ÷ A 

Such algebraic conversions are always true too, and can give us the means to calculate some value we did not know how to determine!

But now let's go to practical things.

Designing power line transformers
While almost any electronician knows that the voltage ratio of a transformer depends on the turns ratio, the question that arises for most novices is: "How many turns-per-volt must I use???"
It's very simple. Assume that you have an iron core, and you want to wind it. First, measure the section of iron through which the magnetic flux will go. Say, the central leg of the transformer core measures 2cm wide, and the entire stack of laminations, well compressed, measures 3 cm. This gives you 6cm2, or 0.0006m2 of cross-sectional area.
Now you need to decide how much flux density you will put through your iron. At low frequencies, like those of power line applications, the limiting factor is usually saturation of the core. The most humble transformer iron saturates at roughly 1T, but more typical values are 1.2 or 1.3T, and good grain-aligned material may work up to 1.6 or even 1.7T.  If you really don't know what material you have, better stay at 1T, so you are on the safe side.

For this example, let's assume your iron is good enough to put 1.2T through it.

Applying formula 2, the maximum magnetic flux has to be limited to 0.00072Wb. But now, before going ahead, stop a moment, and think!!!  Iron can be magnetized in both senses. So, the total change of magnetic flux, from maximum negative to maximum positive, can be 0.00144Wb! This said, let's go to formula 1, and calculate the turns. Let's assume that we are talking about Chile, or any other country where the power lines work at 220V, 50Hz:

0.00144Wb = 220V × 0.01s ÷ t

which is     220V × 0.01s ÷ 0.00144Wb = 1528 turns  (for the 220V primary)

Simple, isn't it?   In fact, the above is too simple to be true. There is another factor which I skipped: The above would be true if the power line was carrying a 220V square wave!! But in truth, nominally it carries a sine wave, that has 220V RMS, while the average is slightly different. And magnetic flux buildup depends on the average, not on the RMS! So, we have to introduce a small correction factor, which is derived via calculus from the sine function. Instead of bothering with the exact maths, I suggest you simple take my cookbook recipe: 11% in our favor! So, we can get away with just 1376 turns!

Where does the 0.01s come from, you may ask?   Think again. The change from maximum negative to maximum positive flux happens during one semicycle. And at 50Hz, a semicycle lasts 0.01s.

We can well reduce all this to a simple, universal formula, valid for calculating turns number for all sine wave transformers and coils:

(4)    t = V(RMS) ÷ 4.44 ÷ m2 ÷ Hz ÷ T

The 4.44 factor isn't a conversion constant, but a factor composed by 2 × 2 × 1.11. One of the "2" is for the fact that the total magnetic swing is twice the single-sided one (so we can give the saturation limit as input data), the other "2" reflects the two semicycles in each full cycle (hertz refers to full cycles), and the "1.11" compensates for the ratio of RMS to average voltage in sine waves.
 

Power
Another question that usually arises is how much power a transformer of a given size can deliver. Let's analyze this.
The magnetic flux in the core depends on the voltage applied to the transformer's windings, the frequency, but NOT on the current the transformer is delivering! Oh, well, yes, there is a small dependence caused by real-world effects: As you draw more current, the resistance of the wires causes a voltage drop, slightly reducing the effective AC voltage applied to the windings, and thus reducing the magnetic flux by the proportional amount. But my point is that the transformer core is not involved in limiting the power delivered. This limit comes from the windings, and has two faces: One is the voltage drop, which is proportional to load current and will at some point be so much that the voltage is no longer sufficient for your load, and the other is heating. As the load current increases, the power dissipation in the windings increases to the square, and if you draw enough power from the transformer for enough time, you will burn it up.

A purist might argue that there is also another factor limiting the power output of a transformer: Imperfect coupling between the primary and secondary, expressed as leakage inductance (uncoupled series inductance) in both the primary and secondary windings. In practical power line transformers, though, this is usually not the limiting factor, and has relatively small effect, except when a transformer is specially designed to limit the coupling, usually by adding magnetic shunts (like in ballast transformers, microwave oven transformers, welding transformers, etc).

All the above makes clear that a transformer's power handling ability depends as much on its magnetic cross section (because more section requires fewer turns, allowing thicker wire to be used), and on its window size, that is, the cross-sectional area of the space where the windings go. But there is no linear formula relating the product of the two areas to power! As the transformer grows larger, the path length for heat evacuation becomes longer, and more importantly, the heat-generating volume of material increases at a faster rate than the heat-dissipating surface. So the increase in power handling ability isn't linearly related to the area product, nor to total volume, weight nor surface area. The best approximation for the rate of change of power capability when transformer size changes, is that the power changes at a slightly slower rate than the volume of the transformer. 

Given all this fuzziness, it's best to do the real calculation: For a given iron core, calculate the amount of turns needed, then consider the space available for them, calculate the wire size, and from the copper volume resistivity of 1.75 * 10-8 Ohm meter, calculate the total winding resistance. Now it may help to know that for small transformers a maximum loss of 10% (5% in each winding) is usually considered acceptable. This should allow you to calculate the power you can safely extract from a given transformer, if you are geometrically knowledgeable enough to do these calculations! It requires nothing more than the math you learned in school, by about the fifth grade.

Hey, I can hear your cry!!! OK, OK, to make things clearer, I will give you an example!!! Let's suppose that the core mentioned above, with 6cm2 cross-sectional area, has 10cm2 available for windings, and that the average turn length is 20cm. We will distribute the room evenly among the primary and secondary windings. And we will assume that 40% of the space gets actually used for copper, the rest being insulation, air, and lost space. This is approximately correct for practical work, and gives us 2cm2 of copper area for each winding. At 1376 turns, the primary will have 0.14mm2 for each turn, and the total wire length will be 275m.

The number given for copper resistivity refers to the resistance across a block of copper measuring 1m on each side. But our winding has a cross section of only 0.00000014m2, and a length of 275m. So, its resistance will be:

0.00000001.75Ohm m  × 275m ÷ 0.00000014m2 = 34 Ohm.

We will allow 5% loss in each winding. At 220V, 5% is 11V. Now simply apply Ohm's law, and the maximum primary current comes out as 0.32A, which multiplied by those 220V, gives a gross maximum input power of 70W for this transformer.

Cool, huh?   :-)

But this is only an approximation. The true value will depend on the allowable heating, which in turn depends on the thermal class of the wire and insulation you use. Fan cooling, or even oil immersion, increases the power rating quite a lot. Using a low loss core material allows running a higher flux density, increasing the power rating. And so on...
Note that the magnetizing current is not being considered here. You may say that even if it is only 10 or 20% of the maximum current, it should be considered! If you say that, you are quite wrong...  Because the magnetizing current is 90 degrees out of phase with the reflected load current, and so, even if it is 20% of the load current, the magnitude of the vectorial sum of the two is very close to that of the load current alone. It doesn't pay to care for the small difference!
Switching power supply transformers
The above chapter can be applied almost in full to the design of higher frequency transformers for switching power supplies. There are just some practical differences which I will mention now.
For one, at frequencies much above a few hundred Hertz, saturation is no longer the limiting factor when choosing the maximum flux density. The reason is that the losses in the magnetic material become so large that the flux density has to be kept much below saturation, simply in order to keep the core losses at an acceptable level!

You really need the core's data, provided by the manufacturer or measured by you, to determine how much flux density is acceptable. To give you a rough idea, first consider that at these frequencies almost always ferrite material is used. And ferrite saturates at about 0.3 to 0.4T, so this will be the absolute limit. For typical power-type ferrite material, at 25kHz you will have to keep the flux density below 0.15T, and at 100kHz below 0.05T. But a lot depends on core size too. A larger core will have to run at lower flux density in order to avoid overheating. And low loss ferrites can be run at somewhat higher flux densities.

Normally switching power supply transformers operate with square wave, or something close to it, which means that you must eliminate the 11% "sine factor" from the equations. And then, many of these things don't apply full symmetric AC to the transformer, but instead use only one side of the magnetization loop! In short, you will have to understand the circuit you are designing for, and then use a little gray matter to determine which of the "2" factors apply, if any. For all the rest, the calculation is the same as for power line transformers.

Don't be surprised if you end up with very few turns. In fact it's quite common to have just 10 or 20 turns for a 300V primary winding in a large switching power supply!
Broadband RF transformers
Maybe you have seen those ferrite transformers often used at the output of solid state RF power amplifiers. They look like two ferrite tubes, side-by-side, with two copper tubes inserted in them, forming a 1-turn primary winding. Through these copper tubes a few turns of insulated wire are looped, forming the secondary. Let me use such a transformer as a further design example.
Our hypothetical case will be a 100W push-pull amplifier for 1.8 to 30MHz, powered from 13.8V, like there are millions in daily use by radio amateurs and various commercial services.

Each transistor can pull its side of the transformer primary quite close to ground, but not fully, because of its saturation voltage. RF transistors typically saturate at about 1V, so it's reasonable to assume that the transistors can swing over a ±12.8V range, giving a 25.6V peak value over the primary, or about 18VRMS    . On the other hand, the secondary is expected to deliver the RF power to a 50 Ohm load, and 100W over 50 Ohm equates to 70.7V. Thus, we need a voltage (and turns) ratio of about 3.9. As we have a one-turn primary, we can only implement integer ratios, and thus we will choose a 4-turn secondary. The effect of this is that at 100W the transistors will be running at 17.67VRMS , or 25V peak between them. Thus they will swing over 12.5V from the power supply voltage, giving a headroom of 1.3V for saturation and power supply voltage drop. So far, so good.

At 1.8MHz, our lowest frequency, a typical ferrite can be safely driven to about 0.012T. We have a nice, pure sine wave, so let's use equation 4:

1 turn = 17.7V ÷ 4.44 ÷ m2 ÷ 1800000Hz ÷ 0.012T

or, rearranging,  17.7V ÷ 4.44 ÷ 1 turn ÷ 1800000Hz ÷ 0.012T = 0.00018m²

So, we need a total core cross-section of 1.8cm2. A smaller core would overheat when running at full power for a long time, while a larger core would be more expensive, but bring us an advantage in terms of spectral purity, as lower flux density means lower distortion too! But for this exercise let's keep the 1.8cm2 figure.

We still have work to do. We could use long and thin ferrite tubes, or short and fat ones. And we can choose among several different ferrite types! To narrow our choices, let's see the inductance requirements. The rule is that the transformer should exhibit a self-inductance that's high enough to be of little effect when placed in parallel with the load resistance. An old rule of thumb is to have 10 times as much inductive reactance as load resistance. You can choose if you want to calculate using 4 turns and 50Ohm (secondary side), or 1 turn and 3.1Ohm (primary side). The result will be the same. I will pick the primary side.

The inductive reactance is calculated as

XL ( Ω) = 2 × PI × Hz × H

So we end up with    31Ω ÷ 2 ÷ 3.14 ÷ 1800000Hz = 0.0000027H

We need 2.7µH primary inductance to satisfy the rule of thumb for 10 times as much transformer reactance as load resistance. Now we can go to the manufacturer's data tables and pick suitable cores. For this example, I will use the Amidon catalog.

Let's give the very common FT-50-43 a try. This toroid has 0.133cm2 cross sectional area. Two stacks of 7 each would fill the request for flux density. Its AL is 0.52µH per turns squared, so 14 cores with 1 turn would give 7.3µH, several times as much as needed. Since broadband amplifiers tend to oscillate at low frequencies, where the transistors have a very high gain, it may not be a good idea to provide more low-frequency performance than necessary!  Let's try another core type.

The 43 material has a permeability of 850. A core of the same size, but with a permeability of only about 330 would be nice. But Amidon sells no core in this size in any permeability close to that... Hey, you can't get free rides in every case! :-)   The next lower permeability available from Amidon in sizes that are usable for this project is 125, and that's too low, unless we are willing to compromise a little on inductance...  So we will have to stick to the 43 material. Let's see what we can do.

There is the FT-82-43, made of the same material. It's much thicker, has 0.25cm2 of cross-sectional area, and an AL  value quite close to that of the other core: 0.55µH per turns squared. Two stacks of four each of these would give more than enough cross-sectional area, at 4.4µH. This could be a usable solution, also providing more space for the windings.

On the higher frequencies the flux density will be lower, always staying below the limit of the material at each given frequency. The ratio of inductive reactance to load resistance will improve as the frequency goes up, but at the highest frequencies the parasitic capacitances may take over, so it's a good idea to consider this in the design of the amplifier.
Energy storage in magnetic cores
Do you know how much energy a coil is storing? It's defined by the same old formula that shows up so often in good old Newtonian physics:  "a" is equal to one-half "b" times "c" squared!
(5)     J = H ÷ 2 * A2

Energy is expressed in Joule (J). H is the inductance in henry, while A refers to the amount of ampere magnetizing the core. In the case of a transformer, this current must be calculated as the net current after subtracting any opposing currents, such as primary and secondary ones, giving proper care to the number of turns. In short, this current is the magnetizing current.
 
In most typical transformer applications this storage of energy is not really desired, but is an unavoidable side effect. But there are applications that make good use of this storage! One very important example is the fly-back topology of switching power supplies. Basically, such power supplies make the transformer store energy coming from the primary circuit, and then dump this energy into the secondary circuit - often at a voltage that is unrelated to turns ratio! As primary and secondary currents do not flow at the same time, it is no longer true that the voltage ratio must be equal to the turns ratio!

Let's suppose that we will make a switching power supply using this principle. We want 13.8V output, while the input should be 110 or 220Vac. The logical approach in this case is to use a primary rectifier that can be configured as bridge for 220V, or as voltage doubler for 110V, so we will end up with about 300Vdc in either case, and the rest of the switching power supply will be the same, regardless of line voltage.

Let's further suppose that we have a ferrite core of 2cm2 cross-sectional area, 12cm path length, sporting an initial permeability of 2000, saturating at 0.35T, and we want to run it at 100kHz. To design this unit, we need one more piece of information: The AL value, which relates turns number to inductance. If we don't get this value from the core's manufacturer, we can calculate it from physical dimensions and ferrite properties, or we can wind a test coil and measure it, but it's definitely easier to get it from the catalog! Let's suppose that for our core, this value is 6µH per turns squared. Which means that 1 turn will give 6µH, 10 turns will give 600µH, and so on.

The values assumed above are quite typical for practical cases.

In order to reduce voltage strain on the primary switching transistor, we will assign 30% of the time to transformer charging, and 60% to discharging. This allows to perform the discharge at half the voltage level, so the switching transistor will only see 450V instead of 600V. Also this reduces peak current on the secondary rectifier, while increasing requirements for primary current handling and secondary voltage handling, both of which are not a problem in our supposed situation.
The 10% of time remaining is there to account for time lost during switching, dead time of the controller, etc.
At 100kHz, our charge time will be 3µs, and the discharge time will be 6µs. A look into the ferrite's data table lets us know that at 100kHz, maximum single-sided flux density should be limited to 0.1T. Applying formulas 2 and 1, we quickly end up with the following:

300V × 0.000003s ÷ 0.1T ÷ 0.0002m2 = 45 turns

So, 45 turns will load up this core to 0.1T in 3µs, when applying 300V. Nice and easy. On the secondary side we need 13.8V, plus about 1V for diode drop, giving about 15V. We can use the same formula again, inserting the different values for voltage and time:

15V × 0.000006s ÷ 0.1T ÷ 0.00002m2 = 4.5 turns

Did you like this? The turns ratio is 10:1, while the voltage ratio is 20:1, because the time ratio is 1:2!

Feel free to use either 4 or 5 turns instead of the fractional number. This will simply cause a slight alteration in the charge-discharge times.

Now, how much power can we extract from this switching power supply? No, don't run for the things I wrote above about line power transformers! Here we have two limits: One is the limit of the transformer proper, as of heat dissipation, but then there is a functional limit too, which is more important: Our switching power supply works by energy storage, and for each cycle, only a very specific amount of energy is stored, strictly limiting the power that can be transferred!

According to the AL value assumed above, our 45-turn primary winding will have an inductance of about 12mH. Using the definition of inductance, we can calculate the peak current that will flow at the end of the charging cycle:

 300V × 0.000003s ÷ 0.012 H = 0.075A

Only 75mA!  Doesn't look like much...  Let's calculate the stored energy for each cycle:

0.012H ÷ 2 * [0.075A]2 = 0.000034J

You can also calculate this energy from another approach: As the current will ramp up linearly from zero to 75mA, its average will be 37.5mA. At 300V, and 3µs, that is:

300V × 0.0375A × 0.000003s = 0.000034J

Isn't it nice when things agree...?      :-)

Considering that at 100kHz we have 100000 of these tiny specks of energy per second, and that joule is simply watt multiplied by second, we end up with a sad power level of only 3.4W for our glamorous switching power supply!  Seems like a mighty bad use for a core of that size, doesn't it? That core is rated at "250W typical" by the manufacturer!!!

So we must see how to increase the amount of energy stored in the core. If we increase the inductance, then the current will drop, and the current carries squared weight! Not a good idea. It's better to reduce the inductance, so the current increases. Given that the stored energy depends linearly on the inductance, and on the square of the current, it's obvious that as we reduce the inductance, the stored energy increases in proportion.

How do we do that?  We cannot simply reduce the number of turns! This would bring us into the claws of equation 1, and increase the flux density to a level that is far higher than what the ferrite can tolerate! Do you realize the problem? We need to reduce the inductance, while keeping the number of turns in order to preserve flux density!

There is a very simple tool for doing this: Air! Simply force the magnetic flux to jump through an air gap, by separating the two core halves by a small distance. The effect of this air gap is reducing the effective permeability of the core, thus reducing its AL value, without affecting other parameters. Let's see what happens if we add an air gap of just 1 mm total, which is done by separating the core halves by 0.5mm:

The magnetic flux will now travel through 120mm of ferrite having a permeability of 2000, and 1mm of air having a permeability of unity. 2000mm of ferrite would have as much magnetic reluctance as our single mm of air! So the air gap has added 16.667 times as much reluctance as the ferrite core originally had. Plus the original reluctance, we have now 17.667 times as much reluctance as before.Which means that our core now has an effective permeability of only 113.2, instead of the former 2000!  

To be more precise, this would be the effective permeability if the flux in the air gap would have the same cross sectional area as in the core. But it doesn't! It spreads out slightly, covering a larger area, and for that reason the gap introduces a little less reluctance than calculated. How much so is relatively hard to calculate, because the larger the gap is, relative to the core leg dimensions, the stronger this effect becomes. So it's better to simply round up the calculated value of effective permeability for the gapped core. Let's say, to 120. And then keep in mind that this is only an approximate value. 

The new AL value is then about 0.36µH per turns squared, and our 45-turn primary winding now has only 0.00072H inductance. This in turn means that it will charge up to 1.25A, and store 0.00056J per cycle, resulting in an output power of 56W for our switching power supply, which looks very much better than the meager 3.4W obtained without an air gap! All this is while maintaining exactly the same magnetic flux in the core!

Did you ever think that a 1mm thick layer of air can be so tremendously important???

Your next question should be if there is a limit to air-gapping. Sure, there are two limits! One is simply that as you increase energy storage and power transfer, you are also increasing the losses in the windings. At some point you will reach the thermal limit from copper loss, just as you did with the humble, simple line-frequency transformer. This amount of air-gapping is often chosen as optimum trade-off by designers.  But then there is another problem: As the effective permeability drops, so does the coupling between the windings. The transformer will start producing a strong stray field, and the stray (uncoupled) inductances will become large relative to the main inductances and the circulating currents, which can lead to the destruction of power transistors and diodes, and in most cases will require additional snubbering. So, the designer must in some cases settle for less air-gapping than what the thermal limits of the windings would suggest. In any case, these coupling problems can be minimized by proper construction of the transformer: The primary and secondary windings can be interleaved, bifiliar winding is sometimes possible, and it's often a good idea to add a thick copper foil around the entire, completed transformer, core and all, forming a shorted turn. Such a shorted turn will force the total external flux to zero, making sure that the flux through the coil assembly will be equal to that outside it (in the side legs of the core), thus improving the coupling.

In many cases it's better to use a core material of lower permeability, like powdered iron. Our transformer above would behave almost the same if we built it on a core made of 120-permeability material, with no air gap, but it would produce less stray field. On the other hand, the large advantage of the air-gap technique is that the designer can decide exactly how much effective permeability he wants, without having to order a new core! 

Lastly in this air gapping business, you should be aware that air isn't always air! Very often cores are "airgapped" by inserting sheets of paper, plastic, or other non-magnetic, insulating materials between the core halves. The effect is the same.

DC chokes
One of the worst things I have seen in electronics textbooks is giving different formulas for AC and DC behavior of coils. This is complete, utter nonsense!!! There is no fundamental difference between AC and DC. At any given instant of an AC waveform, you have pure DC, and in a "DC" application, you have an AC component too, at least during power-up and power-down. So, we can, and should, use the same design approach for DC chokes.
Let's see how this applies to practice. One common task is designing a choke that has to exhibit a certain inductance, while being capable of conducting a certain amount of current before saturating. Note that for DC applications the limit of flux density is always saturation-related. Do you remember what I wrote further up? At high frequencies, the limit is given by core loss, and at low frequencies, by saturation. And DC is simply a very, very low frequency... :-)

Let's suppose that we need to make a choke of 100µH, that can take at least 10A of current before saturating. Let's suppose that we have a toroidal core for it, made from powdered iron, that has a cross section of 1cm2 and a path length of 10cm. The permeability is 75, and saturation begins at 0.5T. The AL value of this core is 0.08µH per turns squared.

From the AL value alone, we can easily calculate that 35 turns would be required. Now, how can we calculate the flux density?   After all, there is no voltage applied across the winding!

Think again! There MUST have been a voltage applied, in order to get the current going! If we had applied 1V, it would have taken 1ms to ramp the current up to 10A in this 100µH coil. So, now we have a voltage and a time to insert in our universal formula number 1! The flux would be

1V × 0.001s ÷ 35t = 0.0000286Wb

resulting in a flux density of 0.286T in our 0.0001m2 core! Bingo! This choke could actually conduct almost twice the required current before saturating.

A cost-conscious designer would run the same exercise with the next smaller core available, which probably would be just large enough to allow making the required 100µH, 10A choke.
Picking cores
There are countless shapes and sizes of magnetic cores available, and each of them comes in many different materials! It's a good idea to know, at least in principle, what's available.
Materials:

The most ancient material is transformer iron, known also as silicon steel. It usually comes in thin sheets, which need to be insulated from each other in order to avoid eddy losses. Only in pure DC applications is it acceptable to use solid steel or uninsulated sheets.
Transformer iron can usually be counted upon as to be able to take at least 1T before saturating, while 1.2T is OK for most, 1.5 for some, and 1.7T is possible with the best grades. The permeability of this material is usually around 2000 to 5000. Those iron alloys that allow higher flux densities tend to have the lower permeabilities.
The losses are so high that for frequencies above 100 Hz or so they become the limiting factor, rather than saturation.

Iron is also used in the form of dust, mixed with resin and molded into magnetic cores. The permeability depends on the iron contents in the mix. Given that even a small amount of resin has much more reluctance than a large amount of iron, the permeability is usually quite low: Values from 2 to 100 are typical. For the higher permeabilities the iron grain size and shape becomes very important, since very dense grain packing is needed to achieve them.
Saturation effects start earlier than for massive iron, because edges and corners of the iron particles that are very close to each other tend to concentrate the flux and saturate first. 0.5T may be a typical value, but in any case the saturation is very "soft", there is no well-defined point at which the core would clearly enter saturation. The losses can be low enough to make the lower permeability versions of this material usable well into the RF range, while the higher permeability versions tend to have much higher loss than ferrite.

These dust cores also are made from other metal alloys, like Permalloy, in some cases offering attractive features, like higher permeability and lower loss than iron powder cores. And transformer laminations are also available in other materials than silicon steel, making them usable throughout the audio range and even much higher, if the laminations are thin enough, but such laminations aren't easy to obtain.

Ferrites are the most versatile materials available for magnetic cores. While they saturate at lower values, typically 0.3T, they exists in a tremendous range of permeabilities: It's not hard to find ferrites with permeabilities as low as 20, or as high as 15000! The inexperienced user cannot tell a ferrite core's magnetic characteristics simply from looking at it. Even when two ferrite cores look exactly alike, there could be a 1000-fold difference between them! So, make sure you KNOW what material you have, before starting your calculations!

In any case, the most common ferrites fall into two categories: Power ferrites, used for switching power supplies, fly-backs, TV yokes, etc, that most commonly have a permeability of around 2000 and low loss at frequencies of about 20 to 200kHz; and RF-type ferrite, with permeabilities of around 100 to 1000, and loss characteristics that makes them useful at least through 30MHz. But there are many ferrite types that can work at much higher frequencies and have lower permeability. The permeability values significantly above 2000 are reserved to rather special-purpose cores intended for wideband transformers, pulse transformers, current sensing transformers, transducers, or noise absorption.

Regarding shapes, I will mention just a few:

Toroids: They are simple, cheap, easy to design for, have low flux dispersion, good self-shielding, but cannot be airgapped by the user, and winding 10000 turns on a toroid is not something I would like to do by hand.

E-cores: Very convenient for most applications, but the sharp edges cause more dispersion.

E-cores with round center leg: These are much easier to wind than the square-leg versions, when the wire is stiff.

U-cores: Slightly cheaper and slightly less efficient (due to longer path length) than E-cores.

U-cores with one round leg: They were very common in the flyback transformers of CRT TVs and monitors.

Pot cores: Join the convenience of E-cores with the good self shielding of toroids (in fact, it's even better!), but they are more expensive. Some are adjustable.

RM cores: Sort of a space-saving cross breed between a pot core and an E core.

Solenoids (bars): Usable for chokes. They have really large airgaps! :-)  For that very reason, they are not suited to transformer use. The coupling would be too poor.

Bobbins: These are basically solenoids with end plates, so their airgaps are slightly smaller than those of solenoids. They make good chokes and are convenient to wind.

E-I laminations: It's pretty much the only shape you can buy transformer iron in!

I suggest you download a few catalogs from magnetic material manufacturers and distributors, so you can learn about the other 991 shapes available...  May I suggest Amidon Associates, Fair-Rite, Ferroxcube, Ferrinox (Thomson Composants), SiFerrit (Siemens), TDK, Philips, to name just a few. Several of these companies have been absorbed, acquired, renamed, merged into other ones, so the same catalogs might now bear different brand names. I have worked mostly with Amidon (Fair-Rite, American Micrometals), Ferrinox, Ferronics, and junk-box cores. The best performances seem to come from certain Japanese ferrites.

this post copy from: https://ludens.cl/Electron/Magnet.html
