# 🚨 SOLUÇÃO RÁPIDA - Remover Containers Antigos

## ❌ Erro Atual:
```
Bind for 0.0.0.0:1935 failed: port is already allocated
```

**Causa**: Os containers antigos (core + ui separados) ainda estão rodando.

---

## ✅ **SOLUÇÃO 1: Remover Containers Antigos** (Recomendado)

### No terminal do EasyPanel ou SSH:

```bash
# Parar e remover containers antigos
docker stop radyou_restreamer-custom-core-1 radyou_restreamer-custom-ui-1
docker rm radyou_restreamer-custom-core-1 radyou_restreamer-custom-ui-1

# Ou use o comando com --remove-orphans
docker compose -p radyou_restreamer-custom down --remove-orphans

# Depois refaça o deploy
```

### Ou no EasyPanel:

1. Vá em **Services** → `restreamer-custom`
2. Clique em **"Stop"** ou **"Delete"**
3. Espere parar completamente
4. Clique em **"Deploy"** novamente

---

## ✅ **SOLUÇÃO 2: Usar Portas Alternativas**

Se não conseguir parar os containers antigos, use este docker-compose com portas diferentes:

### 📝 **Cole no EasyPanel** (`docker-compose.allinone-altports.yml`):

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
      - "8080:8080"    # UI + API
      - "1936:1935"    # RTMP (porta 1936 ao invés de 1935)
      - "6001:6000/udp" # SRT
    volumes:
      - restreamer-data:/core/data
    environment:
      - CORE_API_AUTH_PASSWORD=SuaSenhaAqui123!

volumes:
  restreamer-data:
```

### 🌐 Acesso com portas alternativas:
- **UI**: `http://seu-servidor:8080`
- **RTMP**: `rtmp://seu-servidor:1936` (porta 1936!)

---

## ✅ **SOLUÇÃO 3: Limpar Tudo** (Se nada funcionar)

```bash
# Parar TODOS os containers do projeto
docker compose -p radyou_restreamer-custom down -v --remove-orphans

# Verificar se ainda tem algo rodando
docker ps | grep restreamer

# Se tiver, force a parada
docker stop $(docker ps -q --filter name=restreamer)
docker rm $(docker ps -aq --filter name=restreamer)

# Agora faça o deploy novamente
```

---

## 🎯 **RECOMENDAÇÃO:**

1. **Primeiro tente**: Parar o serviço no EasyPanel e refazer deploy
2. **Se não funcionar**: Use `docker-compose.allinone-altports.yml`
3. **Última opção**: Execute os comandos de limpeza via SSH

---

## 📊 **Checklist:**

- [ ] Parar serviço antigo no EasyPanel
- [ ] Aguardar containers pararem completamente
- [ ] Fazer redeploy com docker-compose.allinone.yml
- [ ] OU usar docker-compose.allinone-altports.yml (portas 1936, 6001)
- [ ] Configurar CORE_API_AUTH_PASSWORD
- [ ] Acessar http://seu-servidor:8080

---

**🔧 A boa notícia: O BUILD FUNCIONOU! 🎉**
**Só precisa resolver o conflito de portas!**
