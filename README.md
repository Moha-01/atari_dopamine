# atari_dopamine https://github.com/google/dopamine#getting-started

## Getting Started
1. Ubuntu 22.04.1 LTS installieren
2. sudo apt list --upgradeable
3. sudo apt update
4. sudo apt upgrade
5. https://docs.docker.com/engine/install/ubuntu/
6. https://github.com/git-guides/install-git
7. Install Python
8. Install pip
9. Install from pip -> cmake
10. apt-get install -y zlib1g-dev
11. apt-get install -y zlib1g
12. pip install atari_py
13. https://github.com/openai/atari-py#roms
14. python3 -m atari_py.import_roms /home/mohamed/Downloads/Roms.rar
15. pip install ale-py
16. sudo apt-get install unrar
17. mkdir ROMS
18. apt install unrar
19. unrar e /home/mohamed/Downloads/Roms.rar /atari_dopamine/ROMS/
20. ale-import-roms ./ROMS/


## Installing from Source
1. git clone https://github.com/google/dopamine
2. pip install -r dopamine/requirements.txt

## Running Tests
1. cd dopamine
2. export PYTHONPATH=$PYTHONPATH:$PWD
3. python -m tests.dopamine.atari_init_test

# Docker Setup
## Core Image
1. cd /atari_dopamine/dopamine
2. docker build -f docker/core/Dockerfile -t dopamine/core .


## Atari Image
1. mkdir /root/roms
2. cp /home/mohamed/Downloads/Roms.rar /root/roms/
3. apt install rar
4. apt install zip unzip
5. zip -r /root/roms/ROMS.zip /root/roms/ROMS
6. 

## Mögliche Komplett Lösung:
- https://gitee.com/ford25v6/dopamine/blob/master/README.md#https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Fgoogle%2Fdopamine%2Fblob%2Fmaster%2Fdopamine%2Fdiscrete_domains%2Ftrain.py

- https://github.com/llSourcell/Google_Dopamine_LIVE/blob/master/Google_Dopamine_(LIVE)%20(1).ipynb























 
