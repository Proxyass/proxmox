## Api Proxmox
## Using Php , easy.

### Code ,
```PHP
// Exmaple by using form.
<?php
Here for your admin panel.
session_start();
$id = $_SESSION['email'];
if(!preg_match($id)){
    header('Location : index.php');
    // Plus en ban la personne par ip et par MAC
    // Ou on fait pop la flash alert + redirection + ban.

}


?>
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
<form action="app_container.php" method="post">
  <select name="os" size="1">
<option>local:templates/debian-8.0-standard_6.0-4_amd64.tar.gz
<option>local:templates/debian-8.6-standard_6.0-4_amd64.tar.gz
<option>local:templates/centos-7.0-standard_6.0-4_amd64.tar.gz

</select>
  <input type="number" name="vmid">
  <input type="number" name="cpus">
  <input type="description" name="description">
  <input type="number" name="disk">
  <input type="text" name="hostname">
  <input type="number" name="memory" placeholder="ram">
  <input type="text" name="nameserver">
    <input type="submit" name="submit">



</body>
</html>
```
