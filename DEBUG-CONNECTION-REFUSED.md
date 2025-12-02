# 🔍 DIAGNÓSTICO - Conexão Recusada

## ❌ Erro: "Conexão Recusada"

Isso significa que o container **não está rodando** ou **falhou ao iniciar**.

---

## 🔧 **PASSO 1: Verificar Status do Container**

### No EasyPanel:
1. Vá em **Services** → `restreamer-custom`
2. Verifique o **Status**: Deve estar **"Running"** (verde)
3. Se estiver **"Stopped"** ou **"Error"** (vermelho), há um problema

### Via SSH/CLI:
```bash
# Ver containers rodando
docker ps | grep restreamer

# Ver TODOS os containers (incluindo parados)
docker ps -a | grep restreamer
```

---

## 📋 **PASSO 2: Verificar LOGS do Container**

### No EasyPanel:
1. Vá no serviço `restreamer-custom`
2. Clique em **"Logs"** ou **"View Logs"**
3. **Procure por erros** (linhas em vermelho)

### Via SSH/CLI:
```bash
# Ver logs do container
docker logs restreamer-custom-alt

# Ver logs em tempo real
docker logs -f restreamer-custom-alt

# Ver últimas 50 linhas
docker logs --tail=50 restreamer-custom-alt
```

---

## 🔍 **PASSO 3: Erros Comuns e Soluções**

### ❌ Erro: "port is already allocated"
**Solução**: Usar portas diferentes ou parar containers antigos

### ❌ Erro: "failed to build"
**Possíveis causas**:
- Falta de memória
- Timeout de build
- Erro no Dockerfile

**Solução**:
```bash
# Limpar cache do Docker
docker system prune -a

# Tentar build novamente
```

### ❌ Erro: "permission denied"
**Solução**:
```bash
# Dar permissões aos volumes
docker volume inspect radyou_restreamer-custom_restreamer-data
```

### ❌ Container inicia e para imediatamente
**Causa**: Erro na inicialização do Core

**Verificar logs**:
```bash
docker logs restreamer-custom-alt 2>&1 | grep -i error
```

---

## 🚀 **SOLUÇÃO RÁPIDA: Usar Imagem Pronta**

Se o build está falhando, use a **imagem oficial**:

### 📝 Cole este docker-compose NO EASYPANEL:

```yaml
version: '3.8'

services:
  # Core oficial com limite de 500MB
  core:
    image: datarhei/core:latest
    container_name: restreamer-core-official
    restart: always
    ports:
      - "9000:8080"
      - "1936:1935"
    volumes:
      - core-data:/core/data
    environment:
      # Limite de 500MB
      - CORE_HTTP_CLIENT_MAX_BODY_SIZE=524288000
      - CORE_API_AUTH_PASSWORD=SuaSenhaAqui123!
      - CORE_STORAGE_DISK_MAX_SIZE_MBYTES=0

volumes:
  core-data:
```

### ⚠️ **IMPORTANTE:**
Esta versão usa a **UI padrão** (limite de 25MB), mas o **Core está configurado para 500MB**.

**Para uploads >25MB**:
1. Faça upload de arquivo pequeno pela UI
2. Substitua manualmente no volume em `/core/data/`

---

## 📊 **COMANDOS ÚTEIS DE DIAGNÓSTICO**

```bash
# 1. Ver todos os containers
docker ps -a

# 2. Ver portas em uso
netstat -tulpn | grep -E ':(9000|8080|1936|1935)'

# 3. Verificar volumes
docker volume ls | grep restreamer

# 4. Ver uso de recursos
docker stats

# 5. Inspecionar container
docker inspect restreamer-custom-alt

# 6. Verificar rede
docker network ls
docker network inspect radyou_restreamer-custom_default
```

---

## 🆘 **SE NADA FUNCIONAR:**

### Opção 1: Reset Completo
```bash
# Parar e remover tudo
docker compose -p radyou_restreamer-custom down -v

# Limpar imagens antigas
docker image prune -a

# Refazer deploy
```

### Opção 2: Usar Core Oficial Simples
Use o docker-compose acima (Core oficial) - É mais rápido e confiável.

### Opção 3: Deploy Manual
```bash
# Pull da imagem oficial
docker pull datarhei/core:latest

# Rodar manualmente
docker run -d \
  --name restreamer-simple \
  -p 9000:8080 \
  -p 1936:1935 \
  -e CORE_HTTP_CLIENT_MAX_BODY_SIZE=524288000 \
  -e CORE_API_AUTH_PASSWORD=SuaSenhaAqui123! \
  -v core-data:/core/data \
  datarhei/core:latest
```

Depois acesse: `http://93.127.141.215:9000`

---

## 💡 **PRÓXIMO PASSO:**

**Me envie os LOGS do container** para eu ver o erro exato:

```bash
docker logs restreamer-custom-alt
```

Ou me diga:
1. ✅ O container está rodando? (docker ps)
2. ✅ Qual o status no EasyPanel?
3. ✅ Há erros nos logs?

---

**🔍 Com os logs posso identificar o problema exato!**
