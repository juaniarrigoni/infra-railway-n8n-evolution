# Infraestrutura N8N + Evolution para Railway

## **Descrição do Projeto**

Este projeto contém uma estrutura completa utilizando **Docker Compose** para gerenciar os serviços da plataforma **n8n** e **Evolution API**, integrados com **PostgreSQL** e **Redis** para persistência de dados e cache. Ele foi projetado para ser facilmente implantado na **Railway**, utilizando variáveis de ambiente da plataforma para uma configuração otimizada e segura.

---

## **O que foi Implementado**

### **Serviços Configurados**

1. **n8n**:

   - Serviço principal responsável por gerenciar automações e workflows.
   - Utiliza **PostgreSQL** como banco de dados e **Redis** para gerenciar filas.
   - Inclui autenticação básica e suporte a execução em modo fila.

2. **Evolution API**:

   - Serviço de API utilizado para integração e processamento de mensagens e dados.
   - Configurado para compartilhar banco de dados e cache com o n8n.

3. **PostgreSQL**:

   - Banco de dados relacional compartilhado entre n8n e Evolution API.
   - Configurado para persistir todos os dados necessários das plataformas.

4. **Redis**:
   - Serviço de cache usado tanto pelo n8n quanto pela Evolution API.
   - Gerencia filas de execução e cache temporário para melhorar o desempenho.

---

## **Arquitetura do Docker Compose**

### **Estrutura Geral**

- **Volumes**:

  - `n8n_data`: Persistência de dados do n8n.
  - `evolution_data`: Persistência de dados da Evolution API.
  - `postgres_data`: Persistência de dados do PostgreSQL.
  - `redis_data`: Persistência de dados do Redis.

- **Serviços**:

  - Cada serviço está configurado com variáveis de ambiente específicas para integração no Railway.

- **Conexões Compartilhadas**:
  - Banco de dados PostgreSQL e Redis são compartilhados entre o n8n e a Evolution API.

---

## **Como Subir o Ambiente na Railway**

### **Passo 1: Preparar o Repositório**

1. Clone este repositório para o seu ambiente local:

   ```bash
   git clone <URL-DO-SEU-REPOSITORIO>
   cd <NOME-DO-REPOSITORIO>
   ```

2. Verifique se o arquivo `docker-compose.yml` está no diretório raiz do repositório.

3. Confirme que o repositório contém o arquivo atualizado com as configurações necessárias.

### **Passo 2: Configurar na Railway**

1. Acesse o painel da Railway e crie um novo projeto.

2. Escolha a opção **Deploy from GitHub**.

3. Conecte a Railway ao repositório contendo o arquivo `docker-compose.yml`.

4. Configure as seguintes variáveis de ambiente no painel da Railway:

#### **PostgreSQL**

```bash
POSTGRES_USER=n8n_admin
POSTGRES_PASSWORD=S3cur3P@ssw0rd!
POSTGRES_DB=n8n_agentsia_db
PGHOST=${{RAILWAY_TCP_PROXY_DOMAIN}}
PGHOST_PRIVATE=${{RAILWAY_PRIVATE_DOMAIN}}
PGPORT=${{RAILWAY_TCP_PROXY_PORT}}
PGPORT_PRIVATE=5432
```

#### **Redis**

```bash
REDIS_PASSWORD=S3cur3P@ssw0rd!
REDISUSER=default
REDISHOST=${{RAILWAY_TCP_PROXY_DOMAIN}}
REDISHOST_PRIVATE=${{RAILWAY_PRIVATE_DOMAIN}}
REDISPORT=${{RAILWAY_TCP_PROXY_PORT}}
REDISPORT_PRIVATE=6379
```

#### **n8n**

```bash
N8N_USER=admin
N8N_PASSWORD=adminpassword
N8N_ENCRYPTION_KEY=S3cur3P@ssw0rd!
WEBHOOK_URL=https://${{RAILWAY_PUBLIC_DOMAIN}}
```

#### **Evolution API**

```bash
AUTHENTICATION_API_KEY=d91f2743-1587-4967-b8f8-04cf1cc1dadd
WA_BUSINESS_TOKEN_WEBHOOK=evolution
WA_BUSINESS_URL=https://graph.facebook.com
WA_BUSINESS_VERSION=v20.0
WA_BUSINESS_LANGUAGE=pt_BR
```

### **Passo 3: Realizar o Deploy**

1. Após configurar as variáveis de ambiente, clique em **Deploy** no painel da Railway.
2. Acompanhe os logs no painel para verificar se todos os serviços foram iniciados corretamente.

### **Passo 4: Testar os Serviços**

- Acesse o n8n no endereço gerado pela Railway (porta `5678`).
- Verifique se a Evolution API está funcionando no endereço gerado (porta `8080`).

---

## **Principais Benefícios**

1. **Infraestrutura Automatizada**:

   - Toda a configuração é gerenciada pelo `docker-compose.yml`, simplificando o processo de deploy.

2. **Segurança**:

   - Uso de variáveis de ambiente para proteger credenciais e chaves de acesso.

3. **Escalabilidade**:

   - Possibilidade de escalar serviços como Redis e PostgreSQL diretamente pelo painel da Railway.

4. **Flexibilidade**:
   - Arquitetura compartilhada e modular, permitindo ajustes futuros com facilidade.

---

## **Considerações Finais**

Se precisar de suporte ou ajustes adicionais, entre em contato ou consulte a documentação oficial dos serviços utilizados (n8n, Evolution API, PostgreSQL e Redis). Boa sorte com o deploy! 🚀
