[← Back to contents](../README.md)

# 4. Connecting to Spot CORE

Spot CORE is the payload computer mounted on the robot. Use it when code needs
to run onboard — continuous logging, sensor capture, or anything that must keep
working when your laptop walks away.

## Your account

| | |
|---|---|
| Username | `maaden` |
| Password | shared in person |
| Rights | full `sudo` |

## Connecting

Your laptop must be on the robot's WiFi. SSH goes through the robot, which
forwards port **20022** to the CORE:

```bash
ssh -p 20022 maaden@192.168.80.3
```

To save typing, add this to your `~/.ssh/config`:

```
Host spot-core
  HostName 192.168.80.3
  Port     20022
  User     maaden
```

then simply:

```bash
ssh spot-core
```

### Passwordless login

Once connected the first time, install your SSH key so you stop typing the
password:

```bash
ssh-copy-id -p 20022 maaden@192.168.80.3
```

If that fails with a permission error on `~/.ssh`, fix the ownership once:

```bash
ssh -t -p 20022 maaden@192.168.80.3 'sudo chown -R maaden:maaden ~/.ssh && sudo chmod 700 ~/.ssh'
```

*(This caught us out too — the directory can be created root-owned.)*

## What's on it

| | |
|---|---|
| OS | Ubuntu 18.04.5 LTS |
| Kernel | 4.15 |
| System Python | 3.6 and 2.7 |
| Disk | 468 GB, mostly free |

**Ubuntu 18.04 is end-of-life**, so `apt` repositories are limited and many
modern Python wheels won't install. If you need a newer Python, install a
standalone build in your home directory rather than replacing the system one —
replacing it breaks the Boston Dynamics services.

## Copying files

```bash
scp -P 20022 myscript.py maaden@192.168.80.3:~/     # to the robot
scp -P 20022 maaden@192.168.80.3:~/data.csv .       # from the robot
```

Or with the config alias: `scp myscript.py spot-core:~/`

## A request

Please keep your work inside `/home/maaden`. The `spot` account holds our
project files — you have `sudo` and could read them, but we'd appreciate you
leaving that account alone.

Likewise, please don't change the CORE's network configuration. It's reachable
only through the robot's port forward, and a change there disconnects everyone.

## Wired alternative

If you connect a cable to the robot's rear ethernet port instead of using WiFi,
the CORE is reachable at `10.0.0.3` on the same port 20022. WiFi is easier for
most work; the cable is useful for moving large datasets.

---

[← Computer setup](03-computer-setup.md) · [Back to contents](../README.md) · [Next: Payloads and GXP →](05-payloads-gxp.md)
