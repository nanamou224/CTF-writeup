# Week 1 (May 03 - 09) : Forensic Disk Image 

### Mise en situation  
_Un cybercrime vient d'être commis quelque part dans le monde sur une entité. En tant qu'Analyste Cybersécurité (DFIR), le First Responder vient nous remettre [l'image du disque de l'ordinateur](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Ressources/Africa-DFIRCTF-2021-WK01_archive.torrent) impliqué dans cette cyberattaque. Notre mission, si nous l'acceptons, sera de résoudre les challenges ci-dessous afin de retracer les évènements du malveillant et collecter le maximum de preuves contre lui._   


### Préparation de nos outils d'analyse
_Le rôle du First responder était d'acquérir les images. Il a bien dû utiliser des outils à cet effet comme `FTK Imager` qui reste l'un des plus populaires dans ce domaine. C'est un outil graphique (GUI) facilitant l'acquisition de la copie du disque dur, du dump de la RAM et l'extraction de la MFT ou encore des fichiers supprimés..._   

_Aussi, dans l'analyse d'artifacts (traces d'attaque) sur des disques durs, des smatphones et clés USB, `Autopsy` et `Sleuth Kit` sont sans doute les outils les plus utilisés. Le premier est un outil graphique (GUI) tandis que le second est un outil en ligne de commande (CLI)._  

_Pour les challenges de la semaine 1 (Week 1), nous utiliserons principalement `FTK Imager` et `Autopsy` sous `Windows` à cause de leur inteface intuitive facilitant la navigation dans les différents dossiers/fichiers du disque dur à analyser._   

_Commencez par télécharger et installer [FTK Imager](https://accessdata.com/product-download/ftk-imager-version-4-5) et [Autopsy](https://www.autopsy.com/download/) depuis leurs sites officiels sur votre OS Windows. Ensuite, chargez l'image disque de la semaine 1 (Week 1) dans chacun des deux outils `FTK Imager` et `Autopsy`. Enfin, nous sommes prêts à débuter notre investigation numérique sur cette image disque fournie._  

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

**Notez bien**: Une bonne habitude est de commencer toujours par faire le tour des fichiers mis à disposition !  

## 1- Suspect Disk Hash
> ####Enoncé  
> What is the MD5 hash value of the suspect disk?  
Pour les plus anglophones d'entre vous (:stuck_out_tongue:), on demande le hash en MD5 de l'image disque (fichier 

 
