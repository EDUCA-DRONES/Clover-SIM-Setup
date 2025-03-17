# Setup de Simulador Gazebo + Coex clover Rodando Nativamente

Esse tutorial tem como objetivo ensinar a rodar a simulação **Gazebo** junto com o Framework **Clover** e **ROS**

Para Prosseguir com esse tutorial antes se deve cumprir certos Requisitos:

- [Ubuntu 20.04](ubuntu.md)
- [(Recomendado) Git clone por ssh](Git_config.md)
- 7 Gigas Livres em Disco

> ![NOTE] > **Voce pode Clicar para seguir esses links para poder ser redirecionado para o tópico**

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

- Dependências para Construir os pacotes:

  Até agora você instalou o que precisa para executar os pacotes ROS principais. há várias ferramentas e requisitos que são distribuídos separadamente. Por exemplo, rosinstall é uma ferramenta usada com frequência que permite que você baixe facilmente árvores de origem para pacotes ROS.

  Para instalar esta ferramenta e outras dependências para construir pacotes ROS, execute em seu terminal:

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
