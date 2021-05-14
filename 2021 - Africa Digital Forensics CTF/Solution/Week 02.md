## Week 2 (May 10 - 16) : RAM Image Analysis   

|  N°  | Intitulé du challenges        | Nombre de points  |      Outils utilisés              |
| -----| ------------------------------|:-----------------:| ---------------------------------:|
|   1  | Be Brave                      |         3         |                                   |
|   2  | Image Verification            |         3         |                                   |
|   3  | Let's Connect                 |         3         |                                   |
|   4  | RAM Acquisition Time          |         3         |                                   |
|   5  | Chrome Connection             |         6         |                                   |
|   6  | Hash Hash Baby                |         6         |                                   |
|   7  | Offset Select                 |         6         |                                   |
|   8  | Process Parents Please        |         6         |                                   |
|   9  | Finding Filenames             |         9         |                                   |
|  10  | Hocus Focus                   |         9         |                                   |
|  11  | Meetings                      |        12         |                                   |




# 1- Be Brave
---
![Be Brave](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Challenge%20Be%20Brave.PNG)

Nous allons afficher les informations des processus en cours d'exécution lors du dump de la mémoire RAM. 
Pour ce faire, nous pouvons utiliser l'un des trois plugins: `windows.pslist`, `windows.psscan` ou `windows.pstree`.  
1.a- Commençons par afficher les en-têtes de l'output du plugin que nous utiliserons à savoir `windows.pslist`.

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist | head 
```
![En-tête de plist](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/en-tete%20plist.png)

2.b- Nous pouvons enfin chercher le PID du processus demandé avec la commande

```console
┌──(root💀kali)-[~/Desktop/Forensics/volatility3]
└─# python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist | grep brave.exe
```
<!-- [spoiler] ![Flag](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/flag%20Be%20brave.png) [/spoiler] -->

Flag= <!--   [spoiler] 4856 [/spoiler] -->



# To do
---
- [x] Ecrire la trame du writeup
- [ ] Expliquer d'avantage chaque étape
- [ ] Afficher les flags en PlainText
- [ ] Inviter d'autres participants à partager leurs démarches si différentes de la mienne
