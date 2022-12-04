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

- [X] Setup NVIDIA Jetson AGX mit JetPack
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
sudo docker run hello-world
```

<br>

# Dopamine

<div align="center">
  <img src="https://google.github.io/dopamine/images/dopamine_logo.png"><br><br>
</div>

Dopamine is a research framework for fast prototyping of reinforcement learning
algorithms. It aims to fill the need for a small, easily grokked codebase in
which users can freely experiment with wild ideas (speculative research).

Our design principles are:

- _Easy experimentation_: Make it easy for new users to run benchmark
  experiments.
- _Flexible development_: Make it easy for new users to try out research ideas.
- _Compact and reliable_: Provide implementations for a few, battle-tested
  algorithms.
- _Reproducible_: Facilitate reproducibility in results. In particular, our
  setup follows the recommendations given by
  [Machado et al. (2018)][machado].

In the spirit of these principles, this first version focuses on supporting the
state-of-the-art, single-GPU _Rainbow_ agent ([Hessel et al., 2018][rainbow])
applied to Atari 2600 game-playing ([Bellemare et al., 2013][ale]).
Specifically, our Rainbow agent implements the three components identified as
most important by [Hessel et al.][rainbow]:

- n-step Bellman updates (see e.g. [Mnih et al., 2016][a3c])
- Prioritized experience replay ([Schaul et al., 2015][prioritized_replay])
- Distributional reinforcement learning ([C51; Bellemare et al., 2017][c51])

For completeness, we also provide an implementation of DQN
([Mnih et al., 2015][dqn]).
For additional details, please see our
[documentation](https://github.com/google/dopamine/tree/master/docs).

This is not an official Google product.

## What's new

- **30/01/2019:** Dopamine 2.0 now supports general discrete-domain gym
  environments.
- **01/11/2018:** Download links for each individual checkpoint, to avoid
  having to download all of the checkpoints.
- **29/10/2018:** Graph definitions now show up in Tensorboard.
- **16/10/2018:** Fixed a subtle bug in the IQN implementation and upated
  the colab tools, the JSON files, and all the downloadable data.
- **18/09/2018:** Added support for double-DQN style updates for the
  `ImplicitQuantileAgent`.
  - Can be enabled via the `double_dqn` constructor parameter.
- **18/09/2018:** Added support for reporting in-iteration losses directly from
  the agent to Tensorboard.
  - Set the `run_experiment.create_agent.debug_mode = True` via the
    configuration file or using the `gin_bindings` flag to enable it.
  - Control frequency of writes with the `summary_writing_frequency`
    agent constructor parameter (defaults to `500`).
- **27/08/2018:** Dopamine launched!

## Instructions

### Install via source

Installing from source allows you to modify the agents and experiments as
you please, and is likely to be the pathway of choice for long-term use.
These instructions assume that you've already set up your favourite package
manager (e.g. `apt` on Ubuntu, `homebrew` on Mac OS X), and that a C++ compiler
is available from the command-line (almost certainly the case if your favourite
package manager works).

The instructions below assume that you will be running Dopamine in a _virtual
environment_. A virtual environment lets you control which dependencies are
installed for which program; however, this step is optional and you may choose
to ignore it.

Dopamine is a Tensorflow-based framework, and we recommend you also consult
the [Tensorflow documentation](https://www.tensorflow.org/install)
for additional details.

Finally, these instructions are for Python 2.7. While Dopamine is Python 3
compatible, there may be some additional steps needed during installation.

#### Ubuntu

First set up the virtual environment:

```
sudo apt-get update && sudo apt-get install virtualenv
virtualenv --python=python2.7 dopamine-env
source dopamine-env/bin/activate
```

This will create a directory called `dopamine-env` in which your virtual
environment lives. The last command activates the environment.

Then, install the dependencies to Dopamine. If you don't have access to a
GPU, then replace `tensorflow-gpu` with `tensorflow` in the line below
(see [Tensorflow instructions](https://www.tensorflow.org/install/install_linux)
for details).

```
sudo apt-get update && sudo apt-get install cmake zlib1g-dev
pip install absl-py atari-py gin-config gym opencv-python tensorflow-gpu
```

During installation, you may safely ignore the following error message:
_tensorflow 1.10.1 has requirement numpy<=1.14.5,>=1.13.3, but you'll have
numpy 1.15.1 which is incompatible_.

Finally, download the Dopamine source, e.g.

```
git clone https://github.com/google/dopamine.git
```

#### Mac OS X

First set up the virtual environment:

```
pip install virtualenv
virtualenv --python=python2.7 dopamine-env
source dopamine-env/bin/activate
```

This will create a directory called `dopamine-env` in which your virtual
environment lives. The last command activates the environment.

Then, install the dependencies to Dopamine:

```
brew install cmake zlib
pip install absl-py atari-py gin-config gym opencv-python tensorflow
```

During installation, you may safely ignore the following error message:
_tensorflow 1.10.1 has requirement numpy<=1.14.5,>=1.13.3, but you'll have
numpy 1.15.1 which is incompatible_.

Finally, download the Dopamine source, e.g.

```
git clone https://github.com/google/dopamine.git
```

#### Running tests

You can test whether the installation was successful by running the following:

```
export PYTHONPATH=${PYTHONPATH}:.
python tests/dopamine/atari_init_test.py
```

The entry point to the standard Atari 2600 experiment is
[`dopamine/discrete_domains/train.py`](https://github.com/google/dopamine/blob/master/dopamine/discrete_domains/train.py).
To run the basic DQN agent,

```
python -um dopamine.discrete_domains.train \
  --base_dir=/tmp/dopamine \
  --gin_files='dopamine/agents/dqn/configs/dqn.gin'
```

By default, this will kick off an experiment lasting 200 million frames.
The command-line interface will output statistics about the latest training
episode:

```
[...]
I0824 17:13:33.078342 140196395337472 tf_logging.py:115] gamma: 0.990000
I0824 17:13:33.795608 140196395337472 tf_logging.py:115] Beginning training...
Steps executed: 5903 Episode length: 1203 Return: -19.
```

To get finer-grained information about the process,
you can adjust the experiment parameters in
[`dopamine/agents/dqn/configs/dqn.gin`](https://github.com/google/dopamine/blob/master/dopamine/agents/dqn/configs/dqn.gin),
in particular by reducing `Runner.training_steps` and `Runner.evaluation_steps`,
which together determine the total number of steps needed to complete an
iteration. This is useful if you want to inspect log files or checkpoints, which
are generated at the end of each iteration.

More generally, the whole of Dopamine is easily configured using the
[gin configuration framework](https://github.com/google/gin-config).

#### Non-Atari discrete environments

We provide sample configuration files for training an agent on Cartpole and
Acrobot. For example, to train C51 on Cartpole with default settings, run the
following command:

```
python -um dopamine.discrete_domains.train \
  --base_dir=/tmp/dopamine \
  --gin_files='dopamine/agents/rainbow/configs/c51_cartpole.gin'
```

You can train Rainbow on Acrobot with the following command:

```
python -um dopamine.discrete_domains.train \
  --base_dir=/tmp/dopamine \
  --gin_files='dopamine/agents/rainbow/configs/rainbow_acrobot.gin'
```

### Install as a library

An easy, alternative way to install Dopamine is as a Python library:

```
# Alternatively brew install, see Mac OS X instructions above.
sudo apt-get update && sudo apt-get install cmake
pip install dopamine-rl
pip install atari-py
```

Depending on your particular system configuration, you may also need to install
zlib (see "Install via source" above).

#### Running tests

From the root directory, tests can be run with a command such as:

```
python -um tests.agents.rainbow.rainbow_agent_test
```

### References

[Bellemare et al., _The Arcade Learning Environment: An evaluation platform for
general agents_. Journal of Artificial Intelligence Research, 2013.][ale]

[Machado et al., _Revisiting the Arcade Learning Environment: Evaluation
Protocols and Open Problems for General Agents_, Journal of Artificial
Intelligence Research, 2018.][machado]

[Hessel et al., _Rainbow: Combining Improvements in Deep Reinforcement Learning_.
Proceedings of the AAAI Conference on Artificial Intelligence, 2018.][rainbow]

[Mnih et al., _Human-level Control through Deep Reinforcement Learning_. Nature, 2015.][dqn]

[Mnih et al., _Asynchronous Methods for Deep Reinforcement Learning_. Proceedings
of the International Conference on Machine Learning, 2016.][a3c]

[Schaul et al., _Prioritized Experience Replay_. Proceedings of the International
Conference on Learning Representations, 2016.][prioritized_replay]

### Giving credit

If you use Dopamine in your work, we ask that you cite our
[white paper][dopamine_paper]. Here is an example BibTeX entry:

```
@article{castro18dopamine,
  author    = {Pablo Samuel Castro and
               Subhodeep Moitra and
               Carles Gelada and
               Saurabh Kumar and
               Marc G. Bellemare},
  title     = {Dopamine: {A} {R}esearch {F}ramework for {D}eep {R}einforcement {L}earning},
  year      = {2018},
  url       = {http://arxiv.org/abs/1812.06110},
  archivePrefix = {arXiv}
}
```

[machado]: https://jair.org/index.php/jair/article/view/11182
[ale]: https://jair.org/index.php/jair/article/view/10819
[dqn]: https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf
[a3c]: http://proceedings.mlr.press/v48/mniha16.html
[prioritized_replay]: https://arxiv.org/abs/1511.05952
[c51]: http://proceedings.mlr.press/v70/bellemare17a.html
[rainbow]: https://www.aaai.org/ocs/index.php/AAAI/AAAI18/paper/download/17204/16680
[iqn]: https://arxiv.org/abs/1806.06923
[dopamine_paper]: https://arxiv.org/abs/1812.06110
