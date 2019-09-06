# Creer son site avec un raspberry


## Kesako ce tutorial

Cette serie de tutorial va vous apprendre à installer et héberger un site web sur votre raspberry pi personnelle. Nous utiliserons OVH pour acheter le nom de domain; let's encrypt pour obtenir le https et apache sql et wordpress pour créer le site.
* Sources:
1. **Article - Avantages de l’auto-hébergement (ref. 1):** https://serveuramoi.ovh/

## Le matériel nécessaire

- En premier lieu il vous faut évidemment un raspberry pi. J'ai acheter la [Raspberry pi 3B](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/)
- Il vous faudra une [micro sd](https://www.samsung.com/us/computing/memory-storage/memory-cards/microsdxc-evo-plus-memory-card-w--adapter-64gb--2017-model--mb-mc64ga-am/) rated class 10 or UHS class 1. La UHS class 1 est meilleur. 
- Il vous faudra aussi [une bonne alimentation](https://www.amazon.de/Aukru-transparent-Case-Stromversorgung-K%C3%BChlk%C3%B6rper-durchsichtig/dp/B01DDFFOYK/ref=pd_sbs_147_31?_encoding=UTF8&pd_rd_i=B01DDFFOYK&pd_rd_r=a86e7774-d2a2-4e8f-8448-71f5ae880bf4&pd_rd_w=G6Tiq&pd_rd_wg=IThQ6&pf_rd_p=74d946ea-18de-4443-bed6-d8837f922070&pf_rd_r=BA75FXG4NY694YA3RTTX&psc=1&refRID=BA75FXG4NY694YA3RTTX) sinon le raspberry ne fonctionnera parfaitement. Voici ce que j'ai acheté. Le point positif est que l'offre comprend un boitier. 
- Il est également nécessaire d'avoir un lecteur micro sd.
- L'utilisation d'un clavier usb est nécessaire.
- Une connexion internet est aussi nécessaire.
- Un ecran sur lequel vous pouvez connecter le raspberry en HDMI.

Il est vraiment important de prendre une bonne alimentation de 3V et non le chargeur de votre portable. Mes premiers essais ont été réalisés avec le chargeur de mon portable: au premier démarrage le raspberry boot normalement. Puis lors des boots suivants, j'avais systématiquement ma PI bloquée au rainbow screen...
La SD est aussi importante si la classe n'est pas 10 ou UHS classe 1, la PI risque de ne pas sauvegarder vos fichiers correctement... Facheux !

## Démarrer pour la premiere fois sa Raspberry

Dans cette partie du tutorial nous allons faire toutes les manipulations requisent afin de faire démarrer notre petite raspberry pour la première fois.

Nous allons d'abord télécharger l'image de l'[OS Raspian](https://www.raspberrypi.org/downloads/raspbian/). à l'heure où j'écris cette article la version "Buster" de Raspian est la plus récente. Je vous conseille de télécharger la version Lite de Raspian: Le Raspberry la supportera mieux et vous apprendrez à utiliser l'invité de commandes. Deux bons points!
Voici le lien vers le download: 

Pendant que le téléchargement de l'OS se déroule, nous nous rendons sur le site d'[ETCHER](https://www.balena.io/etcher/). Afin de télécharger Etcher qui nous permettra d'installer l'OS sur le raspberry.
Lorsque les deux téléchargements sont terminés, nous flashons la SD pour y installer l'OS. Tout ça avec l'outil Etcher. 

Image n`3: Etcher nous confirme que l'installation a eu lieu.

On peut maintenant insérer la SD dans la pi et l'allumer.

Saississez les identifiants suivants:
login: pi
password: rapsberry

N.B: Si votre clavier est francais(azerty et non qwerty il vous faudra écrire "rqspberry" au lieu de "raspberry" 
Bravo vous êtes maintenant loger dans votre pi.
Je vous conseille de l'explorer un peu: Familiarisez vous avec les commandes "ls"; "pwd"; "cd"; "sudo"; "nano"; "mv"; "cp";"sudo shutdown -h now"; "sudo halt". Et faites attention à la commande "rm -rf". 
Quoiqu'il arrive vous êtes maintenant capable d'installer et de réinstaller l'OS Raspian sur votre pi! Bravo.

Pensez à changer les parametres de du layout du clavier. pour cela changer le fichier /etc/default/keyboard:

`sudo nano /etc/default/keyboard`

Changez les lignes suivantes:

XKNMODEL="pc105"
XKBLAYOUT="us"

cliquez sur ctrl + o puis entrer et enfin ctrl + x

enfin faites la commande:

`sudo reboot`

PS: Si vous êtes bloqué au rainbow screen, la solution est généralement d'ajouter la ligne 'boot_delay=1' dans le fichier config.txt qui se trouve dans le dossier /boot de la sd. Vous devez faire cela depuis la machine qui vous a permis d'installer l'OS sur la SD. 

PPS: Pour voir le prompt, après s'être identifier, il faut parfois faire une des manipulations suivantes:
- clicker à de multiples reprises sur la touche "flêche vers le haut".
- Clicker sur ctrl + l: afin d'effacer les anciens prompt.
- taper la commande exit: afin de se déconnecter de la session pour se reconnecter plus tard.

Source:
1. **Login + password:** https://www.raspberrypi.org/forums/viewtopic.php?t=45072
1. **How to mount your image:** https://thepi.io/how-to-install-raspbian-on-the-raspberry-pi/
1. **Thread - Stuck on the Rainbow Screen:** https://www.raspberrypi.org/forums/viewtopic.php?t=55464
1. **Shutdown your rapsberry:** https://raspi.tv/2012/how-to-safely-shutdown-or-reboot-your-raspberry-pi
1. **XKBMOEL="pc104" xkblayout="US - KeyBoard layout:** https://www.raspberrypi.org/forums/viewtopic.php?t=5278
--------------------------------------------------------
if you are stuck at the rainbow screen you may need to add 'boot_delay=1' in the file config.txt i the /boot folder of your sd. 

to see the prompt:
- Click multiple times on arrow up!
- click ctrl + l: to clear the prompt
- command exit: to restart the session



## Connecter son PI à Internet

Dans cette partie nous allons faire en sorte que la PI soit connecté à Internet afin de télécharger des images de chats trop mignons.

C'est une manipulation finalement assez simple. Mais avant de la faire il faut absolument changer le mot de passe!
On lance donc la commande:
`sudo raspi-config`

On selectionne la première option "Change User Password". Et on suit les étapes du changement de mot de passe. Prenez un mot de passe très sécurisé, sinon la PI risque d'être hacké.

Puis pour valider que le nouveau mdp est activé on reboot la pi.

Après avoir changé votre mot de passe, vous pouvez vous connecter à internet. Il suffit de lancer la commande suivante:

`sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

Maintenant ajoutez à la fin du fichier les lignes suivantes:

`network={
	ssid="nom_de_la_box"
	psk="password"
}`

Puis taper ctrl+x et "Y".
Ensuite on lance la commande:

`sudo reboot`

Après le reboot, je vous conseille de tester si vous êtes connecté à internet. 

`sudo ping -c 5 www.google.com`

Cela vous permettra de ping google si vous avez un retour Bravo vous êtes connecté à internet. 

Ensuite installez vim, git, zsh et ohmyzsh.

Tout d'abord faites la commande suivante:

`sudo apt-get update && sudo apt-get upgrade`

Cette commande met à jour la liste des pacquet et met à jour les paquet qui ne sont pas à jour.

ensuite faites la commande:

`sudo apt-get install vim git zsh`

avec cette commande vous installez vim git et zsh.

Vim est un editeur de texte accessible dans la console et ultra performant.
Git est un software de version control.
zsh est un shell amélioré.

Il faut maintenant localiser zsh pour l'installer:

`whereis zsh`

puis on valide zsh comme le shell de base avec la commande suivante:

`sudo usermod -s /usr/bin/zsh $(whoami)`

maintenant ou reboot le pi:

`sudo reboot`

choississez l'option 2 pour installer la configuration recommandé
h
ensuite installez oh my zsh:

`$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

Bravo votre PI commence à être personnalisé et elle est maintenant connectée à internet.

Ressources:
1. **Thread - Change your default password:** https://www.raspberrypi.org/forums/viewtopic.php?t=193620
1. **How to param the wifi network didwork for me:** https://alcalyn.github.io/raspberry-connectee-telephone/
1. **Connect your Raspberry to the internet WIFI:** https://docs.dataplicity.com/docs/get-pi-connected-to-the-internet
1. **Install ZSH:** https://www.howtoforge.com/tutorial/how-to-setup-zsh-and-oh-my-zsh-on-linux/
1. **Article - How to Install ZSH Shell on Ubuntu 18.04 LTS (ref.3):** https://linuxhint.com/install_zsh_shell_ubuntu_1804/
1. **Article - Raspberry Pi Changing Default User Password and Creating Additional Accounts:** https://www.theurbanpenguin.com/raspberry-pi-changing-the-default-users-password-and-creating-addtional-accounts/

## avoir acces en local au Raspberry via ssh

### Qu'est ce que SSH?

SSH est un protocole qui vous permettra de vous connecter à distance à votre pi depuis une autre machine.

### Comment faire?

Il vous faut tout d'abord installer activer ssh sur votre pi.

Pour cela, il suffit de brancher la carte MicroSD de la Raspberry sur votre ordinateur, et de créer un fichier nommé ssh dans la partition boot de la carte MicroSD.

(photo de la partition boot avec le fichier boot).

Ensuite vous pouvez remettre votre MicroSD dans la Raspberry. Lors du reboot de la pi, SSH sera activé. 

Faites maintenant très attention, votre raspberry est maintenant accessible par n'importe qui sur internet. Il vous faut donc installer des password forts pour chaque compte utilisateur du pi.

Vous devez maintenant installer un client SSH sur votre PC Linux.

Veuillez lancer la commande:

sudo apt-get update && sudo apt-get install openssh-client

pour utiliser ssh, lancez la commande suivante:

`ssh utilisateur_du_pi@ip_local_ou_ip_fixe_ou_url`

Vous pouvez entrer le mdp de la pi. Bravo vous êtes maintenant connecté au pi depuis votre pc.

Votre premier connection sera en local. pour savoir quelle ip est attribuer par votre box au  raspberry vous pouvez aller recupérer l'information sur votre box.
Dans votre navigateur web, saississez l'url "192.168.1.1" aller dans les parametre et trouvez l'ip de votre pi.

(photo de l4ip)

dans mon cas ou a ssh pi@192.168.1.54 (puisque je suis en local)

Malheureusement tout n'est pas finit, en effet il vous faudra certainement activer le port forwarding de votre box.

Pour cela aller sur l'interface utilisateur de votre box en ecrivant l'adresse ip de votre box. Chez moi 192.168.1.1.

Là chercher le port forwarding, et activer le port 22. Pour pouvez activer le port 88 et 443 qui seront utiles pour rendre disponibles vos sites web sur internet.

(photo port forwarding)

(photo parametre autorisation)

n'oubliez pas d'activer le port forwarding.

Voila vous pouvez enfin utiliser le ssh.

Bravo!

ps: on verra la semaine prochaine comment fixer l'adresse ip que la box attribue à la pi. Cela est très utile notamment lorsque la box redémarre... En effet sans cette manipulation la box risque d'attribuer une autre ip local ce qui fera tout bugger...

1. **Article - gerer son Raspberry depuis son ordinateur avec openssh:** https://raspbian-france.fr/controlez-raspberry-pi-ssh-ordinateur/
1. **Article - SSH-Verbindung aufbauen und SSH verwenden:** https://jankarres.de/2014/02/raspberry-pi-ssh-verbindung-aufbauen-und-ssh-verwenden/
1. **Article - DynDNS einrichten:** https://jankarres.de/2012/11/raspberry-pi-dyndns-einrichten/
1. **Article - Portforwarding bei box und speedport:** https://jankarres.de/2013/01/raspberry-pi-portforwarding-bei-fritzbox-und-speedport/

1. **Article - Accéder de l’extérieur au flux http d’un Raspberry Pi(ref.6):** https://knowledge.parcours-performance.com/acceder-de-lexterieur-flux-http-dun-raspberry-pi/
1. **Website - info IP:** https://www.whatismyip.com/fr/


1. **Youtube - Ports bei einem o2 Router freischalten:** https://www.youtube.com/watch?v=Pvf-tlu7VgE
1. **mon ip publique:** https://www.monippublique.com/




## Avoir acces au PI via SSH depuis n'importe ou!

### Acheter un nom de domaine afin d'avoir acces partout a son raspberry

Vous devez aller sur www.ovh.com
puis acheter un nom de domain. Pendant la proccedure, n'ajoutez aucune option, choississez le service Gold. Pour l'hébergement choississez l'option "Je n'ai pas besoin d'hébergement Web avec mon nom de domaine."
Continuez jusqu'à valider la commande.

Ensuite allez dans l'onglet de votre domaine, et chercher la section "DynHost". Choississez l'option "Add a DynHost".

Dans la champ Subdomain inscrivez "raspberry" et dans le champ "Enter the current target IP. It will then be updated dynamically" entrer l'adresse dynamique de votre box.

pour trouvez celle ci, entrez la commande suivante sur votre pi:

`curl -4 ifconfig.co`

grace à votre adresse "raspberry.nom-de-domaine.extension-de-domaine" Vous pouvez maintenant vous connectez depuis n'importe où sur votre PI !

Malheureusement cela ne sera actif que pour un certain laps de temps... En effet les box internet des particuliers s'identifient au réseau internet avec des ip dynamiques. Et cette ip change tous les jours. Ainsi au prochain changement d'ip de votre box votre raspberry ne sera plus accessible via l'adresse: "raspberry.nom-de-domaine.extension-de-domaine".

Heureusement il a une solution ! On peut demander à la PI d'informer ovh et mettre à jour l'url lorsque notre ip dynamique change.

Pour ce faire on va utiliser le [script de yann.me](https://yann.me/dynhost-ovh-pour-raspberry-pi-ou-une-machine-linux/)

Le script qui permettra de mettre à jour automatiquement votre adresse IP sur votre DynHost sera composé de 4 parties :

1. on récupère l’adresse IP configurée sur le DynHost
1. on récupère l’adresse IP courante
1. on les compare
1. on met à jour le DynHost s’il y a besoin


Avant de créer le script, installez dnsutils:

`sudo apt-get update`
`sudo apt-get install dnsutils`


Voici le script complet:

	#/bin/sh
	#
	# CONFIG
	#

	HOST="votre.url.dynhost"
	LOGIN="votre-login-dynhost"
	PASSWORD="votre-mot-de-passe"
	PATH_LOG=/var/log/dyndns
	#
	# GET IPs
	#
	HOST_IP=`dig +short $HOST`
	CURRENT_IP=`curl -4 ifconfig.co`
	#
	# LOG
	#
	echo > $PATH_LOG
	echo "Run dyndns" >> $PATH_LOG
	date >> $PATH_LOG
	echo "Current IP" >> $PATH_LOG
	echo "$CURRENT_IP" >> $PATH_LOG
	echo "Host IP" >> $PATH_LOG
	echo "$HOST_IP" >> $PATH_LOG
	#
	# DO THE WORK
	#
	if [ -z $CURRENT_IP ] || [ -z $HOST_IP ]
	then
	        echo "No IP retrieved" >> $PATH_LOG
	else
	        if [ "$HOST_IP" != "$CURRENT_IP" ]
	        then
	                echo "IP has changed" >> $PATH_LOG
	                RES=`curl --user "$LOGIN:$PASSWORD" "https://www.ovh.com/nic/update?	system=dyndns&hostname=$HOST&myip=$CURRENT_IP"`
	                echo "Result request dynHost" >> $PATH_LOG
	                echo "$RES" >> $PATH_LOG
	        else
	                echo "IP has not changed" >> $PATH_LOG
	        fi
	fi`


Créez un folder script dans /home/pi. créez et éditez le fichier /script/dynhostraspberry_nom-de-domaine_extention-de-domaine.
Il faut modifier les lignes 7, 8, et 9 pour y mettre les parametres qui correspondent à votre cas.

Pour s'assurer que le fichier est bien valide. Aller sur ovh et changer l'ip du dynhost. Reconnectez vous au PI via l'addresse local et entrer la commande:

`sudo sh /script/dynhostraspberry_nom-de-domaine_extention-de-domaine'

Verifiez si l'ip du dynhost sur ovh est changé. Si cela ne marche pas vérifiez que les identifiants et l'url du site est bonne(n'oubliez pas de mettre les identifiants entre guillemets!

Maintenant il faut que ce script s'active chaque heure. On fait ça avec la commande [crontab](https://www.adminschoice.com/crontab-quick-reference):

`sudo crontab -u root -e`

Ajoutez à la fin du fichier:

`0 * * * * /bin/sh/ /path/to/file`

Vérifiez à nouveau que tout marche bien en changeant l'ip enregistrée sur ovh par une fausse ip et attendez la prochaine heure pleine. Si il y a une erreur, vérifier que le /path/to/file est le bon nom du fichier pour vous quelque chose comme: /script/dynhostraspberry_nom-de-domaine_extention-de-domaine

Si tout cela fonctionne bravo vous pouvez maintenant vous connectez depuis n'importe où au PI!




## Attribuer une IP local fixe au raspberry

Ce tutorial vous permettrait de fixer l'ip de votre raspberry dans votre reseau local. En effet dès que la box internet reboot l'ip du raspberry changera et rendra votre connexion au PI par le ssh impossible.

Cela peut être très facheux!

Pour il faut connaître l'ip de la gateway; pour cela on écrit la commande:

`sudo netstat -nr`

notez l'ip de la gateway

Ensuite ouvrez avec vim le fichier /etc/dhcpcd.conf

`sudo vim /etc/dhcpcd.conf`

Si vous êtes connecté en ethernet ajoutez à la fin du document ceci:

`interface eth0

static ip_address=192.168.1.53/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1`


Si vous êtes connecté en ethernet ajoutez à la fin du document ceci:

`interface wlan0

static ip_address=192.168.1.53/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1`

Explication des lignes:

- interface = elle indique quelle type de connexion est utilisée.
- static ip_address = elle indique l'ip local de la PI.
- static routers = Elle indique l'ip du gateway
- static domain_name_servers = Elle indique aussi l'ip du gateway.

Remplacez les valeurs par celle de votre cas.

Sauvegardez le document puis on reboot.

`sudo reboot`

après le reboot, on peut ping la nouvelle ip local de la PI:

`ping -c 5 192.168.1.53`

On peut aussi vérifier directement dans les paramètres de la gateway et mettez à jour les port forwarding:

photo.

1. **How to give your RaspberryPi a Static IP Address**: https://thepihut.com/blogs/raspberry-pi-tutorials/how-to-give-your-raspberry-pi-a-static-ip-address-update
1. **Article - Access your raspberry Pi from anywhere:** https://pavelfatin.com/access-your-raspberry-pi-from-anywhere/


## Host un site sur sa PI

Dans ce tutorial on va apprendre à créer un site web qui sera hébergé sur la PI! Il y a du taff donc on commence dès maintenant: C'est parti!

`sudo apt-get update && sudo apt-get upgrade`

puis

`sudo apt-get apache2`

Votre site sera sauvegardé dans le fichier /var/www/html. Il faut donc donner les droits au dossier d'apache. Ainsi vous pourrez administrer le site plus facilement! Lancez ces commandes:

`sudo chown -R pi:www-data /var/www/html/`
`sudo chmod -R 770 /var/www/html/`

Maintenant on va vérifier que le site marche bien:

`sudo wget -O verif_apache.html http://127.0.0.1`

puis 

`cat verif_apache.html`

Bravo, Si vous voyez quelque chose comme ça:

(photo)
Sachez que si vous vous voulez changer le dossier où votre site est sauvegardé pensez à indiquer le chemin dans le fichier /etc/apache2/site-available/000-default.conf

Maintenant on va installer les dépendances pour php 

`sudo apt-get install php7 libapache2-mod-php7.0 php-mbstring`

(Pour installer le serveur LAMP j'ai suivi le tutorial de raspbian-france(ref. 15). J'ai juste du faire la commande "apt-get install php5 libapache2-mod-php7.0" pour que tout marche bien(ref. 11).)

Bon maintenant il faut tester si php est bien installer. Pour cela on va d'abord jeter tout potentiel fichier dans /var/www/html puis on va créer un fichier .php:

`sudo rm /var/www/html/index.html`
`echo "<?php phpinfo(); ?>" > /var/www/html/index.php`

ensuite sur votre réseau local aller checker sur 192.168.1.53 et tout va bien lorsque vous avez cela:

photo

Maintenant on installe MySQL:

`sudo apt install mariadb-server php-mysql`

Pour tester l'installation de mysql on fait:

`sudo mysql --user=root`

Il faut maintenant supprimer l'utilisateur root et le recréer pour qu'il soit accessible par les script php.
Dans la console de mysql entrez les 3 commandes suivantes.

`DROP USER 'root'@'localhost';`
`CREATE USER 'root'@'localhost' IDENTIFIED BY 'password';`
`GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIES BY 'password' WITH GRANT OPTION;`

Changer le terme 'password' par votre password.

vous pouvez maintenant quitter mysql avec la commande suivante:

`exit`

(Lors de vos prochaine connections, vous pourrez donc utilisez la commande mysql --user=root --password=votremotdepasse).


L’installation de PHPMyAdmin n’est pas du tout obligatoire. Nous ferons ici une installation sans paramètres de sécurité particuliers.

L’installation de PHPMyAdmin se fait très simplement, via le gestionnaire de paquets, en utilisant la commande suivante :

`sudo apt install phpmyadmin`

choississez apache2 comme server. (photo)
Comme nous avons déjà configurez la base de données, choisissez no à la question concernant l’utilisation de dbconfig-common. 

Vous devez aussi activer l’extension mysqli si cela n’est pas encore fait. Pour cela, utilisez les commandes ci-dessous.

`sudo phpenmod mysqli`
`sudo /etc/init.d/apache2 restart`

Vérifier l’installation de PHPMyAdmin

Pour vérifier le bon fonctionnement de PHPMyAdmin, vous allez simple tenter d’y accéder, en utilisant l’adresse de votre Raspberry suivi de /phpmyadmin. Par exemple, en local ce sera « http://127.0.0.1/phpmyadmin ».

Si jamais vous avez une erreur, cela peut venir du fait que PHPMyAdmin se soit installé dans un autre dossier. Dans ce cas, essayez la commande

`sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin`

### Wordpress

On utilise wordpress pour installer le site

Ensuite on va dans le répertoire du site afin d'installer wordpress:

`cd /var/www/html`
`sudo wget http://wordpress.org/latest.tar.gz && sudo tar xzvf latest.tar.gz`
`sudo mv  wordpress/* /var/www/html`
`sudo rm -rdf wordpress`

La première commande nous rend dans le dossier du site:
La seconde nous fait download la dernière version de wordpress et décompresse les fichiers.
La troisième déplace les fichiers compris dans le dossier /var/www/html/wordpress vers /var/www/html/
La quatrième ligne supprime dossier wordpress qui est maintenant vide

verifiez que le fichier /var/www/html/index.php a été écrasé. Il contient maintenant un text en lien avec wordpress:

`cat index.php`

(photo) 


Il faut maintenant créer la base de donnée pour notre site wordpress:

On va se connecter en tant que root user à la db.

`sudo mysql -uroot -p`

`create database wordpress_site;`

`GRANT ALL PRIVILEGES ON wordpress_site.* TO 'root'@localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;`

`FLUSH PRiVILEGES;`

`exit`

Enfin allez sur votre page 192.168.1.53 dans votre reseau local. Installer wordpress avec les identifier et la db que vous avez créé.

La semaine prochaine on voit comment rendre ce site accessible depuis l'internet mondial.


        

- Install WordPress on Ubuntu 16.04 LTS with Apache2, MariaDB and PHP 7.1 Support. set up du fichier de config pour le virtual host (ref. 21.3).

1. **Thread - phpmyAdmin admin page displays only “demo server”(ref. 11):** https://stackoverflow.com/questions/41191400/phpmyadmin-admin-page-displays-only-demo-server
1. **Article - Uninstalling MySQL(ref. 12):** https://help.cloud66.com/maestro/how-to-guides/databases/shells/uninstall-mysql.html
1. **Article - Installer un serveur web sur votre Raspberry (Apache + PHP + MySQL) (ref. 15):** https://raspbian-france.fr/installer-serveur-web-raspberry-lamp/
https://www.raspberrypistarterkits.com/how-to/install-wordpress-on-raspberry-pi/


---------------------------- TO DO -----------------------------------------------------
---------------------------- TO DO -----------------------------------------------------

## rendre accessible son site a tous les utilisateurs.

- port forwarding OK -> go to the gateway 192.168.1.1
- OVH DYNHOST config OK -> creer un user pour chaque dynhost (/manage access/create a user) 
- script mise a jour du DYNHOST OK 
- Crontab ok -> sudo crontab -u root -e 
ajoutez 
`0 * * * * /bin/sh/ /path/to/file`

- APACHE OK 
ajouter un fichier quentinparrot.conf dans  /etc/apache2/site-available/
	<VirtualHost *:80>
		ServerAdmin admin@quentinparrot.com
		ServerName quentinparrot.com
		ServerAlias www.quentinparrot.com
		DocumentRoot /var/www/quentinparrot.com/html

		ErrorLog ${APACHE_LOG_DIR}/error.log
		CustomLog ${APACHE_LOG_DIR}/access.log combined
	</VirtualHost>

puis on creer un mysql avec un user spe pour ce proteger et compartimenter

`sudo mysql -uroot -p`

`create database wordpress_site;`
`create user 'user'@'localhost' IDENTIFIED BY 'passwoord';`
`GRANT ALL PRIVILEGES ON wordpress_site.* TO 'user'@localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;`

`FLUSH PRiVILEGES;`

`exit`
		
 puis

`sudo a2ensite quentinparrot.conf`
`sudo service apache2 restart`
Normalement tout marche bien!

on peut set up le wordpress

Cependant depuis votre pc le site n4est pas disponible il faut y acceder depuis un pc hors du local qui accepte le non ssl. Ma solution utiliser duckduckgo browser sur android.
Prochain tuto : comment installer un ssl. 
 
https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-ubuntu-14-04-lts
http://www.raspberrywebserver.com/sql-databases/using-mysql-on-a-raspberry-pi.html
https://howtoraspberrypi.com/how-to-install-web-server-raspberry-pi-lamp/

## configurer wordpress

Lorsque je ne peux pas installer les plugins, je dois surement changer les droits du folder /var/www/html (ref 10).
	- J'ai change les site locations dans le fichier wp-config.php(ref. 14).
	- J'ai fouille dans le fichier resolv.conf, sans succes (ref. 17).

1. **Thread - why I cannot install a plugin(ref 10):** https://stackoverflow.com/questions/39064553/cant-install-plugin-and-theme
1. **Article - Changing The Site URL(ref. 14):** https://wordpress.org/support/article/changing-the-site-url/

---------------------------- DONE -----------------------------------------------------
---------------------------- DONE -----------------------------------------------------




## Host un second site.
 

on reproduit les meme etape que le tuto rendre accessible son site a tous les utilisateurs.


Pour ce faire on va utiliser le [script de yann.me](https://yann.me/dynhost-ovh-pour-raspberry-pi-ou-une-machine-linux/)
hosts dans /etc/hosts

Creer des username dans le DYNHOST DE OVH1!!! peter

On achete le nom de domaine.

Puis on suit les tutos suivants:
- https://kgaut.net/blog/2016/creer-son-premier-virtual-host-sous-ubuntu.html
- https://perhonen.fr/blog/2017/07/heberger-plusieurs-sites-seul-serveur-apache-3000
- http://community.ovh.com/t/redirection-vers-dynhost-mise-a-jour-en-cours/3780

- 'www.' est aussi un sous domaine: https://www.fanjoe.be/?p=629!
	- https://docs.ovh.com/fr/hosting/mutualise-tout-sur-le-fichier-htaccess/
	- https://docs.ovh.com/fr/domains/redirection-nom-de-domaine/
	- https://openclassrooms.com/forum/sujet/serveur-dns-avec-nom-de-domaine-ovh
**see later**  Wordpress redirect to subdomain configuration:
	- DDNS: Accessing your Raspberry Pi from Internet(ref. 23.1)
	- DynHost per FritzBox aktualisieren(ref. 23.2)
	-  [Tuto] Pour les Nuls : Accès externe en https(ref.23.3)
	- Apache:Redirecting from subdomain to Wordpess page URL Path [closed](ref.23.4)

Le second site est lance.
- Il faut penser a bien changer le fichier /etc/apache2/apache2.conf. (indice trouver avec ce site https://serverfault.com/questions/546941/no-mpm-loaded-but-im-not-even-using-mpm)
- Et relecture du virtualhost ici: https://kgaut.net/blog/2016/creer-son-premier-virtual-host-sous-ubuntu.html
- Pour plus tard je peux voir comment utiliser plusieur subdomain point to different local ip adress (ref. 20)
1. Virtual host a voir plus tard:
	- VirtualHost et nom de domaines sur un VPS OVH (ref. 22.1).
	- Apache Rewrite or Proxy to internal server(ref. 22.2).


### SSL 

On a deja activer le prot 443 du port forwarding

On check si le apache ssl est enable

`sudo a2enmod ssl`
ou
`sudo apachectl -M`

Puis on se rend dans le dossier /opt et on dl certbot

`wget https://dl.eff.org/certbot-auto`
`chmod a+x certbot-auto`

Exécutez-le une première fois « à blanc » pour qu’il télécharge ses dépendances et s’installe.

`./certbot-auto`

Si vous avez ce message:
	Certbot has problem setting up the virtual environment.

	We were not be able to guess the right solution from your pip output.

	Consult https://certbot.eff.org/docs/install.html#problems-with-python-virtual-environment for possible solutions.
	You may also find some support resources at https://certbot.eff.org/support/ .

	on update apache2 Virtualhost
	on restart apache2

Supprimez ce fichier avec cette commande:

`sudo rm -rf /etc/pip.conf` puis on lance la commande

`./certbot-auto`

Avec Enter email address (used for urgent renewal and security notices (enter 'c' to cancel):
entrer 'c'


sudo ./certbot-auto certonly --webroot --webroot-path /var/www/quentinparrot.com/html/ --domain www.quentinparrot.com --email parrot.quentin@gmail.com

Agree/Cancel: A

YES/NO: N

Veuillez changez les autorisations du /etc/letsencrypt/live

`sudo chmod a+x live`

maintenant on change le virtualhost

On a previously:
	<VirtualHost: *.80>
	
	ServerName quentinparrot.com
	ServerAlias www.quentinparrot.com
	ServerAdmin admin@quentinparrot.com
	DocumentRoot /var/www/quentinparrot.com/html

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
	</VirtualHost>

voici le nouveau:

	<VirtualHost: *.80>
	
	ServerName quentinparrot.com
	ServerAlias www.quentinparrot.com
	
	RewriteEngine on
   	RewriteCond %{HTTPS} !on
    	RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}

	</VirtualHost>
	
	<VirtualHost *:443>

	ServerName quentinparrot.com
    	ServerAlias www.quentinparrot.com
	ServerAdmin admin@quentinparrot.com
	DocumentRoot /var/www/quentinparrot.com/html

	<Directory /var/www/quentinparrot.com/html>
        Options -Indexes
        AllowOverride all
        Order allow,deny
        allow from all
    	</Directory>

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/www.quentinparrot.com/cert.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/www.quentinparrot.com/privkey.pem
    SSLCertificateChainFile /etc/letsencrypt/live/www.quentinparrot.com/chain.pem
    SSLProtocol all -SSLv2 -SSLv3
    SSLHonorCipherOrder on
    SSLCompression off
    SSLOptions +StrictRequire
    SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"

    LogLevel warn
	ErrorLog ${APACHE_LOG_DIR}/www.quentinparrot.com-error.log
	CustomLog ${APACHE_LOG_DIR}/www.quentinparrot.com-access.log combined

	</VirtualHost>


Pour controler si la config on utilise la commande suivante:

`sudo apachectl configtest`

Il y a de forte change que ca ne marche pas...

Si il y a cette erreur: `AH00526: Syntax error on line 6 of /etc/apache2/sites-enabled/quentinparrot.conf:
Invalid command 'RewriteEngine', perhaps misspelled or defined by a module not included in the server configuration
Action 'configtest' failed.
The Apache error log may have more information.`

`sudo a2enmod rewrite && sudo service apache2 restart`

Si il y a cette erreur: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
(2)No such file or directory: AH02291: Cannot access directory '/var/log/apache2/www.quentinparrot.com/' for error log of vhost defined at /etc/apache2/sites-enabled/quentinparrot.conf:15
AH00014: Configuration check failed
Action 'configtest' failed.
The Apache error log may have more information

Cela est sans importance!

sudo service apache2 restart

Normalement tout marche bien! Bravo!
https://www.memoinfo.fr/tutoriels-linux/configurer-lets-encrypt-apache/

Pour pouvoir changer de theme vous allez devoir faire les commandes suivantes:

`sudo chown -R www-data:www-data /var/www/quentinparrot.com/html`
`sudo -Rf 770 /var/www/quentinparrot.com/html`

**follow up** Je dois installer le ssl.

	- Comment installer un ssl avec LetsEncrypt ddns est cloudflare( je dois le bypass pour je ne sais quelle raison...) a voir a la maison car je dois d'abord activer le port 443 (ref. 24.2).
	- Comment installer un ssl avec apache raspberry, a voir absolument. Malheureusement le browser ne va pas aimer car le httpS n'est pas enregistrer par une boite certifiee(24.3).
	- Explication de outil certbot. il permet d'automatiser la generation de certificats tls/ssl avec letsencrypt. ca se fait avec apache(ref. 24.4).
	https://www.youtube.com/watch?v=IjL3D9km3II
 	- Explication supplementaire (ref. 24.6).
	Le tuto qui marche pour le ssl: Créer et installer un certificat SSL Let’s Encrypt pour Apache (ref. 24.5)
	Voici la commande a faire pour installer les certificat sur le raspberry.
sudo ./certbot-auto certonly --webroot --webroot-path /var/www/html/ --domain raspberry.quentinparrot.ovh --email parrot.quentin@gmail.com
	Si il y a un pb de 
		"Certbot has problem setting up the virtual environment.
		We were not be able to guess the right solution from your pip
output.

Consult https://certbot.eff.org/docs/install.html#problems-with-python-virtual-environment
for possible solutions.
You may also find some support resources at https://certbot.eff.org/support/"

	La solution est de faire la commande: "sudo rm -rf /etc/pip.conf" (ref. 24.7)

	- Si il y a ce message d'erreur: "Certbot-auto, letsencrypt-auto has insecure permissions"
	Il faut rm les folders /eff.org et "/vc"(ref. 24.8).
	- Si vous avez ce message d'erreur "Invalid Command ‘Header’, Perhaps Misspelled or Defined by a Module Not Included in The Server Configuration"

		When working with the headers in Apache 2 directly, it’s possible to run into this error when you do not have mod_headers enabled. It’s a simple fix: you just need to make sure you enable mod_headers in your configuration.
		You can take a shortcut using a2enmod, a command that enables the module for you automatically:
		$ sudo a2enmod headers
		Enabling module headers.
		 Run '/etc/init.d/apache2 restart' to activate new configuration!
		$ sudo service apache2 restart
		 * Restarting web server apache2
		Upon trying the requested page again, you should now be able to use headers within your configuration. (ref. 24.9)
	- si vous avez ce message "" AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.0.1. Set the 'ServerName' directive globally to suppress this message c'est harmless.(ref. 24.10).
 sudo apachectl configtest                
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.53. Set the 'ServerName' directive globally to suppress this message
Syntax OK

Mon dieu le SSL marche!!!

1. **Thread - [Tuto] Pour les Nuls : Accès externe en https(ref. 23.3):** https://www.jeedom.com/forum/viewtopic.php?t=34167
1. **Article - Hosting WordPress on Raspberry Pi Part 6 – Implement SSL (ref.24.1):** https://www.e-tinkers.com/2016/12/hosting-wordpress-on-raspberry-pi-part-6-implement-ssl/
1. **Article - Raspberry Pi SSL Certificates using Let’s Encrypt (ref. 24.2):** https://pimylifeup.com/raspberry-pi-ssl-lets-encrypt/
1. **Article - How to Enable HTTPS on the Raspberry Pi Apache Web Server (ref. 24.3):** https://variax.wordpress.com/2017/03/18/adding-https-to-the-raspberry-pi-apache-web-server/comment-page-1/
1. **Article - Certbot: challenge DNS OVH & wildcard(ref. 24.4):** Certbot: challenge DNS OVH & wildcard
1. **Article - Créer et installer un certificat SSL Let’s Encrypt pour Apache (ref. 24.5):** https://www.memoinfo.fr/tutoriels-linux/configurer-lets-encrypt-apache/
1. **Article - Ubuntu 18.04 LTS – Installation de certificats SSL/TLS avec Certbot (Let’s Encrypt) (ref. 24.6):** https://howto.wared.fr/ubuntu-certificats-ssl-tls-certbot/
1. **Thread - (ref. 24.7)- https Certbot has problem setting up the virtual environment [Résolu]:** https://www.jeedom.com/forum/viewtopic.php?t=44493
1. **Thread - Certbot-auto, letsencrypt-auto has insecure permissions (ref. 24.8):** https://community.letsencrypt.org/t/certbot-auto-letsencrypt-auto-has-insecure-permissions/93046/3
1. **Thread - How to set 2 wordpress sites on a RPi with different domain names:** https://www.raspberrypi.org/forums/viewtopic.php?t=230253
1. **Article - Apache et virtual hosts:** https://httpd.apache.org/docs/2.4/vhosts/name-based.html


### Tuto mysql

1. **Article - Reset a MySQL root password(ref.4):** https://support.rackspace.com/how-to/mysql-resetting-a-lost-mysql-root-password/
1. **Article - How to Delete a MySQL Database (ref.5):** https://www.wikihow.com/Delete-a-MySQL-Database
1. **Wiki -Moving WordPress (ref.6):** https://wordpress.org/support/article/moving-wordpress/

## Difficulties


1. How to reset the password de mysql (ref. 4)
1. how to reset la database de wordpress(ref.5)
1. Je ne sais pas comment aller sur mon site depuis internet ...(ref. 6)
1. Je n'arrive pas a active le crontab(ref.7). D'abord j'ai changer les variables environnementales(ref.8).
1. Ref.7.1 Je sais maintenant que le lieu on se trouve le script est important. Peut etre faut il que je deplace le script hors de cron.hourly.
1. Le Sub domain marche Avec No ip! Malheureusement, avec ovh ca ne marche pas ...
	- Adrien me dit que cela peut etre lié au sujet du reverse proxy(ref. 9.1). 
	- Comme je n'arrive pas a me connecter avec ovh au site, j'ai change les donnees dans la mysql database "wordpress" en utilisant phpmyadmin(ref. 9.2).
	- Peut etre que le pb vient de mes DNS. Voir (ref. 13) pour les reparametrer.
1. Comment rendre le raspberry accessible depuis wan? 
	- help sur raspbian.fr (ref.18)
	- revoir la configuration du dyndns ici (ref. 19)
	- Thread indiquant que les livebox n'accepte pas toute les entreprises de dyndns (ref. 21.1).
	- Thread with info(ref. 21.2).
1. J'ai un probleme lorsque je charge un des pages du site sur le tel en 4g. J'obtiens une page about:blank#blocked. Je ne sais pas quoi faire(ref. 16)


## LOG
	
- 18.05.2019: J'arrive a faire marcher mon site web. Ca marche enfin...
Je n'ai que changer l''url' et 'home' dans le file 'wp_config.php' pour les faire matcher avec 'raspberry.quentinparrot.ovh': est ce du a une sync avec le serveur de ovh?
- 28.08.2019: 
	- Je n'arrive pas à faire marcher mon site avec le sous domaine www. et celui de nom frere. Je reformate la SD
- 29.08.2019:
	- La SD n'a plus de Kernel et je n'arrive pas a recréé les partitions.
- 01.08.2019:
	- La SD fonctionne à nouveau sans que je comprenne vraiment... Je l'ai plug unplug...
- 02.08.2019:
	- Je viens de refaire les tutoriaux et reconnecter la PI a internet avec un ssh ready avec un lien sur l'internet mondial :)
- 03.08.2019:
	- Installation et tuto de l'ip local fixe pour la raspberry.
	- Installation et verification + tuto pour apache, php et mysql.
- 04.08.2019:
	- installation du site
	- installation du ssl
	- installation multi site
	- installation ssl pour le second site.
- 06.08.2019: "FTP credentials error corrected"
	- WordPress Connection Information (FTP credentials - Kali) error corrected

#### Useful Link

1. **Auto heberger son site avec OVH:** https://www.p3ter.fr/gestion-du-dyndns-sous-linux-avec-ovh.html
1. **Article - Site web auto heberge (ref. 6):** https://artheodoc.wordpress.com/site-web-auto-heberge-sur-un-raspberry-pi/
1. **Article - What’s the Difference Between “WordPress Address” and “Site Address”?(ref. 6):** https://www.wordher.com/whats-difference-wordpress-address-site-address/
1. **First glance to reverse proxy (ref.9.1):** https://httpd.apache.org/docs/2.4/fr/mod/mod_proxy.html
1. **Thread - fermeture de DNSdynamic (ref. 21.2):** https://cyrille-borne.com/forum/discussion/449/fermeture-de-dnsdynamic
1. **Article - VirtualHost et nom de domaines sur un VPS OVH(ref.22.1):** https://blog.velh.fr/2015/02/19/virtualhost-et-nom-de-domaines-sur-un-vps-ovh/
1. **Thread -  Apache Rewrite or Proxy to internal server (ref. 22.2):** https://serverfault.com/questions/413102/apache-rewrite-or-proxy-to-internal-server
1. **Thread - Install WordPress on Ubuntu 16.04 LTS with Apache2, MariaDB and PHP 7.1 Support(ref. 22.3):** https://websiteforstudents.com/install-wordpress-on-ubuntu-16-04-lts-with-apache2-mariadb-and-php-7-1-support/
1. **Article - DDNS: Accessing your Raspberry Pi from Internet(ref. 23.1):** https://domoticproject.com/accessing-raspberry-ddns/
1. **Thread - Apache:Redirecting from subdomain to Wordpess page URL Path [closed](ref. 23.4):** https://superuser.com/questions/1179190/apacheredirecting-from-subdomain-to-wordpess-page-url-path
1. **Article - Invalid Command ‘Header’, Perhaps Misspelled or Defined by a Module Not Included in The Server Configuration(ref.24.9):** https://zaclee.net/apache/errors-apache/invalid-command-header
1. **Free SSL Certificates with Let's Encrypt on Raspberry Pi and Ubuntu**: https://www.youtube.com/watch?v=IjL3D9km3II
1. **Article - https://www.nilovelez.com/2016/03/apache-virtual-hosts-ubuntu-14 (ref. 24.10):** https://www.nilovelez.com/2016/03/apache-virtual-hosts-ubuntu-14/
