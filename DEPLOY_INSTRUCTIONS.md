# 🚀 Instruções de Deploy - Restreamer UI Custom (500MB)

## 📋 Pré-requisitos

1. Conta no GitHub
2. Acesso ao EasyPanel
3. Git instalado localmente (já feito ✅)

---

## 1️⃣ Criar e Subir Repositório no GitHub

### Via GitHub Web:

1. Acesse https://github.com/new
2. Nome do repositório: `restreamer-ui-custom` (ou o nome que preferir)
3. Descrição: "Restreamer UI with 500MB upload limit"
4. Deixe como **Público** ou **Privado**
5. **NÃO** inicialize com README (já temos)
6. Clique em "Create repository"

### Conectar seu repositório local:

```bash
# No terminal, dentro de d:\Projetos\restreamer\restreamer-ui-temp

# Adicione o remote do GitHub (substitua SEU_USUARIO pelo seu usuário GitHub)
git remote add origin https://github.com/SEU_USUARIO/restreamer-ui-custom.git

# Ou se preferir SSH:
git remote add origin git@github.com:SEU_USUARIO/restreamer-ui-custom.git

# Envie o código
git branch -M main
git push -u origin main
```

---

## 2️⃣ Deploy no EasyPanel

### Opção A: Via Docker Compose (Recomendado)

1. **Login no EasyPanel**
   - Acesse seu painel EasyPanel
   - Vá em "Services" ou "Projects"

2. **Criar Novo Serviço**
   - Clique em "Create Service"
   - Selecione "Docker Compose"

3. **Configurar o Serviço**
   - **Nome**: `restreamer-custom`
   - **Docker Compose**: Cole o conteúdo do arquivo `docker-compose.yml`

4. **Variáveis de Ambiente** (Configure no EasyPanel):
   ```
   CORE_USERNAME=admin
   CORE_PASSWORD=SuaSenhaSeguraAqui123!
   REACT_APP_CORE_API=http://core:8080
   ```

5. **Domínio** (opcional):
   - Configure um domínio para o serviço
   - Porta: 3000 (UI)
   - Porta: 8080 (Core API)

6. **Deploy**
   - Clique em "Deploy"
   - Aguarde a build e deploy

### Opção B: Via GitHub + Dockerfile

1. **No EasyPanel**, crie um novo serviço:
   - Selecione "GitHub Repository"
   - Conecte seu repositório `restreamer-ui-custom`
   - Branch: `main`

2. **Build Settings**:
   - Build Method: Dockerfile
   - Dockerfile: `Dockerfile.custom`
   - Build Args (se necessário):
     ```
     NODE_VERSION=18
     ```

3. **Deploy Settings**:
   - Port: 3000
   - Environment Variables:
     ```
     REACT_APP_CORE_API=http://seu-core-url:8080
     ```

4. **Deploy Restreamer Core separadamente**:
   - Crie outro serviço
   - Use a imagem: `datarhei/core:latest`
   - Portas: 8080, 8181, 1935, 6000
   - Volumes necessários

---

## 3️⃣ Configuração de Nginx/Proxy (Importante!)

Para uploads de 500MB funcionarem, você precisa configurar o proxy reverso:

### No EasyPanel (se usar proxy integrado):

Adicione nas configurações do serviço:

```nginx
client_max_body_size 500M;
proxy_read_timeout 600s;
proxy_connect_timeout 600s;
proxy_send_timeout 600s;
```

### Ou configure via Environment Variables no Core:

```env
CORE_HTTP_CLIENT_MAX_BODY_SIZE=524288000  # 500MB em bytes
```

---

## 4️⃣ Teste de Upload

1. Acesse a UI em `https://seu-dominio.com`
2. Vá em "Edit" → "Sources"
3. Escolha "Video Loop" ou "Audio Loop"
4. Faça upload de um arquivo maior que 25MB (até 500MB)
5. Verifique se o upload completa sem erros

---

## 🔧 Troubleshooting

### Upload ainda limitado a 25MB?

**Problema**: Limite de proxy/nginx
**Solução**: Configure `client_max_body_size 500M;` no nginx

### Timeout durante upload?

**Problema**: Timeout do proxy
**Solução**: Aumente `proxy_read_timeout` e `proxy_send_timeout`

### Erro 413 (Request Entity Too Large)?

**Problema**: Limite do servidor web
**Solução**: Configure o nginx/caddy do EasyPanel

### Build falha?

**Problema**: Falta de memória ou timeout
**Solução**: Use uma instância com mais RAM ou aumente o timeout de build

---

## 📊 Estrutura do Projeto

```
restreamer-ui-temp/
├── src/
│   └── views/
│       └── Edit/
│           └── Sources/
│               ├── VideoLoop.js  ← Modificado (500MB)
│               └── AudioLoop.js  ← Modificado (500MB)
├── Dockerfile.custom
├── docker-compose.yml
├── .env.example
├── README_CUSTOM.md
└── .gitignore
```

---

## 🎯 Comandos Rápidos

### Atualizar código:
```bash
git add .
git commit -m "feat: sua mensagem"
git push origin main
```

### Rebuild no EasyPanel:
- Vá em "Services" → Seu serviço
- Clique em "Redeploy"
- Ou configure "Auto Deploy on Push"

---

## 📞 Suporte

- **Documentação oficial**: https://datarhei.github.io/restreamer/
- **Issues originais**: https://github.com/datarhei/restreamer-ui/issues
- **Seu repositório**: https://github.com/SEU_USUARIO/restreamer-ui-custom

---

## ✅ Checklist de Deploy

- [ ] Repositório criado no GitHub
- [ ] Código commitado e pushed
- [ ] Serviço criado no EasyPanel
- [ ] Variáveis de ambiente configuradas
- [ ] Senha do admin alterada
- [ ] Nginx/proxy configurado para 500MB
- [ ] Domínio configurado (opcional)
- [ ] Deploy realizado com sucesso
- [ ] Teste de upload de arquivo >25MB funcionando

---

**Pronto! Seu Restreamer UI customizado com limite de 500MB está pronto para deploy! 🎉**
