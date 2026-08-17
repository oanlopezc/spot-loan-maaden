[← Back to contents](../README.md)

# 5. Payloads and the GXP

What's mounted, and how to attach your Emesent equipment.

## What's on the robot

The robot ships to you with the **Spot EAP** (Enhanced Autonomy Payload):

| component | what it is |
|---|---|
| **VLP-16** | Velodyne 16-beam lidar |
| **Spot CORE** | payload computer. See [Spot CORE](04-spot-core.md) |
| **Spot GXP** | General Expansion Payload, power and ethernet breakout |
| 4× rail mount kit | for attaching payloads to the body rails |

You are welcome to unmount any of this, or use it partially, for example the
GXP alone. **Please reassemble it as delivered before the return.** Photograph
the arrangement before you take it apart; it makes reassembly much easier.

## The GXP, what you'll use for Emesent

The GXP is the piece that matters for integrating third-party hardware. It
breaks Spot's main payload port out into standard connectors.

| | |
|---|---|
| Dimensions | 50 × 192 × 67 mm (L×W×H) |
| Mass | 430 g |
| Power out | 5 V, 12 V, 24 V regulated |
| Power budget | 12 V / 24 V at **150 W**; 5 V at **10 W** |
| Data | RJ45 standard ethernet |
| Mounting | directly into the body rails; fits under the Spot Arm |
| Safety | supports hardware motor inhibition and motor power interlock |

**Check your power budget before connecting anything.** 150 W on the 12/24 V
rail is the hard ceiling, and it is shared. A Hovermap plus the VLP-16 plus the
CORE may not all fit within it. Worth totting up beforehand rather than
discovering it as a brownout mid-mission.

> **Two things that catch people out**, both explained in
> [The GXP in depth](05b-gxp-in-depth.md):
>
> 1. Four interlock loops in the payload connector must stay continuous. Break
>    one and motor power cuts instantly, so the robot sits down and the symptom
>    looks like a robot fault rather than a wiring mistake.
> 2. Pin 7 carries a 1 Hz Pulse Per Second signal at 5 ppm. If you are running
>    SLAM, that is your hardware time sync, and it is easy to miss.

**Read [The GXP in depth](05b-gxp-in-depth.md) before designing a harness or
mounting the Emesent equipment.**

## Registering a payload

Spot needs to know about a payload's mass and where it sits, or its balance and
its self-protection limits will be wrong. This is not optional for anything
heavy.

- [Payload Developer Guide](https://dev.bostondynamics.com/docs/payload/README.html)
- [Configuration Requirements](https://dev.bostondynamics.com/docs/payload/payload_configuration_requirements.html)
- [Electrical Interface](https://dev.bostondynamics.com/docs/payload/robot_electrical_interface.html)
- [Spot GXP support page](https://support.bostondynamics.com/s/article/Spot-General-Expansion-Payload-GXP-72066)
- [Spot GXP datasheet (PDF)](https://www.bostondynamics.com/wp-content/uploads/2023/05/spot-gxp.pdf)

Note the developer site defaults to the 5.1.4 documentation while this robot
runs 4.1.1. The payload interface is stable across those versions, but see
[Useful links](08-links.md) if you need the exact 4.1.1 pages.

## The VLP-16

The lidar streams UDP on **port 2368**, the Velodyne default. If you want the
data on your laptop rather than the CORE, the simplest approach is to forward
those packets from the CORE. We had a small script doing exactly that, and
we're happy to share the approach if useful.

VeloView is installed on the CORE for visualising the stream.

## Sharing what you learn

If you get the Emesent equipment working, we'd genuinely appreciate hearing
how, either as you go or once it's set up. We're likely to want the same
integration later, and we'd rather learn from you than repeat the work.

---

[← Spot CORE](04-spot-core.md) · [Back to contents](../README.md) · [Next: The GXP in depth →](05b-gxp-in-depth.md)
