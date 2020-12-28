# Note on IPMI Config

https://calvin.me/quick-how-to-decrease-ipmi-fan-threshold
This helped!

```
ipmitool -I lan -U ADMIN -H 10.0.0.4 sensor thresh FAN1 lower 150 225 300
```
