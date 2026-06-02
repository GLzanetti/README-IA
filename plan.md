# Plano de Desenvolvimento: Agente Gerador de README.md

## 🎯 Objetivo
Criar um agente que analisa a estrutura e o código de um projeto de software para gerar um `README.md` profissional e bem estruturado. O foco são projetos pequenos e médios — microserviços backend, APIs REST, páginas web simples — onde a documentação costuma ser negligenciada.

---

## 🧱 Stack e Ambiente de Execução

| Componente            | Escolha                                                                 |
|-----------------------|-------------------------------------------------------------------------|
| **IDE / Harness**     | Zed — agente conectado ao repositório do projeto                        |
| **Modelos**           | Via OpenRouter (ex: `gpt-o4s-120b`, modelos gratuitos ou de baixo custo)|
| **Orquestração**      | Subagent dispatching nativo do modelo — sem código externo              |
| **Formato**           | 100% Markdown — nenhum arquivo de código no projeto                     |
| **Estado entre agentes** | Arquivos escritos na pasta `state/` durante a execução              |

> **Como funciona:** O modelo lê o `AGENTS.md` como ponto de entrada, interpreta o roteiro, e usa seu mecanismo nativo de dispatching para invocar cada subagente na ordem certa. Cada subagente recebe seu prompt (do arquivo correspondente em `agents/`), executa sua tarefa, escreve o resultado em `state/`, e o modelo orquestrador segue o fluxo.

---

## 📋 `AGENTS.md` — Ponto de Entrada

O `AGENTS.md` é o arquivo que o modelo lê primeiro. Ele cumpre três funções:

1. **Contexto geral** — descreve o objetivo do sistema e o escopo de uso.
2. **Registro dos subagentes** — lista quais subagentes existem, qual arquivo contém seu prompt e quais ferramentas cada um usa.
3. **Roteiro de execução** — descreve a ordem das etapas, o que cada subagente recebe como input e o que deve produzir como output.

O `AGENTS.md` não contém system prompts detalhados — esses ficam nos arquivos `agents/*.md` para manter o arquivo raiz legível e fácil de editar.

---

## 🤖 Subagentes

### 1. Explorador (`agents/explorer.md`)
- **Quando é despachado:** Início da execução.
- **O que faz:** Varre os diretórios do projeto, identifica a stack tecnológica, localiza arquivos de manifesto (`package.json`, `requirements.txt`, `go.mod`, `Cargo.toml`, etc.) e mapeia os entry points.
- **Ferramentas:** `list_directory`, `read_file`, `grep`.
- **Output:** Escreve `state/project_map.md` com a estrutura e stack encontradas.

### 2. Analista (`agents/analyst.md`)
- **Quando é despachado:** Após o Explorador.
- **O que faz:** Lê o `state/project_map.md`, prioriza os arquivos mais relevantes (entry points, interfaces públicas, arquivos de config), extrai a finalidade do projeto, funcionalidades principais, dependências e instruções de instalação e uso inferidas do código.
- **Ferramentas:** `read_file`, `grep`.
- **Output:** Preenche o template `templates/project_analysis_template.md` e escreve o resultado em `state/analysis.md`.

### 3. Escritor (`agents/writer.md`)
- **Quando é despachado:** Após o Analista.
- **O que faz:** Lê `state/analysis.md`, aplica os padrões de `skills/readme_standards.md` e usa `templates/readme_template.md` como estrutura base para redigir o `README.md` final.
- **Ferramentas:** `read_file`, `write_file`.
- **Output:** Escreve o `README.md` na raiz do projeto analisado.

---

## 🔄 Fluxo de Execução

```
[AGENTS.md]
     │
     ▼
[Explorador] → state/project_map.md
     │
     ▼
[Analista] → state/analysis.md
     │
     ▼
[Escritor] → README.md
```

O modelo orquestrador valida a entrega de cada etapa antes de despachar o próximo subagente. Se um arquivo de estado esperado não for produzido, o orquestrador tenta novamente ou reporta o erro ao usuário antes de continuar.

---

## 📂 Estrutura do Projeto

```text
readme-ia/
├── AGENTS.md                         # Ponto de entrada: contexto, registro e roteiro
├── agents/
│   ├── explorer.md                   # Prompt do subagente Explorador
│   ├── analyst.md                    # Prompt do subagente Analista
│   └── writer.md                     # Prompt do subagente Escritor
├── skills/
│   └── readme_standards.md           # Boas práticas injetadas no contexto do Escritor
├── templates/
│   ├── readme_template.md            # Estrutura base do README.md gerado
│   └── project_analysis_template.md  # Template preenchido pelo Analista
├── state/
│   └── .gitkeep                      # Arquivos de estado gerados durante a execução
└── README.md                         # Documentação do próprio projeto readme-ia
```

---

## 💾 Gerenciamento de Estado

A pasta `state/` é a memória compartilhada entre os subagentes durante uma execução. Cada arquivo tem responsável e formato definidos:

| Arquivo                   | Produzido por | Consumido por | Conteúdo                                      |
|---------------------------|---------------|---------------|-----------------------------------------------|
| `state/project_map.md`    | Explorador    | Analista      | Estrutura de pastas, stack, entry points       |
| `state/analysis.md`       | Analista      | Escritor      | Análise técnica completa baseada no template   |

Os arquivos de `state/` não são commitados — devem constar no `.gitignore` do projeto `readme-ia`. São descartados ao fim de cada execução.

---

## ⚠️ Tratamento de Erros

Descrito diretamente no `AGENTS.md` como instruções ao modelo orquestrador:

| Situação                              | Comportamento esperado                                                  |
|---------------------------------------|-------------------------------------------------------------------------|
| `state/project_map.md` vazio          | Reportar ao usuário que o projeto não foi reconhecido e encerrar        |
| Arquivo muito grande para o contexto  | Ler apenas as primeiras e últimas 100 linhas; registrar aviso no estado |
| Diretório sem arquivos reconhecíveis  | Ignorar e continuar; listar o que foi ignorado no `project_map.md`      |
| Arquivo de estado ausente após dispatch | Tentar novamente uma vez; se falhar, reportar a etapa e encerrar      |
| Arquivo sensível detectado (`.env`, `*.key`) | Ignorar sem ler; nunca incluir conteúdo no estado              |

---

## 🧭 Escopo de Uso

Este agente é otimizado para:
- Microserviços backend (Node.js, Python, Go, Rust)
- APIs REST com estrutura clara de rotas e controllers
- Páginas web simples (HTML/CSS/JS, React, Vue)
- Projetos com até ~50 arquivos de código relevantes

Não é o foco (pelo menos por ora):
- Monorepos com múltiplos pacotes
- Projetos sem nenhum arquivo de manifesto ou entry point reconhecível
- Codebases com lógica majoritariamente em arquivos de configuração

---

## 🚀 Próximos Passos

1. [x] Escrever o `AGENTS.md` com contexto geral, registro dos subagentes e roteiro de execução.
2. [x] Escrever o prompt de `agents/explorer.md`.
3. [x] Escrever o prompt de `agents/analyst.md`.
4. [x] Escrever o prompt de `agents/writer.md`.
5. [x] Criar `templates/project_analysis_template.md` com as seções obrigatórias.
6. [x] Criar `templates/readme_template.md` com a estrutura base do README.
7. [x] Escrever `skills/readme_standards.md` com os padrões de documentação.
8. [x] Testar em projetos reais pequenos de diferentes linguagens.
9. [x] Escrever o `README.md` do próprio projeto `readme-ia`.
