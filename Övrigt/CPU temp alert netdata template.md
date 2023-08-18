```conf

template: core_1
alarm: core_1
on: sensors.coretemp-isa-0000_temperature
lookup: average -4m unaligned of Core 0
hosts   : *
units   : Celsius
every   : 10s
warn: $this > 55
crit: $this > 80
info    : CPU temperature alert
to      : sysadmin

template: core_2
alarm: core_2
on: sensors.coretemp-isa-0000_temperature
lookup: average -4m unaligned of Core 1
hosts   : *
units   : Celsius
every   : 10s
warn: $this > 55
crit: $this > 80
info    : CPU temperature alert
to      : sysadmin

template: core_3
alarm: core_3
on: sensors.coretemp-isa-0000_temperature
lookup: average -4m unaligned of Core 2
hosts   : *
units   : Celsius
every   : 10s
warn: $this > 55
crit: $this > 80
info    : CPU temperature alert
to      : sysadmin

template: core_4
alarm: core_4
on: sensors.coretemp-isa-0000_temperature
lookup: average -4m unaligned of Core 3
hosts   : *
units   : Celsius
every   : 10s
warn: $this > 55
crit: $this > 80
info    : CPU temperature alert
to      : sysadmin

```