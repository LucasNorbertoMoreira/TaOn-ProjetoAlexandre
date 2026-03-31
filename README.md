# 🟢 TáOn? | Uptime Monitor Micro-SaaS

> O seu site caiu e o seu cliente foi embora. O TáOn? te avisa no WhatsApp antes que perceba.

![Status](https://img.shields.io/badge/Status-Em_Desenvolvimento-green)
![Versão](https://img.shields.io/badge/Version-1.0.0-blue)
![Licença](https://img.shields.io/badge/License-MIT-lightgrey)

## 📖 Sobre o Projeto
**TáOn?** é um Micro-SaaS desenvolvido para resolver uma dor silenciosa e cara: o *downtime* (tempo de inatividade) não detetado de websites e e-commerces. 

Em vez de dashboards complexos ou alertas por e-mail que caem na caixa de spam, o sistema foca na urgência, testando as rotas a cada 1 minuto e disparando um alerta com diagnóstico imediato diretamente no **WhatsApp** do gestor.

## 🚀 Funcionalidades (Features)
- **Monitorização Real-Time:** Verificação de latência (ping) e status HTTP a cada 60 segundos.
- **Alertas no WhatsApp:** Integração via Evolution API para envio instantâneo de alertas de queda e recuperação.
- **Dashboard Reativo:** Painel de controlo dinâmico onde o estado dos monitores atualiza sem necessidade de recarregar a página.
- **Diagnóstico Clínico:** Identificação da causa raiz da falha (Ex: `Timeout`, `DNS ENOTFOUND`, `Erro 500`).
- **Segurança Multitenant (SaaS):** Isolamento de dados por cliente utilizando Row Level Security (RLS) no Supabase.

## 🛠️ Stack Tecnológica (O Motor)

O projeto está dividido numa arquitetura moderna e escalável, separando o Frontend da Orquestração de Backend.

**Frontend:**
* [React.js](https://react.dev/) + [Vite](https://vitejs.dev/) - Performance e componentização.
* **HTML/CSS Nativo** - Interfaces e Landing Page otimizadas para conversão.
* [React Router](https://reactrouter.com/) - Navegação Single Page Application (SPA).

**Backend & Base de Dados:**
* [Supabase](https://supabase.com/) - PostgreSQL como serviço, Autenticação e WebSockets (Realtime DB).
* [n8n](https://n8n.io/) - Motor de orquestração e fluxos lógicos (Cron Jobs, HTTP Requests, Lógica de falhas).
* **Evolution API** - Gateway para envio de mensagens via WhatsApp.

## ⚙️ Arquitetura e Fluxo de Dados (Workflow)

1. **Agendamento (n8n):** Um nó Cron aciona o fluxo a cada minuto.
2. **Busca (Supabase):** O n8n puxa todos os monitores ativos via *Service Role Key*.
3. **Teste (HTTP Request):** Medição do tempo de resposta (start_time - end_time) e captura do HTTP Status Code.
4. **Decisão (Lógica JavaScript):** Tratamento de falsos positivos, erros de rede (`null` status) e erros de servidor (5xx).
5. **Ação:** Atualização da DB em tempo real. Se o status alterar para `offline`, dispara o Webhook para o WhatsApp.

## 📂 Estrutura de Diretórios (Frontend)

A organização das pastas foi pensada para escalar, separando páginas de negócio (Landing/Planos) do produto (Dashboard/Auth).

```text
📦 taon-app
 ┣ 📂 public/              # Assets estáticos globais
 ┣ 📂 src/
 ┃ ┣ 📂 assets/
 ┃ ┃ ┣ 📂 css/             # Estilos de cada página (Dashboard.css, etc)
 ┃ ┃ ┗ 📂 img/             # Logotipos, ícones e backgrounds
 ┃ ┣ 📂 components/        # Componentes reutilizáveis (Futuro: Botões, Navbar)
 ┃ ┣ 📂 pages/             # Ecrãs principais da aplicação
 ┃ ┃ ┣ 📜 Landing.jsx      # Página de Vendas (Home)
 ┃ ┃ ┣ 📜 Planos.jsx       # Pricing / Assinatura
 ┃ ┃ ┣ 📜 Cadastro.jsx     # Registo de novo utilizador
 ┃ ┃ ┣ 📜 Login.jsx        # Autenticação
 ┃ ┃ ┗ 📜 Dashboard.jsx    # Painel de Monitorização (Produto)
 ┃ ┣ 📜 App.jsx            # Configuração de Rotas (React Router)
 ┃ ┗ 📜 main.jsx           # Ponto de entrada do React
 ┣ 📜 index.html           # Template base
 ┣ 📜 package.json         # Dependências do projeto
 ┗ 📜 vite.config.js       # Configuração do bundler
