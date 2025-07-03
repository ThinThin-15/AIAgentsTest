# syntax=docker/dockerfile:1
FROM python:3.11.11-slim-bookworm

WORKDIR /code

COPY requirements.txt .
COPY src/api /code/api
COPY src/frontend /code/frontend

RUN pip install --no-cache-dir --upgrade -r requirements.txt

# Install Node.js and pnpm
RUN apt-get update \
    && apt-get install -y curl gnupg \
    && curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g pnpm@10.4.1 \
    && node --version \
    && pnpm --version

WORKDIR /code/frontend
RUN pnpm install && pnpm build

WORKDIR /code
RUN apt-get purge -y krb5-user libkrb5-3 libkrb5support0 libgssapi-krb5-2 perl && apt-get update && apt-get install -y libcap2

EXPOSE 50505

# 🔧 Switch to uvicorn directly to debug startup
CMD ["python", "-m", "api.main"]
