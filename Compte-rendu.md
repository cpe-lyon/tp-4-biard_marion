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

&nbsp;

**7. Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.**

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;

****

&nbsp;
