# TP3 : Attaques sur les mots de passe
## Mise en oeuvre et utilisation d'un gestionnaire de mots de passe, usage avancé

## Classe : B3B
## Élèves : Emma Durand **[@emmadrd912](https://github.com/emmadrd912)** et Pierre Ceberio **[@PierreYnov](https://github.com/PierreYnov)** 

![](https://cdnx.nextinpact.com/data-next/images/bd/wide-linked-media/799.jpg)

# Sommaire 

- [Le Lab](#le-lab)
- [Découverte et utilisation de sshfs](#d%C3%A9couverte-et-utilisation-de-sshfs)
- [Découverte et utilisation de syncthing](https://github.com/PierreYnov/tp3_offensif_keepass_avance#d%C3%A9couverte-et-utilisation-de-syncthing)



## Le lab

- poste avec keepass
- serveur distant Linux

## Découverte et utilisation de sshfs 

    - configurer un accès ssh sur le serveur linux :
    - activer le strictmode de ssh
    - désactiver les connexions distantes depuis root
    - modifier le port par défaut de ssh
    - créer un user sur le serveur avec un mdp fort et devra avoir accès au répertoire de keepass


On ``vi /etc/ssh/sshd_config`` et ligne 35 on dé-commente la ligne ``StrictModes yes``

On ``vi /etc/ssh/sshd_config`` et ligne 34 on met ``PermitRootLogin`` en ``no``

On ``vi /etc/ssh/sshd_config`` et ligne 15 on dé-commente ``Port`` et on met ``23``

On crée un utilisateur ``useradd -m pierre`` et je lui donne comme mot de passe avec ``passwd pierre`` : 

``AJ@b9R-g7ZZ^cVd+``


    - installer sshfs sur le client.
    - le configurer pour qu'il monte dans l'arborescence du dossier contenant la base.

``apt get install sshfs``

On ajoute sur le serveur, l'utilisateur pierre au groupe fuse

``sudo groupadd fuse``

``usermod -a -G fuse pierre``

Pour autoriser l'accès aux utilisateurs non-root, il faut dé-commenter la ligne ```user_allow_other``` dans le fichier ```/etc/fuse.conf```.

``sshfs pierre@207.180.201.151:/home/pierre/keepass3 keepass -C -p 23 -o allow_other``


![](img/keepass.png)


C'est fonctionnel !

## Découverte et utilisation de syncthing

    - installer et configurer syncthing pour que le dossier contenant les bases de mot de passe keepass soit synchronisé.

- Pour installer sous Linux :

    ```sudo pacman -S syncthing ```

- Pour installer sous Windows :

    Téléchargez le [ZIP](https://github.com/syncthing/syncthing/releases/download/v1.12.0/syncthing-windows-386-v1.12.0.zip) depuis le Github officiel de Syncthing :



    Dézippez et ouvrez-le, puis lancez ``syncthing.exe``

    Un terminal s'ouvre 
    
    ![](https://i.gyazo.com/a997e0b65e33909a76053f285b5626a8.png) 
    
    et sur votre navigateur l'interface de Syncthing s'ouvre à http://127.0.0.1:8384


À partir d'ici, cliquez sur ``Afficher mon ID``

![](https://i.gyazo.com/252f9b18dda82fdd38a30f71b67c13c4.png) 

et copiez cet ID pour l'ajouter sur l'interface de la machine distante 

![](https://i.gyazo.com/168b51ccbd61048fe99076b4564263e7.png)

Enregistrez et de l'autre côté acceptez la demande de liaison.

Votre nouveau dossier partagé apparaît :

![](https://i.gyazo.com/938a6ac8767d2116ba54d32b06173f47.png)

On peut y voir dessus le dernier fichier de la BDD Keepass qui a pu être envoyé et que j'ai réceptionné sur :

![](https://i.gyazo.com/0173955c4092ab6d8caaeee4c3a6833b.png)


Le dossier des mots de passe est donc bien synchronisé avec la machine distante !
