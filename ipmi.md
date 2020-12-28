# Note on IPMI Config

This helped to lower the fan rpm threshold - something SuperMicro IPMI web does not allowed to do.
https://calvin.me/quick-how-to-decrease-ipmi-fan-threshold

```
ipmitool -I lan -U ADMIN -H 10.0.0.4 sensor thresh FAN1 lower 150 225 300
```

More:
https://quickpacket.com/billing/knowledgebase/20/Supermicro-Server-Remote-Access-via-IPMItool.html
