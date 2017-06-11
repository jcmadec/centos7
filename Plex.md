# Installation de Plex sur Centos 7

    yum update

## Installation de Plex
Installation de wget

    yum install wget

téléchargement de plex

    wget https://downloads.plex.tv/plex-media-server/1.5.6.3790-4613ce077/plexmediaserver-1.5.6.3790-4613ce077.x86_64.rpm

Installation du paquet

    yum localinstall plexmediaserver-1.5.6.3790-4613ce077.x86_64.rpm

Installation de cifs

    yum install cifs-utils
## accès au médias

mkdir /mnt/synology
cd synology
mkdir music_flac
mkdir music_mp3
mkdir video
mkdir photo

mount.cifs //192.168.0.100/music_flac /mnt/synology/music_flac -o user= user

Ici le mot de passe pour se connecter est demandé
Pour réaliser une connexion automatique, création du fichier credentials dans /etc/samba
et ajout de ces lignes:

username=user
password=password

ajout dans le fichier /etc/fstab
//192.168.0.100/music_mp3 /mnt/synology/music_mp3 cifs credentials=/etc/samba/credentials,defaults  0 0



## Configuration du firewall

Se connecter en root au serveur

    vi /etc/firewalld/services/plexmediaserver.xml

ajouter au fichier:

    <?xml version="1.0" encoding="utf-8"?>
    <service version="1.0">
     <short>plexmediaserver</short>
     <description>Plex TV Media Server</description>
     <port port="1900" protocol="udp"/>
     <port port="5353" protocol="udp"/>
     <port port="32400" protocol="tcp"/>
     <port port="32410" protocol="udp"/>
     <port port="32412" protocol="udp"/>
     <port port="32413" protocol="udp"/>
     <port port="32414" protocol="udp"/>
     <port port="32469" protocol="tcp"/>
    </service>

Démarrage du firewall

   systemctl start firewalld.service

Ajout de la règle:

    firewall-cmd --permanent --add-service=plexmediaserver
    firewall-cmd --permanent --zone=public --add-service=plexmediaserver

    systemctl restart firewalld.service
    systemctl start plexmediaserver.service
