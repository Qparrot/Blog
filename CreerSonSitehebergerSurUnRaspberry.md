# Creer son site avec un raspberry

## Difficulties

Je ne comprends pas comment acceder au site wordpress depuis internet. Dans le reseau interne ca marche mais en externe j'y ai acces.
J'ai compris que le sous domaine raspberry.quentinparrot.ovh me permet d'avoir acces au raspberry pi et non au site wordpress(ref.1).
Je ne sais pas si le script de changement de l4ip dynamique marche(ref. 2).
J'ai perdu le mdp de la database mysql, donc j'ai installe zsh(ref.3) pour utiliser l'autocompletion des commande precedente(snas succes: old bash). 
How to reset the password de mysql (ref. 4)
how to reset la database de wordpress(ref.5)
Je ne sais pas comment aller sur mon site depuis internet ...(ref. 6)

Je n'arrive pas a active le crontab(ref.7). D'abord j'ai changer les variables environnementales(ref.8).
Ref.7.1 Je sais maintenant que le lieu on se trouve le script est important. Peut etre faut il que je deplace le script hors de cron.hourly.

Pour installer le serveur LAMP j'ai suivi le tutorial de raspbian-france(ref. 15). J'ai juste du faire la commande "apt-get install php5 libapache2-mod-php7.0" pour que tout marche bien(ref. 11).

Le Sub domain marche Avec No ip! Malheureusement, avec ovh ca ne marche pas ...
	- Adrien me dit que cela peut etre lié au sujet du reverse proxy(ref. 9.1). 
	- Comme je n'arrive pas a me connecter avec ovh au site, j'ai change les donnees dans la mysql database "wordpress" en utilisant phpmyadmin(ref. 9.2).
	- Peut etre que le pb vient de mes DNS. Voir (ref. 13) pour les reparametrer.
Lorsque je ne peux pas installer les plugins, je dois surement changer les droits du folder /var/www/html (ref 10).
	- J'ai change les site locations dans le fichier wp-config.php(ref. 14).
	- J'ai fouille dans le fichier resolv.conf, sans succes (ref. 17).


Lorsque je n'arrive pas a me connecter à la page de phpinfo() ou phpmyqdmin il faut surement que j'installe "apt-get install php5 libapache2-mod-php7.0" (ref 11). 
Lorsque j'ai fait une mauvaise manipulation avec mysql je dois utiliser sudo apt-get remove --purge mysql (ref .12)


Comment rendre le raspberry accessible depuis wan? 
	- description de raspbian-france. J'apprends ici qu'il faut config le dns aussi sur la box (ref.18)
	- revoir la configuration du dyndns ici (ref. 19)
	- Thread indiquant que les livebox n'accepte pas toute les entreprises de dyndns (ref. 21.1).
	- Thread with info(ref. 21.2).
	- Install WordPress on Ubuntu 16.04 LTS with Apache2, MariaDB and PHP 7.1 Support. set up du fichier de config pour le virtual host (ref. 21.3).


J'ai un probleme lorsque je charge un des pages du site sur le tel en 4g. J'obtiens une page about:blank#blocked. Je ne sais pas quoi faire(ref. 16)


Virtual host a voir plus tard:
	- VirtualHost et nom de domaines sur un VPS OVH (ref. 22.1).
	- Apache Rewrite or Proxy to internal server(ref. 22.2).
	

	
	
18.05.2019: ca marche enfin...
Je n'ai que changer l''url' et 'home' dans le file 'wp_config.php' pour les faire matcher avec 'raspberry.quentinparrot.ovh': est ce du a une sync avec le serveur de ovh?

**follow up** Je dois installer le ssl.
	- why to install ssl et comment l'intaller avec nginx (ref. 24.1).
	- Comment installer un ssl avec LetsEncrypt ddns est cloudflare( je dois le bypass pour je ne sais quelle raison...) a voir a la maison car je dois d'abord activer le port 443 (ref. 24.2).
	- Comment installer un ssl avec apache raspberry, a voir absolument. Malheureusement le browser ne va pas aimer car le httpS n'est pas enregistrer par une boite certifiee(24.3).
	- Explication de outil certbot. il permet d'automatiser la generation de certificats tls/ssl avec letsencrypt. ca se fait avec apache(ref. 24.4).
	https://www.youtube.com/watch?v=IjL3D9km3II
 

**see later**  Wordpress redirect to subdomain configuration:
	- DDNS: Accessing your Raspberry Pi from Internet(ref. 23.1)
	- DynHost per FritzBox aktualisieren(ref. 23.2)
	-  [Tuto] Pour les Nuls : Accès externe en https(ref.23.3)
	- Apache:Redirecting from subdomain to Wordpess page URL Path [closed](ref.23.4)

Pour plus tard je peux voir comment utiliser plusieur subdomain point to different local ip adress (ref. 20)
	
## hebergement chez OVH

## acheter le raspberry
micro sd
download rapsbian without desktop
Install the image with etcher

## installer rasbian
## demarrer son raspberry
login: pi
password: rapsberry

to see the prompt:
- Click multiple times on arrow up!
- click ctrl + l: to clear the prompt
- command exit: to restart the session
 

if you are stuck at the rainbow screen you may need to add 'boot_delay=1' in the file config.txt i the /boot folder of your sd. 

upgrade && update


## installer php

Installer nginx et no need to have password just use the sudo /etc/www/html restart

the wordpress folder is directly on the /etc/var/www/ folder


#### Useful Link

**Auto heberger son site avec OVH:** https://www.p3ter.fr/gestion-du-dyndns-sous-linux-avec-ovh.html
**Connect your Raspberry to the internet WIFI:** https://docs.dataplicity.com/docs/get-pi-connected-to-the-internet
**Install ZSH:** https://www.howtoforge.com/tutorial/how-to-setup-zsh-and-oh-my-zsh-on-linux/
**PAM error:** https://askubuntu.com/questions/812420/chsh-always-asking-a-password-and-get-pam-authentication-failure
**Have access to multiple terminal:** https://raspberrypi.stackexchange.com/questions/7023/open-multiple-terminals-without-gui-startx
**XKBMOEL="pc104" xkblayout="US - KeyBoard layout:** https://www.raspberrypi.org/forums/viewtopic.php?t=5278
**How to use nano:**
**How to param the wifi network didwork for me:**https://alcalyn.github.io/raspberry-connectee-telephone/
**Login + password:** https://www.raspberrypi.org/forums/viewtopic.php?t=45072
**Dyndns linux + OVH:** https://www.p3ter.fr/gestion-du-dyndns-sous-linux-avec-ovh.html
**Serveur lamp on raspberry:** https://raspbian-france.fr/installer-serveur-web-raspberry-lamp/
**Etcher - build your image:** https://www.balena.io/etcher/
**How to mount your image:** https://thepi.io/how-to-install-raspbian-on-the-raspberry-pi/
**Thread - Stuck on the Rainbow Screen:** https://www.raspberrypi.org/forums/viewtopic.php?t=55464
**Shutdown your rapsberry:** https://raspi.tv/2012/how-to-safely-shutdown-or-reboot-your-raspberry-pi
**Thread - Change your default password:** https://www.raspberrypi.org/forums/viewtopic.php?t=193620
**Youtube - Ports bei einem o2 Router freischalten:** https://www.youtube.com/watch?v=Pvf-tlu7VgE
**mon ip publique:** https://www.monippublique.com/
**Article - gestion du dynds sous linux avec ovh:** https://www.p3ter.fr/gestion-du-dyndns-sous-linux-avec-ovh.html
**Connect to your raspberry with android app SSHJuice:** https://www.youtube.com/watch?v=S-ZgU3kiacM
**Article - gerer son Raspberry depuis son ordinateur avec openssh:** https://raspbian-france.fr/controlez-raspberry-pi-ssh-ordinateur/
**Article - connaitre son ip via sa box:** https://alain-michel.canoprof.fr/eleve/tutoriels/raspberry/premiers-pas-raspberrypi/activities/connaitre-et-attribuer-ip-fixe.xhtml
**Article - SSH-Verbindung aufbauen und SSH verwenden:** https://jankarres.de/2014/02/raspberry-pi-ssh-verbindung-aufbauen-und-ssh-verwenden/
**Article - DynDNS einrichten:** https://jankarres.de/2012/11/raspberry-pi-dyndns-einrichten/
**Article - Portforwarding bei box und speedport:** https://jankarres.de/2013/01/raspberry-pi-portforwarding-bei-fritzbox-und-speedport/
**Article - Connexion a distance au raspberry:** https://www.supinfo.com/articles/single/4185-connexion-distance-au-raspberry-pi-3-ssh
**Article - installer wordpress sur une raspberry PI:** https://raspbian-france.fr/installer-wordpress-raspberry-pi-nginx/
**Article - Installer Nginx Raspbian, et accelerez votre serveur web raspberry:** https://raspbian-france.fr/installer-nginx-raspbian-raspberry/
**Article - Nginx failed to start a high performance web server and a reverse proxy server:** https://stackoverflow.com/questions/51525710/nginx-failed-to-start-a-high-performance-web-server-and-a-reverse-proxy-server
**Article - Can't start Nginx - Job for nginx.service failed:** https://stackoverflow.com/questions/51525710/nginx-failed-to-start-a-high-performance-web-server-and-a-reverse-proxy-server
**Thread - Script called in crontab every hour keeps sending emails at pi@raspberrypi:** https://raspberrypi.stackexchange.com/questions/71368/script-called-in-crontab-every-hour-keeps-sending-emails-at-piraspberrypi
**Github - Script send a email and shutdown the pi if too high temperature:** https://gist.github.com/LeonardoGentile/7a5330e6bc55860feee5d0dd79e7965d
**Article - Access your raspberry Pi from anywhere:** https://pavelfatin.com/access-your-raspberry-pi-from-anywhere/
**Hosting WordPress on Raspberry Pi Part 3 – (Right) Setup WordPress:** https://www.e-tinkers.com/2016/11/hosting-wordpress-on-raspberry-pi-part-3-setup-wordpress/
**Change the password of the nginx server:** https://www.digitalocean.com/community/tutorials/how-to-set-up-basic-http-authentication-with-nginx-on-ubuntu-14-04
**Thread - Why does nginx starts process as root? (I forget to sudo):** https://unix.stackexchange.com/questions/134301/why-does-nginx-starts-process-as-root
**Article - Raspberry Pi Changing Default User Password and Creating Additional Accounts:** https://www.theurbanpenguin.com/raspberry-pi-changing-the-default-users-password-and-creating-addtional-accounts/
**Article - Tutorial – Install WordPress on a Raspberry Pi using Nginx:** https://www.stewright.me/2014/06/tutorial-install-wordpress-on-a-raspberry-pi-using-nginx/
**Article - Tutorial – Install Nginx and PHP on Raspbian:** https://www.stewright.me/2014/06/tutorial-install-nginx-and-php-on-raspbian/
**Article - How To Set Up Password Authentication with Nginx on Ubuntu 14.04:** https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-nginx-on-ubuntu-14-04
**Article - Avantages de l’auto-hébergement (ref. 1):** https://serveuramoi.ovh/
**Article - DynHost OVH pour raspberry pi ou une machine linux(ref.2):** https://yann.me/dynhost-ovh-pour-raspberry-pi-ou-une-machine-linux/
**Article - RaspberryPi dyndns OVH(ref.2):**https://www.ccaillat.fr/2016/06/raspberrypi-dyndns-ovh/?doing_wp_cron=1560361192.5327041149139404296875
**Article - How to Install ZSH Shell on Ubuntu 18.04 LTS (ref.3):** https://linuxhint.com/install_zsh_shell_ubuntu_1804/
**Article - Reset a MySQL root password(ref.4):** https://support.rackspace.com/how-to/mysql-resetting-a-lost-mysql-root-password/
**Article - How to Delete a MySQL Database (ref.5):** https://www.wikihow.com/Delete-a-MySQL-Database
**Thread - Problème site web - Raspberry Pi - OVH(ref.6):** http://community.ovh.com/t/probleme-site-web-raspberry-pi-ovh/16137
**Article - Accéder de l’extérieur au flux http d’un Raspberry Pi(ref.6):** https://knowledge.parcours-performance.com/acceder-de-lexterieur-flux-http-dun-raspberry-pi/
**Website - info IP:** https://www.whatismyip.com/fr/
**Wiki -Moving WordPress (ref.6):** https://wordpress.org/support/article/moving-wordpress/
**Article - Mettre en ligne votre serveur web Raspbian, rendre votre Raspberry Pi accessible depuis internet avec DynDNS et le port forwarding (ref.6):** https://raspbian-france.fr/mettre-en-ligne-serveur-web-raspbian-dydns-port-forwarding/
**Article - Site web auto heberge (ref. 6):** https://artheodoc.wordpress.com/site-web-auto-heberge-sur-un-raspberry-pi/
**Article - What’s the Difference Between “WordPress Address” and “Site Address”?(ref. 6):** https://www.wordher.com/whats-difference-wordpress-address-site-address/
**Tuto - serveur web a la maison(ref.6):** https://raspberry-pi.developpez.com/cours-tutoriels/serveur-web/
**Article - tips for .htaccess:** https://www.ionos.fr/digitalguide/hebergement/aspects-techniques/les-meilleures-astuces-htaccess/
**Thread - why /etc/cron.hourly/myjob not working?(ref.7 1):** https://unix.stackexchange.com/questions/268503/why-etc-cron-hourly-myjob-not-working
**Thread - locale not found/setting locale failed - what should I do? (ref.8.1 essai):** https://unix.stackexchange.com/questions/110757/locale-not-found-setting-locale-failed-what-should-i-do
**Article - https://www.skyminds.net/linux-cannot-set-default-locale/ (ref.8.2 Reussite):** https://www.skyminds.net/linux-cannot-set-default-locale/
**Article -how to use crontab:** https://tecadmin.net/crontab-in-linux-with-20-examples-of-cron-schedule/
**Thread - why crontab is not working:**https://askubuntu.com/questions/23009/why-crontab-scripts-are-not-working
**Thread - Can't start Nginx - Job for nginx.service failed (read your input of sudo service nginx restart):** https://www.digitalocean.com/community/questions/can-t-start-nginx-job-for-nginx-service-failed
**Thread - Port forwarding on router:** https://www.raspberrypi.org/forums/viewtopic.php?t=200213
**Article - How to Change Nginx Port in Linux (potential ideas for connecting wordpress to the net):** https://www.tecmint.com/change-nginx-port-in-linux/
**Thread - Nginx with sites-enabled:** https://stackoverflow.com/questions/12715871/nginx-not-picking-up-site-in-sites-enabled
**Thread - How to set 2 wordpress sites on a RPi with different domain names:** https://www.raspberrypi.org/forums/viewtopic.php?t=230253
**Thread - Task-centered iproute2 user guide:**  https://baturin.org/docs/iproute2/
**Article - Apache et virtual hosts:** https://httpd.apache.org/docs/2.4/vhosts/name-based.html
**Thread - Redirect dynamic DNS How to redirect Dynamic DNS to a specific local ip address?:** https://superuser.com/questions/585070/how-to-redirect-dynamic-dns-to-a-specific-local-ip-address
**Thread - 404 Not Found nginx/1.10.0 (Ubuntu):** https://www.digitalocean.com/community/questions/404-not-found-nginx-1-10-0-ubuntu
**Thread - Nginx&Wordpress:** https://wordpress.org/support/article/nginx/
**Article - nginx.conf¶:** https://www.nginx.com/resources/wiki/start/topics/examples/full/
**Article - Nginx wordpress:** https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-nginx-on-ubuntu-14-04
**NO-IP:** https://my.noip.com/#!/dynamic-dns
**First glance to reverse proxy (ref.9.1):** https://httpd.apache.org/docs/2.4/fr/mod/mod_proxy.html
**Thread - WordPress incorrectly redirects to local IP address? (ref. 9.2):** https://wordpress.stackexchange.com/questions/263008/wordpress-incorrectly-redirects-to-local-ip-address
**Thread - why I cannot install a plugin(ref 10):**https://stackoverflow.com/questions/39064553/cant-install-plugin-and-theme
**Thread - phpmyAdmin admin page displays only “demo server”(ref. 11):** https://stackoverflow.com/questions/41191400/phpmyadmin-admin-page-displays-only-demo-server
**Article - Uninstalling MySQL(ref. 12):** https://help.cloud66.com/maestro/how-to-guides/databases/shells/uninstall-mysql.html
**Thread - reparametrer les dns (ref.13):** https://forums.cnetfrance.fr/topic/158796-comment-changer-ses-dns-manuellement-windows-mac-ios-android/
**Article - Changing The Site URL(ref. 14):** https://wordpress.org/support/article/changing-the-site-url/
**Article - Installer un serveur web sur votre Raspberry (Apache + PHP + MySQL) (ref. 15):** https://raspbian-france.fr/installer-serveur-web-raspberry-lamp/
**Google search - about:blank#blocked (ref. 16):** https://www.google.com/search?client=ubuntu&channel=fs&q=wordpress+raspberry+about%3Ablank%23blocked&ie=utf-8&oe=utf-8
**Thread - How do I get resolvconf to regenerate resolv.conf after I change /etc/network/interfaces?(ref. 17):** https://askubuntu.com/questions/224966/how-do-i-get-resolvconf-to-regenerate-resolv-conf-after-i-change-etc-network-in
**Article - Mettre en ligne votre serveur web Raspbian, rendre votre Raspberry Pi accessible depuis internet avec DynDNS et le port forwarding (ref. 18):** https://raspbian-france.fr/mettre-en-ligne-serveur-web-raspbian-dydns-port-forwarding/
**Article -  Mémo : Gestion du DynDNS sous Linux avec OVH(ref. 19):** https://www.p3ter.fr/gestion-du-dyndns-sous-linux-avec-ovh.html
**Thread - Quelle solution pour configurer un DynHost sur une Livebox ?(ref. 21.1):** https://lafibre.info/dyndns/quelle-solution-pour-configurer-un-dynhost-sur-une-livebox/
**Thread - fermeture de DNSdynamic (ref. 21.2):** https://cyrille-borne.com/forum/discussion/449/fermeture-de-dnsdynamic
**Article - VirtualHost et nom de domaines sur un VPS OVH(ref.22.1):** https://blog.velh.fr/2015/02/19/virtualhost-et-nom-de-domaines-sur-un-vps-ovh/
**Thread -  Apache Rewrite or Proxy to internal server (ref. 22.2):** https://serverfault.com/questions/413102/apache-rewrite-or-proxy-to-internal-server
**Thread - Install WordPress on Ubuntu 16.04 LTS with Apache2, MariaDB and PHP 7.1 Support(ref. 22.3):** https://websiteforstudents.com/install-wordpress-on-ubuntu-16-04-lts-with-apache2-mariadb-and-php-7-1-support/
**Article - DDNS: Accessing your Raspberry Pi from Internet(ref. 23.1):** https://domoticproject.com/accessing-raspberry-ddns/
**Thread - DynHost per FritzBox aktualisieren(ref. 23.2):** https://forum.ovh.de/showthread.php/12005-DynHost-per-FritzBox-aktualisieren
**Thread - [Tuto] Pour les Nuls : Accès externe en https(ref. 23.3):** https://www.jeedom.com/forum/viewtopic.php?t=34167
**Thread - Apache:Redirecting from subdomain to Wordpess page URL Path [closed](ref. 23.4):**https://superuser.com/questions/1179190/apacheredirecting-from-subdomain-to-wordpess-page-url-path

**Thread - How to make subdomains point to different local IP addresses? (ref. 20):** https://askubuntu.com/questions/172937/how-to-make-subdomains-point-to-different-local-ip-addresses
**Article - Hosting WordPress on Raspberry Pi Part 6 – Implement SSL (ref.24.1):** https://www.e-tinkers.com/2016/12/hosting-wordpress-on-raspberry-pi-part-6-implement-ssl/
**Article - Raspberry Pi SSL Certificates using Let’s Encrypt (ref. 24.2):** https://pimylifeup.com/raspberry-pi-ssl-lets-encrypt/
**Article - How to Enable HTTPS on the Raspberry Pi Apache Web Server (ref. 24.3):** https://variax.wordpress.com/2017/03/18/adding-https-to-the-raspberry-pi-apache-web-server/comment-page-1/
**Article - Certbot: challenge DNS OVH & wildcard(ref. 24.4):** Certbot: challenge DNS OVH & wildcard

https://www.youtube.com/watch?v=IjL3D9km3II


