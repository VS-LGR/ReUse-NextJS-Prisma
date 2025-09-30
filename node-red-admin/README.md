# 🚀 ReUse Admin Panel

Painel administrativo completo para o sistema ReUse, desenvolvido com Node-RED e integrado com as APIs do backend.

## ✨ Funcionalidades

- **📊 Dashboard Interativo**: Gráficos de atividade e métricas em tempo real
- **👥 Gerenciamento de Usuários**: Visualizar, bloquear/desbloquear, trocar senhas
- **📦 Gerenciamento de Ofertas**: Visualizar, ativar/desativar, remover ofertas
- **🔐 Sistema de Login**: Autenticação segura para administradores
- **📱 Interface Responsiva**: Design moderno e adaptável

## 🏗️ Arquitetura

```
Frontend (HTML/CSS/JS) → Node-RED (Middleware) → ReUse API (Backend)
```

- **Node-RED**: Serve como middleware e proxy
- **ReUse API**: Backend principal com dados
- **Proxy Pattern**: Resolve problemas de CORS automaticamente

## 🚀 Instalação e Uso

### **Pré-requisitos**
- Node.js 16+ instalado
- ReUse API rodando em `localhost:3000`

### **Instalação Rápida**
```bash
# 1. Instalar dependências
npm install

# 2. Iniciar o painel
npm start

# 3. Acessar
# http://localhost:1880/admin
```

### **Comandos Disponíveis**
```bash
npm start          # Iniciar painel (Windows/Linux)
npm run start:windows  # Iniciar no Windows
npm run dev        # Modo desenvolvimento
```

## 📁 Estrutura do Projeto

```
node-red-admin/
├── 📄 README.md                    # Este arquivo
├── 📄 package.json                 # Configurações do projeto
├── 📄 start.js                     # Script de inicialização
├── 📄 start-windows.js             # Script para Windows
├── 📁 data/                        # Dados do Node-RED
│   ├── flows.json                  # Fluxos principais
│   └── settings.js                 # Configurações
├── 📁 flows/                       # Fluxos organizados
│   └── simple-admin-flow.json      # Fluxo principal
├── 📁 api/                         # APIs de integração
│   ├── admin-auth.js               # Autenticação
│   └── reuse-api.js                # Cliente da API
└── 📁 arquivos-js/                 # Códigos das páginas
    ├── login-page-with-logo.js     # Página de login
    ├── dashboard-with-chart.js     # Dashboard com gráficos
    ├── users-page-fixed-ids.js     # Gerenciamento de usuários
    └── offers-page-updated-modal.js # Gerenciamento de ofertas
```

## 🔧 Configuração

### **URLs Importantes**
- **Painel Admin**: http://localhost:1880/admin
- **Editor Node-RED**: http://localhost:1880
- **ReUse API**: http://localhost:3000

### **Fluxos Node-RED**
1. **Login Flow**: Autenticação de administradores
2. **Dashboard Flow**: Página principal com métricas
3. **Users Flow**: Gerenciamento de usuários
4. **Offers Flow**: Gerenciamento de ofertas
5. **Proxy Flows**: Integração com APIs ReUse

## 📊 Funcionalidades Detalhadas

### **Dashboard**
- Gráficos interativos com Chart.js
- Métricas em tempo real
- Períodos flexíveis (7, 30, 90 dias)
- Atividades recentes

### **Usuários**
- Lista completa de usuários
- Busca e filtros
- Bloquear/desbloquear usuários
- Trocar senhas
- Modais elegantes com confirmação

### **Ofertas**
- Lista de ofertas com imagens
- Visualizar detalhes completos
- Ativar/desativar ofertas
- Remover ofertas
- Títulos dinâmicos gerados

## 🔒 Segurança

- **Autenticação**: Sistema de login com tokens
- **CORS**: Resolvido via proxy server-side
- **Validação**: Dados validados antes do processamento
- **Headers**: Configurações de segurança adequadas

## 🛠️ Desenvolvimento

### **Adicionar Nova Página**
1. Criar Function Node no Node-RED
2. Adicionar HTTP In/Response nodes
3. Implementar lógica JavaScript
4. Fazer Deploy

### **Modificar Interface**
1. Editar arquivos `.js` correspondentes
2. Copiar código para Function Node
3. Fazer Deploy

### **Debug**
- Use o editor Node-RED para visualizar fluxos
- Console do navegador para erros frontend
- Logs do Node-RED para erros backend

## 📚 Documentação

- **ARQUITETURA-INTEGRACAO.md**: Explicação completa da arquitetura
- **FLUXO-INTEGRACAO.md**: Fluxos de dados detalhados

## 🚨 Troubleshooting

### **Problemas Comuns**
1. **API não responde**: Verificar se ReUse API está rodando
2. **CORS errors**: Usar sempre endpoints proxy
3. **Dados não carregam**: Verificar logs do Node-RED
4. **Interface não atualiza**: Fazer Deploy após mudanças

### **Logs Importantes**
```bash
# Verificar se API está rodando
curl http://localhost:3000/api/users

# Verificar proxy
curl http://localhost:1880/api/users-proxy
```

## 🎯 Próximos Passos

- [ ] Implementar autenticação real com JWT
- [ ] Adicionar ações CRUD completas
- [ ] Implementar cache para performance
- [ ] Adicionar websockets para tempo real
- [ ] Sistema de logs de auditoria

## 📄 Licença

Este projeto é parte do sistema ReUse e segue a mesma licença.

---

**Desenvolvido com ❤️ para o ReUse**