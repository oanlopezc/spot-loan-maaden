[← Back to contents](../README.md)

# 3. Setting up your computer

For controlling Spot from a laptop with Python, rather than the controller.

> ## ⚠ Read this before you `pip install`
>
> **This robot runs software version 4.1.1. Install SDK version 4.1.1, not the
> latest.**
>
> The current public SDK is 5.x. A 5.x client talking to a 4.1.1 robot will
> fail in confusing ways: some calls work, others raise version errors, and the
> messages rarely say "version mismatch". `pip install bosdyn-client` with no
> version gives you 5.x. Pin it.
>
> The same applies to documentation. `dev.bostondynamics.com` now serves the
> **5.1.4** docs by default. See [Useful links](08-links.md) for how to get the
> 4.1.1 versions.

## Install

Tested on Ubuntu. A virtual environment of some kind is strongly recommended.
The example below uses conda, but `venv` or `uv` work equally well.

```bash
conda create --name spot python=3.10
conda activate spot

# Pinned to match the robot. Do not drop the ==4.1.1
python3 -m pip install \
    bosdyn-client==4.1.1 \
    bosdyn-mission==4.1.1 \
    bosdyn-choreography-client==4.1.1 \
    bosdyn-api==4.1.1 \
    bosdyn-core==4.1.1

# Commonly needed alongside
python3 -m pip install pytz matplotlib pillow psutil PyYAML scipy
```

If you'll work with the VLP-16 point clouds:

```bash
python3 -m pip install open3d==0.18.0
```

## Credentials

The SDK reads these environment variables directly, so you never need to put a
password in your source:

```bash
export BOSDYN_CLIENT_USERNAME=maaden
export BOSDYN_CLIENT_PASSWORD='...'      # shared in person
```

Add them to your `~/.bashrc` if you'd rather not retype them.

> Please don't hardcode the password into scripts. We learned this the hard
> way. The SDK logs the full authentication request at `DEBUG` level, so a
> debug log becomes a plaintext password file. If you enable DEBUG logging,
> don't keep the logs.

## Connect

Your laptop must be on the robot's WiFi. The robot is at **`192.168.80.3`**.

```bash
# Quickest check that everything works
python3 -m bosdyn.client 192.168.80.3 id
```

That prints the robot's software version and serial. If it works, your install
and credentials are correct.

## Get the examples

The SDK repository has a large set of worked examples. Check out the tag that
matches the robot:

```bash
git clone https://github.com/boston-dynamics/spot-sdk.git
cd spot-sdk
git checkout v4.1.1
```

Good starting points inside it:

- `python/examples/hello_spot`, connect, stand, sit
- `python/examples/wasd`, drive from the keyboard, well commented
- `python/examples/get_image`, pull camera frames
- `python/examples/estop`, **worth reading**, explains the E-Stop model

## The E-Stop, in software

Any program that moves the robot needs an E-Stop endpoint, and the robot will
refuse to move without one. This is deliberate. Keep the physical controller in
reach even when driving from a laptop. A script that hangs is much less
alarming when someone is holding the stop button.

## Reference

- [Python QuickStart](https://dev.bostondynamics.com/docs/python/quickstart.html)
- [Understanding Spot Programming](https://github.com/boston-dynamics/spot-sdk/blob/master/docs/python/understanding_spot_programming.md)
- [Python examples index](https://dev.bostondynamics.com/python/examples/README.html)

---

[← Using the controller](02-controller.md) · [Back to contents](../README.md) · [Next: Spot CORE →](04-spot-core.md)
