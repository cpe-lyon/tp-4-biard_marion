# Administration système - TP 3

## Auteurs :

*BIARD Gauthier*

*MARION Audran*

*Le 28/02/2020*


***

## Exercice 1. Gestion des utilisateurs et des groupes

**1. Commencez par créer deux groupes groupe1 et groupe2**

Dans le bash, nous exécutons les commandes :
```bash
sudo groupadd groupe1
sudo groupadd groupe2
```
> Nous avons du exécuter la commande en *sudo* car nous n'avions pas les droits sinon.

Afin de vérifier que les groupes ont bien été ajoutés, nous exécutons :
```bash
cat /etc/group | tail -2
```

En retour nous obtenons :
```bash
groupe1:x:1001:
groupe2:x:1002:
```
Nos groupes ont donc bien été créés.

&nbsp;

**2. Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell**

Nous avons exécuter les commandes suivantes :
```bash
sudo useradd -mksd u1
sudo useradd -mksd u2
sudo useradd -mksd u3
sudo useradd -mksd u4
```

&nbsp;

**3. Ajoutez les utilisateurs dans les groupes créés :**
- **u1, u2, u4 dans groupe1**
- **u2, u3, u4 dans groupe2**

Afin d'ajouter les différents utilisateurs à leurs groupes, il faut exécuter :
```bash
usermod -a -G groupe1 u1
usermod -a -G groupe1 u2
usermod -a -G groupe1 u4
usermod -a -G groupe2 u2
usermod -a -G groupe2 u3
usermod -a -G groupe2 u4
```

> -a -G permet d'ajouter un utilisateur existant à un groupe déjà existant. (Nous pouvons également rajouter d'autres groupes après le premier groupe dans la même ligne de commande)

&nbsp;

**4. Donnez deux moyens d’afficher les membres de groupe2**

Le premier moyen d'afficher les membres de groupe2 est la commande :
```bash
grep groupe2 /etc/group
```
Dans ce cas, en retour nous avons :
```bash
groupe2:x:1002:u2,u3,u4
```

Le deuxième moyen d'afficher les membres de groupe2 est la commande :
```bash
members groupe2
```

> Il faut pour cela installer le paquet <em>members</em> : `sudo apt install members`

Dans ce cas, en retour nous avons :
```bash
u2 u3 u4
```

&nbsp;

**5. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4**

```bash
sudo chgrp groupe1 /home/u1
sudo chgrp groupe1 /home/u2
sudo chgrp groupe2 /home/u3
sudo chgrp groupe2 /home/u4
```

&nbsp;

**6. Remplacez le groupe primaire des utilisateurs :**
- **groupe1 pour u1 et u2**
- **groupe2 pour u3 et u4**

Pour changer le groupe primaire des utilisateurs u1 et u2 :
```bash
sudo usermod -g groupe1 u1
sudo usermod -g groupe1 u2
```

Pour changer le groupe primaire des utilisateurs u3 et u4 :
```bash
sudo usermod -g groupe2 u3
sudo usermod -g groupe2 u4
```

Nous pouvons vérifier à quels groupes appartiennent les différents utilisateurs :
```bash
gauthieraudran@serveur:~$ groups u1
u1 : groupe1
gauthieraudran@serveur:~$ groups u2
u2 : groupe1 groupe2
gauthieraudran@serveur:~$ groups u3
u3 : groupe2
gauthieraudran@serveur:~$ groups u4
u2 : groupe2 groupe1
```

&nbsp;

**7. Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.**

Pour le groupe 1 :
```bash
mkdir /home/groupe1
chgrp groupe1 /home/groupe1
chmod g+w /home/groupe1
```
Pour le groupe 2:
```bash
mkdir /home/groupe2
chgrp groupe2 /home/groupe2
chmod g+w /home/groupe2
```

> Tout d'abord, nous créons le dossier, puis nous transférons la possession du groupe et enfin nous donnons le droit d'écriture au groupe.

&nbsp;

**8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?**

```bash
find . -type f -print0 | xargs -0 chmod 644
```
> Cette commande nous permet de changer uniquement les permissons des fichiers sans changer les permissions que nous avons appliqué sur les dossiers précédemment.

&nbsp;

**9. Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?**

Nous ne pouvons pas nous connecter en tant que *u1* car le compte n'est pas actif et n'a pas de mot de passe.

> Lorsque nous tapons la commande *su u1*, il faut rentrer un mot de passe mais comme u1 n'en a pas, nous ne pouvons pas nous connecter.

&nbsp;

**10. Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.**

Afin d'activer le compte *u1* :
```bash
gauthieraudran@serveur:~$ sudo passwd u1
New password:
Retype new password:
passwd: password updated successfully
```

> Nous avons choisi *u1* pour mot de passe de *u1*

&nbsp;

**11. Quels sont l’uid et le gid de u1 ?**

Afin d'afficher l'uid et le gid de u1 :
```bash
gauthieraudran@serveur:~$ id u1
uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)
```

> Ainsi, l'uid de *u1* est **1001** et son gid est **1001**

&nbsp;

**12. Quel utilisateur a pour uid 1003 ?**

Nous pouvons utiliser la commande :
```bash
gauthieraudran@serveur:~$ cat /etc/passwd | grep 1003
u3:x:1003:1002::/home/u3:/bin/sh
```
L'utilisateur qui a pour uid *1003* est **u3**

> Afin de n'avoir que le nom de l'utilisatuer, nous pouvons utiliser la commande: *cat /etc/passwd | cut -d: -f1,3 | grep 1003 | cut -d: -f1*
> cat /etc/passwd Permet de récupérer le contenu du fichier.
> cut -d: -f1,3 Permet de récupérer les colonnes 1 et 3.
> grep 1003 Permet de récupérer les lignes contenant 1003.
> cut -d: -f1 Permet de récupérer la 1ère colonne qui est le nom d'utilisateur.

&nbsp;

**13. Quel est l’id du groupe groupe1 ?**

Pour récupérer l'id du groupe *groupe1*, il faut exécuter la commande suivante :
```bash
gauthieraudran@serveur:~$ cat /etc/group | cut -d: -f1,3 | grep groupe1 | cut -d: -f2
1001
```

L'id de *groupe1* est **1001**

&nbsp;

**14. Quel groupe a pour guid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)**

Il suffit de faire la commande :
```bash
getent group 1002
```

Il sagit donc du groupe groupe2 qui a pour guid **1002**.

&nbsp;

**15. Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez**

Afin de retirer l'utilisateur u3 du groupe *groupe2*, il faut exécuter la commande suivante:
```bash
gauthieraudran@serveur:~$ gpasswd -d u3 groupe2
Removing user u3 from group groupe2
```

Cependant, lorsque nous vérifions à quel groupe appartient *u3*, nous pouvons voir que *u3* appartient toujours à *groupe2*:
```bash
gauthieraudran@serveur:~$ groups u3
u3 : groupe2
```

&nbsp;

**16. Modifiez le compte de u4 de sorte que :**
— **il expire au 1er juin 2020**
— **il faut changer de mot de passe avant 90 jours**
— **il faut attendre 5 jours pour modifier un mot de passe**
— **l’utilisateur est averti 14 jours avant l’expiration de son mot de passe**
— **le compte sera bloqué 30 jours après expiration du mot de passe**

Tout d'abord, nous vérifions les caractéristiques du compte de u4 :
```bash
gauthieraudran@serveur:~$ chage -l u4
Last password change                              : Mar 13, 2020
Password expires                                  : never
Password inactive                                 : never
Account expires                                   : never
Minimum number of days between password change    : 0
Maximum number of days between password change    : 99999
Number of days of warning before password expires : 7
```

|          Commande            |                                     Fonction                                                          |
|------------------------------|-------------------------------------------------------------------------------------------------------|
| chage -E 2020-06-1 u4        | change la date d'éxpiration du compte de l'utilisateur                                                |
| chage -M 90 u4               | change la durée maximale de validité du mot de passe                                                  |
| chage -m 5 u4                | changer le délai avant de pouvoir rechanger son mot de passe                                          |
| chage -W 14 u4               | change le nombre de jour où l'utilisateur est averti avant l'expiration de son mot de passe           |
| chage -I 30 u4               | change le nombre de jours après lequel, suite à l'expiration de son mot de passe, l'utilisateur est bloqué|


De ce fait, lorsque nous regardons les caractéristiques du compte après les modifications, nous obtenons :
```bash
gauthieraudran@serveur:~$ chage -l u4
Last password change                              : Mar 13, 2020
Password expires                                  : Jun 11, 2020
Password inactive                                 : Jul 11, 2020
Account expires                                   : Jun 01, 2020
Minimum number of days between password change    : 5
Maximum number of days between password change    : 90
Number of days of warning before password expires : 14
```

&nbsp;

**17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?**

On se connecte tout d'abord en tant que root :
```bash
sudo su
```

Ensuite :
```bash
root@server:~# printenv SHELL
/bin/bash
```

&nbsp;

**18. à quoi correspond l’utilisateur *nobody* ?**

Il s'agit d'un utilisateur qui existe par défaut et qui ne détient aucun document, n'appartient à aucun groupe de provilèges et qui n'a aucun droits à part ceux d'utilisateur de base. On utilise souvent cet utilisateur pour faire des test sans risquer de faire des erreurs.

&nbsp;

**19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?**

Par défaut, la commande **sudo** conserve le mot de passe pendant 15 minutes.

La commande qui permet de forcer sudo à oublier notre mot de passe est la commande : **sudo -k** (ou la commande **sudo --reset-timestamp**)

> Nous pouvons également lorsque nous sommes connecter en tant que root taper la commande **visudo** et ainsi modifier le fichier :
```bash
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset,timestamp_timeout=0
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
```

&nbsp;

## Exercice 2. Gestion des permissions

****

&nbsp;

****

&nbsp;

****

&nbsp;
