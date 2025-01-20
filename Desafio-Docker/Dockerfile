FROM python:3.9-slim

# Define o diretório de trabalho dentro do container
WORKDIR /app

# Copia as dependências para o container
COPY requirements.txt .

# Instala as dependências
RUN pip install --no-cache-dir -r requirements.txt

# Copia o restante do código para o container
COPY . /app/

# Expõe a porta que será usada pela aplicação
EXPOSE 5000

# Comando de inicialização do Gunicorn
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
