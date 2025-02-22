## Zabbix MTR Template

##### (designed to motinor network jitter and traceroute changes via zabbix triggers :)

This template inspired by https://github.com/brunosantos-shell/mtr_json_zabbix

in contrast to mtr_json_zabbix (using zabbix-agent)
this template based upon external scripts feature

REF: https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/external


REF2: https://www.zabbix.com/documentation/current/en/manual/appendix/command_execution


ADDIT: https://www.zabbix.com/documentation/7.2/en/manual/config/items/preprocessing/jsonpath_functionality

MTR homepage: https://www.bitwizard.nl/mtr/


##### HOWTO:
1. install MTR (i.e. ```sudo dnf install mtr```)
2. check location of external scripts: ``` sudo cat /etc/zabbix/zabbix_server.conf | grep external ```
3. create external script:


```echo '#!/usr/bin/env bash' | sudo tee -a /usr/lib/zabbix/externalscripts/mtr.sh```


```echo '/sbin/mtr -c $1 -j -n -o "SRDLNBAWJMX" $2' | sudo tee -a /usr/lib/zabbix/externalscripts/mtr.sh ```


3. make executable: ```chmod +x /usr/lib/zabbix/externalscripts/mtr.sh```


4. test script output:

```/usr/lib/zabbix/externalscripts/mtr.sh 3 1.1.1.1```

5. import template "Zabbix MTR.yaml", add host (i.e. 1.1.1.1) and imported template to this host



---

JSON Path tested at: https://jsonpath.com/


example json:
```{
  "report": {
    "mtr": {
      "src": "localhost.localdomain",
      "dst": "1.1.1.1",
      "tos": 0,
      "tests": 3,
      "psize": "64",
      "bitpattern": "0x00"
    },
    "hubs": [
      {
        "count": 1,
        "host": "10.0.2.2",
        "Snt": 3,
        "Rcv": 3,
        "Drop": 0,
        "Loss%": 0.0,
        "Last": 0.255,
        "Best": 0.255,
        "Avg": 0.477,
        "Wrst": 0.664,
        "Jttr": 0.258,
        "Javg": 0.136,
        "Jmax": 0.258
      },
      {
        "count": 2,
        "host": "192.168.1.1",
        "Snt": 3,
        "Rcv": 3,
        "Drop": 0,
        "Loss%": 0.0,
        "Last": 1.607,
        "Best": 1.549,
        "Avg": 1.791,
        "Wrst": 2.219,
        "Jttr": 0.612,
        "Javg": 0.427,
        "Jmax": 0.67
      },
      {
        "count": 3,
        "host": "???",
        "Snt": 3,
        "Rcv": 0,
        "Drop": 3,
        "Loss%": 100.0,
        "Last": 0.0,
        "Best": 0.0,
        "Avg": 0.0,
        "Wrst": 0.0,
        "Jttr": 0.0,
        "Javg": 0.0,
        "Jmax": 0.0
      },
      {
        "count": 9,
        "host": "162.158.100.7",
        "Snt": 3,
        "Rcv": 3,
        "Drop": 0,
        "Loss%": 0.0,
        "Last": 18.442,
        "Best": 11.475,
        "Avg": 13.822,
        "Wrst": 18.442,
        "Jttr": 6.89,
        "Javg": 2.322,
        "Jmax": 6.89
      },
      {
        "count": 10,
        "host": "1.1.1.1",
        "Snt": 3,
        "Rcv": 3,
        "Drop": 0,
        "Loss%": 0.0,
        "Last": 11.245,
        "Best": 10.207,
        "Avg": 10.799,
        "Wrst": 11.245,
        "Jttr": 1.038,
        "Javg": 0.592,
        "Jmax": 1.038
      }
    ]
  }
}
```

```$.report.hubs[-1:]["Jttr"]``` results: ```[ 1.038 ]```

where "-1" - its "last"


altenatively can be specified ip ```$.report.hubs[?(@.host=='1.1.1.1')].Javg ```
