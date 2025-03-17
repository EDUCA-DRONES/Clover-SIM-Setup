# Setup de Simulador Gazebo + Coex clover Rodando Nativamente

<p>Para Prosseguir com esse tutorial antes se deve cumprir certos Requisitos</p>

- [Ubuntu 20.04](ubuntu.md)
- [(Recomendado) Git clone por ssh](Git_config.md)
- 7 Gigas de Espaço livres

Voce pode Clicar para seguir esses links para poder ser redirecionado para o tópico

# Instalar o ROS

## Instalação

1. Primeiro se deve configurar os repositórios "restricted", "universe" e "multiverse" rodando os seguintes comandos.

```Bash
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

2. Agora devemos configurar seu computador para aceitar software dos packages.ros.org:

<br>

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

3. Adicionar as Chaves

```bash
sudo apt install curl # Se você ainda não tiver o curl baixado
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

<br>

4. Instalar ROS
   primeiro, tenha certeza que seus indices de pacotes estão atualizados:

```bash
sudo apt update
```

Rode o comando para instalação do ROS:

---

- Desktop-Full Install: (Recomendado): Tudo na versão desktop mas as ferramentas 2d/3d de simulação e pacotes de percepção

```bash
sudo apt install ros-noetic-desktop-full
```

mais informações em [wiki.ros.org](https://wiki.ros.org/noetic/Installation/Ubuntu)

<br>

5. Configuração do seu ambiente
   O seguinte comando irá escrever no caminho de configuração do seu terminal o source do ROS, para que toda vez que o for aberto o terminal ele inicie junto.

**Bash**

```bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

**Zsh**

```zsh
echo "source /opt/ros/noetic/setup.zsh" >> ~/.zshrc
source ~/.zshrc
```

6. Dependências para Construir os pacotes:
Até Agora que você já instalou tudo que precisa para rodar o núcleo dos pacotes ROS. Para criar e gerenciar sua própria area de trabalho.