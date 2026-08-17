[← Back to contents](../README.md)

# 7. Current versions

**As delivered, 17 August 2026.** Please return the robot on these versions, or
talk to us first.

## Software

| component | version | notes |
|---|---|---|
| **Spot (robot)** | **4.1.1** | the number that matters — pin your SDK to it |
| **Spot CORE** | **3.0** (Legacy) | Ubuntu 18.04.5 LTS, kernel 4.15 |
| **Controller / tablet** | **4.1.1** | matched to the robot |

*No SpotCAM is fitted for this loan.*

## Hardware

| | |
|---|---|
| KAUST Equipment ID | 14290358 |
| KAUST Asset | 3011119 |
| Payload | Spot EAP — VLP-16, Spot CORE, Spot GXP |
| Mounting | 4× rail mount kit |
| Batteries | 2 |
| Charger | 1, with power / Spot / battery cables |
| Controller | 1 |

## Network

| | |
|---|---|
| Robot over its own WiFi AP | `192.168.80.3` |
| Robot over rear ethernet | `10.0.0.3` |
| Spot CORE via SSH | port `20022` on the addresses above |
| VLP-16 lidar UDP | port `2368` |

SSID and WiFi password are on the sheet shared in person.

## About changing firmware

You're free to change versions if your work needs it — we only ask that you
**tell us first**, for two practical reasons:

1. **Downgrades aren't always reversible.** Getting back to 4.1.1 afterwards
   may not be straightforward, so "we'll put it back at the end" is not a
   guarantee either of us can rely on.
2. **Our Spot CORE integration is tied to this combination.** Spot CORE Legacy
   3.0 is matched to robot 4.1.1. Moving the robot forward without the CORE can
   leave the payload computer unable to talk to it.

If you need a newer version, message us and we'll work out the safest route —
this is an offer of help, not an obstacle.

## Version mismatch symptoms

If someone reports odd SDK behaviour, check version alignment first. A 5.x
client against a 4.1.1 robot typically shows as: some calls succeeding while
others fail, unclear serialisation errors, or features documented online that
simply aren't present. See [Computer setup](03-computer-setup.md).

---

[← Charging](06-charging.md) · [Back to contents](../README.md) · [Next: Useful links →](08-links.md)
