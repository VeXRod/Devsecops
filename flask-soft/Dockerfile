FROM python:3.11-alpine

# Eviter l'écriture de fichiers pyc + affichage live dans le terminal
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Installation des dépendances système
RUN apk update && apk add --no-cache \
    build-base \
    python3-dev \
    libffi-dev \
    musl-dev \
    gcc \
    jpeg-dev \
    zlib-dev \
    postgresql-dev \
    mariadb-dev \
    linux-headers

# Dossier de travail
WORKDIR /app

# Copier requirements en premier pour profiter du cache
COPY requirements.txt .

# Installer les dépendances
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copier tout le reste du code
COPY . .

# Port exposé : 8080 (selon les besoins du client)
EXPOSE 8080

# Lancement de l’app avec Gunicorn en mode production
CMD ["gunicorn", "--bind", "0.0.0.0:8080", "run:app"]
