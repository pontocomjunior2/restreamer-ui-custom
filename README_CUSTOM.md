# Restreamer UI Custom - 500MB Upload Limit

Esta é uma versão customizada do Restreamer UI com limite de upload aumentado de 25MB para **500MB** para arquivos de vídeo e áudio em loop.

## 🎯 Modificações

- **VideoLoop**: Upload de vídeo aumentado para 500MB
- **AudioLoop**: Upload de áudio aumentado para 500MB

## 🚀 Deploy no EasyPanel

### Opção 1: Via Docker Compose (Recomendado)

1. No EasyPanel, crie um novo serviço
2. Selecione "Docker Compose"
3. Cole o conteúdo do arquivo `docker-compose.yml`
4. Configure as variáveis de ambiente necessárias
5. Clique em Deploy

### Opção 2: Build Manual

```bash
# Clone o repositório
git clone <seu-repositorio-url>
cd restreamer-ui-temp

# Copie o .env.example
cp .env.example .env

# Edite o .env com suas configurações
nano .env

# Inicie os serviços
docker-compose up -d
```

## 📝 Variáveis de Ambiente

- `CORE_USERNAME`: Usuário admin do Restreamer Core (padrão: admin)
- `CORE_PASSWORD`: Senha do admin (defina uma senha forte!)
- `REACT_APP_CORE_API`: URL da API do Core (padrão: http://core:8080)

## 🔧 Portas

- `3000`: Restreamer UI
- `8080`: Restreamer Core API (HTTP)
- `8181`: Restreamer Core API (HTTPS)
- `1935`: RTMP Server
- `6000`: SRT Server (UDP)

## 📦 Acesso

Após o deploy:
- **UI**: http://seu-dominio:3000
- **Core API**: http://seu-dominio:8080

## 🔐 Segurança

**IMPORTANTE**: Altere a senha padrão em `.env` antes do deploy em produção!

## 📚 Documentação Original

Para mais informações sobre o Restreamer, visite:
- https://datarhei.github.io/restreamer/
- https://github.com/datarhei/restreamer

## 🛠️ Build Local

Se desejar compilar localmente:

```bash
# Instalar dependências
yarn install

# Build de produção
yarn build

# A pasta build/ conterá os arquivos estáticos
```

## ⚠️ Notas

- Esta customização aumenta apenas o limite da **interface**
- Certifique-se que seu servidor/hosting suporta uploads de 500MB
- Verifique as configurações de timeout do nginx/proxy reverso
- Ajuste `CORE_STORAGE_DISK_MAX_SIZE_MBYTES` conforme necessário
