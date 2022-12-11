# Basic Setup

Sinnvolle Voreinstellungen, damit das Arbeiten mit Linux mehr Spass macht.

## Mit SSH Arbeiten

Das Arbeiten im VirtualBox VM Fenster ist eher mühsam, da Copy-and-paste etc. nicht funktioniert.
Stattdessen empfiehlt es sich per SSH auf die VM zuzugreifen,
somit können wir lokal mit einem richtigen Terminal arbeiten.

Vorteile:

* Copy-and-paste funktioniert
* Nicht verpixelt, bessere Schrift, Schriftgrösse änderbar etc.
* Filetransfer in/nach VM möglich
* Mehrere Terminal-Fenster möglich

### Installation

SSH-Server in VM installieren:

```shell
# update package index
apt-get update

# ssh server installieren
apt-get install openssh-server
```

* Danach Port Forwarding in Virtualbox einrichten
* Beim Netzwerk-Adapter, NAT

```
Protokoll: TCP
Host-Port: 2222
Gast-Port: 22
```

### Benutzung

Ab da an kann aus einem lokalen PowerShell Terminal oder Putty (oder andere SSH Client Software) gearbeitet werden.

Verbinden:

```shell
ssh -l <username> -p 2222 127.0.0.1
```

### Optional

Falls SSH Login auch mit `root` funktionieren soll:

```shell
# configuration öffnen
nano /etc/ssh/sshd_config

# am ende folgendes setzen
PermitRootLogin yes

# ssh neustarten
service ssh restart
```

## Sudo einrichten

Es empfiehlt sich, nicht mit dem `root`-User zu arbeiten,
sondern mit einem eigenen User und `sudo` zu verwenden,
falls `root`-Rechte benötigt werden. Empfohlen weil:

* Möglicherweise an der Prüfung auch so
* Kann weniger kaputt gehen
* Haben wir im Vorbereitungskurs auch so gemacht

### Installation

Sudo installieren:

```shell
# update package index
apt-get update

# sudo installieren
apt-get install sudo
```

Bestehender User zur Sudo Gruppe hinzufügen:

```shell
usermod -a -G sudo <user>
```

### Benutzung

Nicht als `root` Arbeiten.

Befehl ausführen, ohne `root`-Rechte:

```shell
<befehl>
```

Befehl ausführen, welcher `root`-Rechte benötigt:

```
sudo <befehl> 
```

### Benutzerrechte

Falls ihr Dateien mit `root` angelegt habt könnt ihr die Benutzerrechte mit chown anpassen und mit `mv <Datei> <Ziel>` in das User Verzeichniss verschieben.
```shell
chown <user>:<gruppe> <datei>
```

# SFTP Filetransfer

Zum Übertragen von Dateien und zum verstehen (Grafisch darstellen) des Dateisystems kann ein FTP Programm hilfreich sein, dieses muss allerdings SFTP Unterstützen.

* [FileZilla](https://filezilla-project.org/)
* [WinSCP](https://winscp.net/eng/download.php)

Wichtig hierbei, das Protocol muss auf `SFTP` gestellt sein und die restlichen Einstellungen genau wie bei SSH (Host: `127.0.0.1` Port: `2222`)
