# Desafio Docker - Imersão DevOps & Cloud

Este repositório documenta o desafio de criação de um contêiner Docker para a aplicação "Conversão de Distâncias", desenvolvido durante a Imersão DevOps & Cloud ministrada por Fabricio Veronez. Aqui estão descritos todos os passos, aprendizados e desafios enfrentados no processo.

---

## **Objetivo**
Criar um contêiner Docker funcional para uma aplicação Python, utilizando boas práticas na construção de imagens e configurações.

## **Passos Realizados**

### **1. Configuração do Ambiente**
1. Fork do repositório original: [KubeDev/conversao-distancia](https://github.com/KubeDev/conversao-distancia).
2. Clonagem do repositório:
   ```bash
   git clone https://github.com/rodrigoacelio/Imersao-DevOps-Cloud.git
   ```
3. Criação do arquivo `Dockerfile` na raiz do projeto:
   ```dockerfile
   FROM python:3.9-slim

   WORKDIR /app

   COPY requirements.txt .

   RUN pip install --no-cache-dir -r requirements.txt

   COPY . .

   EXPOSE 5000

   CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
   ```

### **2. Construção da Imagem Docker**
O comando a seguir foi utilizado para construir a imagem Docker:
```bash
docker build -t conversao-distancia .
```

### **3. Execução do Contêiner**
Para executar o contêiner e expor a aplicação na porta 5000:
```bash
docker run -p 5000:5000 conversao-distancia
```

A aplicação foi acessável em `http://localhost:5000` e testada com:
```bash
curl -X POST -H "Content-Type: application/json" -d '{"value": 10, "from_unit": "meters", "to_unit": "kilometers"}' http://localhost:5000/convert
```

### **4. Publicação da Imagem no Docker Hub**
A imagem foi publicada no Docker Hub utilizando os comandos:
```bash
docker login
docker tag conversao-distancia rodrinesco/conversao-distancia
docker push rodrinesco/conversao-distancia
```
A imagem está disponível em: [Docker Hub - rodrinesco/conversao-distancia](https://hub.docker.com/r/rodrinesco/conversao-distancia).

---

## **Erros Enfrentados**

1. **Erro ao construir a imagem**:
   ```bash
   ERROR: failed to solve: failed to read dockerfile: open Dockerfile: no such file or directory
   ```
   **Solução**: Certifique-se de que o arquivo `Dockerfile` está no diretório correto e com o nome correto (sensível a maiúsculas e minúsculas).

2. **Erro de conexão com Docker Desktop**:
   ```bash
   ERROR: error during connect: Head "http://%2F%2F.%2Fpipe%2FdockerDesktopLinuxEngine/_ping": open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.
   ```
   **Solução**: Certifique-se de que o Docker Desktop está aberto antes de executar os comandos.

3. **Erro de Worker Timeout no Gunicorn**:
   ```bash
   [CRITICAL] WORKER TIMEOUT (pid: X)
   ```
   **Solução**: Aumente o tempo limite dos workers no comando do Gunicorn. Ajuste o `CMD` no Dockerfile para:
   ```dockerfile
   CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--timeout", "120", "app:app"]
   ```

4. **Erro ao fazer push para o Docker Hub**:
   ```bash
   tag does not exist: seu-usuario/conversao-distancia:latest
   ```
   **Solução**: Certifique-se de usar o nome correto do repositório e que a imagem foi previamente criada com o comando:
   ```bash
   docker tag conversao-distancia seu-usuario/conversao-distancia
   ```

---

## **Estrutura do Repositório**
```plaintext
Desafio-Docker/
├── README.md
├── Dockerfile
├── requirements.txt
├── app.py
└── templates/
```

---

## **Aprendizados**
- Estruturação e construção de imagens Docker.
- Teste e execução de contêineres localmente.
- Resolução de problemas comuns ao trabalhar com Docker.
- Publicação de imagens no Docker Hub.

---

**Dúvaidas ou feedbacks? Entre em contato!**

