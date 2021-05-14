## Week 2 (May 10 - 16) : RAM Image Analysis   

|  N°  | Intitulé du challenges        | Nombre de points  |      Outils utilisés              |        Statut         |
| -----| ------------------------------|:-----------------:| ---------------------------------:| ---------------------:|
|   1  | Be Brave                      |         3         | Volatility 3                      | :heavy_check_mark:    |
|   2  | Image Verification            |         3         |                                   | :heavy_check_mark:    |
|   3  | Let's Connect                 |         3         |                                   | :x:                   |
|   4  | RAM Acquisition Time          |         3         |                                   | :x:                   |
|   5  | Chrome Connection             |         6         |                                   | :x:                   |
|   6  | Hash Hash Baby                |         6         |                                   | :x:                   |
|   7  | Offset Select                 |         6         |                                   | :x:                   |
|   8  | Process Parents Please        |         6         |                                   | :x:                   |
|   9  | Finding Filenames             |         9         |                                   | :x:                   |
|  10  | Hocus Focus                   |         9         |                                   | :x:                   |
|  11  | Meetings                      |        12         |                                   | :x:                   |



# 0- A nos marques. Prêts? Partons ...   
1- Téléchargez les ressources de la semaine 2 via [ce torrent.](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Ressources/Africa-DFIRCTF-2021-WK02_archive.torrent)  
2- Décompressez le fichier `20210430-Win10Home-20H2-64bit-memdump.mem.7z` avec votre [outil favori d'archivage](https://www.clubic.com/telecharger/actus-logiciels/article-880911-1-meilleurs-logiciels-archivage-compression.html) supportant le format `.7z`.  
3- Hashez le fichier décompressé en `md5` et/ou `sha256` et comparez ces différents hashs avec ceux contenus dans le fichier `hashes.hd` fourni par le concepteur du CTF. Pour ce faire, plusieurs outils existent sur Kali Linux mais nous allons utiliser `hashdeep` qui nous fournit les deux hashs à la fois.  

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# hashdeep ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem
```
![Vérification de téléchargement](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/start%20Image%20Verification.png)

Comme on peut le constater sur la capture ci-dessus, le fichier est bien intègre car les hashs sont identiques.  
NB: Ce dernier point doit être une habitude à adopter pour tous vos futurs téléchargements sur Internet afin de vous assurer que les fichiers téléchargés sont bien intègres.



# 1- Be Brave 
***
![Be Brave](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Be%20Brave.PNG)

Nous allons afficher les informations des processus en cours d'exécution lors du dump de la mémoire RAM. 
Pour ce faire, nous pouvons utiliser l'un des trois plugins: `windows.pslist`, `windows.psscan` ou `windows.pstree`.  
* a) Commençons par afficher les en-têtes de l'output du plugin que nous utiliserons à savoir `windows.pslist`.

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist | head 
```
![En-tête de plist](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/en-tete%20plist.png)

* b) Nous pouvons enfin chercher le PID du processus demandé avec la commande ci-dessous.

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist | grep brave.exe
```
<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Be%20brave.png) [/spoiler] -->

Flag= <!-- 4856 -->




# 2- Image Verification 
***
![Image Verification](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Image%20Verification.PNG)

Nous allons hasher l'image de la mémoire RAM à disposition avec l'algorithme SHA256. Pour ce faire, différentes méthodes s'offrent à nous comme :  
- utiliser un outil en ligne 
- utiliser un utilitaire Unix/Linux comme `sha256sum` de la `GNU Core Utilities` ou `sha256deep` de la `MD5deep suite` etc.
- etc

Ayant déjà le fichier de hashs `hashes.hd` fourni par le concepteur du CTF à disposition, il suffit simplement de lire la valeur qui nous intéresse.  
<!-- ![Flag Image Verification](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Image%20Verification.png) --> 

flag= <!-- 9db01b1e7b19a3b2113bfb65e860fffd7a1630bdf2b18613d206ebf2aa0ea172 -->


























# To do
***
- [x] Ecrire la trame du writeup
- [ ] Expliquer d'avantage chaque étape
- [ ] Afficher les flags en PlainText
- [ ] Ajouter des ancres de navigation au tableau
- [ ] Inviter d'autres participants à partager leurs démarches si différentes de la mienne
