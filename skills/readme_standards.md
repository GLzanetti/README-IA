# Padrões e Boas Práticas para Escrita de README.md

Este documento estabelece as diretrizes de qualidade, estilo e formatação que devem ser aplicadas pelo subagente **Escritor** ao redigir arquivos `README.md`.

---

## 🧭 Princípio Fundamental: "Rode o projeto em 5 minutos"
O principal objetivo de um bom README é permitir que qualquer pessoa desenvolvedora (júnior ou sênior) consiga instalar, configurar e rodar o projeto em sua máquina local sem frustração e no menor tempo possível.

---

## 🎨 Diretrizes de Estilo e Escrita

### 1. Tom de Voz e Clareza
- **Profissional e Direto:** Escreva de forma objetiva, eliminando palavras vazias e floreios (ex: substitua "Este fantástico e incrível projeto serve para..." por "Este projeto gerencia faturas e...").
- **Voz Ativa:** Prefira instruir diretamente a ação (ex: "Crie o arquivo .env" em vez de "Deve ser criado o arquivo .env").
- **Gênero Neutro:** Escreva de forma inclusiva e acolhedora para toda a comunidade.

### 2. Formatação Técnica e Blocos de Código
- **Destaque de Linguagem:** Todos os blocos de código devem especificar explicitamente a linguagem para que o parser aplique o realce de sintaxe correto (ex: ````js`, ````bash`, ````python`, ````yaml`).
- **Instalações Limpas:** Forneça comandos de instalação que sejam fáceis de copiar e colar. Se o comando precisar de múltiplos passos, separe-os claramente em blocos ou com `&&` caso façam sentido juntos.
- **Não inclua valores reais em exemplos:** Ao mostrar variáveis de ambiente ou segredos no exemplo de `.env`, sempre use valores fictícios ilustrativos (ex: `PORT=3000` ou `DATABASE_URL=mongodb://localhost:27017/meu-banco`).

### 3. Padrão de Tabelas para Endpoints (APIs)
- APIs REST exigem tabelas estruturadas de rotas.
- O método HTTP deve vir destacado em negrito ou em bloco de código: `GET`, `POST`, `PUT`, `DELETE`.
- Indique claramente se os parâmetros exigidos estão no corpo da requisição (`Body`), na query string (`Query`) ou no próprio caminho (`Path`).
- **Exemplo de Formato:**
  | Método | Endpoint | Parâmetros | Descrição |
  |--------|----------|------------|-----------|
  | `GET`  | `/users` | `?limit=10` (query) | Retorna uma lista paginada de usuários ativos. |
  | `POST` | `/users` | `{ name, email }` (body) | Cria um novo usuário no sistema. |

### 4. Visual do Markdown
- **Emojis Estruturais:** Use emojis com moderação para atuar como marcadores visuais no início de seções importantes (ex: `🚀 Funcionalidades`, `⚙️ Pré-requisitos`, `📦 Dependências`). Não abuse deles no meio de parágrafos de texto comum.
- **Badges:** Use badges no topo do README para demonstrar de forma rápida a tecnologia do projeto, status de builds, cobertura de testes ou tipo de licença (utilize geradores públicos como shields.io).
- **Estruturas de Pastas Legíveis:** Ao desenhar a árvore de diretórios, use blocos de texto comuns de formato `text` e utilize símbolos unicode para desenhar as ramificações (`├──`, `└──`, `│`), deixando-a limpa e legível.

---

## 🚨 O que Evitar a Todo Custo
1. **Instruções Defasadas:** Nunca recomende ferramentas obsoletas de instalação ou comandos incorretos da stack.
2. **Textos Longos Sem Formatação:** Não crie "paredes de texto". Use listas com marcadores (`-`), negritos para ênfase e subtítulos (`###`) para quebrar o conteúdo.
3. **Links Quebrados:** Certifique-se de que caminhos relativos de pastas ou arquivos citados no README correspondam a diretórios reais do repositório (ex: `[LICENSE](LICENSE)` ou `[config/](config/)`).
