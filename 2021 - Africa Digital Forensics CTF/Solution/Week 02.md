## Week 2 (May 10 - 16) : RAM Image Analysis   

|  N°  | Intitulé du challenge        | Nombre de points  | Outils utilisés                        |        Statut         |
| -----| ------------------------------|:-----------------:| --------------------------------------:| ---------------------:|
|   1  | Be Brave                      |         3         | Imagination, Volatility 3              | :heavy_check_mark:    |
|   2  | Image Verification            |         3         | Imagination, Volatility 3              | :heavy_check_mark:    |
|   3  | Let's Connect                 |         3         | Imagination, Volatility 3              | :heavy_check_mark:    |
|   4  | RAM Acquisition Time          |         3         | Imagination, Volatility 3              | :heavy_check_mark:    |
|   5  | Chrome Connection             |         6         | Imagination, Volatility 3              | :heavy_check_mark:    |
|   6  | Hash Hash Baby                |         6         | Imagination, Volatility 3, MD5SUM      | :heavy_check_mark:    |
|   7  | Offset Select                 |         6         | Imagination, HxD                       | :heavy_check_mark:    |
|   8  | Process Parents Please        |         6         | Imagination, Volatility 3              | :heavy_check_mark:    |
|   9  | Finding Filenames             |         9         | Imagination, Volatility 3              | :heavy_check_mark:    |
|  10  | Hocus Focus                   |         9         | Imagination, Volatility 3              | :heavy_check_mark:    |
|  11  | Meetings                      |        12         | Imagination, Autopsy, Google           | :heavy_check_mark:    |



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


**Remarque** : Dans ces challenges, nous utiliserons Volatility 3 pour sa compatibilité avec les versions de Windows 10 et supérieures.

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
<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Be%20brave.png) -->

Flag= <!-- 4856 -->




# 2- Image Verification 
***
![Image Verification](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Image%20Verification.PNG)

Nous allons hasher l'image de la mémoire RAM à disposition avec l'algorithme SHA256. Pour ce faire, différentes méthodes s'offrent à nous :  
- utiliser un outil en ligne 
- utiliser un utilitaire Unix/Linux comme `sha256sum` de la `GNU Core Utilities` ou `sha256deep` de la `MD5deep suite` etc.
- etc

Ayant déjà le fichier de hashs `hashes.hd` fourni par le concepteur du CTF à disposition, il suffit simplement de lire la valeur qui nous intéresse.  
<!-- ![Flag Image Verification](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Image%20Verification.png) --> 

flag= <!-- 9db01b1e7b19a3b2113bfb65e860fffd7a1630bdf2b18613d206ebf2aa0ea172 -->




# 3- Let's Connect 
***
![Let's Connect](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Let's%20Connect.PNG)

Nous allons afficher les informations réseaux contenues dans le dump de la mémoire RAM.  
Pour ce faire, nous pouvons utiliser l'un des deux plugins: `windows.netscan`, `windows.netstat`.  

* a) Commençons par afficher les connexions réseaux établies en utilisant le plugin `windows.netscan`.

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.netscan | grep ESTABLISHED 
```
![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Established.png)

* b) On obtient le résultat de la figure ci-dessous sur laquelle nous pouvons compter manuellement le nombre de connexion réseaux établies mais imaginons des millions de lignes ... cela deviendrait très fastidieux alors plus proprement avec la commande suivante: 

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.netscan | grep ESTABLISHED | wc -l
```


<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Let's%20Connect.png) -->

Flag= <!-- 10 -->




# 4- RAM Acquisition Time 
***
![RAM Acquisition Time](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20RAM%20Acquisition%20Time.PNG)

Nous allons afficher les informations générales (le profile) du dump de la mémoire RAM .  
Pour ce faire, nous pouvons utiliser le plugin: `windows.info` et nous intéresser à la valeur de `SystemTime`. 

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.info
```
<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20RAM%20Acquisition%20Time.png) -->

Flag= <!-- 2021-04-30 17:52:19 -->




# 5- Chrome Connection
***
![Chrome Connection](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Chrome%20Connection.PNG)

Nous allons afficher les informations réseaux contenues dans le dump de la mémoire RAM et s'intéresser aux connexions établies par le programme `chrome`.  
Pour ce faire, nous pouvons utiliser l'un des deux plugins: `windows.netscan`, `windows.netstat`.  

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.netscan | grep ESTABLISHED | grep chrome
```
![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Publique%20IP.png)

Sur la figure ci-dessus, nous remarquons que `chrome.exe` essaie d'établir une connexion avec l'IP publique `185.70.41.130`  sur le port 443.
L'objectif est donc de retrouver le domaine correspondant à cette IP. La technique la plus simple et évidente est d'ouvrir notre navigateur et de se rendre sur `https://185.70.41.130`; une résolution de nom est effectuée et l'on retrouve `https://mail.protonmail.com/login` duquel on retire le domaine.  

<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Chrome%20Connection.png) -->

Flag= <!-- mail.protonmail.com -->




# 6- Hash Hash Baby 
***
![Hash Hash Baby](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Hash%20Hash%20Baby.PNG)

Nous allons afficher les informations des processus en cours d'exécution lors du dump de la mémoire RAM et s'intéresser au processus de PID `6988`, si il existe! 
Pour ce faire, nous pouvons utiliser l'un des trois plugins: `windows.pslist`, `windows.psscan` ou `windows.pstree`. 

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist | grep 6988
```

![Grep processus de PID 6988](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/grep%20PID%206988.png)

Nous pouvons enfin dumper le processus de PID `6988` avec la commade ci-dessous.
```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist --pid 6988 --dump 
```

![Dump processus de PID 6988](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Processus%20PID.png)

Il nous reste plus qu'à hasher le fichier dumpé `pid.6988.0x1c0000.dmp` avec l'algorithme MD5. Pour ce faire, différentes méthodes s'offrent à nous :  
- utiliser un outil en ligne 
- utiliser un utilitaire Unix/Linux comme `md5sum` de la `GNU Core Utilities` ou `md5deep` de la `MD5deep suite` etc.
- etc

Ici, nous faisons le choix de l'outil `md5sum` installé par défaut sur Kali Linux.  

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# md5sum pid.6988.0x1c0000.dmp
```

<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Hash%20Hash%20Baby.png) -->

Flag= <!-- 0b493d8e26f03ccd2060e0be85f430af -->





# 7- Offset Select 
***
![Offset Select](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Offset%20Select.PNG)

Nous allons avoir besoin d'un outil qui nous permet d'afficher la correspondance  Hexadecimal - ASCII. Pour cela, plusieurs outils existent mais nous allons préférer utiliser l'outil graphique `HxD`. 
* a- Charger le dump mémoire `20210430-Win10Home-20H2-64bit-memdump.mem` dans le logiciel graphique `HxD` et suivre la démarche: 
 `Rechercher` > `Atteindre` > Coller la chaine `"45BE876"` > `Ok`  

![HxD 01](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/HxD%2001.png)

![HxD 02](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/HxD%2002.png)


* b- Lire le debut de notre offset `"45BE87"`en hexaxadécimal à gauche et sélectionner la chaine `"68 61 63 6B 65 72"` correspondante pour voir surligner le flag à droite.

<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Offset%20Select.png) -->

Flag= <!-- hacker -->




# 8- Process Parents Please 
***
![Process Parents Please](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Process%20Parents%20Please.PNG)

Nous allons afficher les informations des processus en cours d'exécution lors du dump de la mémoire RAM. 
Pour ce faire, nous pouvons utiliser l'un des trois plugins: `windows.pslist`, `windows.psscan` ou `windows.pstree`.  
* a) Commençons par afficher les en-têtes de l'output du plugin que nous utiliserons à savoir `windows.pslist`.

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist | head 
```
![En-tête de plist](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/pslist%20head%2001.png)


* b) Ensuite, trouvons le PPID de `powershell.exe`

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist | grep powershell.exe
```

![Grep de plist](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/ppid%20poweshell.png)


* c) Maintenant que nous avons le PID du processus parent de `powershell.exe`, nous pouvons enfin trouver la date de sa création.

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist | grep 4352
```
<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Process%20Parents%20Please.png) -->

Flag= <!-- 2021-04-30 17:39:48 -->





# 9- Finding Filenames
***
![Finding Filenames](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Finding%20Filenames.PNG)

Supposons que l'utilisateur ait ouvert le fichier en  question en ligne de commande, nous cherchons alors l'historique des commandes qui ont été saisies.
Pour ce faire, nous pouvons utiliser le plugin: `windows.cmdline`. 

* a) Trouvons le PID du processus `notepad.exe`.

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist | grep notepad.exe
```
![grep notepad](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/pid%20notepad.png)

* b) Nous pouvons ensuite afficher l'historique des commandes tapées sur la machine en filtrant par le PID `2520` du processus `notepad.exe`

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.cmdline.CmdLine --pid 2520
```
<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Finding%20Filenames.png) -->

Flag= <!-- C:\Windows\system32\NOTEPAD.EXE" C:\Users\JOHNDO~1\AppData\Local\Temp\7zO4FB31F24\accountNum -->





# 10- Hocus Focus
***
![Hocus Focus](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Hocus%20Focus.PNG)

Nous allons afficher les clés de registre et les informations userassist contenues dans le dump de la mémoire RAM. 
Pour ce faire, nous pouvons utiliser le plugin: `windows.registry.userassist`.  

* a) Commençons par afficher les en-têtes de l'output du plugin que nous utiliserons à savoir `windows.registry.userassist`.

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.registry.userassist | head
```
![En-tête registry](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/registry%20head.png)

* b) Nous pouvons ensuite afficher l'historique des commandes tapées sur la machine en filtrant par le PID `2520` du processus `notepad.exe`

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.registry.userassist | grep Brave
```
<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Hocus%20Focus.png) -->

Flag= <!-- 4:01:54 -->






# 11- Meetings
***
![Hocus Focus](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Meetings.PNG)

Nous allons avoir besoin du framework `Autopsy` pour récuperer tous les fichiers `PDF` contenus dans le dump du disque dur afin de les analyser.

* a- Charger le dump du disque dur `001Win10.E01` dans le logiciel graphique `Autopsy` et suivre la démarche:  
`File Types` > `By Extension` > `Documents` > `PDF` > Clic droit > `Extract file(s)`

* b) Ouvrir le seul fichier PDF extrait lisible du lot nommé `"29152-almanac-start-a-garden.pdf"`
* c) Lire l'information `2021-06-13 17°55'27.4"S 25°51'26.4"E` en bas de la page 12/21, en extraire les coordonnées GPS `"17°55'27.4"S 25°51'26.4"E"` à recopier sur 
 le moteur de recherche de Google pour obtenir la place et le pays ou le suspect prévoit de se rendre à lq date du `2021-06-13`. 

![HxD 01](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/extract%20files.png)

![HxD 02](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/GPS%20Meetings.png)



<!-- ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Meetings.png) -->

Flag= <!-- victoria falls -->





# To do
***
- [x] Ecrire la trame du writeup
- [ ] Expliquer d'avantage chaque étape
- [ ] Afficher les flags en PlainText
- [ ] Ajouter des ancres de navigation au tableau
- [ ] Inviter d'autres participants à partager leurs démarches si différentes de la mienne


**Author** : [@nanamou224](https://twitter.com/_nanamou224)
