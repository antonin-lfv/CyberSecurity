---
description: Liste non exhaustive
---

# 📀 - Tools

## Nmap

C'est un outil d'exploration réseau et scanneur de ports/sécurité. La documentation est [ici](https://nmap.org/man/fr/).

Cette commande fait un scan du site, avec les ports ouverts etc.

```bash
$ nmap -A IP
```

Cette commande cherche si ce site contient des vulnérabilités connues.

```bash
$ nmap --script=vuln IP
```



## SearchSploit

[Documentation](https://www.exploit-db.com/searchsploit)

Cette commande cherche des vulnérabilités connues d'une technologie, exemple :

```bash
$ searchsploit lighttpd 1.4.
```



## MetaSploit

[Documentation](https://docs.rapid7.com/metasploit/getting-started/)

On lance la console Metasploit :

```bash
$ msfconsole
```

On cherche des vulnérabilités connues d'une technologie avec :

```bash
$ search lighttpd
```



## DirBuster

[Documentation](https://tools.kali.org/web-applications/dirbuster)

Grâce à dirbuster, on va scanner les fichiers du site, pour y accéder :

```bash
$ dirbuster
```

Après ça, une fenêtre s'ouvrira pour lancer le scan

