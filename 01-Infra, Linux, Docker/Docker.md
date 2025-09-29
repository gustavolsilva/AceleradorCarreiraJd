# Infra, Linux, Docker

## Introdução
Baseado no conteúdo da [Jornada de Dados](https://suajornadadedados.com.br/), com o Fundador e CEO, Luciano Vasconcelos.<br>
[Repositorio do Github](https://github.com/lvgalvao/workshop-docker-aovivo-)<br>
[Excalidraw](https://app.excalidraw.com/l/8pvW6zbNUnD/6MNAkqnvTPt)

## Docker
### Por que usar Docker?
- Facilita a criação, o deploy e a execução de aplicações usando containers.
- Permite que você empacote uma aplicação com todas as partes necessárias, como bibliotecas e outras dependências, e envie tudo como um único pacote.
- Garante que a aplicação funcione em qualquer outro ambiente, independentemente das configurações do sistema operacional ou das dependências instaladas
- Isolamento: Cada container é isolado dos outros, o que significa que você pode executar múltiplas aplicações no mesmo host sem conflitos.
- Portabilidade: Containers podem ser executados em qualquer lugar, desde o laptop de um desenvolvedor até servidores na nuvem.
- Eficiência: Containers compartilham o kernel do sistema operacional, o que os torna mais leves e rápidos do que máquinas virtuais tradicionais.
- Escalabilidade: Facilita o escalonamento de aplicações, permitindo que você aumente ou diminua o número de containers conforme a demanda.
- Consistência: Garante que a aplicação funcione da mesma maneira em diferentes ambientes, desde o desenvolvimento até a produção.
- Facilidade de gerenciamento: Ferramentas como Docker Compose e Kubernetes facilitam o gerenciamento de múltiplos containers e serviços.

### Histórico do Docker
- Servidores -> Caros e difíceis de escalar ($$$$)
- Motivação -> aproveitamento de recursos
- Docker Surgimento -> 2013 -> complemento a Virtualização -> Utilização do Computador da melhor forma possível.

VM + Pesada x Docker
Docker é focado em Aplicação. 

### Como Instalar o Docker?
- Linux (Ubuntu)
```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release -y
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- Windows
https://docs.docker.com/desktop/install/windows-install/

- Mac
https://docs.docker.com/desktop/install/mac-install/

### Tutorial Básico do Docker

No exemplo, o Luciano está utilizando o Docker Desktop no Windows. Aqui vou reproduzir os passos usando o Linux (Ubuntu 24.04), com uma imagem no WSL.

- Verificar a versão do Docker
```bash
docker --version
```
![alt text](./img/01-docker_version.png)

- Rodar um container com a imagem do welcome-to-docker
```bash
docker pull docker/welcome-to-docker
```
![Docker Pull Welcome](./img/02-docker_pull_welcome-to-docker.png)

```bash
docker run -d -p 8080:80 docker/welcome-to-docker
```
![Docker Run Welcome](./img/03-docker_run_welcome-to-docker.png)

Abra o navegador e acesse http://localhost:8080
![Welcome to Docker](./img/04-welcome_to_docker.png)

No Docker, executa um Container e o Sistema Operacional dele é o Linux.

Para pegar outras imagens, acesse o Docker Hub: https://hub.docker.com/

- Listar os containers em execução
```bash
docker ps
```

### Hello World do Docker

1) Adicione o streamlit ao seu ambiente virtual
```bash
poetry add streamlit
```
2) Crie um arquivo chamado `app.py` com o seguinte conteúdo:
```python
import streamlit as st

def hello_world():
    return "Olá Turma de Dados! Aula de Docker"

def main():
    st.write(hello_world())

if __name__ == "__main__":
    main()
```

3) Teste a aplicação localmente
```bash
poetry run streamlit run app.py
```

Você terá um resultado parecido com este:
![Streamlit Local](./img/05-streamlit_local.png)

4) Crie um arquivo chamado `Dockerfile` com o seguinte conteúdo:
```dockerfile
FROM python:3.12 # imagem base
RUN pip install poetry # instala o poetry
COPY . /src # copia os arquivos para o container
WORKDIR /src # define o diretório de trabalho
RUN poetry install # instala as dependências
EXPOSE 8501 # expõe a porta 8501
ENTRYPOINT [ "poetry", "run", "streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0" ] # comando para rodar a aplicação
```

5) Adicione o .dockerignore
```
.venv
```

6) Construa a imagem Docker
```bash
docker build -t minha-primeira-imagem .

7) Liste as imagens Docker disponíveis
```bash
docker images
```

8) Execute o container
```bash
docker run -d -p 8501:8501 minha-primeira-imagem
``` 

9) Acesse a aplicação no navegador
Abra o navegador e acesse http://localhost:8501

Imagem da aplicação rodando no Docker:
![Streamlit Docker](./img/06-streamlit_docker.png)


### Informações importantes do Workshop sobre Docker

```bash
git clone ...
cd ...
```

```bash
docker build ...
docker run ...
```

