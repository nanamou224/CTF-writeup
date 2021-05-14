## Week 2 (May 10 - 16) : RAM Image Analysis  
Sommaire des challenges


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





![Week 02 challenges](https://github.com/nanamou224/CTF-writeup/blob/main/2021%20-%20Africa%20Digital%20Forensics%20CTF/Screenshots/Week%2002%20-%20RAM%20Analysis.png)





week 02.  


Le dump memoire etant de windows 10, mous allons utiliser la version adeauate de volatility3 et pas volatility 2.6s
volatility3 détecte automatiquement le profil poru nous, pqs lq peine de le trqiner qvec nous

git clone https://github.com/volatilityfoundation/volatility3.git
cd volatility3
python3 setup.py install
python3 vol.py —h




Image Verification
--------------------------
What is the SHA256 hash value of the RAM image?
M1. regarder dans le fichier hashed.hd fourni
M2. sha256sum 20210430-Win10Home-20H2-64bit-memdump.mem

flag=9db01b1e7b19a3b2113bfb65e860fffd7a1630bdf2b18613d206ebf2aa0ea172


Be Brave
----------------
What is the process ID of "brave.exe"?
python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist.Pslist

flag=









RAM Acquisition Time
---------------------------------
What time was the RAM image acquired according to the suspect system? (YYYY-MM-DD HH:MM:SS)

python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.info.Info
SystemTime      2021-04-30 17:52:19
flag=2021-04-30 17:52:19




Let's Connect
-----------------------
How many established network connections were there at the time of acquisiton? (number)
python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.netscan.NetScan | grep ESTABLISHED | wc -l
10ogress:  100.00               PDB scanning finished

python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.netstat.NetStat | grep ESTABLISHED | wc -l

flag=10



Be Brave
--------------------
What is the process ID of "brave.exe"?
python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist.PsList | grep brave.exe

flag=4856



Process Parents Please
---------------------------------------
What is the creation date and time of the parent process of "powershell.exe"? (YYYY-MM-DD HH:MM:SS)
python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist.PsList

5096	4352	powershell.exe	0xbf0f6d97f2c0	12	-	1	False	2021-04-30 17:51:19.000000 	N/A	Disabled
4352	4296	explorer.exe	0xbf0f6ca662c0	82	-	1	False	2021-04-30 17:39:48.000000 	N/A	Disabled

flag=2021-04-30 17:39:48



Chrome Connection
--------------------
What domain does Chrome have an established network connection with?
python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.netscan.NetScan | grep ESTABLISHED

0xbf0f6c7104d0  TCPv4   10.0.2.15       49778   185.70.41.130   443     ESTABLISHED     1840    chrome.exe      2021-04-30 17:45:00.000000 
                                                                                                                                                                                     
on voit aue chrome essaie detablir une connexion avec une IP publique 185.70.41.130  sur le port 443.
Il suffit de pinguer cette IP ou de l'ouvrir dqns le navigateur

flag=mail.protonmail.com


Hash Hash Baby
--------------
What is the MD5 hash value of process memory for PID 6988?
python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.pslist.PsList --pid 6988 --dump
md5sum pid.6988.0x1c0000.dmp 

flag=0b493d8e26f03ccd2060e0be85f430af


Offset Select
----------------
What is the word starting at offset 0x45BE876 with a length of 6 bytes?
Un editeur hexqdecimql est loutil qui nous permet de voir lq correspondqnce Hexadecimal et ASCII

Utiliser lediteur hexa HxD 
Charger le dump memoire et faire Rechercher > Atteindre > Coller la chaine "45BE876" > ok
Lire le debut de notre offset "45BE87"en hexa a gauche et selectionner la chaine "68 61 63 6B 65 72" correspondante pour voir surligner le flag a droite

flag=hacker


Finding Filenames
-------------------
What is the full path and name of the last file opened in notepad?
python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.cmdline.CmdLine --pid 2520 


2520    notepad.exe    C:\Windows\system32\NOTEPAD.EXE" C:\Users\JOHNDO~1\AppData\Local\Temp\7zO4FB31F24\accountNum

flag=C:\Windows\system32\NOTEPAD.EXE" C:\Users\JOHNDO~1\AppData\Local\Temp\7zO4FB31F24\accountNum


Hocus Focus
-------------
How long did the suspect use Brave browser? (#:##:##)
$python3 vol.py -f ../Africa-DFIRCTF-2021-WK02/20210430-Win10Home-20H2-64bit-memdump.mem windows.registry.userassist.UserAssist | grep Brave > time

Resultats de la commande
Hive Offset	Hive Name	Path	Last Write Time	Type	Name	ID	Count	Focus Count	Time Focused	Last Updated	Raw Data

* 0xa80333cda000	\??\C:\Users\John Doe\ntuser.dat	ntuser.dat\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA}\Count	2021-04-30 17:52:18.000000 	Value	%ProgramFiles%\BraveSoftware\Temp\GUM20E0.tmp\BraveUpdate.exe	N/A	0	0	0:00:03.531000	N/A	

* 0xa80333cda000	\??\C:\Users\John Doe\ntuser.dat	ntuser.dat\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA}\Count	2021-04-30 17:52:18.000000 	Value	%ProgramFiles%\BraveSoftware\Update\BraveUpdate.exe	N/A	0	1	0:00:24.797000	N/A	

* 0xa80333cda000	\??\C:\Users\John Doe\ntuser.dat	ntuser.dat\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA}\Count	2021-04-30 17:52:18.000000 	Value	Brave	N/A	9	22	4:01:54.328000	2021-04-30 17:48:45.000000 	

* 0xa80333cda000	\??\C:\Users\John Doe\ntuser.dat	ntuser.dat\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{F4E57C4B-2036-45F0-A9AB-443BCFE33D9F}\Count	2021-04-30 17:51:18.000000 	Value	C:\Users\Public\Desktop\Brave.lnk	N/A	8	0	0:00:00.508000	2021-04-30 17:48:45.000000 	


L'avant derniere ligne * contient le flag
flag=4:01:54



WK02 CODE WORD
-----------------
Watch the week 2 hint videos, and listen for the CODE WORD.
Le flag est dicté à 20:53 dans la video https://www.youtube.com/watch?v=swYUA_9c11I&ab_channel=DFIR.Science
flag=volatile





Meetings
---------------
Using the disk image from week 01 and the RAM image from week 02 answer the following:
What place and country will the suspect go to on 2021-06-13?

Ouvrir le dump disk du week01 avec volatility.

File Types > By Extension > Documents > PDF 
Extraire tous les PDFs, on remarque que le seul lisible est "29152-almanac-start-a-garden.pdf"
L'ouvrir et lire les coordonnées GPS en bas de la page 12/21

2021-06-13 17°55'27.4"S 25°51'26.4"E

Coller "17°55'27.4"S 25°51'26.4"E" sur Google et boom

flag= victoria falls
