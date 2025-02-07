# Recomendaciones despues de instalar Ubuntu

* [Instalación completa](#instalación-completa)
* [Instalación individual](#instalación-individual)
  * [Paquetes necesarios](#paquetes-recomenados)
  * [Visual Studio Code](#vs-code)
  * [ZSH](#interprete-de-comandos-zsh)
  * [Arduino 2](#arduino-ide-2)
  * [ROS2](#ros2)
  * [Software Opcional](#software-opcional)
    * [Mission Center](#visor-de-uso-de-hardware-parecido-a-windows)
    * [Fastfetch](#fastfetch)
    * [Ranger](#gestor-de-archivos-desde-la-terminal-ranger)
* [Configuración](#configuración)

## Instalación completa
### Intalación de paquetes necesarios

```shell
sudo apt install curl clang software-properties-common wget power-profiles-daemon
```

* `curl`: Conexión a internet para transferir datos.
* `clang`: Compilador de C/C++/ObjetiveC...
* `software-properties-common`: Scripts para agregar fuentes de software extra.
* `wget`: Parecido a curl.
* `power-profiles-daemon`: Perfiles de energia.

### Agregar claves de los nuevos repositorios
#### ROS2 gpg key
```shell
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

#### VS Code gpg key 
```shell
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
```

### Agregar repositorios extra
#### Drivers de video
```shell
sudo add-apt-repository ppa:kisak/kisak-mesa
```

#### VS Code
```shell
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
```

#### Tienda de apps, Flatpak
```shell
sudo add-apt-repository ppa:flatpak/stable
```

#### Software variado
```shell
sudo add-apt-repository universe
```

#### ROS2 
```shell
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

#### Visor de info del sistema, fastfetch
```shell
sudo add-apt-repository ppa:fastfetch/stable
```

### Actualización del sistema
```shell
sudo apt update ; sudo apt upgrade
```

### [OPCIONAL] Cambio de interprete de comandos sell
[Wiki de ohmyzsh](https://github.com/ohmyzsh/ohmyzsh/wiki#getting-started)

#### Instalación de zsh y ohmyzsh
```shell
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

* `zsh`: Shell mejorada de la shell instalada por defecto bash.
* `ohmyzsh`: Framework para bustear zsh.

#### Cambio de shell
```shell
chsh -s /usr/bin/zsh
```

### Instalación de flatpack
```shell
sudo apt install flatpak gnome-software-plugin-flatpak
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

* `flatpak`: Software base.
* `gnome-software-plugin-flatpak`: Interfaz gráfica de la tienda de flatpak.

### Instalación de VS Code
```shell
sudo apt install code
```

### Instalación de Arduino IDE 2
```shell
flatpak install flathub cc.arduino.IDE2
```

#### Agregar permisos para usar los dispositivos `/dev/tty*`
```shell
sudo usermod -a -G dialout $USER
```

### Instalación de ROS2
#### Instalación de paquetes base
```shell
sudo apt install -y \
  python3-flake8-blind-except \
  python3-flake8-class-newline \
  python3-flake8-deprecated \
  python3-mypy \
  python3-pip \
  python3-pytest \
  python3-pytest-cov \
  python3-pytest-mock \
  python3-pytest-repeat \
  python3-pytest-rerunfailures \
  python3-pytest-runner \
  python3-pytest-timeout \
  ros-dev-tools
```

#### Descarga de ROS2
```shell
mkdir -p ~/ros2_jazzy/src
cd ~/ros2_jazzy
vcs import --input https://raw.githubusercontent.com/ros2/ros2/jazzy/ros2.repos src

sudo apt upgrade
```

#### Instalación de dependencias de ROS2
```shell
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```

#### Build ROS2
```
colcon build --symlink-install
```
> [!WARNING]
> Esta operación tarda bastante.

#### Iniciar las herramientas de ROS2 junto con el interprete de comandos
```shell
echo "source $HOME/ros2_jazzy/install/local_setup.bash" >> ~/.bashrc
echo "source $HOME/ros2_jazzy/install/local_setup.zsh" >> ~/.zshrc
```

> [!WARNING]
> Esto hará más tardado el inicio del interprete de comandos

### Software opcional
#### Visor de uso de hardware parecido a Windows
``` shell
flatpak install flathub io.missioncenter.MissionCenter
```

#### Visor de info del sistema en la terminal
```shell
sudo apt install fastfetch
```

#### Gestor de archivos desde la terminal
```shell
sudo apt install ranger
```

## Instalación individual
### Paquetes recomenados

```shell
sudo apt install curl clang software-properties-common wget power-profiles-daemon
```

#### Drivers de video
```shell
sudo add-apt-repository ppa:kisak/kisak-mesa
```

#### Software variado
```shell
sudo add-apt-repository universe
```

#### Tienda de apps, Flatpak
```shell
sudo add-apt-repository ppa:flatpak/stable
```

#### Actualización del sistema
```shell
sudo apt update ; sudo apt upgrade
```

#### Instalación de flatpack
```shell
sudo apt install flatpak gnome-software-plugin-flatpak
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

### VS Code

#### Agragar clave gpg de VS code
```shell
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
```

#### Agregar repositorio de VS Code
```shell
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
```

#### Actualización del sistema
```shell
sudo apt update ; sudo apt upgrade
```

#### Instalación de VS Code
```shell
sudo apt install code
```

### Interprete de comandos ZSH
[Wiki de zsh](https://github.com/ohmyzsh/ohmyzsh/wiki#getting-started)

#### Instalación de zsh
```shell
sudo apt install zsh
```

#### Instalación de ohmyzsh
```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### Cambio de shell
```shell
chsh -s /usr/bin/zsh
```







### Arduino IDE 2
```shell
flatpak install flathub cc.arduino.IDE2
```

#### Agregar permisos para usar los dispositivos `/dev/tty*`
```shell
sudo usermod -a -G dialout $USER
```


### ROS2
#### Agregar clave gpg de ROS2
```shell
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```


#### Agregar repositorio de ROS2 
```shell
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

#### Actualización del sistema
```shell
sudo apt update ; sudo apt upgrade
```



#### Instalación de paquetes necesarios por ROS2
```shell
sudo apt install -y \
  python3-flake8-blind-except \
  python3-flake8-class-newline \
  python3-flake8-deprecated \
  python3-mypy \
  python3-pip \
  python3-pytest \
  python3-pytest-cov \
  python3-pytest-mock \
  python3-pytest-repeat \
  python3-pytest-rerunfailures \
  python3-pytest-runner \
  python3-pytest-timeout \
  ros-dev-tools
```

#### Descarga de ROS
```shell
mkdir -p ~/ros2_jazzy/src
cd ~/ros2_jazzy
vcs import --input https://raw.githubusercontent.com/ros2/ros2/jazzy/ros2.repos src

sudo apt upgrade
```

#### Instalación de dependencias de ROS
```shell
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```

#### Build ROS
```
colcon build --symlink-install
```
> [!WARNING]
> Esta operación tarda bastante.

#### Agregar las herramientas de ROS2 a la configuración del interprete de comandos
```shell
echo "source $HOME/ros2_jazzy/install/local_setup.bash" >> ~/.bashrc
echo "source $HOME/ros2_jazzy/install/local_setup.zsh" >> ~/.zshrc
```

> [!WARNING]
> Esto hará más tardado el inicio del interprete de comandos

### Software opcional
#### Visor de uso de hardware parecido a Windows
``` shell
flatpak install flathub io.missioncenter.MissionCenter
```

#### Fastfetch
##### Agregar repositorio
```shell
sudo add-apt-repository ppa:fastfetch/stable
```

##### Instalar fastfetch
```shell
sudo apt install fastfetch
```

#### Gestor de archivos desde la terminal, ranger
```shell
sudo apt install ranger
```

## Configuración
### Extensiones para VS Code

1. `Ctrl + Shift + x` Para abrir la tienda de extensiones.
2. Instalar `Spanish Language` y `Python`.

|![](https://i.imgur.com/1Bg4QCs.png)|![](https://i.imgur.com/ieua0hs.png)|
|---|---|

### Configuraciones de Arduino
1. Cambiar el idioma de la IDE a español `File > Preferences > Languaje : español`
2. Instalar `esp32 by Espressif` desde el gestor de tarjetas de Arduino
![](https://i.imgur.com/3CAvWoM.png)

### Librerias de python
```shell
pip install colorama \
  progress \
  matplotlib \
  opencv-python \
  numpy \
  python-dotenv \
  pandas \
  PySide6 \
  toml \
  vtk \ 
  pyserial \
  pyperclip \
  pygame \
  notify_py 
```
