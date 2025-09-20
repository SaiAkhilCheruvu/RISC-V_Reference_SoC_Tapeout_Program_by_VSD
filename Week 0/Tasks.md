
# 📆 WEEK 0



Welcome to **Week 0** of the **RISC-V Reference SoC Tapeout Program** by **VLSI System Design (VSD)!** Here are the tasks completed this week:

  1.Created the GitHub repository to track progress.

2.Installed and set up Yosys, Iverilog, GTKWave, NGSpice, and Magic on Ubuntu 24.04.3 LTS.

3.Installed essential tools like Python3, Docker, Make, and Git.

4.Confirmed all tools were installed and working properly.

5.Added installation screenshots to the repository.

## 💻 Virtual Machine (VM) Specifications:

The latest version of Oracle Virtual Machine was installed to replicate the Ubuntu interface, with the following specifications chosen to balance performance and resource efficiency for smooth simulation and compilation:

**Operating System:** Ubuntu 24.04+

**RAM:** 6GB

**vCPUs:** 4

<img width="709" height="369" alt="Oracle VM VirtualBox Manager 20-09-2025 19_28_26" src="https://github.com/user-attachments/assets/0e6c3b35-3f37-4a42-8d0c-e1e170ec72ef" />


**Storage:** 50GB HDD

<img width="709" height="369" alt="Oracle VM VirtualBox Manager 20-09-2025 19_28_50" src="https://github.com/user-attachments/assets/836fe6cb-c2a3-481a-8a5b-15f398c7aaba" />


## 📥 Installation And Verification Of The Tools:

The following software tools were installed to support the different phases of hardware design, including synthesis, simulation, waveform analysis, circuit evaluation, and layout creation:


### 1. Yosys:
->  Open-source software for synthesizing and optimizing Verilog RTL designs. It transforms high-level hardware descriptions into gate-level netlists for digital chip implementation.

```
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make (If make is not installed please install it)
$ sudo apt-get install build-essential clang bison flex \
 libreadline-dev gawk tcl-dev libffi-dev git \
 graphviz xdot pkg-config python3 libboost-system-dev \
 libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ make
$ sudo make install 
```
### Yosys - Setup Confirmation:

<img width="816" height="340" alt="Yosys" src="https://github.com/user-attachments/assets/11e89ee7-1a5c-48cb-abc5-395f33b5cfe1" />


### 2. Iverilog:
-> A widely-used open-source simulator that compiles and runs Verilog code, helping verify digital designs through simulation before hardware implementation.
```
$ sudo apt-get update
$ sudo apt-get install iverilog
```
### Iverilog - Setup Confirmation:

<img width="815" height="388" alt="Iverilog" src="https://github.com/user-attachments/assets/fff6bb51-7d30-4147-ad47-527af6d6cd36" />


### 3. GTKWave:

-> A graphical waveform viewer that allows detailed visualization and analysis of simulation output, aiding in debugging and verification of digital signals.
```
$ sudo apt update
$ sudo apt install gtkwave
```
### GTKWave - Setup Confirmation:

<img width="1280" height="800" alt="gtkwave" src="https://github.com/user-attachments/assets/f97b08d7-a2b0-4bcd-b305-834276bf57cc" />


### 4. Ngspice:
-> An advanced circuit simulator that performs detailed analog and mixed-signal analysis, essential for validating circuit behavior and performance.

```
$ sudo apt update
$ sudo apt install ngspice
```
### Ngspice - Setup Confirmation:

<img width="815" height="360" alt="ngspice" src="https://github.com/user-attachments/assets/227c3f9a-f1be-45f1-9250-248af321dc84" />


### 5. Magic VLSI:

-> An interactive tool used for designing, editing, and verifying physical integrated circuit layouts, facilitating the translation from design to manufacturable chip structures.

```
$ sudo apt-get install m4
$ sudo apt-get install tcsh
$ sudo apt-get install csh
$ sudo apt-get install libx11-dev
$ sudo apt-get install tcl-dev tk-dev
$ sudo apt-get install libcairo2-dev
$ sudo apt-get install mesa-common-dev libglu1-mesa-dev
$ sudo apt-get install libncurses-dev

$ git clone https://github.com/RTimothyEdwards/magic
$ cd magic
$ ./configure
$ make
$ sudo make install
```
### Magic VLSI - Setup Confirmation:

<img width="1280" height="800" alt="magic vlsi" src="https://github.com/user-attachments/assets/37fdfd49-9698-4a4b-964e-f4763d9b9283" />


### 6. OpenLane and Docker:
-> OpenLane is an automated open-source ASIC design flow for creating chip layouts, and Docker provides a containerized environment to run OpenLane smoothly and consistently across different systems.

```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt install -y build-essential python3 python3-venv python3-pip make git

$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o
/usr/share/keyrings/docker-archive-keyring.gpg

$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg]
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee
/etc/apt/sources.list.d/docker.list > /dev/null

$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io
$ sudo docker run hello-world
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ sudo reboot

$ docker run hello-world
```
### Docker - Setup Confirmation:

<img width="1280" height="800" alt="docker" src="https://github.com/user-attachments/assets/5846f140-69b4-4da6-9cfa-d6155ced0da9" />


### Version Check:
```
$ git --version
$ docker --version
$ python3 --version
$ python3 -m pip --version
$ make --version
```
<img width="820" height="443" alt="versions1" src="https://github.com/user-attachments/assets/a5c8b722-09b8-4312-96f8-bc49606568a7" />


```
$ python3 -m venv -h 
```

<img width="1280" height="800" alt="versions2" src="https://github.com/user-attachments/assets/8a3f8749-d09f-46f4-9a5f-e75914379080" />


## 📜 Installation Summary

 **Yosys - Complete (✔️)**

 **Iverilog - Complete (✔️)**

 **GTKWave - Complete (✔️)**

 **Ngspice - Complete (✔️)**

 **Magic VLSI - Complete (✔️)**

 **OpenLane - Complete (✔️)**

 **Docker - Complete (✔️)**


**With all necessary tools successfully installed and verified, the environment is now fully set up and ready for efficient development and testing.**  🚀 📈 
