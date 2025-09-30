# 🔄 Fluxo de Integração Node-RED + ReUse API

## 📊 Diagrama de Arquitetura

```
┌─────────────────┐    HTTP Request    ┌─────────────────┐
│                 │ ──────────────────▶│                 │
│   Admin Panel   │                    │   Node-RED      │
│   (Frontend)    │◀────────────────── │   (Middleware)  │
│                 │    HTML Response   │                 │
└─────────────────┘                    └─────────────────┘
         │                                       │
         │ AJAX Call                             │ HTTP Request
         │ /api/users-proxy                      │ localhost:3000
         ▼                                       ▼
┌─────────────────┐                    ┌─────────────────┐
│                 │                    │                 │
│   Proxy Nodes   │                    │   ReUse API     │
│   (Node-RED)    │◀────────────────── │   (Backend)     │
│                 │    JSON Response   │                 │
└─────────────────┘                    └─────────────────┘
                                               │
                                               ▼
                                       ┌─────────────────┐
                                       │                 │
                                       │   Database      │
                                       │   (Prisma)      │
                                       │                 │
                                       └─────────────────┘
```

## 🔧 Componentes Detalhados

### 1. **HTTP In Nodes** (Entrada de Requisições)
```json
{
  "id": "admin-login",
  "type": "http in",
  "url": "/admin",
  "method": "get",
  "upload": false
}
```

**Função**: Receber requisições HTTP do navegador
- **Rota**: `/admin` → Página de login
- **Método**: GET
- **Headers**: `Content-Type: text/html`

### 2. **Function Nodes** (Processamento de Lógica)
```javascript
// Exemplo: Dashboard HTML Generator
msg.payload = `<!DOCTYPE html>
<html>
<head>
    <title>ReUse Admin - Dashboard</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>/* CSS inline */</style>
</head>
<body>
    <!-- Interface HTML -->
    <script>
        // JavaScript para interatividade
        async function loadData() {
            const response = await fetch('/api/users-proxy');
            const data = await response.json();
            // Processar e exibir dados
        }
    </script>
</body>
</html>`;

msg.headers = { 'Content-Type': 'text/html; charset=utf-8' };
return msg;
```

**Função**: Gerar HTML dinâmico e processar lógica
- **Input**: Requisição HTTP
- **Processamento**: JavaScript server-side
- **Output**: HTML completo com CSS/JS inline

### 3. **HTTP Request Nodes** (Integração com API)
```json
{
  "id": "http-request-users",
  "type": "http request",
  "method": "GET",
  "url": "http://localhost:3000/api/users",
  "headers": {
    "Content-Type": "application/json"
  }
}
```

**Função**: Fazer requisições para o backend ReUse
- **URL**: `http://localhost:3000/api/users`
- **Método**: GET
- **Headers**: JSON
- **Retorno**: Dados da API ReUse

### 4. **HTTP Response Nodes** (Resposta ao Cliente)
```json
{
  "id": "admin-response",
  "type": "http response",
  "statusCode": "200",
  "headers": {
    "Content-Type": "text/html; charset=utf-8"
  }
}
```

**Função**: Enviar resposta HTTP para o navegador
- **Status**: 200 (sucesso)
- **Headers**: HTML com charset UTF-8
- **Conteúdo**: Página HTML completa

## 🔗 Fluxo de Dados Detalhado

### **Cenário 1: Acessar Dashboard**

```
1. Usuário acessa: http://localhost:1880/admin/dashboard
   ↓
2. HTTP In Node recebe requisição GET /admin/dashboard
   ↓
3. Function Node "Dashboard HTML" executa:
   - Gera HTML completo com Chart.js
   - Inclui CSS e JavaScript inline
   - Configura event listeners
   ↓
4. HTTP Response Node envia HTML para o navegador
   ↓
5. Navegador renderiza a página
   ↓
6. JavaScript executa e faz fetch para /api/users-proxy
   ↓
7. Proxy Node busca dados na API ReUse
   ↓
8. Dados são retornados e exibidos no gráfico
```

### **Cenário 2: Carregar Lista de Usuários**

```
1. JavaScript executa: fetch('/api/users-proxy')
   ↓
2. HTTP In Node recebe requisição GET /api/users-proxy
   ↓
3. HTTP Request Node faz requisição para:
   http://localhost:3000/api/users
   ↓
4. ReUse API processa e retorna JSON:
   [
     {
       "id": "cmg5z8x7x0000kc70tuq2bsub",
       "name": "João Silva",
       "email": "joao@email.com",
       "createdAt": "2025-09-30T00:07:41.000Z",
       "isBlocked": false
     }
   ]
   ↓
5. HTTP Response Node retorna dados para o frontend
   ↓
6. JavaScript processa e exibe na interface
```

## 🎯 Vantagens da Arquitetura

### **1. Separação de Responsabilidades**
- **ReUse API**: Lógica de negócio, validação, banco de dados
- **Node-RED**: Interface administrativa, proxy, processamento
- **Frontend**: Interação do usuário, visualização

### **2. Resolução de CORS**
- **Problema**: Frontend não pode acessar API diretamente
- **Solução**: Proxy server-side no Node-RED
- **Resultado**: Mesmo domínio, sem problemas de CORS

### **3. Flexibilidade de Desenvolvimento**
- **Modificações**: Sem tocar no backend principal
- **Deploy**: Independente do sistema principal
- **Debug**: Interface visual do Node-RED

### **4. Performance**
- **Proxy**: Reduz latência de requisições
- **Processamento**: Server-side para transformação de dados
- **Cache**: Possível implementar cache no Node-RED

## 🔧 Configuração Técnica

### **Node-RED Settings**
```javascript
// settings.js
module.exports = {
    httpAdminRoot: '/admin',
    httpNodeRoot: '/api',
    userDir: './data',
    flowFile: 'flows.json',
    credentialSecret: 'reuse-admin-secret',
    ui: {
        path: 'ui',
        middleware: function(req, res, next) {
            // Middleware personalizado se necessário
            next();
        }
    }
};
```

### **Dependências Necessárias**
```json
{
  "node-red": "^3.0.0",
  "node-red-contrib-http-request": "^1.0.0"
}
```

### **Portas e URLs**
- **Node-RED Editor**: `http://localhost:1880`
- **Admin Panel**: `http://localhost:1880/admin`
- **API Proxy**: `http://localhost:1880/api/*`
- **ReUse API**: `http://localhost:3000/api/*`

## 🚀 Próximos Passos

### **Melhorias Implementáveis**
1. **Autenticação Real**: Integrar com JWT do backend
2. **Ações CRUD**: Implementar POST/PUT/DELETE
3. **Cache Inteligente**: Redis para melhor performance
4. **Websockets**: Atualizações em tempo real
5. **Logs de Auditoria**: Sistema completo de logs

### **Extensões Possíveis**
1. **Relatórios Avançados**: Mais tipos de gráficos
2. **Notificações Push**: Sistema de alertas
3. **Backup Automático**: Exportação de dados
4. **Multi-tenant**: Suporte a múltiplas organizações
5. **API Gateway**: Roteamento inteligente de requisições

---

## 📚 Conclusão

Esta arquitetura **Node-RED + ReUse API** oferece:

✅ **Interface administrativa moderna** sem modificar o backend
✅ **Resolução automática de CORS** via proxy
✅ **Flexibilidade total** para adicionar funcionalidades
✅ **Manutenção simplificada** via interface visual
✅ **Performance otimizada** com processamento server-side

É uma solução **ideal para sistemas que precisam de painéis administrativos** robustos e flexíveis, mantendo a **separação de responsabilidades** e **facilitando a evolução** do sistema.
