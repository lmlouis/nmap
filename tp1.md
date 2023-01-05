## Security scanner 

pré-requis:
* Kali Linux
* nmap 

## Utilisation de nmap 

Utilisation: nmap [Type(s) de scan] [Options] {spécifications des cibles}

sudo nmap [ adresse reseau ]/n 

etat de la machine :
up : active 
down : fermée 
filtred :  filtrée


les IDS permettent de detecter les tentatives d'intrusion, une methode pour savoir si on quelqu'un essaye de s'introduire dans ta machine on peut verifier les fichiers log/journaux de son réseaux un agent malveillant aura tendance à vouloir scanner plusieurs ports ou tout le réseau 

## commandes réseaux 
```
$ nmap 
```


```
$ nmap | more
```

```
$ ifconfig
```
## trouver les hotes connnecté à un réseau 
```
$ nmap [adresse Réseau]/[adresse logique]
```
exemple : 
```
$ nmap 172.16.174.0/24 
```
## Techniques de scan de ports [click me](https://nmap.org/man/fr/man-examples.html)
-sS(Scan TCP SYN) : renvoie PORT, STATE, SERVICE
```
$ sudo nmap -sS scanme.nmap.org
[sudo] password for kali: 
Sorry, try again.
[sudo] password for kali: 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-18 10:30 EDT
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.20s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
514/tcp   filtered shell
9929/tcp  open     nping-echo
31337/tcp open     Elite

Nmap done: 1 IP address (1 host up) scanned in 4.15 seconds
```

-sT(Scan TCP connect())

-sU(Scan UDP)

-sP(scan Ping) :  scanner l'adresse ip de la machine 
```
$ nmap -sP 45.33.32.156
Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-18 10:18 EDT
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.24s latency).
Nmap done: 1 IP address (1 host up) scanned in 0.37 seconds
```
-sV : Version du programme du service active on donc detecter les version des programmes vulnérables 
```
$ sudo nmap -sV scanme.nmap.org
Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-18 10:38 EDT
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (1.1s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE    VERSION
22/tcp    open     ssh        OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
80/tcp    open     http       Apache httpd 2.4.7 ((Ubuntu))
514/tcp   filtered shell
9929/tcp  open     nping-echo Nping echo
31337/tcp open     tcpwrapped
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


ajouter sudo si necessaire (sudo)
voir les port tcp 

```
nmap -sS [ adresse cible ]
```
Le scan SYN (-sS) identifie un port qui doit être ouvert en envoyant un "SYN" à la cible. Si elle reçoit un SYN-ACK ou un SYN, elle marque ce port comme étant ouvert. S'il reçoit un "RST", il marque le port comme étant filtré (filtered), c'est à dire inaccessible à cause d'un pare-feu par exemple.
 
voir les port udp 

```
nmap -sU [ adresse cible ]
```

exemple : 

```
sudo nmap -sS 172.16.174.235
```
```
sudo nmap -sU 172.16.174.235
```

-Pn 
sudo nmap -Pn 172.16.174.*


-sL (Liste simplement): List Scan - Liste simplement les cibles à scanner

-sP(Scan ping): Ping Scan - Ne fait que déterminer si les hôtes sont en ligne -P0: Considère que tous les hôtes sont en ligne -- évite la découverte des hôtes

-PN (Pas de scan ping): Considérer tous les hôtes comme étant connectés -- saute l'étape de découverte des hôtes


machine de test 
```
$ ping  scanme.nmap.org

```

```
$ nmap scanme.nmap.org 
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-18 10:06 EDT
    Nmap scan report for scanme.nmap.org (45.33.32.156)
    Host is up (0.20s latency).
    Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
    Not shown: 995 closed tcp ports (conn-refused)
    PORT      STATE    SERVICE
    22/tcp    open     ssh
    80/tcp    filtered http
    514/tcp   filtered shell
    9929/tcp  open     nping-echo
    31337/tcp open     Elite

    Nmap done: 1 IP address (1 host up) scanned in 28.60 seconds

```



# Les bases du scan de ports

Même si le nombre de fonctionnalités de Nmap a considérablement augmenté au fil des ans, il reste un scanner de ports efficace, et cela reste sa fonction principale. La commande de base nmap <target> scanne plus de 1 660 ports TCP de l'hôte <target>. Alors que de nombreux autres scanners de ports ont partitionné les états des ports en ouverts ou fermés, Nmap a une granularité bien plus fine. Il divise les ports selon six états: ouvert (open), fermé (closed), filtré (filtered), non-filtré (unfiltered), ouvert|filtré (open|filtered), et fermé|filtré (closed|filtered).

Ces états ne font pas partie des propriétés intrinsèques des ports eux-mêmes, mais décrivent comment Nmap les perçoit. Par exemple, un scan Nmap depuis le même réseau que la cible pourrait voir le port 135/tcp comme ouvert alors qu'un scan au même instant avec les mêmes options au travers d'Internet pourrait voir ce même port comme filtré.
Les six états de port reconnus par Nmap

## ouvert (open)

    Une application accepte des connexions TCP ou des paquets UDP sur ce port. Trouver de tels ports est souvent le but principal du scan de ports. Les gens soucieux de la sécurité savent pertinemment que chaque port ouvert est un boulevard pour une attaque. Les attaquants et les pen-testers veulent exploiter ces ports ouverts, tandis que les administrateurs essaient de les fermer ou de les protéger avec des pare-feux sans gêner leurs utilisateurs légitimes. Les ports ouverts sont également intéressants pour des scans autres que ceux orientés vers la sécurité car ils indiquent les services disponibles sur le réseau.
## fermé (closed)

    Un port fermé est accessible (il reçoit et répond aux paquets émis par Nmap), mais il n'y a pas d'application en écoute. Ceci peut s'avérer utile pour montrer qu'un hôte est actif (découverte d'hôtes ou scan ping), ou pour la détection de l'OS. Comme un port fermé est accessible, il peut être intéressant de le scanner de nouveau plus tard au cas où il s'ouvrirait. Les administrateurs pourraient désirer bloquer de tels ports avec un pare-feu, mais ils apparaîtraient alors dans l'état filtré décrit dans la section suivante.
## filtré (filtered)

    Nmap ne peut pas toujours déterminer si un port est ouvert car les dispositifs de filtrage des paquets empêchent les paquets de tests (probes) d'atteindre leur port cible. Le dispositif de filtrage peut être un pare-feu dédié, des règles de routeurs filtrants ou un pare-feu logiciel. Ces ports ennuient les attaquants car ils ne fournissent que très peu d'informations. Quelques fois ils répondent avec un message d'erreur ICMP de type 3 code 13 (« destination unreachable: communication administratively prohibited »), mais les dispositifs de filtrage qui rejettent les paquets sans rien répondre sont bien plus courants. Ceci oblige Nmap à essayer plusieurs fois au cas où ces paquets de tests seraient rejetés à cause d'une surcharge du réseau et pas du filtrage. Ceci ralenti terriblement les choses.
## non-filtré (unfiltered)

    L'état non-filtré signifie qu'un port est accessible, mais que Nmap est incapable de déterminer s'il est ouvert ou fermé. Seul le scan ACK, qui est utilisé pour déterminer les règles des pare-feux, catégorise les ports dans cet état. Scanner des ports non-filtrés avec un autre type de scan, comme le scan Windows, SYN ou FIN peut aider à savoir si un port est ouvert ou pas.
## ouvert|filtré (open|filtered)

    Nmap met dans cet état les ports dont il est incapable de déterminer l'état entre ouvert et filtré. Ceci arrive pour les types de scans où les ports ouverts ne renvoient pas de réponse. L'absence de réponse peut aussi signifier qu'un dispositif de filtrage des paquets a rejeté le test ou les réponses attendues. Ainsi, Nmap ne peut s'assurer ni que le port est ouvert, ni qu'il est filtré. Les scans UDP, protocole IP, FIN, Null et Xmas catégorisent les ports ainsi.
## fermé|filtré (closed|filtered)

    Cet état est utilisé quand Nmap est incapable de déterminer si un port est fermé ou filtré. Cet état est seulement utilisé par le scan Idle basé sur les identifiants de paquets IP.


nmap -sP  : port TCP:UDP di host up ( verifier si la machine est up)

-p : verifie si le port est ouvert dans la machine cible 
```
$ nmap -p 80,443 45.33.32.156
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-18 10:52 EDT
    Nmap scan report for scanme.nmap.org (45.33.32.156)
    Host is up (0.27s latency).

    PORT    STATE  SERVICE
    80/tcp  open   http
    443/tcp closed https

    Nmap done: 1 IP address (1 host up) scanned in 0.58 seconds
```
savoir si les port 80,443 sont ouvert, de voir les version de façon agressive
```
$ sudo nmap -sV -p 80,443  45.33.32.156 -A
    Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-18 10:56 EDT
    Nmap scan report for scanme.nmap.org (45.33.32.156)
    Host is up (0.047s latency).

    PORT    STATE  SERVICE VERSION
    80/tcp  open   http    Apache httpd 2.4.7 ((Ubuntu))
    |_http-title: Go ahead and ScanMe!
    |_http-favicon: Nmap Project
    |_http-server-header: Apache/2.4.7 (Ubuntu)
    443/tcp closed https
    Aggressive OS guesses: Linux 3.2 (94%), Linux 4.4 (93%), Actiontec MI424WR-GEN3I WAP (93%), DD-WRT v24-sp2 (Linux 2.4.37) (92%), Microsoft Windows XP SP3 (90%), Microsoft Windows XP SP3 or Windows 7 or Windows Server 2012 (90%), BlueArc Titan 2100 NAS device (87%), TiVo series 1 (Sony SVR-2000 or Philips HDR112) (Linux 2.1.24-TiVo-2.5, PowerPC) (87%), TiVo series 1 (Linux 2.1.24-TiVo-2.5) (85%)
    No exact OS matches for host (test conditions non-ideal).
    Network Distance: 2 hops

    TRACEROUTE (using port 80/tcp)
    HOP RTT     ADDRESS
    1   0.45 ms 172.16.174.2
    2   0.26 ms scanme.nmap.org (45.33.32.156)

    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 23.52 seconds

```

Les différents types de scannings de ports -> [click me](http://babin.nelly.free.fr/scan.htm)


172.16.174.0/24
172.16.174.*
172.16.174.1-50

decouvrir les machines des host dans le reseau 
nmap -sP net@
     -sL       : utilise le service dns 
     -
