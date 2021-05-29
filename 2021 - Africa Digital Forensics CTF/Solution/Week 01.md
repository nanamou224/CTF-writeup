# Week 1 (May 03 - 09) : Forensic Disk Image 

### :balloon: Mise en situation  
_Un cybercrime vient d'être commis quelque part dans le monde sur une entité. En tant qu'Analyste Cybersécurité (DFIR), le First Responder vient nous remettre [l'image du disque de l'ordinateur](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Ressources/Africa-DFIRCTF-2021-WK01_archive.torrent) impliqué dans cette cyberattaque. Notre mission, si nous l'acceptons, sera de résoudre les challenges ci-dessous afin de retracer les évènements du malveillant et collecter le maximum de preuves contre lui._   


### :balloon: Préparation de nos outils d'analyse
_Le rôle du First responder était d'acquérir les images. Il a bien dû utiliser des outils à cet effet comme `FTK Imager` qui reste l'un des plus populaires dans ce domaine. C'est un outil graphique (GUI) facilitant l'acquisition de la copie du disque dur, du dump de la RAM et l'extraction de la MFT ou encore des fichiers supprimés..._   

_Aussi, dans l'analyse d'artifacts (traces d'attaque) sur des disques durs, des smatphones et clés USB, `Autopsy` et `Sleuth Kit` sont sans doute les outils les plus utilisés. Le premier est un outil graphique (GUI) tandis que le second est un outil en ligne de commande (CLI)._  

_Pour les challenges de la semaine 1 (Week 1), nous utiliserons principalement `FTK Imager` et `Autopsy` sous `Windows` à cause de leur inteface intuitive facilitant la navigation dans les différents dossiers/fichiers du disque dur à analyser._   

_Commencez par télécharger et installer [FTK Imager](https://accessdata.com/product-download/ftk-imager-version-4-5) et [Autopsy](https://www.autopsy.com/download/) depuis leurs sites officiels sur votre OS Windows. Ensuite, en le supposant dans le même repertoire que les 14 autres fichiers nommés `001Win10.E0k` avec k=1, 2, ... 15, chargez uniquement l'image disque `001Win10.E01` de la semaine 1 (Week 1) dans chacun des deux outils `FTK Imager` et `Autopsy`. Enfin, nous sommes prêts à débuter notre investigation numérique sur cette image disque fournie._  

_Ci-dessous les challenges à résoudre pour cette semaine 1 (Week 1)._  

|  N°  | Intitulé du challenge    | Nombre de points  | Outils utilisés          |        Statut         |
| -----| -------------------------|:-----------------:| ------------------------:| ---------------------:|
|   1  | Server Connection        |         3         | Imagination, Autopsy     | :heavy_check_mark:    |
|   2  | Suspect Disk Hash        |         3         | Imagination, Autopsy     | :heavy_check_mark:    |
|   3  | Web Search               |         3         | Imagination, Autopsy     | :heavy_check_mark:    |
|   4  | Possible Location        |         6         | Imagination, Autopsy     | :heavy_check_mark:    |
|   5  | Tor Browser              |         6         | Imagination, Autopsy     | :heavy_check_mark:    |
|   6  | User Email               |         6         | Imagination, Autopsy     | :heavy_check_mark:    |
|   7  | Web Scanning             |         6         | Imagination, Autopsy     | :heavy_check_mark:    |
|   8  | Copy Location            |         9         | Imagination, Autopsy     | :heavy_check_mark:    |
|   9  | Hash Cracker             |         9         | Imagination, Autopsy     | :heavy_check_mark:    |
|  10  | User Password            |        12         | Imagination, Autopsy     | :heavy_check_mark:    |

### :balloon: Résolution des challenges
**Notez bien**: Une bonne habitude est de commencer toujours par faire le tour des fichiers mis à disposition !  

## :one: Suspect Disk Hash  
>What is the MD5 hash value of the suspect disk?  

_Pour les plus anglophones d'entre vous (:stuck_out_tongue:), on demande le hash en MD5 du disque dur physique suspecté (disque dur original) et non celui de la copie ce disque dur (image du disque dur)!_  

_En regardant dans l'image disque dur à disposition, nous voyons 15 fichiers compressés (format image `EWF` très riche et sollicité en forensics) nommés de la forme `001Win10.E0k` avec k=1, 2, ... 15. Ces fichiers représentent, chacun, une partie du disque dur physique suspecté._  

_Nous allons donc dégainer notre outil capable de lire, interpréter et extraire intelligemment les informations utiles contenues dans le format `E01` que j'ai appélé `FTK Imager`, nous verrons que nous pourrons également le faire avec `Autopsy`._  


_Le plus souvent, c'est la parition 2 d'une image au format `E01` qui va nous intéressera car c'est elle qui contient le plus d'information (voir sa taille spécifiée entre crochet dans l'arbre d'évidence `FTK Imager` mais pour d'autres outils CLI, on est obligé de calculer son offset en posant `offset=512*taille_partition_1`)._


_:zap: **Méthode 1**: Utilisation de `FTK Imager`  
1- Sélectionnez l'image disque `001Win10.E01` dans l'arbre d'évidence sur la fénêtre d'à gauche     
2- Allez dans `File` > `Verify Drive/Image...`_   
![`File`>`Verify Drive`](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Hash%20Hard%20Disk%20FTK%20Imager.png)  

_3- Notez le hash MD5 générer après vérification de la source/image disque `001Win10.E01`    
![ hash MD5](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Suspect%20Disk%20Hash.png)  

flag :triangular_flag_on_post: = `430d0f91dc30b6c6de407ad622f12427`_ 

_:zap: **Méthode 1**: Utilisation de `Autopsy`   
Voir le module `Hash Lookup` de `Autopsy`  
