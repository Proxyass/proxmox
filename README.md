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
### Génération of CT by the form
```PHP
<?php
/*
 * @autor Ermal Ahmedi
 * @created for , creat CT in Proxmox via form
 * @Php 5.4 , 7
 */

require("pve2_api.class.php");

if(isset($_POST['submit'])) {
    // On récupère les valeurs envoie sur le formulaire.

    // Choix du serveur
    $hostname = $_POST['hostname'];
    $realm = $_POST['realm'];
    $password = $_POST['password'];
    $templates = $_POST['templates'];
    $mail = $_POST['mail'];

# You can try/catch exception handle the constructor here if you want.
    $pve2 = new PVE2_API($hostname, "root", $realm, $password);
# realm above can be pve, pam or any other realm available.

    /* Optional - enable debugging. It print()'s any results currently */
// $pve2->set_debug(true);

    if ($pve2->login()) {

        # Get first node name.
        $nodes = $pve2->get_node_list();
        $first_node = $nodes[0];
        unset($nodes);

        # Create a VZ container on the first node in the cluster.
        $new_container_settings = array();
        $new_container_settings['ostemplate'] = "local:vztmpl/debian-6.0-standard_6.0-4_amd64.tar.gz";
        $new_container_settings['vmid'] = $_POST['vmid'];
        $new_container_settings['cpus'] = $_POST['cpus'];;
        $new_container_settings['description'] = $_POST['description'];;
        $new_container_settings['disk'] = $_POST['disk'];;
        $new_container_settings['hostname'] = $_POST['hostname'];;
        $new_container_settings['memory'] = $_POST['memory'];;
        $new_container_settings['nameserver'] = $_POST['nameserver'];;

        // print_r($new_container_settings);
        print("---------------------------\n");

        print_r($pve2->post("/nodes/" . $first_node . "/openvz", $new_container_settings));
        print("\n\n");
        $valide = 1;
        $bdd = new PDO('mysql:host=localhost;dbname=vps', 'root', 'password');
        while ($valide == 1) {

            $bdd->query("INSERT INTO `vpsok` (`ostemplate`, `vmid`, `cpus`,`disk`,`hostname`,`memory`,`nameserver`,`date`)) VALUES ('" . $new_container_settings['vmid'] . "','" . $new_container_settings['cpus'] . "',NOW())");

        }

        //Ici on va envoyer un email , à nous meme est à la personne pour lui dire que son vps est en cours de réalisation


    } else {
        print("Login to Proxmox Host failed.\n");
        exit;
    }
}```
#Require The Proxmox Api PHP LIB Is on the github repesotory
