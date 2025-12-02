# 🚀 ESCOLHA SUA VERSÃO DE DOCKER-COMPOSE

Criei **3 versões diferentes** para você escolher qual funciona melhor no seu EasyPanel.

---

## ✅ **OPÇÃO 1: ALL-IN-ONE** (RECOMENDADO! ⭐)

**Arquivo**: `docker-compose.allinone.yml`

### ✨ Vantagens:
- ✅ **Mais simples** - Uma única imagem
- ✅ **Core + UI integrados**
- ✅ **Menos portas** - Tudo em 8080
- ✅ **Build direto do GitHub**

### 📝 Como usar no EasyPanel:

```yaml
version: '3.8'

services:
  restreamer:
    build:
      context: https://github.com/pontocomjunior2/restreamer-ui-custom.git
      dockerfile: Dockerfile.allinone
    ports:
      - "8080:8080"
      - "1935:1935"
    volumes:
      - restreamer-data:/core/data
    environment:
      - CORE_API_AUTH_PASSWORD=SuaSenhaAqui123!

volumes:
  restreamer-data:
```

### 🌐 Acesso:
- **UI + API**: `http://seu-servidor:8080`
- **RTMP**: `rtmp://seu-servidor:1935`

---

## 🔄 **OPÇÃO 2: SEPARADO** (Original)

**Arquivo**: `docker-compose.yml`

### ✨ Características:
- ✅ Core e UI separados
- ✅ UI na porta **9199**
- ✅ Mais flexível

### ⚠️ Problema no EasyPanel:
- Pode não encontrar o `Dockerfile.custom` se o build for local

### 💡 Solução:
Use `docker-compose.easypanel.yml` que faz build do GitHub:

```yaml
ui:
  build:
    context: https://github.com/pontocomjunior2/restreamer-ui-custom.git
    dockerfile: Dockerfile.custom
```

---

## ⚡ **OPÇÃO 3: APENAS CORE** (Mais Rápido)

**Arquivo**: `docker-compose.simple.yml`

### ✨ Características:
- ✅ **Mais rápido** - Usa imagem pronta
- ✅ Apenas o Core oficial
- ⚠️ **UI com limite de 25MB** (padrão)

### 📝 Use se:
- Você só quer testar rápido
- Vai fazer build da UI separadamente
- Não precisa urgentemente do limite de 500MB

```yaml
version: '3.8'

services:
  core:
    image: datarhei/core:latest
    ports:
      - "8080:8080"
    environment:
      - CORE_HTTP_CLIENT_MAX_BODY_SIZE=524288000
    volumes:
      - core-data:/core/data

volumes:
  core-data:
```

---

## 🎯 **QUAL ESCOLHER?**

| Situação | Recomendação |
|----------|--------------|
| **Primeira vez / Fácil** | ⭐ **OPÇÃO 1** (allinone) |
| **Quer separar Core e UI** | OPÇÃO 2 (easypanel.yml) |
| **Teste rápido** | OPÇÃO 3 (simple.yml) |
| **Erro "Dockerfile not found"** | ⭐ **OPÇÃO 1** (allinone) |

---

## 🚀 **RECOMENDAÇÃO FINAL:**

### Para usar no EasyPanel AGORA:

**Cole este docker-compose** no EasyPanel:

```yaml
version: '3.8'

services:
  restreamer:
    build:
      context: https://github.com/pontocomjunior2/restreamer-ui-custom.git
      dockerfile: Dockerfile.allinone
    container_name: restreamer-custom
    restart: always
    ports:
      - "8080:8080"
      - "1935:1935"
      - "6000:6000/udp"
    volumes:
      - restreamer-data:/core/data
    environment:
      - CORE_API_AUTH_USERNAME=admin
      - CORE_API_AUTH_PASSWORD=TroquePorSuaSenha123!

volumes:
  restreamer-data:
```

### ✅ Configurar no EasyPanel:

**Environment Variables:**
```
CORE_API_AUTH_PASSWORD=SuaSenhaSeguraAqui123!
CORE_API_AUTH_USERNAME=admin
```

### 🌐 Após deploy, acesse:

```
http://seu-servidor:8080
```

**Login:**
- Usuário: `admin`
- Senha: A que você configurou

---

## 📝 **RESUMO DAS OPÇÕES:**

| Arquivo | Descrição | Portas | Complexidade |
|---------|-----------|--------|--------------|
| `docker-compose.allinone.yml` | ⭐ Tudo integrado | 8080 | ⭐ Fácil |
| `docker-compose.yml` | Core + UI separados | 8080 + 9199 | Média |
| `docker-compose.easypanel.yml` | Build do GitHub | 8080 + 9199 | Média |
| `docker-compose.simple.yml` | Apenas Core | 8080 | ⭐ Muito Fácil |

---

**🎉 Use a OPÇÃO 1 (allinone) que vai funcionar!**
