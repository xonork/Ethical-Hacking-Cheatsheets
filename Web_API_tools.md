# Web API Tools
- [Kiterunner](https://github.com/assetnote/kiterunner#installation): Endpoint Brutforcing
  - [routes-large.kite](https://wordlists-cdn.assetnote.io/data/kiterunner/routes-large.kite.tar.gz)
  - API Scanning:
    ```
    # single target
    kr scan https://target.com:8443/ -w routes.kite -A=apiroutes-210228:20000 -x 10 --ignore-length=34

    # single target, but you want to try http and https
    kr scan target.com -w routes.kite -A=apiroutes-210228:20000 -x 10 --ignore-length=34

    # a list of targets
    kr scan targets.txt -w routes.kite -A=apiroutes-210228:20000 -x 10 --ignore-length=34
    ```
- [Arjun](https://github.com/s0md3v/Arjun): HTTP Parameter Discovery
  ```
   arjun -u https://api.example.com/endpoint -m POST
   ```-m GET
