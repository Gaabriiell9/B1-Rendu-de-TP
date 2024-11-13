# Compte Rendu TP3

## 🌞 Afficher la ligne du fichier qui concerne votre utilisateur

    cat /etc/passwd | grep fariasgomes

fariasgomes:x:1000:1000:fariasgomes,,,:v/home/fariasgomes:/bin/bash

## 🌞 Afficher la ligne du fichier qui concerne votre utilisateur ET celle de root en même temps

 

    cat /etc/passwd | grep -E "fariasgomes|root"

root:x:0:0:root:/root:/bin/bash
fariasgomes:x:1000:1000:fariasgomes,,,:/home/fariasgomes:/bin/bash


## 🌞 Afficher la liste des groupes d'utilisateurs de la machine

    cat /etc/group

root:x:0:  
daemon:x:1:  
bin:x:2:  
sys:x:3:  
adm:x:4:  
tty:x:5:  
disk:x:6:  
lp:x:7:  
mail:x:8:  
news:x:9:  
uucp:x:10:  
man:x:12:  
proxy:x:13:  

j'ai pris que les 13 premiers pour pas que ça soit trop sur le compte rendu.


## 🌞 Afficher la ligne du fichier qui concerne votre utilisateur ET celle de root en même temps

cat /etc/passwd | grep -E "root|$(whoami)" | cut -d":" -f1

root  
fariasgomes
# 2. Hash des passwords

## 🌞 Afficher la ligne qui contient le hash du mot de passe de votre utilisateur


    sudo cat /etc/shawdow | grep "fariasgomes"

[sudo] password for fariasgomes:   
fariasgomes:$y$j9T$lqyf6fLDNNtPW.dywmHTX/$lzUbJ8MmfR..Oh2LCc49wjtwugi33CEE7Una7A.4vl/:20033:0:99999:7:::

## 🌞 Faites en sorte que votre utilisateur puisse taper n'importe quelle commande sudo

    sudo usermod -aG sudo toto


# B. Practice
## 🌞 Créer un groupe d'utilisateurs


    getent group stronk_admins  
    sudo adduser imbob

## 🌞 Créer un utilisateur

    sudo usermod -aG stronk_admins imbob 


il devra appartenir aux groupes imbob et stronk_admins


    sudo usermod -aG stronk_admins imbob

## 🌞 Prouver que l'utilisateur imbob a un password défini

    sudo cat /etc/shadow | grep "imbob"

imbob:$y$j9T$lERJMFUoCSlWOgBU1Rb8E.  $ZlIaFxCsjUZ22q3tZFCnQAFylAQpNWZROZtVUB8i6GA:20040:0:99999:7:::

## 🌞 Prouver que l'utilisateur imbob appartient au groupe stronk_admins

    sudo cat /etc/group | grep "imbob"
users:x:100:fariasgomes,toto,marmotte,imbob  
stronk_admins:x:1003:imbob  
imbob:x:1004:  

## 🌞 Créer un deuxième utilisateur

il devra appartenir au groupe imnotbobsorry ET stronk_admins

    sudo adduser imnotbobsorry  
    sudo usermod -aG stronk_admins imnotbobsorry


## 🌞 Modifier la configuration de sudo pour que

    sudo nano /etc/sudoers

j'ai mit la ligne ```%stronk_admins ALL=(ALL:ALL) ALL```

    # Allow members of group sudo to execute any command
    %sudo   ALL=(ALL:ALL) ALL
    %stronk_admins ALL=(ALL:ALL) ALL

## 🌞 Créer le dossier /home/goodguy (avec une commande)

    sudo mkdir /home/goodguy

## 🌞 Changer le répertoire personnel de imbob

    sudo usermod -d /home/goodguy imbob

## 🌞 Créer le dossier /home/badguy

    sudo mkdir /home/badguy
    sudo usermod -d /home/badguy imnotbobsorry

Prouvez que le changement est effectif en affichant le contenu du fichier passwd

    cat /etc/passwd | grep "imnotbobsorry"
imnotbobsorry:x:1005:1005:,,,:/home/badguy:/bin/bash

## 🌞 Prouver que les permissions du dossier /home/gooduy sont incohérentes

    s -l ..  

total 28 
drwxr-xr-x  2 root          root          4096 Nov 13 15:03 badguy  
drwx------ 16 fariasgomes   fariasgomes   4096 Nov 13 14:09 fariasgomes  
drwxr-xr-x  2 root          root          4096 Nov 13 14:53 goodguy  
drwx------  2 imbob         imbob         4096 Nov 13 14:49 imbob  
drwx------  2 imnotbobsorry imnotbobsorry 4096 Nov 13 14:31 imnotbobsorry  
drwx------  2 marmotte      marmotte      4096 Nov 13 00:47 marmotte  
drwx------  2 toto          toto          4096 Nov 12 23:03 toto  

## 🌞 Modifier les permissions de /home/goodguy

    sudo chown imbob:imbob /home/goodguy


## 🌞 Modifier les permissions de /home/badguy

    sudo chown imnotbobsorry:imnotbobsorry /home/badguy 


## 🌞 Connectez-vous sur l'utilisateur imbob

    su - imbob

Password:   
imbob@tpos:~$  

    mbob@tpos:~$ pwd

/home/goodguy