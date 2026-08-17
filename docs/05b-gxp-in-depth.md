[← Back to contents](../README.md)

# 5b. The GXP in depth

Background for integrating third-party hardware such as the Emesent Hovermap.
The previous page is the quick reference; this one explains why the GXP behaves
the way it does, and covers the two things people usually discover the hard
way.

## What the GXP actually is

Spot has two payload ports on its back. Each is a **DB25 male connector**
carrying raw, unregulated battery voltage: **35 to 59 V** depending on state of
charge, with excursions to a clamped 72 V. That is not a friendly thing to plug
a sensor into.

The GXP is a **regulator and breakout board**. It converts that hostile DB25
into connectors you can actually design against:

| the GXP gives you | |
|---|---|
| Regulated power | 5 V, 12 V, 24 V |
| Power budget | 150 W on 12/24 V, 10 W on 5 V |
| Data | RJ45 standard ethernet |
| Size and mass | 50 × 192 × 67 mm, 430 g |
| Mounting | directly into the body rails, fits under the Spot Arm |

That is the whole value proposition. Without it you are building a custom
harness against a DB25 and doing your own DC to DC conversion across a 24 V
input swing.

## What the raw port underneath provides

Worth knowing, because it explains where the GXP's limits come from.

| | |
|---|---|
| Power pins | 12, 13, 24, 25. Grounds on 9, 10, 21, 22 |
| Current per pin | 3 A |
| Power per port | 150 W, with 150 µF bulk capacitance allowed |
| Combined limit | a resettable limiter cuts power above 9 to 13 A across **both** ports |
| Ethernet | 1000Base-T on pins 1 to 4 and 14 to 17 |
| **Pin 7** | **Pulse Per Second, 1 Hz at 5 ppm**, referenced to payload ground |

### The PPS pin is worth your attention

If you are running SLAM, timestamp alignment matters more than almost anything
else. Pin 7 gives you a hardware 1 Hz pulse accurate to 5 ppm, which lets you
discipline the payload's clock against the robot's rather than relying on NTP
over ethernet.

This matters if you ever want to fuse the Hovermap trajectory with Spot's own
odometry. Sharing a hardware time base is the difference between a clean result
and a smeared one. Most people do not notice this pin exists until after they
have collected a dataset they cannot align.

## The safety interlock, which you must not break

Four pin pairs have to stay electrically continuous for the robot to run:

| pins | loop |
|---|---|
| 5 ↔ 18 | payload power interlock |
| 6 ↔ 19 | motor power interlock |
| 8 ↔ 20 | additional safety loop |
| 11 ↔ 23 | additional safety loop |

**Break any of them and motor power cuts immediately. The robot sits down.**

This is deliberate. It is how a payload can hardware-inhibit the robot, and how
the robot notices a payload that has been damaged or has fallen off.

It is also the strongest practical reason to go through the GXP rather than a
hand-built cable. The GXP supports motor inhibition and the power interlock,
meaning it passes these loops through correctly. A homemade harness that leaves
them open produces a robot that refuses to stand, and the symptom looks like a
robot fault rather than a wiring mistake. People lose days to this.

## Power budget: do the arithmetic first

- **150 W per port**, and the GXP's 150 W **is** that port's budget rather than
  an addition to it.
- There are two ports, but the **9 to 13 A combined limit** means you cannot
  treat 300 W as freely available.
- **The 5 V rail is only 10 W.** Easy to overlook when powering something small
  from it.

The question to answer on paper before picking up a screwdriver: does the
Hovermap draw, plus anything else sharing its port, stay under 150 W?

**Check which port each payload occupies.** If the Spot CORE and VLP-16 sit on
one port and the GXP has the other to itself, you have room. If they share, you
may not. That is a five minute inspection which prevents a brownout in the
middle of a survey line.

## Mass, and why the ceiling is not the real constraint

Spot supports **14 kg of total payload**. The limit that actually causes
trouble is placement, not weight: Boston Dynamics asks that the combined
centre of mass lie **between the front and rear hips**.

Get this wrong and the robot does not refuse to work. It walks, badly. You see
degraded gait, worse behaviour on slopes and stairs, and faults triggering
earlier than they should. Mounting a sensor out on the front or rear overhang
is a very easy way to push the centre of mass outside that window while still
being comfortably under 14 kg.

You also need to **register the payload** so the robot knows its mass and where
it sits. An unregistered heavy payload leaves Spot's balance controller and its
self-protection limits working from wrong numbers.

## Checklist before mounting the Emesent equipment

1. Confirm **which port** the GXP is on, and what shares it.
2. Add up the **power draw** on that port against the 150 W ceiling.
3. Decide **where it mounts** relative to the hips, not just whether the mass fits.
4. **Register the payload** with its real mass and position.
5. Decide whether you want **PPS time sync**, and wire for it if so.
6. Verify the **interlock loops** are intact before wondering why the robot will not stand.

## Sources and one caveat

- [Electrical Interface](https://dev.bostondynamics.com/docs/payload/robot_electrical_interface.html), for the DB25 pinout, power limits, PPS, and interlock loops
- [Configuration Requirements](https://dev.bostondynamics.com/docs/payload/payload_configuration_requirements.html), for the 14 kg capacity and centre of mass guidance
- [Payload Developer Guide](https://dev.bostondynamics.com/docs/payload/README.html)
- [Spot GXP support page](https://support.bostondynamics.com/s/article/Spot-General-Expansion-Payload-GXP-72066)
- [Spot GXP datasheet, PDF](https://www.bostondynamics.com/wp-content/uploads/2023/05/spot-gxp.pdf)

**Caveat.** The GXP figures above (dimensions, regulated voltages, the 150 W
and 10 W split) come from Boston Dynamics' published specification summary. We
could not machine-read the datasheet PDF, so connector-level detail on the
GXP's own outputs is unverified: specifically how many 12 V versus 24 V
connectors it presents, and of what type. Confirm that physically before
designing a harness around it. The DB25 pinout, power limits, PPS, and
interlock details are from the developer documentation and are reliable.

Note also that the developer site serves the 5.1.4 documentation while this
robot runs 4.1.1. The payload electrical interface is stable across those
versions, but see [Useful links](08-links.md) if you need the exact 4.1.1 pages.

---

[← Payloads and the GXP](05-payloads-gxp.md) · [Back to contents](../README.md) · [Next: Checking the lidar →](05c-lidar-example.md)
