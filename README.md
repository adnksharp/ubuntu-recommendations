# Recomendaciones después de instalar Ubuntu

* [Instalación completa](#instalación-completa)
* [Instalación individual](#instalación-individual)
  * [Paquetes necesarios](#paquetes-necesarios)
  * [Visual Studio Code](#vs-code-1)
  * [ZSH](#interprete-de-comandos-zsh)
  * [Arduino 2](#arduino-ide-2)
  * [LaTex](#latex)
  * [ROS2](#ros2-1)
  * [Software Opcional](#software-opcional-1)
    * [Mission Center](#visor-de-uso-de-hardware-parecido-a-windows-1)
    * [Fastfetch](#fastfetch)
    * [Ranger](#gestor-de-archivos-desde-la-terminal-ranger)
* [Configuración](#configuración)
  * [VS Code](#extensiones-para-vs-code)
  * [Arduino](#configuraciones-de-arduino)
  * [Python](#librerías-de-python)
  * [Micro ROS](#uros)

## Instalación completa
### Instalación de paquetes necesarios

```shell
sudo apt install curl clang software-properties-common wget power-profiles-daemon
```

> [!NOTE]
> * `curl`: Conexión a internet para transferir datos.
> * `clang`: Compilador de C/C++/ObjetiveC...
> * `software-properties-common`: Scripts para agregar fuentes de software extra.
> * `wget`: Parecido a curl.
> * `power-profiles-daemon`: Perfiles de energia.

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

### [OPCIONAL] Cambio de intérprete de comandos sell
[Wiki de ohmyzsh](https://github.com/ohmyzsh/ohmyzsh/wiki#getting-started)

#### Instalación de zsh y ohmyzsh
```shell
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

> [!NOTE]
> * `zsh`: Shell mejorada de la shell instalada por defecto bash.
> * `ohmyzsh`: Framework para bustear zsh.

#### Cambio de shell
```shell
chsh -s /usr/bin/zsh
```

### Instalación de flatpack
```shell
sudo apt install flatpak gnome-software-plugin-flatpak
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

> [!NOTE]
> * `flatpak`: Software base.
> * `gnome-software-plugin-flatpak`: Interfaz gráfica de la tienda de flatpak.

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

### Instalación de LaTeX
```shell
sudo apt install latexmk texlive
```

> [!NOTE]
> * `texlive`: Paquete base de latex
> * `latexmk`: Auto-compilador de latex

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

#### Descarga de las herramientas de ROS2
```shell
sudo apt update && sudo apt install ros-dev-tools
```

#### Descarga de ROS2

<details><summary>Ubuntu 24</summary>

```shell
sudo apt install ros-jazzy-desktop
```
</details>
<details><summary>Ubuntu 22</summary>

```shell
sudo apt install ros-humble-desktop
```
</details>
<details><summary>Ubuntu 20</summary>

```shell
sudo apt install ros-foxy-desktop python3-argcomplete
```
</details>

#### Agregar las herramientas de ROS2 a la configuración del intérprete de comandos

Editar el archivo 

```shell
nano ~/.bashrc
```



```shell
nano ~/.zshrc
```

<details><summary>ROS2 Rolling</summary>

```shell
run-ros() 
{
     export ROS_DOMAIN_ID=49
     export ROS_VERSION=2
     export ROS_PYTHON_VERSION=3
     export ROS_DISTRO=rolling
     source /opt/ros/rolling/setup.zsh
}
```
</details>

<details><summary>ROS2 Jazzy</summary>

```shell
run-ros() 
{
     export ROS_DOMAIN_ID=41
     export ROS_VERSION=2
     export ROS_PYTHON_VERSION=3
     export ROS_DISTRO=jazzy
     source /opt/ros/jazzy/setup.zsh
}
```
</details>

<details><summary>ROS2 Humble</summary>

```shell
run-ros() 
{
     export ROS_DOMAIN_ID=41
     export ROS_VERSION=2
     export ROS_PYTHON_VERSION=3
     export ROS_DISTRO=humble
     source /opt/ros/humble/setup.zsh
}
```

</details>

<details><summary>ROS2 Foxy</summary>

```shell
run-ros() 
{
     export ROS_DOMAIN_ID=41
     export ROS_VERSION=2
     export ROS_PYTHON_VERSION=3
     export ROS_DISTRO=foxy
     source /opt/ros/foxy/setup.zsh
}
```
</details>

> [!NOTE]
> Con esto, al ejecutar `run-ros` se cargarán las herramientas de ros junto con las definiciones `ROS_DOMAIN_ID`, `ROS_VERSION`, etc.

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
### Paquetes necesarios

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

#### Agragar clave gpg de VS Code
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

### LaTeX
```shell
sudo apt install latexmk texlive
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
  python3-colcon-common-extensions \
  ros-dev-tools
```

#### Descarga de ROS

<details><summary>Ubuntu 24</summary>

```shell
sudo apt install ros-jazzy-desktop
```
</details>
<details><summary>Ubuntu 22</summary>

```shell
sudo apt install ros-humble-desktop
```
</details>
<details><summary>Ubuntu 20</summary>

```shell
sudo apt install ros-foxy-desktop python3-argcomplete
```
</details>


#### Agregar las herramientas de ROS2 a la configuración del intérprete de comandos

Editar el archivo 

```shell
nano ~/.bashrc
```



```shell
nano ~/.zshrc
```

<details><summary>ROS2 Rolling</summary>

```shell
run-ros() 
{
     export ROS_DOMAIN_ID=49
     export ROS_VERSION=2
     export ROS_PYTHON_VERSION=3
     export ROS_DISTRO=rolling
     source /opt/ros/rolling/setup.zsh
}
```
</details>

<details><summary>ROS2 Jazzy</summary>

```shell
run-ros() 
{
     export ROS_DOMAIN_ID=41
     export ROS_VERSION=2
     export ROS_PYTHON_VERSION=3
     export ROS_DISTRO=jazzy
     source /opt/ros/jazzy/setup.zsh
}
```
</details>

<details><summary>ROS2 Humble</summary>

```shell
run-ros() 
{
     export ROS_DOMAIN_ID=41
     export ROS_VERSION=2
     export ROS_PYTHON_VERSION=3
     export ROS_DISTRO=humble
     source /opt/ros/humble/setup.zsh
}
```

</details>

<details><summary>ROS2 Foxy</summary>

```shell
run-ros() 
{
     export ROS_DOMAIN_ID=41
     export ROS_VERSION=2
     export ROS_PYTHON_VERSION=3
     export ROS_DISTRO=foxy
     source /opt/ros/foxy/setup.zsh
}
```
</details>

> [!NOTE]
> Con esto, al ejecutar `run-ros` se cargarán las herramientas de ros junto con las definiciones `ROS_DOMAIN_ID`, `ROS_VERSION`, etc.

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

### Librerías de Python
```shell
pip install colorama progress matplotlib opencv-python numpy python-dotenv pandas PySide6 toml vtk pyserial pyperclip pygame notify_py 
```

### uROS
Tutorial completo de $$\mu$$ ROS en [micro.ros.org](https://micro.ros.org/docs/tutorials/core/first_application_linux/).

#### 1. Crear un espacio de trabajo de $$\mu$$ ROS

```shell
mkdir ~/uros_ws/
cd ~/uros_ws
```

#### 2. Clonar los repositorios de $$\mu$$ ROS

<details><summary>ROS2 Rolling</summary>
  <details><summary>shell bash</summary>
 
  ```shell
  source /opt/ros/rolling/setup.bash
  ```
  </details><details><summary>shell ZSH</summary>
       
  ```shell
  source /opt/ros/rolling/setup.zsh
  ```
  </details>
  
  ```shell
  git clone -b rolling https://github.com/micro-ROS/micro_ros_setup.git src/micro_ros_setup
  ```
</details>

<details><summary>ROS2 Jazzy</summary>
  <details><summary>shell bash</summary>
 
  ```shell
  source /opt/ros/jazzy/setup.bash
  ```
  </details><details><summary>shell ZSH</summary>
       
  ```shell
  source /opt/ros/jazzy/setup.zsh
  ```
  </details>
  
  ```shell
  git clone -b jazzy https://github.com/micro-ROS/micro_ros_setup.git src/micro_ros_setup
  ```
</details>

<details><summary>ROS2 Humble</summary>
  <details><summary>shell bash</summary>
 
  ```shell
  source /opt/ros/humble/setup.bash
  ```
  </details><details><summary>shell ZSH</summary>
       
  ```shell
  source /opt/ros/humble/setup.zsh
  ```
  </details>
  
  ```shell
  git clone -b humble https://github.com/micro-ROS/micro_ros_setup.git src/micro_ros_setup
  ```
</details>

<details><summary>ROS2 Foxy</summary>
  <details><summary>shell bash</summary>
 
  ```shell
  source /opt/ros/foxy/setup.bash
  ```
  </details><details><summary>shell ZSH</summary>
       
  ```shell
  source /opt/ros/foxy/setup.zsh
  ```
  </details>
  
  ```shell
  git clone -b foxy https://github.com/micro-ROS/micro_ros_setup.git src/micro_ros_setup
  ```
</details>

#### 3. Actualizar las dependencias

```shell
sudo apt update
rosdep update
rosdep install --from-path src --ignore-src -y
```

#### 4. Compilar $$\mu$$ ROS

```shell
colcon build
```

#### 5. Iniciar las herramientas de $$\mu$$ ROS

<details><summary>bash</summary>

```shell
source install/local_setup.bash
```
</details><details><summary>ZSH</summary>

```shell
source install/local_setup.zsh
```
</details>

#### 6. Construir el firmware

```shell
ros2 run micro_ros_setup build_firmware.sh
```

<details><summary>bash</summary>

```shell
source install/local_setup.bash
```
</details>
<details><summary>ZSH</summary>

```shell
source install/local_setup.zsh
```
</details>

#### 7. Crear el agente de $$\mu$$ ROS

```shell
ros2 run micro_ros_setup create_agent_ws.sh
ros2 run micro_ros_setup build_agent.sh
```

<details><summary>bash</summary>

```shell
source install/local_setup.bash
```
</details><details><summary>ZSH</summary>

```shell
source install/local_setup.zsh
```
</details>

#### 8. Descaargar las librerías de $$\mu$$ ROS

<details><summary>ROS2 Rolling</summary>

```shell
git clone -b rolling https://github.com/micro-ROS/micro_ros_arduino.git ~/Arduino/libraries/micro_ros_arduino
```
</details>

<details><summary>ROS2 Jazzy</summary>

```shell
git clone -b jazzy https://github.com/micro-ROS/micro_ros_arduino.git ~/Arduino/libraries/micro_ros_arduino
```
</details>

<details><summary>ROS2 Humble</summary>

```shell
git clone -b humble https://github.com/micro-ROS/micro_ros_arduino.git ~/Arduino/libraries/micro_ros_arduino
```

</details>

<details><summary>ROS2 Foxy</summary>

```shell
git clone -b foxy  https://github.com/micro-ROS/micro_ros_arduino.git ~/Arduino/libraries/micro_ros_arduino
```
</details>

#### 9. Cargar un ejemplo de Arduino

1. Abre el ejemplo `micro_ros_arduino_examples/publisher` en Arduino IDE 2.
2. Carga el ejemplo en la placa ESP32.

![](https://i.imgur.com/x3n5BMu.png)

#### 10. Ejecutar el agente de $$\mu$$ ROS

```shell
ros2 run micro_ros_agent micro_ros_agent serial --dev /dev/ttyUSB0
```

<details><summary>Dónde `/dev/ttyUSB0` es el puerto donde se conecta el ESP32.</summary>

```shell
ls /dev/tty* | grep -E 'USB|ACM|AMA'
```
</details>

La terminal debería mostrar algo como esto:

![](img/out1.svg)

#### 11. Verficar el tópico

```shell
ros2 node list
```

debería mostrar el nodo `micro_ros_arduino_node` (linea **55** del ejemplo).

```shell
ros2 topic list
```

debería mostrar el tópico `/micro_ros_arduino_node_publisher` (linea **62** del ejemplo).

#### 12. Escuchar el tópico
```shell
ros2 topic echo /micro_ros_arduino_node_publisher
```

mostrará los mensajes que envía el ESP32.

