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
        

## Service SSH
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
       
        
                  
