# Introduction
Le Guide suivant illustre les étapes pour fetcher les fichiers du certificat ssl (letsencrypt) d'un domaine spécifique et les mettre en place dans le serveurs sur lequel on veut activer le certificat (le serveur comportant le sous-domaine)

# Démarche

* Accédez à la VM d'IP : c'est la VM qui contient les fichiers valides des certificats.
```
ssh root@62.4.0.182
```
password: certifcertif


* Accédez au chemin **/home/certified/letsencrypt_management** sous lequel vous trouverez les dossiers qui contiennent  les fichiers de certificats de chaque domaine.

```
cd /home/certified/letsencrypt_management
```

**NB:** Chaque dossier contient les fichiers certificats du domaine que le nom de ce dernier indique (nom de dossier = le nom de domaine a certifier) 

* Récupérez les fichier en question
```
sftp root@62.4.0.182
cd /home/certified/letsencrypt_management
cd /nom_de_domaine_correspondant
get fullchain.pem
get privkey.pem
```

* Accédez  au serveur auquel on veut mettre en place les fichiers du certificat . 
```
cd /etc/ssl/
mkdir letsencrypt
cd letsencrypt
put fullchain.pem
put privkey.pem
```

* Pointez les fichiers du certificat dans le fichier. : **/etc/apache2/sites-available/default-ssl.conf**
```
vim  /etc/apache2/sites-available/default-ssl.conf
```
- Changez les lignes correspondantes comme suit.
```
- SSLCertificateFile     /etc/ssl/letsencrypt/fullchain.pem
- SSLCertificateKeyFile /etc/ssl/letsencrypt/privkey.pem
```

### Restart apache
```
systemctl restart apache2.service
```




Cette documentation supplimentaire sera utile pour réaliser un certificat wildcard dès le début:
https://docs.google.com/document/d/1NeIZWD-sqCCL56OjZ3jy_0lvIzyIEiqG0RMICxflVHc/edit
