# Week 1 (May 03 - 09) : Forensic Disk Image 

### :balloon: Mise en situation  
_Un cybercrime vient d'être commis quelque part dans le monde sur une entité. En tant qu'Analyste Cybersécurité (DFIR), le First Responder vient nous remettre [l'image du disque de l'ordinateur Windows](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Ressources/Africa-DFIRCTF-2021-WK01_archive.torrent) impliqué dans cette cyberattaque. Notre mission, si nous l'acceptons, sera de résoudre les challenges ci-dessous afin de retracer les évènements du malveillant et collecter le maximum de preuves contre lui._   


### :balloon: Préparation de nos outils d'analyse
_Le rôle du First responder était d'acquérir les images. Il a bien dû utiliser des outils à cet effet comme `FTK Imager` qui reste l'un des plus populaires dans ce domaine. C'est un outil graphique (GUI) facilitant l'acquisition de la copie du disque dur, du dump de la RAM et l'extraction de la MFT ou encore des fichiers supprimés..._   

_Aussi, dans l'analyse d'artifacts (traces d'attaque) sur des disques durs, des smatphones et clés USB, `Autopsy` et `Sleuth Kit` sont sans doute les outils les plus utilisés. Le premier est un outil graphique (GUI) tandis que le second est un outil en ligne de commande (CLI)._  

_Pour les challenges de la semaine 1 (Week 1), nous utiliserons principalement `FTK Imager` et `Autopsy` sous `Windows` à cause de leur inteface intuitive facilitant la navigation dans les différents dossiers/fichiers du disque dur à analyser._   

_Commencez par télécharger et installer [FTK Imager](https://accessdata.com/product-download/ftk-imager-version-4-5) et [Autopsy](https://www.autopsy.com/download/) depuis leurs sites officiels sur votre OS Windows. Ensuite, en le supposant dans le même repertoire que les 14 autres fichiers nommés `001Win10.E0k` avec k=1, 2, ... 15, chargez uniquement l'image disque `001Win10.E01` de la semaine 1 (Week 1) dans chacun des deux outils `FTK Imager` et `Autopsy`. Enfin, nous sommes prêts à débuter notre investigation numérique sur cette image disque fournie._  

_Ci-dessous les challenges à résoudre pour cette semaine 1 (Week 1)._  

|  N°  | Intitulé du challenge    | Nombre de points  | Outils utilisés                 |        Flags                               |
| -----| -------------------------|:-----------------:| -------------------------------:| ------------------------------------------:|
|   1  | Suspect Disk Hash        |         3         | Imagination, FTK Imager/Autopsy | 430d0f91dc30b6c6de407ad622f12427           |
|   2  | Deleted                  |         3         | Imagination, Autopsy            | 2021-04-29 18:22:17                        | 
|   2  | Server Connection        |         3         | Imagination, Autopsy            | :heavy_check_mark:                         |
|   3  | Web Search               |         3         | Imagination, Autopsy            | :heavy_check_mark:                         |
|   4  | Possible Location        |         6         | Imagination, Autopsy            | :heavy_check_mark:                         |
|   5  | Tor Browser              |         6         | Imagination, Autopsy            | :heavy_check_mark:                         |
|   6  | User Email               |         6         | Imagination, Autopsy            | :heavy_check_mark:                         |
|   7  | Web Scanning             |         6         | Imagination, Autopsy            | :heavy_check_mark:                         |
|   8  | Copy Location            |         9         | Imagination, Autopsy            | :heavy_check_mark:                         |
|   9  | Hash Cracker             |         9         | Imagination, Autopsy            | :heavy_check_mark:                         |
|  10  | User Password            |        12         | Imagination, Autopsy            | :heavy_check_mark:                         |

### :balloon: Résolution des challenges
_**Notez bien**: Une bonne habitude est de commencer toujours par faire le tour des fichiers mis à disposition !_  






## :one: Suspect Disk Hash  
>What is the MD5 hash value of the suspect disk?  

_Pour les plus anglophones d'entre vous (:stuck_out_tongue:), on demande le hash en MD5 du disque dur physique suspecté (disque dur original) et non celui de la copie de ce disque dur (image du disque dur)!_  

_En regardant dans l'image disque dur à disposition, nous voyons 15 fichiers compressés (format image `EWF` très riche et sollicité en forensics) nommés de la forme `001Win10.E0k` avec k=1, 2, ... 15. Ces fichiers représentent, chacun, une partie du disque dur physique suspecté._  

_Nous allons donc dégainer notre outil capable de lire, interpréter et extraire intelligemment les informations utiles contenues dans le format `E01` que j'ai appélé `FTK Imager`,`Autopsy` permet également de le faire._  

_Le plus souvent, c'est la parition 2 d'une image au format `E01` qui nous intéressera car c'est elle qui contient le plus d'informations (voir sa taille spécifiée entre crochets dans l'arbre d'évidence `FTK Imager` mais pour d'autres outils CLI, on est obligé de calculer son offset en posant `offset=512*taille_partition_1`)._

_:zap: **Méthode 1**: Utilisation de `Autopsy`   
Voir le module `Hash Lookup` de `Autopsy`_   

_:zap: **Méthode 2**: Utilisation de `FTK Imager`  
1- Sélectionnez l'image disque `001Win10.E01` dans l'arbre d'évidence sur la fénêtre d'à gauche     
2- Allez dans `File` > `Verify Drive/Image...`_   
![`File`>`Verify Drive`](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Hash%20Hard%20Disk%20FTK%20Imager.png)  

_3- Notez le hash MD5 générer après vérification de la source/image disque `001Win10.E01`_    
![ hash MD5](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Suspect%20Disk%20Hash.png)  

_flag_ :triangular_flag_on_post: = `430d0f91dc30b6c6de407ad622f12427` 

 






## :two: Deleted  
>What date and time was a password list deleted in UTC? (YYYY-MM-DD HH:MM:SS)  

_Bon ... vous commencez à comprendre la langue de shakespeare, pas la peine de traduire non?_   

_Puisqu'il s'agit de retrouver la date et le temps de suppression d'un fichier, il est légitime de se poser ces deux questions:_    
- _Où vont les éléments supprimés sous Windows? La réponse évidente est : ils vont dans une sorte de dossier poubelle._   
- _Quel est le dossier poubelle dans le système que nous sommes entrain d'analyser à savoir Windows? Eh bien, une petite recherche sur le moteur de recherche Google nous indique 
qu'il y'en a un seul par disque dur et il s'appelle `$Recycle.Bin`. C'est un répertoire situé à la racine du disque dur, masqué par défaut et seul `SYSTEM` a tous les droits dessus, autrement vous ne pouvez pas le vider même en vidant votre corbeille!_  

_A l'aide de notre outil `Autopsy`, nous pouvons naviguer agréablement vers ce dossier à la recherche du trésor._   

_:zap: **Méthode 1**: Utilisation de `Deleted Files` de `Autopsy`  
Possible mais prend du temps pour identifier le fichier supprimé parmi les 2382 éléments même combinée avec l'option `Keywords Search`_      

_:zap: **Méthode 2**: Exploitation de `$Recycle.Bin`_  
1- _Obtenir `2021-04-29 20:22:17 CEST` par l'un des moyens suivants._
- _Soit s'intéresser à `Time Deleted` en se rendant à `Results` > `Extracted Content` > `Recycle Bin (1)` > `$RW9BJ2Z.txt`_  
![Flag Deleted](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20bis%20Deleted.png)  

- _Soit sintéresser à `Change Time` en se rendant à_ `001Win10.E01` > `vol3 (NTFS / exFAT (0x07): 104448-103830231)` > `$Recycle.Bin` > `S-1-5-21-3061953532-2461696977-1363062292-1001` > `$RW9BJ2Z.txt`  
![Flag Deleted](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Deleted.png) 

- _Soit utiliser la fonctionalité `Keywords Search` de_ `Autopsy`.  

_2- Convertir cette date en UTC en utilisant cette formule trouvée sur le moteur de recherche Google : `UTC=CEST-2h`   
2021-04-29 20:22:17 CEST = 2021-04-29 20:22:17 - 2:00:00 UTC = 2021-04-29 18:22:17 UTC_  
_flag_ :triangular_flag_on_post: = `2021-04-29 18:22:17` 


## :three: Server Connection  
>What is the IPv4 address of the FTP server the suspect connected to?  

_Pour ce challenge, il s'agit de retrouver l'adresse IP version 4 (format: `a.b.c.d` avec a, b, c et d des entiers naturels compris entre 0 et 255.) du server FTP sur lequel le suspect s'est connecté. Puisque nous avons à disposition uniquement l'image d'une machine impliquée dans un cybercrime, nous pouvons donc supposer qu'elle a été utilisée par le malveillant pour se connecter au serveur FTP en question. En cohérence avec cette hypothèse, le malveillant devrait donc avoir installé un Client FTP sur la machine afin de pouvoir joindre ce serveur FTP._  

_Maintenant que nous avons analysé la situation, il est temps de retourner à notre outil `Autopsy`._  
_1- Se rendre à `Results` > `Extracted Content` > `Installed Programs`ou encore `Results` > `Extracted Content` > `Run Programs`_  
_Nous remarquons `FileZilla Client 3.53.1 v.3.53.1` dans la liste des programmes installés sur la machine, cela confirme notre hypothèse._  
![programme installés](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/programmes%20install%C3%A9s.png)

_2- Rechercher sur Google le chemin absolu des fichiers relatifs à FileZilla Client sous Windows pour une installation par défaut   
Cela nous doonne comme résultat: `C:\Users\<username>\AppData\Roaming\FileZilla` qu'on traduit en arborescence comme ce qui suit_  
_3- Se rendre à `001Win10.E01` > `vol3 (NTFS / exFAT (0x07): 104448-103830231)` > `Users` > `John Doe` > `AppData` > `Roaming` > `FileZilla` > `recentservers.xml`_  

![flag Server Connection](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Server%20Connection.png)

_flag_ :triangular_flag_on_post: = `192.168.1.20` 







## :four: Web Search   
>What phrase did the suspect search for on 2021-04-29 18:17:38 UTC? (three words) 

_Nous devrons trouver les mots tapés par le malveillant sur le navigateur lors de sa recherche du 2021-04-29 18:17:38 UTC. Retournons en compagnie de notre fidèle ami `Autopsy`._ 

_:zap: **Méthode 1**: Utilisation de la foncitonnalité `Web Search` de `Autopsy`_  
_1- Se rendre à `Results` > `Extracted Content` > `Web Search`  
En effet, le titre du challenge nous fait penser déjà à cela._  
_2- Convertir la date 2021-04-29 18:17:38 UTC en CEST par cette formule trouvée sur le moteur de recherche Google : `UTC+2=CEST`     
2021-04-29 18:17:38 UTC = 2021-04-29 18:17:38 UTC + 02:00:00 CEST = 2021-04-29 20:17:38 CEST_  
_2- Trier les lignes par date et rechercher celle qui correspond à la fois à la date 2021-04-29 20:17:38 CEST et à trois mots (three words)_

![flag Web Search](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Web%20Search.png)

_flag_ :triangular_flag_on_post: = `password cracking lists` 


_:zap: **Méthode 2**: Utilisation de `Autopsy` + Google_  
_1- Se rendre à `Results` > `Extracted Content` > `Run Programs`_  
_Nous remarquons que les trois navigateurs `BraveBrowser`, `Chrome` et `Tor` sont installés sur la machine._ 

![Brave](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/brave%201.png)  
![Chrome](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/chrome%202.png)  
![Tor](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/tor%203.png)  

_2- Rechercher sur Google le chemin sous Windows des fichiers contenant l'historique de navigation de chacun des trois navigateurs trouvés._      
_Cela nous doonne comme résultat:_ 
- _Pour Chrome: `C:\Users\<username>\AppData\Local\Google\Chrome\User Data\Default` 
- _Pour Tor: ...
- _Pour Brave: ... _

_On  traduit ensuite ces chemins en arborescence `001Win10.E01` > `vol3 (NTFS / exFAT (0x07): 104448-103830231)` > `Users` > `John Doe` > `AppData` > `Local` > `Google` > `Chrome` > `User Data` > `Default` > `History` qu'on suit sous `Autopsy`. Nos recherches dans ces fichiers ont été fructueuses que pour le navigateur Google Chrome_

![](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20bis%20Web%20Search.png)

_flag_ :triangular_flag_on_post: = `password cracking lists`_  
_**Remarque** Pour cette question, bizarrement la phrase `how to hack` recherchée par le malveillant valide également le challenge._




## :five: Possible Location  
>What country was picture "20210429_152043.jpg" allegedly taken in?

_Hummm... ça sentirait pas de la stéganographie d'image par ici? Eh bien, oui c'est ce que nous allons voir dans la suite._   

_:zap: **Méthode 1**: Utilisation de la fonctionnalité `Geolocation` de `Autopsy`_  
_En cliquant sur `Geolocation` dans le menu d'en haut, nous observons que deux pays sont pointés: `Nigeria` et `Zambia`. Cliquons sur chacune des deux locations, et nous voyons s'afficher les images prises à ces endroits avec leur métadonnées. Celle qui nous intéresse est `20210429_152043.jpg` qui se trouve sur la `Zambia`, d'ou le flag._

![](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Possible%20Location.png)

_flag_ :triangular_flag_on_post: = `Zambia`  


_:zap: **Méthode 2**: Utilisation de `exiftool`   
1- Rechercher l'image `20210429_152043.jpg` en utilisant la fonctionnalité `Keywords Search` intégrée à `Autopsy`    
2- Exporter l'image sur notre machine Windows en faisant clic droit dessus puis `Extract File(s)`   
3- Lire les métadonnées de l'image    
Sachant qu'il faille chercher des coodonnées GPS, voici la commande à exécuter sous Kali Linux pour un accès direct à cette information._   
```console
┌──(root💀kali)-[~/Desktop/DFIR_CTF_2021/Africa-DFIRCTF-2021-WK03]
└─# exiftool -gpslatitude -gpslongitude 20210429_152043.jpg
```
_Nous obtenons comme résultats les coordonnées GPS précises (NB: avec Windows, il manquerait les précisions E et S)  

![](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/GPS.png)


_4- Rechercher le pays du globe terrestre ayant pour coordonnées GPS: Latitude = 16 deg 0' 0.00" S  et Longitude = 23 deg 0' 0.00" E   
Il suffit de bien formater ces coordonnées GPS en `16°0'0.00" S 23°0'0.00" E` puis copier-coller dans le moteur de recherche Google pour obtenir `Namasiku` comme résultat._    
![](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/namasiku.png)

5- Enfin, une petite recherche Google permet de savoir que `Namasiku` est en `Zambia`  
_flag_ :triangular_flag_on_post: = `Zambia`




# A bientôt pour la suite du writeup




## :two: Tor Browser   
>How many times was Tor Browser ran on the suspect computer? (number only) 

_Je sais que la langue de chez l'oncle Sam n'a plus aucun secret pour toi, permets-moi, toutefois, de te confirmer qu'ici, nous cherchons le nombre de fois (donc un entier, c'est-à-dire x tel que x>=0) ou le navigateur `Tor Browser` a été exécuté sur la machine de la victime. Notons bien l'égaglité large  ;), cette information est capitale comme tu le constateras par toi-même dans la suite._


_:zap: **Méthode 1**: Brute-forcer le flag  
Cette méthode est tout à fait légitime ici dans la mesure ou le le navigateur `Tor Browser` ne va pas non plus s'exécuter une infinité de fois sur la machine suspectée... Donc, il suffit d'utiliser un outil (un programme fait par vos soins ou un outil tel `BurpSuite`, `Hydra` ... qui se connecte au CTF et soumet une foultitude de valeurs que vous aurez paramétrées). Cela n'a pas été nécessaire ici car mon tout premier test manuel avec la valeur `0` a été concluant, eh oui !!!_

_Je tiens cependant à signaler ceci: lorsque vous serez appelé à analyser une scène de crime dans la vie réelle, on ne tatonne pas (le brute forcing est donc proscrit), on trouve un IOC point barre._  

_flag_ :triangular_flag_on_post: = `0`


_:zap: **Méthode 2**: Rechercher un IOC (Indice Of Compromise) dans le dossier AppData de l'utilisateur
Puisque nos preuves doivent être intangibles et judicièrement valides alors la méthode élégante est la recherche d'artifact. En se rendant au chemin `001Win10.E01` > `vol3 (NTFS / exFAT (0x07): 104448-103830231)` > `Users` > `John Doe` > `AppData`, nous ne trouvons pas `Tor Browser` ni dans dans `Local`, ni dans `LocalLow` encore moins dans `Roaming`. Nous pouvons donc conclure, qu'il a été installé sans jamais être exécuté sur la machine suspecte. Et donc logiquement, l'absence d'IOC équivaut à la négative de ce que nous demande l'exercice, c'est-à-dire `0` d'ou le flag)._ 
_flag_ :triangular_flag_on_post: = `0`

![flag tor browser](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Tor%20browser.png)



_:zap: **Méthode 3**: Rechercher un IOC (Indice Of Compromise) dans le fichier .pf (prefetched file)  
Deux éléments sont à l'origine de l'accélération du démarrage de Windows ou d'un programme par la mise en cache de certaines informations reauises (la prefetcher) et de l'analyse des traces recueillies par le prefetcher (la task scheduler). En conséquence, ils travaillent en synergie: il faut les démarrer/activer tous les deux pour le résultat escompté._

_Dans notre cas, puisqu'il s'agit de `Tor Browser`, inous recherchons le fichier PF portant son nom dans la liste de l'ensemble des fichiers .pf disponibles sur la machine suspectée de l'une des deux manières suivantes :_  
_- Soit se rendre à `Results` > `Extracted Content` > `Run Programs` puis clic droit sur n'importe quel fichier `.pf` et enfin `View Source File in Directory`_  
_- Soit se rendre directement à `001Win10.E01` > `vol3 (NTFS / exFAT (0x07): 104448-103830231)` > `Windows` > `Prefetch`_  

![prefetched 1](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/prefetch%201.png)


_Une fois ce fichier trouvé, nous observons la valeur `1` dans le paramètre `count` qui veut simplement dire, selon Wikipedia :  
0 = prélecture (prefietching) désactivée ;  
1 = prélecture activée pour les applications ;  
2 = prélecture activée pour Windows (option de défaut pour Windows XP uniquement7) ;  
3 = prélecture activée pour les applications et Windows (par défaut pour les versions Windows Vista et suivantes6)._  

_Grrrrrr ! Nous décidons alors de l'exporter et analyser le fichier excel généré. A notre grande surprise, aucune en-tête de ce fichier excel n'indique l'artifact recherché.
En ce moment, une seule conclusion : le flag vaut bien 0_

_flag_ :triangular_flag_on_post: = `0`

![prefetched 2](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/prefetch%202.png)
