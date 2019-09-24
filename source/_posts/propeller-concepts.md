---
title: Propeller geometry and performance
date: 2018-04-06 23:10:26
categories: 曹衣出水
tags: [Principle of Naval Architecture,Marine Hydrodynamics,Propulsion,Propeller]
---

## Introduction

<img src="https://raw.githubusercontent.com/zharuosi/blog/source/source/images/propeller-screw-icons-vector-16504568.jpg" width="50%" height="50%">

A propeller in propulsion system can transmit power by converting rotational motion into thrust, based on Bernoulli's principle. The most common propeller is called the screw propeller, with fixed helical blades rotating around a shaft. The first screw propeller in the world was invented in 1835, by John Ericsson and Francis Pettit Smith. Propellers can be used in marine ships, aircrafts, etc. The basic concepts and priciples in marine propulsion are introduced in this blog.

## Geometry
<!-- more -->
<img src="http://generalcargoship.com/fixed-pitch-propeller-terminology.jpg" width="75%" height="75%">

- Hub: The solid center disk that mates with the propeller shaft and to which the blades are attached.  A smaller hub can lead to a larger thrust, however there is a tradeoff between size and strength.
- Blade: Twisted fins or foils that protrude from the propeller hub. The shape and the speed at which they are driven dictates the torque a given propeller can deliver. Higher diameter equates to higher efficiency for low speed vehicles (<35kn). To obtain higher torque, the rpm (revolution per minute) should be reduced and the diameter should be increased. In high speed vessels, larger diameters lead to high drag. $Z$ is used to represent the number of blades.

Here are basic nomenclatures to describe a blade section:
<img src="http://pubs.sciepub.com/ajme/1/2/3/image/fig1.png" width="75%" height="75%">

- Leading edge: the point at the front of the airfoil that has the maximum curvature.
- Trailing edge: the point of maximum curvature at the rear of the airfoil.
- Meanline (camber): half distance along a section between the upper and lower surfaces. Quite  often the meanline distribution is tabulated forms such as a NACA a=0.8 meanline, where a=0.8 means the meanline can create constant lift of 80% of the chord, then the lift drops linearly to zero at 100%.
- Chord (c): the nose-tail line, connecting the leading edge and trailing edge.
- Camber height (f): the distance between nose-tail line and meanline normal to chord.
- Thickness (t): the section thickness along a line normal to the meanline based on American convention. Thickness measured normal to the chord line is based on British convention.
- Angle of attack (AOA): the angle between a reference line on a body (often the chord line of an airfoil) and the vector representing the relative motion between the body and the fluid through which it is moving. The chord line of the root or the zero lift axis is often chosen as the reference line.

Note that top pressure on the blade section is lower than the bottom pressure. The difference of the pressure leads to the lift force on the blade, thus thrust is generated.

- Face: the pressure face, high-pressure side, faces backwards and pushes the water.
- Back: the suction face, low-pressure side, faces upstream and towards the front of the vessel.
- Leading edge: the side cuts through the fluid.
- Trailing edge: the edge of the downstreams.

<img src="http://www.kepu.net.cn/gb/ydrhcz/ydrhcz_zpzs/ydrh_201710/201710/W020171031591562900957.jpg" width="75%" height="75%">

- Pitch: the axial distance advanced during one complete rotation of screw is called nominal pitch. The distance the ship is propelled forward in one propeller rotation is actually less than the pitch. The trace of the tip points on the blade is a helix. Pitch ratio is the ratio of pitch to diameter.

<img src="http://pubs.sciepub.com/ajme/1/2/3/image/fig2.gif" width="75%" height="75%">

- Slip: The difference between the nominal pitch and the **actual** distance in one retation ($s = pnt - V_At$).
- Propeller section: A circular arc section cut through the blade at some radius. We can expand it to 2-D foil section.
- Midchord line: the line produced from the midpoint of section nose tail line of each section along a blade.
- Rake: Axial distance from the midchord point at the hub section and the section of interest.
- skew angle: the angle between a radial line going through the hub section midchord point and a radial line through the midchord point of the section of interest AND projected.

## Performance

- Speed of advance: the propeller advances through the water at a speed of advance $V_A$, which delivers a thrust $T$. When the speed of advance is zero, the efficiency is also zero, but the propeller still delivers thrust and absorbs power.
- Thrust power:
$$ P_T = T V_A $$
- Effective power:
$$ P_E = R V $$
- **Wake**: In general the water around the stern has acquired a forward motion in the same direction as the ship. This forward-moving water is called wake. The difference between the ship speed V and the speed of advance is the wake speed. The wake fraction is defined as:
$$ w = \frac{V-V_A}{V} $$
- Wake is due to three principal causes:
    1. The frictional drag of the hull causes a following current towards the stern.
    2. The streamline flow past the hull causes an increased pressure around the stern. In this region the relative velocity of the water past the hull will be less than the ship's speed.
    3. The ship forms a wave pattern on the surface of the water, and the water particles in the crests have a forward velocity due to the orbital motion, while in the troughs the orbital velocity is sternward. This wake will be positive or negative according to whether there is a crest or a trough of the wave in the vicinity of the propeller.
- **Thrust deduction**: The propeller close to the hull can induce a low pressure on the hull which increases its drag. The trust must be higher to overcome **the additional drag**. The thrust deduction coefficient is defined as:
$$ t = \frac{T-R}{R} $$
where R is the total ship resistance and T is the propeller thrust.

Here are the non-dimensional characterization of the propeller performance.
- Advance coefficient:
$$ J = \frac{V_A}{nD} $$
- Thrust coefficient:
$$ K_T = T/(\rho n^2D^4) $$
- Torque coefficient:
$$ K_Q = Q/(\rho n^2D^5) $$
- Propeller efficiency:
$$ \eta_0 = \frac{T V_A}{2\pi nQ} $$
- Propulsive efficiency:
$$ \eta_t = \frac{R V}{2\pi nQ} \approx \frac{1-t}{1-w} \eta_0$$

Because of the wake, the propulsive efficiency for a propeller can be greater than 1.0. It means the propeller can reduce the ship resistance by taking advantage of its wake.

<img src="https://quantapublication.files.wordpress.com/2011/04/3.jpg" width="50%" height="50%">

When the local absolute pressure is less than local vapor pressure, **cavitation** occurs, generally on the suction side. The water can boil in a lower pressure (than atomstpheric pressure) condition even though the temperature is lower than $100^{\circ} C$.
- Cavitation number based on inflow velocity:
$$\sigma_v = \frac{P - P_{vap}}{0.5\rho A V_A^2} $$
- Cavitation number based on propeller tip velocity:
$$\sigma_{ND} = \frac{P - P_{vap}}{\rho N^2 D^2} $$

Cavitation causes a great deal of noise, damage to components, vibrations, and a loss of efficiency. The pitting caused by the collapse of cavities produces great wear on components. Highly localized collapses of cavities can erode metals over time.

<img src="https://teamuvdotorg1.files.wordpress.com/2014/10/cavitation.jpg" width="85%" height="85%">

<img src="https://thumbs.gfycat.com/ImperturbableBlaringBettong-size_restricted.gif" width="85%" height="85%">


## Reference
[1] [Wikipedia - Propeller](https://en.wikipedia.org/wiki/Propeller)
[2] [Wikipedia - Cavitation](https://en.wikipedia.org/wiki/Propeller)
[3] [MIT 2.016 Hydrodynamics](http://web.mit.edu/2.016/www/handouts/2005Reading10.pdf)
[4] [An introduction to propeller cavitation](https://www.iims.org.uk/introduction-propeller-cavitation/)
[5] [Principles of Naval Architecture Volume II: Resistance, Propulsion and Vibration](http://opac.vimaru.edu.vn/edata/EBook/Principles%20of%20Naval%20achitecture%202.pdf) 






