---
title:  "Frequency"
date:   2021-04-11 14:03:53
categories: [NoobCTF-0x1]
tags: [ctf,crypto]
---

```
Elliot captured something, while noob called his friend

Flag format: noob{FLAG}

Author: D3v1LaL
```
we were given `Cipher.txt`

```
1209-770 1209-770 1477-697 1477-697 1336-770 
1336-770 1336-770 1336-770 1336-770 1336-770 
1477-770 1477-770 1477-770 1477-697 1336-852 
1477-770 1477-697 1477-697 1477-697
```



decoding it into its respective DTMF KeyPad frequency chart
![image tooltip here](/images/DTMF.png)

```python

cipher = "1209-770 1209-770 1477-697 1477-697 1336-770 1336-770 1336-770 1336-770 1336-770 1336-770 1477-770 1477-770 1477-770 1477-697 1336-852 1477-770 1477-697 1477-697 1477-697".split(" ")

cipher_pair = [a.split("-") for a in cipher]

dtmf = {}
dtmf[1209] = {697:1,770:4,852:7}
dtmf[1336] = {697:2,770:5,852:8,941:0}
dtmf[1477] = {697:3,770:6,852:9}
codes = [dtmf[int(a[0])][int(a[1])] for a in cipher_pair ]
codes = ''.join(map(str,codes))

```

gets us `4433555555666386333`

which using a T9 keyboard we can change into -> `HELLODTMF`

giving the flag `noob{HELLODTMF}`