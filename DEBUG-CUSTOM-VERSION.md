# 🔧 DIAGNÓSTICO E SOLUÇÃO - Versão Customizada

## 🎯 Objetivo: Fazer a versão com 500MB na INTERFACE funcionar

---

## 📋 **PASSO 1: Me envie os logs do EasyPanel**

### No EasyPanel:
1. Vá em **Services** → `restreamer-custom`
2. Clique em **"Logs"** (ícone de documento)
3. **Copie e cole aqui as últimas 50-100 linhas**

Procure por:
- ❌ Erros de build (`error building`)
- ❌ Erros de sintaxe (`syntax error`)
- ❌ Erros de memória (`out of memory`)
- ❌ Erros de permissão (`permission denied`)

**Ou**, se tiver acesso SSH:
```bash
docker logs restreamer-custom-alt --tail=100
```

---

## 🚀 **ENQUANTO ISSO: Teste esta versão SIMPLIFICADA**

Criei uma versão mais **robusta** e **otimizada** do build:

### 📝 **Cole NO EASYPANEL:**

```yaml
version: '3.8'

services:
  restreamer-custom:
    image: ghcr.io/pontocomjunior2/restreamer-ui-custom:latest
    build:
      context: https://github.com/pontocomjunior2/restreamer-ui-custom.git#main
      dockerfile: Dockerfile.allinone
      args:
        - NODE_ENV=production
    container_name: restreamer-500mb
    restart: unless-stopped
    ports:
      - "9000:8080"
      - "1936:1935"
      - "6001:6000/udp"
    volumes:
      - restreamer-data:/core/data
      - restreamer-config:/core/config
    environment:
      - CORE_API_AUTH_USERNAME=admin
      - CORE_API_AUTH_PASSWORD=TroquePorSuaSenha123!
      - CORE_ROUTER_UI_PATH=/core/ui
      - CORE_HTTP_CLIENT_MAX_BODY_SIZE=524288000
      - CORE_LOG_LEVEL=info
    healthcheck:
      test: ["CMD", "wget", "--quiet", "--tries=1", "--spider", "http://localhost:8080/api/v3/config"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 60s

volumes:
  restreamer-data:
  restreamer-config:
```

---

## 🔍 **MELHORIAS nesta versão:**

1. ✅ **Healthcheck** - Verifica se o Core iniciou corretamente
2. ✅ **restart: unless-stopped** - Reinicia se falhar
3. ✅ **2 volumes** - Separa dados e config
4. ✅ **NODE_ENV=production** - Build otimizado
5. ✅ **Todas variáveis necessárias** configuradas

---

## ⚡ **ALTERNATIVA: Build Local no Servidor**

Se o build via GitHub estiver falhando, podemos fazer o build **localmente**:

### Via SSH no servidor:

```bash
# 1. Clonar o repositório
cd /tmp
git clone https://github.com/pontocomjunior2/restreamer-ui-custom.git
cd restreamer-ui-custom

# 2. Build da imagem
docker build -f Dockerfile.allinone -t restreamer-custom:500mb .

# 3. Rodar o container
docker run -d \
  --name restreamer-500mb \
  -p 9000:8080 \
  -p 1936:1935 \
  -p 6001:6000/udp \
  -e CORE_API_AUTH_PASSWORD=SuaSenhaAqui123! \
  -e CORE_ROUTER_UI_PATH=/core/ui \
  -e CORE_HTTP_CLIENT_MAX_BODY_SIZE=524288000 \
  -v restreamer-data:/core/data \
  restreamer-custom:500mb

# 4. Verificar logs
docker logs -f restreamer-500mb
```

---

## 🐛 **POSSÍVEIS PROBLEMAS E SOLUÇÕES:**

### Problema 1: Build demora muito / timeout
**Solução**: Aumentar timeout do build no EasyPanel
```yaml
build:
  context: https://github.com/pontocomjunior2/restreamer-ui-custom.git
  dockerfile: Dockerfile.allinone
  # Adicionar cache do Docker
  cache_from:
    - node:18-alpine
```

### Problema 2: Erro "yarn install failed"
**Causa**: Falta de memória ou timeout
**Solução**: Build local via SSH (comando acima)

### Problema 3: Container inicia e para
**Causa**: Erro na configuração do Core
**Solução**: Verificar logs
```bash
docker logs restreamer-500mb 2>&1 | grep -i error
```

### Problema 4: "Cannot find module"
**Causa**: Build incompleto
**Solução**: Limpar cache e rebuildar
```bash
docker builder prune -a -f
# Fazer deploy novamente
```

---

## 📊 **CHECKLIST DE DIAGNÓSTICO:**

Antes de continuar, me confirme:

- [ ] Qual o **status do container** no EasyPanel? (Running/Stopped/Error)
- [ ] O **build completou**? (100% ou parou antes?)
- [ ] Há **erros nos logs**? (Quais?)
- [ ] Você tem **acesso SSH** ao servidor?
- [ ] Quanto de **RAM** tem o servidor?
- [ ] Há **espaço em disco** suficiente?

```bash
# Verificar recursos
free -h          # RAM disponível
df -h            # Espaço em disco
docker system df  # Espaço usado pelo Docker
```

---

## 🎯 **PRÓXIMOS PASSOS:**

1. **Me envie os logs do container/deploy**
2. **Tente o docker-compose SIMPLIFICADO acima**
3. **Me diga se tem acesso SSH** (posso ajudar com build local)

---

**🔧 Com os logs posso identificar o problema exato e corrigir!**

Me envie:
- ✅ Logs do EasyPanel (últimas 50 linhas)
- ✅ Status do container
- ✅ Mensagens de erro (se houver)
