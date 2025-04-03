# Anleitung zum Einrichten eines SSH-Servers

## Voraussetzungen

- Eine Hyper-V-VM oder eine andere Quelle (z. B. Root-Server)
- Eine Maschine mit Windows oder Linux (in diesem Beispiel wird Linux Debian 12 ohne GUI verwendet)

## 1. Linux installieren

Falls noch nicht geschehen, installiere eine Linux-Distribution (z. B. Debian 12) auf der VM oder dem Server.

Download: [Debian 12](https://www.debian.org/distrib/netinst)

Wählt dort die Minimal-Installation (i386) aus, um eine Basisinstallation ohne GUI zu erhalten.

![Debian installation](debian.gif)

## 2. SSH-Server einrichten

### Installation und Grundkonfiguration

1. Installiere die Maschine ohne GUI, aber mit SSH-Server (Pakete können später geändert werden).  
    **Hinweis:** Kein Root-Passwort vergeben, stattdessen einen Benutzer mit Sudo-Rechten nutzen.

2. Nach der Installation und Verbindung mit der Shell:
     - **System aktualisieren:**
        ```bash
        sudo apt update && sudo apt upgrade -y
        ```
     - **IP-Adresse des Servers ermitteln:**
        ```bash
        ip a
        ```
        Beispiel: `172.168.xxx.xxx` (notieren).  
        **Hinweis:** Eine Anleitung zur Festlegung einer statischen IP-Adresse folgt später.

## 3. VS Code einrichten

### Remote-SSH-Verbindung herstellen

1. **VS Code öffnen und Remote-SSH-Erweiterung installieren:**
    - Erweiterung: *Remote - SSH (Microsoft)*

2. **SSH-Verbindung einrichten:**
    - `STRG + SHIFT + P` → `Remote-SSH: Connect to Host` → `ENTER`
    - `Add new SSH Host` auswählen.
    - Eingabe: `BENUTZERNAME@172.168.xxx.xxx` (Benutzername und IP-Adresse des Servers).
    - Verbindung bestätigen und Passwort des Server-Benutzers eingeben.
    - Konfiguration speichern.

3. **Verbindung herstellen:**
    - Nach erfolgreicher Einrichtung erscheint eine Benachrichtigung → *Verbinden* klicken.
    - Alternativ:  
      - `STRG + SHIFT + P` → `Remote-SSH: Connect Current Window to Host`
      - IP-Adresse auswählen, bestätigen und Passwort eingeben.

4. **Verbindung prüfen:**
    - Unten links in VS Code sollte ein blaues Fenster mit `SSH: 172.168.xxx.xxx` angezeigt werden.

5. **Verbindung trennen:**
    - `STRG + SHIFT + P` → `Remote-SSH: Kill Local Connection for Host`
    - IP-Adresse auswählen und bestätigen.


## 3. Arbeiten mit dem SSH-Server in VS Code

Nach erfolgreicher Verbindung stehen ein SSH-Terminal und ein Datei-Explorer zur Verfügung. Der Server (VM) wird als Ressource genutzt.

### Beispiel: Neues Projekt erstellen

1. **Projektverzeichnis anlegen:**
    ```bash
    mkdir -p ~/workspace/Projekt
    cd ~/workspace
    ```
    Alternativ: Über den Datei-Explorer in VS Code einen neuen Ordner oder eine Datei erstellen.

2. **Python und venv installieren:**
    ```bash
    sudo apt install python3 python3-pip -y
    sudo apt install python3.11-venv
    ```

3. **Virtuelle Umgebung erstellen und aktivieren:**
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```

4. **Django installieren und Projekt starten:**
    ```bash
    pip install django djangorestframework
    django-admin startproject testprojekt .
    ```

5. **Server testen:**
    ```bash
    python manage.py runserver
    ```
    Falls erforderlich, mit `0.0.0.0:8000` testen.  
    Im Browser: `localhost:8000` oder `IP_DES_SERVERS:8000`.  
    **Hinweis:** Der Server-Localhost wird per SSH getunnelt, sodass er lokal erreichbar ist.

---

**Glückwunsch!** Der SSH-Server ist erfolgreich eingebunden und einsatzbereit.

---

### Credits
- [DulliLabs](https://github.com/DulliLabs) : Text und Idee
- [20Max01](https://github.com/20Max01) : Installations-GIF