[← Back to contents](../README.md)

# 5c. Checking the lidar: the velodyne_client example

A working example for confirming the VLP-16 is alive and streaming. It connects
to the robot's point cloud service, polls it, and plots the returns live.

Use it as a first test before trusting the lidar in a longer capture, and as a
starting point if you want to pull point clouds yourself.

The code lives in [`examples/velodyne_client/`](../examples/velodyne_client/).

## Important: this is our working copy, not the stock example

It is the Boston Dynamics SDK example with **one deliberate change**, and that
change affects how you install it:

```python
matplotlib.use('TkAgg')      # upstream ships 'Qt5agg'
```

Consequences:

- **You do not need PyQt5**, even though `requirements.txt` lists it and the
  Boston Dynamics README in the same folder tells you to install it. That
  README is the original and we left it untouched; where it disagrees with this
  page, this page is the one that matches the code.
- **You do need Tkinter.** It ships with most Python installations. On Ubuntu,
  if it is missing: `sudo apt install python3-tk`

We switched to TkAgg because the Qt backend gave us trouble. This version is
known to work. Please do not change it back without a reason.

## Installing

Do **not** run `pip install -r requirements.txt` directly. The first line of
that file is `-f ../../../prebuilt`, a relative path that only resolves inside
the Spot SDK source tree. Outside it, the install fails or reaches for the
wrong place. We left the file as-is so the example stays a faithful copy.

Install the dependencies explicitly instead:

```bash
# Pinned to the robot's version. See docs/03-computer-setup.md for why.
python3 -m pip install bosdyn-client==4.1.1
python3 -m pip install matplotlib numpy
```

Note that `requirements.txt` says `bosdyn-client >= 3.1`, which would happily
install 5.x. This robot runs **4.1.1**, so pin it.

If you already followed [Setting up your computer](03-computer-setup.md), you
have everything except possibly Tkinter.

## Running it

You need to be on the robot's WiFi, with credentials in your environment:

```bash
export BOSDYN_CLIENT_USERNAME=maaden
export BOSDYN_CLIENT_PASSWORD='...'      # shared in person

cd examples/velodyne_client
python3 client.py 192.168.80.3
```

A plot window opens and updates as point clouds arrive. Close the window to
stop.

## What you should see

A live top-down scatter of lidar returns around the robot, with its body drawn
in Spot yellow. Walk in front of the robot and you should appear in the plot.

If the plot is empty but the program does not error, the client is connected
but the lidar is returning nothing. That points at the lidar or its power,
rather than at your setup.

## If it does not work

| symptom | likely cause |
|---|---|
| `ModuleNotFoundError: tkinter` | install `python3-tk` |
| Connection refused or timeout | not on the robot's WiFi, or wrong IP |
| Authentication error | check the two environment variables above |
| Version or serialisation errors | wrong SDK version. Pin `bosdyn-client==4.1.1` |
| Service not found | the point cloud service is not running, so the lidar may be unpowered or unmounted |
| Window opens then closes instantly | matplotlib backend problem. Confirm the file still says `TkAgg` |

The lidar streams UDP on port **2368** if you want to check traffic
independently of the SDK.

## Licence

The contents of `examples/velodyne_client/` are Boston Dynamics SDK example
code, used under the Boston Dynamics Software Development Kit License
(20191101-BDSDK-SL). The copyright headers are intact and the original Boston
Dynamics README is included alongside.

---

[← The GXP in depth](05b-gxp-in-depth.md) · [Back to contents](../README.md) · [Next: Charging →](06-charging.md)
