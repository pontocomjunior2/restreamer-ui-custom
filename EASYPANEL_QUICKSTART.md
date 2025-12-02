# 🚀 Guia Rápido - Deploy no EasyPanel

## ✅ Alterações Feitas:

- ✅ Porta da UI alterada: **3000 → 9199**
- ✅ Variáveis de ambiente essenciais adicionadas
- ✅ Limite de upload configurado: **500MB**

---

## 📝 VARIÁVEIS DE AMBIENTE - SIM, SÃO NECESSÁRIAS!

### ⚠️ **IMPORTANTE**: Configure estas variáveis no EasyPanel:

No EasyPanel, na seção de **Environment Variables**, adicione:

```env
CORE_AUTH_ENABLE=true
CORE_USERNAME=admin
CORE_PASSWORD=SuaSenhaSeguraAqui123!
LOG_LEVEL=info
```

### 🔐 **Por que são importantes?**

1. **CORE_PASSWORD**: Protege seu Restreamer com senha
2. **CORE_HTTP_CLIENT_MAX_BODY_SIZE=524288000**: Permite uploads de 500MB
3. **CORE_AUTH_ENABLE=true**: Ativa a autenticação (SEGURANÇA!)

---

## 🔄 Próximo Passo no EasyPanel:

1. **Atualize o código** no EasyPanel:
   - Vá no serviço `restreamer-custom`
   - Clique em **"Redeploy"** ou **"Pull & Rebuild"**

2. **Configure as variáveis de ambiente**:
   - Vá em "Environment" ou "Settings"
   - Adicione as variáveis acima
   - Salve

3. **Reinicie o serviço**

---

## 🌐 Acesso após Deploy:

- **UI Customizada**: `http://seu-servidor:9199`
- **Core API**: `http://seu-servidor:8080`
- **RTMP Server**: `rtmp://seu-servidor:1935`

### Login:
- **Usuário**: `admin`
- **Senha**: A que você configurou em `CORE_PASSWORD`

---

## ✅ Teste o Upload de 500MB:

1. Acesse `http://seu-servidor:9199`
2. Faça login
3. Vá em **Edit** → **Sources**
4. Selecione **Video Loop** ou **Audio Loop**
5. Faça upload de um arquivo > 25MB (até 500MB)

---

## 🔧 Se ainda der erro de porta no EasyPanel:

Execute este comando no EasyPanel CLI ou substitua manualmente a porta no docker-compose:

```bash
# Verificar portas em uso
docker ps

# Se a porta 9199 também estiver em uso, escolha outra (ex: 9200, 9201, etc)
```

Então edite no GitHub o arquivo `docker-compose.yml` e mude a linha:
```yaml
- "9199:3000"  # Mude 9199 para outra porta livre
```

---

## 📊 Docker Compose Atualizado (Porta 9199):

```yaml
version: '3.8'

services:
  core:
    image: datarhei/core:latest
    restart: always
    ports:
      - "8080:8080"
      - "1935:1935"
    environment:
      - CORE_HTTP_CLIENT_MAX_BODY_SIZE=524288000
      - CORE_AUTH_ENABLE=${CORE_AUTH_ENABLE:-true}
      - CORE_USERNAME=${CORE_USERNAME:-admin}
      - CORE_PASSWORD=${CORE_PASSWORD:-changeme}
    volumes:
      - core-data:/core/data

  ui:
    build:
      context: .
      dockerfile: Dockerfile.custom
    restart: always
    ports:
      - "9199:3000"  # ← PORTA ALTERADA!
    environment:
      - REACT_APP_CORE_API=http://core:8080
    depends_on:
      - core

volumes:
  core-data:
```

---

**🎉 Pronto! Código atualizado no GitHub. Agora é só fazer o Redeploy no EasyPanel!**
