# My Scripts

## Custom PingSweep with bash
```bash
#!/bin/bash 

for i in {1..254}; do
    if ping -c 1 10.0.2.$i > /dev/null 2>&1; then
        echo 10.0.2.$i
    fi
done 
```

## JS Extract
```bash
#!/bin/bash

if [[ $# -ne 1 ]]; then
    echo "Usage: ./js-extract.sh file.txt"
    exit 2
else
    cat $1 | grep "[^\/]*\.js" -o | sort -ud
fi 
```
