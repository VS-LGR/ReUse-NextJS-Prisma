# 🚨 RESOLVER ERRO 405 - Method Not Allowed

## ❌ PROBLEMA:
- Erro 405 ao tentar usar `/api/offer/status`
- Endpoints de ação não estão funcionando
- Node-RED não carregou os novos fluxos

## ✅ SOLUÇÃO RÁPIDA:

### **1. PARAR O NODE-RED:**
- No terminal onde o Node-RED está rodando, pressione `Ctrl+C`
- Aguarde até aparecer a mensagem de parada

### **2. REINICIAR O NODE-RED:**
```bash
cd node-red-admin
npm start
```

### **3. VERIFICAR SE FUNCIONOU:**
- Acesse: `http://localhost:1880/admin/offers`
- Tente clicar em "Desativar" em uma oferta
- Deve funcionar sem erro 405

## 🔍 ALTERNATIVA - VIA NODE-RED EDITOR:

### **1. Abrir Node-RED Editor:**
- Acesse: `http://localhost:1880`

### **2. Verificar se os nós existem:**
- Procure por nós com nomes:
  - "Block User" (usuários)
  - "Change Offer Status" (ofertas)
  - "Delete Offer" (ofertas)

### **3. Se não existirem:**
- Clique no menu **≡** → **Import**
- Cole o conteúdo de `node-red-admin/flows/user-actions-flow.json`
- Clique em **Import**
- Repita para `node-red-admin/flows/offer-actions-flow.json`
- Clique em **Deploy**

## 🎯 RESULTADO ESPERADO:

Após reiniciar, os endpoints devem funcionar:
- ✅ `POST /api/offer/status` - Ativar/Desativar ofertas
- ✅ `POST /api/user/block` - Bloquear/Desbloquear usuários
- ✅ `POST /api/offer/delete` - Remover ofertas
- ✅ `POST /api/user/password` - Trocar senhas

## 🚨 IMPORTANTE:

**A API do ReUse NÃO precisa ser reiniciada!**
- Apenas o Node-RED precisa ser reiniciado
- Os endpoints já foram adicionados ao `flows.json`
- Só falta o Node-RED carregar as mudanças

**Reinicie o Node-RED e teste novamente! 🚀**
