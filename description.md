## MAVLink Router ##

MAVLink Router is a proxy software from Intel corporation, that exposes PX4's (could be any flight controller) serial port (UART) as a network socket (TCP port 5760) communication of MAVLink messages.

It is intereseting for several reasons:
- It solves the routing problem, which would otherwise need to be taken care by SDK or apps.
- You don't need a cable to calibration your drone, because MAVLink messages can be sent to calibration PC over the network.
- Accessing a network port easier than a serial port.
- Several software stacks (For eg: MAVROS and Dronecode SDK) can share access to PX4.

So, instead of sending MAVLink messages over a serial port, send it to the TCP port 5760 (configurable).

![mavlink_router_px4_devguide](https://user-images.githubusercontent.com/26615772/38861319-e8f33afe-424f-11e8-83bd-4fb209769bee.png)

There's one **master** endpoint that should be the flight stack (either on UART or UDP) and other components that can be on UDP or TCP endpoints. TCP endpoints are added automatically if the TCP server is enabled, allowing clients to simply connect to mavlink-router without changing its configuration.

MAVLink Router also listens TCP port 5760 (by default) for connections. Any application may become a client to the same to talk to flight stack.

To connect over UDP, you need to add an UDP endpoint in the MAVLink Router .conf file. By default, `mavlink-routerd` looks for a file `/etc/mavlink-router/main.conf`. File location can be overriden via `MAVLINK_ROUTER_CONF_FILE` environment variable, or via `-c`  switch when running `mavlink-routerd`. An example of conf file can be found on [examples/config.sample](https://github.com/intel/mavlink-router/blob/master/examples/config.sample).

```
# Following, an example of configuration file:
[General]
TcpServerPort=5760
ReportStats=false
Log=/var/lib/mavlink-router/

[UartEndpoint uart]
Device = /dev/ttyS1
Baud = 921600,460800

[UdpEndpoint wifi_offboard]
Mode = Normal
Address = 192.168.8.255
Port = 14540
```

For more information about building, installing etc, refer to [MAVLink Router repository](https://github.com/intel/mavlink-router).
