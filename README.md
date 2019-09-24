# ROGER-sky
projet 19, réseaux

## Création VM
    — Dans le paramètres network ajouter un réseaux hôte privé
    — Taille de 8Go
    — Partitionner
        |
        | ____ Une parition de 4.2Gb
         | ____ Une parition swap de 1Gb
          | ____ Une partition avec le reste
          
## Installer des outils
   Mise a jour des packages
   
    apt install -y vim sudo net-tools iptables-persistent fail2ban sendmail apache
        
## Service *SSH*
   Modifier le fichier /etc/ssh/sshd_config

     port 2222
     PermitRootLogin no
     PubkeyAuthentication no
        
        
   Mettre a jour
 
    service ssh restart
                  
                  
   Générer une clé publique avec
          
    ssh-keygen
          
  Rendre la clé publique éffective
          
    ssh-copy-id -i id_rsa.pub -p 2222 user@192.168.56.3
          

## Interface STATIC
   Mise en place du réseau, modifier le fichier
        
    vim /etc/network/interfaces
        
   enp0s8
                   
     iface enp0s8 inet static
     address 192.168.56.3
     netmask 255.255.255.252
                  
   Mettre a jour
       
    reboot
       
## Firewall
   Mise des règles de pare-feu
   
   Listes les règles existantes
   
    sudo iptables -L
   
  Ajouter ce fichier et écrire les règles de votre choix
  
    sudo vim /etc/network/if-pre-up.d/iptables

  Puis le rendre executables
  
    sudo chmod +x /etc/network/if-pre-up.d/iptables
                  
## Denial Of Service Attack *DOS*
   Création du fichier log pour le serveur apache, fait office de jorunal de bord
   
    sudo touch /var/log/apache2/server.log
   
   Créer un fichier de configuration local permet de ne pas compromettre les règles par defaut
   
    sudo vim /etc/fail2ban/jail.local
    
   Créer un fichier de filtrages
    
    sudo vim /etc/fail2ban/filter.d/http-get-dos.conf
   
   Relancer le tout
   
    sudo systemctl restart fail2ban.service
   
## Vérification des ports ouverts
   La commande
    
    sudo netstat -paunt
    
## Désactiver des services
   La commande
    
    ?????
    
## Sript *Update* && *Watching*
   Créer un script pour updater les packages automatiquement, le fichier doit se finir par ".sh"
    
   Contenu du script *Update*
    
    #! /bin/bash
        apt-get update && apt-get upgrade
    
   Rendez-le executable
   
    chmod +x FICHIER.sh
    
   Ouvrer /etc/crontab avec un éditeur et ajouter ceci
   
    0 4	* * 1	root	/home/USER/FICHIER.sh  >> /var/log/FICHIER.log
    @reboot		root	/home/USER/FICHIER.sh  >> /var/log/FICHIER.log
    
   Copier le contenu de votre crontab
   
    cp /etc/crontab /home/USER/tmp
   
   Créer le contenu du mail a envoyer
   
    vim /home/USER/email.txt
   
   Contenu du script *Watching*
   
    #!/bin/bash
    cat /etc/crontab > /home/USER/new
    DIFF=$(diff new tmp)
    if [ "$DIFF" != "" ]; then
	    sudo sendmail ROOT@MAIL.com < /home/USER/email.txt
	    rm -rf /home/USER/tmp
	    cp /home/USER/new /home/USER/tmp
    fi
    
   Rendez-le executable
   
    chmod +x FICHIER.sh
    
   Ouvrer /etc/crontab avec un éditeur et ajouter ceci
   
    0  0	* * *	root	/home/USER/watch_script.sh
    
   
