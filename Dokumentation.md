# Studienarbeit WS 2022/23

## Allgemeine Informationen

- Autor: Mohamed Haji
- Matrikelnummer: 8528264
- Studiengang: Angewandte Informatik
- Kurs: MOS-TINF20B
- Betreuer: Prof. Dr. Carsten Müller

<br>

## Title

### _Realisierung einer Lernumgebung basierend auf Google Dopamine für Reinforcement Learning_

<br>

## Beschreibung

Auf Basis von Google Dopamine für Reinforcement Learning (https://github.com/google/dopamine) ist
eine Lernumgebung für die schnelle prototypische Konzeptionierung/Implementierung von Algorithmen
zu realisieren. Der Nachweis der Leistungsfähigkeit
erfolgt mit dem Szenario "Atari". Setup, Betrieb und Tutorials sind detailliert zu dokumentieren.

<br>

## Equipment

Nvidia Jetson AGX Xavier Developer Kit (https://developer.nvidia.com/embedded/jetson-agx-xavier-developer-kit)

<br>

## Aufgaben

- [x] Setup NVIDIA Jetson AGX mit JetPack
- [ ] Setup Docker Ubuntu Linux (22.04.1 LTS) auf NVIDIA Jetson AGX
- [ ] Setup Dopamine auf Ubuntu Linux (22.04.1 LTS) in dem erstellten Docker Container
- [ ] Detaillierte Dokumentation der Installation/Konfiguration sowie Nutzung unter Microsoft Windows.
- [ ] Setup Szenario "Atari"
- [ ] Fortschritt des Trainings ist in geeigneter Form zu protokollieren und zu visualisieren.
- [ ] Erstellung Tutorial für das Szenario „Atari“ für Studierende der Angewandten Informatik
- [ ] Ein Studierender der Angewandten Informatik soll das Szenario „Atari“ bauen, das Training durchführen und durch Logging/Visualisierung die Ergebnisse interpretieren.
- [ ] Technische Dokumentation der Lernumgebung

<br>

# JetPack

<div align="center">
  <img src="https://s2.www.theimagingsource.com/application-1.6438.89739/img/embedded_vision/nvidia_jetpack.png" style="height: 150px; width: 200px"><br><br>
</div>

NVIDIA JetPack SDK ist die umfassendste Lösung zum Erstellen von durchgängig beschleunigten KI-Anwendungen. JetPack bietet eine vollständige Entwicklungsumgebung für hardwarebeschleunigte KI-at-the-Edge-Entwicklung. Alle Jetson-Module und Entwicklerkits werden von JetPack unterstützt. Mit JetPack kann man KI-Anwendungen auf dem Jetson-Modul oder -Entwicklerkit ausführen, ohne dass man sich um die Installation von Treibern, Software oder Tools kümmern muss.

<br>

## Allgemein

Für das Optimale Zugriff auf Nvidia Jetson Software & Services, ist hierfür eine Registrierung erforderlich. Die Registrierung erfolgt über die Webseite https://developer.nvidia.com/developer-program

Für die Installation von JetPack wird Ubuntu 20.04.1 LTS verwendet. Diese läuft in eine Virtuelle Machine (VM) auf einem Windows 10 Host. Die VM wird mit VMware Workstation 15 Pro erstellt. Die VM hat 16GB RAM 200GB Festplattenspeicher und 4 CPU Cores.

<br>

## Download

Über den Link (nach Anmeldung) https://developer.nvidia.com/drive/sdk-manager wird JetPack SDK heruntergeladen.

<br>

## Installation

```
cd /home/mohamed/Downloads
sudo apt install ./sdkmanager_1.9.0-10816_amd64.deb
```

<br>

## SDK Manager starten

- Das Jetson AGX Xavier Developer Kit wird mit dem USB-C Kabel an den Host angeschlossen.
- Dann in force Recovery Mode gebracht. Dies geschieht durch Drücken der Recovery Taste und Power On/Off Taste auf der Platine.
- Leuchtet die Power LED auf, ist das Board im Recovery Mode.
- Dann das anschließen des USB-C Kabels an den Host zu der VM leiten.

Nun kann im Terminal folgender Befehl ausgeführt werden:

```
sdkmanager
```

Dann erscheint das SDK Manager Fenster. Hierfür ist eine Anmeldung erforderlich. Diese erfolgt mit den Zugangsdaten, welche bei der Registrierung auf der Webseite https://developer.nvidia.com/developer-program angegeben wurden. Nach der Anmeldung kann nun die Installation von JetPack gestartet werden.

### Step 1:

SDK Manager erkennt die Hardware und zeigt diese an.

<div align="center">
  <img src="./images/nvidia/Step 1.png" ><br><br>
</div>

JetPack Version 5.0.2 ist automatisch ausgewählt. Diese Version wird verwendet. Dann auf Continue to Step 2 klicken.

<br>

### Step 2:

Die zu installierenden Komponenten auswählen.

<div align="center">
  <img src="./images/nvidia/Step 2.png" ><br><br>
</div>

Die Komponenten werden in der Regel automatisch ausgewählt. Hier noch die SDK Components raus machen (Im Abbidung noch ausgewählt). Danach ein häkchen bei "I accept to the terms and conditions" setzen und auf "Continue to Step 3" klicken.
Es wird gefordert den Passwort einzugeben, um mit der Installation weiter zu machen.

<br>

### Step 3:

Nun werden die ausgewählten Komponenten heruntergeladen und installiert.

<div align="center">
  <img src="./images/nvidia/Step 3.png" ><br><br>
</div>

<br>

### Step 3.1:

Flash Optionen auswählen.

<div align="center">
  <img src="./images/nvidia/Flash Options 1.png" ><br><br>
</div>

Hier wird die Flash Option ausgewählt.

- Username: dhbw
- Password: dhbw

Dann auf "Flash" klicken.

<br>

Die Installation dauert ca. 1 Stunde.
Nun kann man über den Jetson AGX Xavier Developer Kit bedienen.
Mit den oben erstellten Benutzerdaten sich anmelden und die Installation von JetPack ist abgeschlossen.

<br>

## Update der Software über dem Terminal

```
sudp apt update
sudo apt list --upgradable
sudo apt upgrade
reboot
```

# Docker

<div align="center">
  <img src="https://www.docker.com/wp-content/uploads/2022/03/vertical-logo-monochromatic.png"  style="height: 150px; width: 200px"><br><br>
</div>

Docker ist eine Software zur Erstellung, Ausführung und Verteilung von Anwendungen in Containern. Docker-Container sind isolierte Umgebungen, in denen Anwendungen laufen. Sie enthalten alles, was sie zum Laufen brauchen: Code, Laufzeitumgebung, Systemwerkzeuge und Bibliotheken. Docker-Container sind Plattformunabhängig, sodass sie auf jedem Betriebssystem laufen, auf dem Docker installiert ist. Docker-Container können leicht erstellt, verteilt und aktualisiert werden.

<br>

## Entferne alte Versionen

```
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Es ist OK, wenn diese Befehle Fehler ausgeben.

<br>

## Installation durch die Repository

### 1. Repository aufsetzen

#### Aktualisiere des Paketindex und Installation der Pakete, die Docker benötigt, um über HTTPS Pakete aus dem Repository zu installieren:

```
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

#### Füge Docker’s offiziellen GPG-Schlüssel hinzu:

```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker-archive-keyring.gpg
```

#### Setze die stabile Repository auf:

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 2. Installation Docker Engine

#### Aktualisiere das Paketindex und installiere die neueste Version von Docker Engine und containerd, oder gehe zum nächsten Schritt, um eine bestimmte Version zu installieren:

```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose plugin
```

#### Starte Docker:

```
sudo systemctl start docker
```

#### Führe den Docker-Hello-World-Container aus, um sicherzustellen, dass Docker ordnungsgemäß installiert wurde:

```
dhbw@ubuntu:~$ sudo docker run hello-world
dhbw@ubuntu:~$ sudo docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

<br>

# Dopamine

<div align="center">
  <img src="https://google.github.io/dopamine/images/dopamine_logo.png"><br><br>
</div>

Dopamine ist ein Framework für die Entwicklung von Deep Reinforcement Learning (DRL) Agenten. Es wurde von Google Brain entwickelt und ist Open Source. Dopamine ist in Python geschrieben und nutzt TensorFlow als Backend. Dopamine ist modular aufgebaut und kann für verschiedene Anwendungsfälle verwendet werden. Es bietet eine Reihe von Agenten, die für verschiedene Anwendungsfälle entwickelt wurden. Dopamine ist auch mit OpenAI Gym kompatibel und kann mit allen Gym-Umgebungen verwendet werden.

Link: https://github.com/google/dopamine#getting-started

<br>

# Setup Lernumgebung

## 1. Erstellen eines neuen Ordners

```
mkdir projekt-dopamine
cd projekt-dopamine
```

## 2. Roms für Atari 2600 herunterladen

```bash
sudo apt-get install wget -y
apt-get install unrar -y
mkdir roms
cd roms
wget http://www.atarimania.com/roms/Roms.rar
unrar x Roms.rar
cd ..
```

## 3. Erstellen einer Dockerfile (Core)

```bash
mkdir core
cd core
touch Dockerfile
```

### 3.1. Dockerfile

```dockerfile
ARG cuda_docker_tag="11.2.2-cudnn8-devel-ubuntu20.04"
FROM nvidia/cuda:${cuda_docker_tag}

COPY . /root/dopamine/

RUN apt-get update
# tzdata is required below. To avoid hanging, install it first.
RUN DEBIAN_FRONTEND="noninteractive" apt-get install tzdata -y
RUN apt-get install git wget libgl1-mesa-glx -y

# Install python3.8.
RUN apt-get install software-properties-common -y
RUN add-apt-repository ppa:deadsnakes/ppa -y
RUN apt-get install python3.8 -y

# Make python3.8 the default python.
RUN rm /usr/bin/python3
RUN ln -s /usr/bin/python3.8 /usr/bin/python3
RUN ln -s /usr/bin/python3.8 /usr/bin/python
RUN apt-get install python3-distutils -y

# Install pip.
RUN wget https://bootstrap.pypa.io/get-pip.py
RUN python get-pip.py
RUN rm get-pip.py

# Install Dopamine dependencies.
RUN pip install -r /root/dopamine/requirements.txt

# Install JAX for GPU, overriding requirements.txt.
RUN pip install --upgrade "jax[cuda111]" -f https://storage.googleapis.com/jax-releases/jax_releases.html

WORKDIR /root/dopamine

```

```bash
cd ..
```

## 4. Erstellen einer Dockerfile (Atari)

```bash
mkdir atari
cd atari
touch Dockerfile
```

### 4.1. Dockerfile

```dockerfile
ARG base_image=dopamine/core
FROM ${base_image}

# Copy ROMs into the image.
RUN mkdir /root/roms
COPY ./Roms.rar /root/roms/

RUN apt-get install rar unzip -y
RUN rar x /root/roms/Roms.rar /root/roms/

# Install ROMs with ale-py.
RUN pip install atari_py ale-py
RUN python -m atari_py.import_roms /root/roms
RUN ale-import-roms /root/roms/ROMS

```

```bash
cd ..
```

## 5. Builden der Docker Images

```bash
docker build -f ./core/Dockerfile -t dopamine/core .
docker build -f ./atari/Dockerfile -t dopamine/atari $ROM_DIR
```

## 6. Starten der Container

```bash
docker run -d -t --network=host -it dopamine/atari bash
```

## 7. Starten des Trainings

```bash
cd /root/dopamine
python -um dopamine.discrete_domains.train \
  --base_dir /tmp/dopamine_runs \
  --gin_files dopamine/agents/dqn/configs/dqn.gin

```
