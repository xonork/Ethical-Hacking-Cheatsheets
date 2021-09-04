# My Scripts

## Custom PingSweep with bash
```bash
#!/bin/bash 

for i in {1..254}
do result=$(ping -c 1 10.0.2.$i)
if echo $result | grep -q "Unreachable"; then
    :
elif echo $result | grep -q "0 received";then
    :
else
    echo $result | awk '{print $2}'
fi
done 
```
