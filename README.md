# Setup de Simulador Gazebo + Coex clover Rodando Nativamente

Esse tutorial tem como objetivo ensinar a rodar a simulação **Gazebo** junto com o Framework **Clover** e **ROS**

##### Todas as instruções a seguir foram tiradas de:

- [Coex Clover - Native setup ](https://clover.coex.tech/en/simulation_native.html)

- [Manual de Instalação oficial do ROS](https://wiki.ros.org/noetic/Installation/Ubuntu)

- [Conectar-se ao GitHub com o SSH](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh)

<br>

Para Prosseguir com esse tutorial antes se deve cumprir certos Requisitos:

- [Ubuntu 20.04](ubuntu.md)
- [Git clone por ssh (Recomendado)](Git_config.md)
- 7 Gigas Livres em Disco

> [!NOTE]  
> Voce pode Clicar para seguir esses links para poder ser redirecionado para o tópico

# Instalar o ROS

### Passo 1

Primeiro se deve configurar os repositórios "restricted", "universe" e "multiverse" rodando os seguintes comandos.

```bash
sudo add-apt-repository universe
sudo add-apt-repository multiverse
sudo add-apt-repository restricted
sudo apt update
```

- Caso o comando retorne esse erro: ` add-apt-repository: command not found` tente os seguintes comandos:

```bash
sudo apt-get update
sudo apt-get install software-properties-common
```

- e em seguida rode os comandos do índice 1

<br>

### Passo 2

1. Agora devemos configurar seu computador para aceitar software dos packages.ros.org:

<br>

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

2. Adicionar as Chaves

```bash
sudo apt install curl # Se você ainda não tiver o curl baixado
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

<br>

### Passo 3

- Instalar ROS

primeiro, tenha certeza que seus indices de pacotes estão atualizados:

```bash
sudo apt update
```

Rode o comando para instalação do ROS:

- Desktop-Full Install: (Recomendado): Tudo na versão desktop mas as ferramentas 2d/3d de simulação e pacotes de percepção

```bash
sudo apt install ros-noetic-desktop-full
```

mais informações em [wiki.ros.org](https://wiki.ros.org/noetic/Installation/Ubuntu)

<br>

### Passo 4

- Configuração do seu ambiente
  O seguinte comando irá escrever no caminho de configuração do seu terminal o source do ROS, para que toda vez que o for aberto o terminal ele inicie junto.

**bash**

```bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

**Zsh**

```zsh
echo "source /opt/ros/noetic/setup.zsh" >> ~/.zshrc
source ~/.zshrc
```

- Dependências para compilar os pacotes:

  Até agora você instalou o que precisa para executar os pacotes ROS principais. há várias ferramentas e requisitos que são distribuídos separadamente. Por exemplo, rosinstall é uma ferramenta usada com frequência que permite que você baixe facilmente árvores de origem para pacotes ROS.

  Para instalar esta ferramenta e outras dependências para compilar pacotes ROS, execute em seu terminal:

  ```bash
  sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
  ```

  - Inicialização do rosdep

  O rosdep permite-lhe instalar facilmente as dependências do sistema para o código fonte que pretende compilar e é necessário para executar alguns componentes principais no ROS. Se você ainda não instalou o rosdep, faça-o da seguinte forma.

  ```bash
  sudo apt install python3-rosdep
  ```

  Com o seguinte, é possível inicializar o rosdep.

  ```bash
  sudo rosdep init
  rosdep update
  ```

---

<br>

# Criando workspace ROS para a simulação

1. Os Seguintes comandos na seguinte ordem:

- cria um diretório chamado `catkin_ws` no diretório `home (~)` e, dentro dele, cria um subdiretório `src`

- muda o diretório de trabalho atual para o diretório `catkin_ws` que acabamos de criar

- compila um espaço de trabalho Catkin no ROS,adiciona uma linha ao final do arquivo` ~/.bashrc`, que é o script de inicialização do bash

- E recarrega o arquivo `~/.bashrc` para que as alterações feitas sejam aplicadas :

```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin_make
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Agora vamos clonar as fontes do **Clover**:

```bash
cd ~/catkin_ws/src
git clone --depth 1 https://github.com/CopterExpress/clover
git clone --depth 1 https://github.com/CopterExpress/ros_led
git clone --depth 1 https://github.com/ethz-asl/mav_comm
```

> [!IMPORTANT]  
> Caso esteja sofrendo erros ao clonar os repositórios leia o tópico [Git_config](Git_config.md)

2. Instale todas as dependências usando `rosdep`:

```bash
cd ~/catkin_ws
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src -y
```

3. Instale as dependências do Python:

```bash
sudo /usr/bin/python3 -m pip install -r ~/catkin_ws/src/clover/clover/requirements.txt
```

# Clonar as fontes do PX4

- O PX4 será compilado juntamente com os outros pacotes no nosso espaço de trabalho. Você pode cloná-lo diretamente no espaço de trabalho ou colocá-lo em algum lugar e fazer um link simbólico para `~/catkin_ws/src`. Nós precisaremos colocar seus submódulos `sitl_gazebo` e `mavlink` em `~/catkin_ws/src` também.

Clonar as fontes do PX4 na `home` e criar as ligações simbólicas necessárias:

```bash
cd ~
git clone --recursive --depth 1 --branch v1.12.3 https://github.com/PX4/PX4-Autopilot.git ~/PX4-Autopilot
ln -s ~/PX4-Autopilot ~/catkin_ws/src/
ln -s ~/PX4-Autopilot/Tools/sitl_gazebo ~/catkin_ws/src/
ln -s ~/PX4-Autopilot/mavlink ~/catkin_ws/src/
```

> [!TIP]
> Você pode tentar utilizar uma versão mais recente do PX4, mas nesse caso, algo pode não funcionar como esperado.

> [!WARNING]  
> Caso esteja sofrendo erros ao clonar os repositórios é imprescindível que se leia o tópico [Git_config](Git_config.md)

# Instalar os pré-requisitos do PX4

O PX4 vem com o seu próprio script para a instalação de dependências. Podemos também aproveitá-lo:

```bash
cd ~/catkin_ws/src/PX4-Autopilot/Tools/setup
sudo ./ubuntu.sh
```

Isto irá instalar tudo o que é necessário para compilar o PX4 e o seu ambiente SITL.

Instalar mais pacotes Python necessários:

```bash
pip3 install --user toml
```

# Adicionar o Frame do Drone do Coex Clover

Adicionar o Frame do Drone Clover ao PX4 utilizando o comando:

```bash
ln -s ~/catkin_ws/src/clover/clover_simulation/airframes/* ~/PX4-Autopilot/ROMFS/px4fmu_common/init.d-posix/airframes/
```

# Instalar os conjuntos de dados da geographiclib

O pacote mavos requer que os conjuntos de dados da geographiclib estejam presentes:

```bash
 sudo /opt/ros/noetic/lib/mavros/install_geographiclib_datasets.sh
```

# Compilar o Simulador:

Compilar a sua área de trabalho:

```bash
cd ~/catkin_ws
catkin_make -j1
```

# Rodar o Simulador

Para ter a certeza de que tudo foi compilado corretamente, tente executar o simulador pela primeira vez:

```bash
roslaunch clover_simulation simulator.launch
```

Você testar voos autônomos utilizando scripts de exemplo no diretório `~/catkin_ws/src/clover/clover/examples.`

---

Chegamos ao **fim de nosso Tutorial**, caso ele te ajudou, peço que também ajude nosso projeto seguindo as **páginas do Instagram**:

- <a href='https://www.instagram.com/dronesguanambi/' target="_blank">dronesguanambi</a>
- <a href='https://www.instagram.com/educa_drones/' target="_blank">educa_drones</a>

caso deseje ver a documentação original clique [aqui](#todas-as-instruções-a-seguir-foram-tiradas-de) para ver as referencias

Feito por: [@msantos7gabriel](https://github.com/msantos7gabriel) <br>
Editado a última vez por: [@msantos7gabriel](https://github.com/msantos7gabriel) 17/03/2025
