FROM python:3.11-alpine

# Env vars
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Dépendances système
RUN apk update && apk add --no-cache \
    gcc \
    python3-dev \
    musl-dev \
    libffi-dev \
    jpeg-dev \
    zlib-dev \
    postgresql-dev \
    build-base \
    linux-headers \
    libxml2-dev \
    libxslt-dev \
    && pip install --upgrade pip

WORKDIR /app

# Copier uniquement requirements.txt pour tirer parti du cache Docker
COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

# Copier le code de l'app
COPY . .

# Collecte des fichiers statiques & migrations
RUN python manage.py collectstatic --noinput && \
    python manage.py makemigrations && \
    python manage.py migrate

EXPOSE 5005

CMD ["gunicorn", "--config", "gunicorn-cfg.py", "core.wsgi"]
