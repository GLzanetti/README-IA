# Template de Análise de Projeto

Use este modelo para documentar as análises efetuadas. Substitua os textos entre colchetes pelos dados reais coletados do projeto sob análise.

---

## 🎯 Visão Geral do Sistema
- **Nome Sugerido para o Projeto:** [Ex: Meu Serviço de Pagamento]
- **Objetivo Principal:** [Descreva de forma clara e resumida o propósito do sistema, ou seja, o problema que ele resolve.]
- **Público / Casos de Uso comuns:** [Ex: Desenvolvedores integrando cobranças via Pix, clientes buscando consultar faturas, etc.]

## 🚀 Funcionalidades Principais
[Descreva as funcionalidades do sistema na forma de uma lista com marcadores. Se possível, agrupe ou detalhe o que cada uma faz.]
- **[Funcionalidade 1]:** [Descrição da funcionalidade 1 e os arquivos envolvidos.]
- **[Funcionalidade 2]:** [Descrição da funcionalidade 2.]

## 🏗️ Arquitetura e Padrões de Design
- **Padrão Arquitetural Detectado:** [Ex: MVC (Model-View-Controller), Clean Architecture, Script Único, Domain-Driven Design, Camadas simples, etc.]
- **Fluxo Geral de Dados:** [Explique resumidamente o fluxo que uma requisição ou comando segue dentro do sistema, por exemplo: Entrada no main.js -> Roteador -> Controller -> Service -> Banco de dados.]

## 📜 Endpoints e Rotas Mapeadas (Se aplicável)
[Caso o projeto seja uma API HTTP ou Webhook, preencha a tabela abaixo. Se não for aplicável, preencha com "Não se aplica a este tipo de projeto e explique por quê".]

| Método | Endpoint | Parâmetros Relevantes (Query/Body) | Descrição curta | Arquivo Responsável |
|--------|----------|------------------------------------|-----------------|----------------------|
| [GET]  | `/rota`  | `id` (query)                       | [Descrição]     | `src/routes/rota.js` |

## ⚙️ Variáveis de Ambiente e Requisitos Técnicos
[Liste os requisitos para o sistema rodar que foram extraídos do código.]
- **Porta padrão de escuta:** [Ex: 3000 ou lida de process.env.PORT]
- **Variáveis de Ambiente Necessárias:**
  - `[NOME_DA_VARIAVEL]`: [Propósito da variável e se há um valor padrão detectado no código.]
- **Dependências Externas / Serviços:** [Ex: Banco de dados MongoDB, Redis, API do Stripe, etc.]

## 📦 Dependências Principais e suas Funções
[Liste as dependências mais críticas que ditam o comportamento do app (ex: express, mongoose, dotenv, axios, pg, click, etc.) e explique brevemente por que o app precisa delas.]
- `[dependencia-1]`: [Finalidade no app]
- `[dependencia-2]`: [Finalidade no app]

## 🛠️ Instruções Técnicas de Inicialização e Execução
[Instruções deduzidas a partir da análise dos arquivos de código e manifesto.]
- **Comando de Instalação:** `[Ex: npm install, pip install -r requirements.txt]`
- **Comando de Execução (Dev):** `[Ex: npm run dev, python main.py]`
- **Comando de Execução (Prod):** `[Ex: npm start, uvicorn main:app --host 0.0.0.0]`
- **Comando de Teste:** `[Ex: npm test, pytest]`

## ⚠️ Observações e Alertas de Varredura
- **Limites de Contexto Aplicados:** [Se algum arquivo ultrapassou 600 linhas e foi cortado na leitura, descreva-o aqui. Caso contrário, declare: "Nenhum arquivo ultrapassou o limite de contexto".]
- **Arquivos Ignorados por Segurança:** [Se chaves ou arquivos `.env` reais foram ignorados pela análise, registre.]
- **Outras Observações:** [Qualquer comportamento estranho, arquivo corrompido, imports quebrados, ou inconsistências detectadas na codebase.]
