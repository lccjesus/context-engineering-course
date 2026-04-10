# Material de apoio — Context Engineering e workflows complexos

Repositório de apoio sobre **desenvolvimento com IA focado em contexto e fluxos de trabalho** (Alura).

Aqui estão **exemplos comentados** (instruções persistentes, prompts e decisão de sessão), além das **ferramentas de medição** usadas na parte inicial do material. O fio condutor é: **você só pode melhorar o que consegue medir**.

---

## Links e referências

O arquivo **[Links.md](Links.md)** reúne URLs e ponteiros citados na montagem deste material (este repositório e roteiros externos relacionados, quando existirem no mesmo workspace do instrutor), com breve contexto e sem duplicar listas enormes de skills linha a linha.

Já o arquivo **[Material.md](Material.md)** oferece um caminho prático de aprofundamento: começando pela documentação oficial, passando por ferramentas de apoio, artigos importantes e espaços de comunidade, com explicações sobre como cada recurso ajuda no aprendizado.

| Classificação | O que entra |
|---|---|
| **Ferramentas deste material** | AgentLens, statusline, Superpowers, npm CLI, scripts raw, repos auxiliares (ex.: claude-eval) |
| **Claude Code** | Documentação em `code.claude.com` (comandos, custos, skills, headless, agendamento, …) |
| **Claude / Anthropic (docs)** | Prompt engineering, settings, memory, subagents, artigo engineering |
| **Plataforma Claude (API)** | Preços, prompt caching, agent skills, testes e eval na console |
| **OpenAI Codex** | Hub developers, AGENTS.md, skills, MCP, plugins, enterprise |
| **Cursor** | Docs, rules, MCP, memories, marketplace |
| **Catálogo Anthropic Skills** | Repositório `anthropics/skills` e skills citadas em roteiros de marketplace |
| **Integrações (exemplos)** | Produtos citados como conectores em roteiros de plugins / MCP (Figma, Slack, DBs, …) |
| **Terceiros** | Blogs, pesquisa Chroma, listas awesome, artigos de evals e pricing |
| **Redes** | Posts em X citados em materiais de apoio |
| **Só texto / interno** | Links relativos que podem falhar no clone isolado, menções a arquivos de exemplo |

---

## O que há neste repositório

### `exemplos-agents/` — regras boas vs. regras que a IA ignora

Dois `AGENTS.md` comentados (bom × ruim) e um resumo em tabela.

| Arquivo | O que é |
|---|---|
| `exemplos-agents/AGENTS_md_BOM.md` | Boas práticas: instruções acionáveis, stack com versões, comandos concretos, progressive disclosure e restrições binárias |
| `exemplos-agents/AGENTS_md_RUIM.md` | Antipadrões: instruções vagas, roleplay desnecessário, princípios sem contexto e tokens desperdiçados |
| `exemplos-agents/AGENTS_md_RESUMO.md` | Resumo rápido do que incluir e o que evitar, e onde versionar o arquivo |

Nos exemplos bom/ruim, comentários em HTML (`<!-- ✅ POR QUÊ / ❌ PROBLEMA -->`) explicam cada decisão. Leia os dois lado a lado para ver o contraste.

### `exemplos-promps/` — prompts ruim × bom

Cenários em **arquivos separados** (`01-code-review.md` … `06-restricao-explicita.md`) para abrir só o trecho em uso. Detalhes: `exemplos-promps/README.md`.

### `clear-compact-subagent/` — /compact vs. nova sessão vs. subagent

Árvore de decisão em Markdown e Mermaid (`compact-vs-nova-sessao-vs-subagent.md` / `.mermaid`) para escolher quando compactar, reabrir sessão ou delegar a um subagent.

---

## Ferramentas usadas neste material

### 🔍 AgentLens — AI Context Cost Scanner

**Repositório:** [github.com/hugohvf/agentlens](https://github.com/hugohvf/agentlens)

AgentLens escaneia repositórios em busca de arquivos de configuração de agentes (`AGENTS.md`, `CLAUDE.md`, `.cursorrules` e outros), resolve todas as referências e imports declarados nesses arquivos, e calcula o **custo real em tokens por requisição** — com suporte a 20+ modelos da Anthropic, OpenAI, Google, DeepSeek e outros.

**Por que entra aqui:**

A linha de raciocínio começa medindo custo antes de otimizar. AgentLens torna esse custo visível: você cola a URL do seu repositório (ou roda via CLI na pasta local) e vê quanto contexto seu `AGENTS.md` está consumindo em cada sessão — e quanto isso custa em dólares. Sem essa medição, melhorar o contexto é tiro no escuro.

**Como usar:**

```bash
# CLI (requer Node.js >= 18)
npm install -g @hugofusinato/agentlens
agentlens              # analisa o diretório atual
agentlens /path/repo   # analisa um caminho específico
agentlens --open       # abre o relatório no navegador
```

Ou baixe `agentlens.html` do repositório e cole a URL de qualquer repo público do GitHub direto no navegador.

---

### 📊 Claude Code Statusline

**Repositório:** [github.com/hugohvf/claude-code-statusline](https://github.com/hugohvf/claude-code-statusline)

Script de statusline para Claude Code que exibe, em tempo real no terminal, três camadas de informação sobre a sessão em andamento:

- Modelo em uso, diretório e branch git
- Barra de progresso da janela de contexto com porcentagem, tamanho, custo em USD/BRL e duração
- Taxa de acerto do prompt cache (tokens em cache custam 90% menos)

A barra muda de cor conforme o contexto se enche (verde → amarelo → vermelho), e emite aviso quando ultrapassa 200K tokens.

**Por que entra aqui:**

Context engineering sem observabilidade é cego. O statusline transforma o custo da sessão em algo que você vê o tempo todo — não como surpresa no final do mês, mas como painel de controle em tempo real. É um baseline visual útil na parte de medição e em comparações antes/depois.

**Como instalar:**

```bash
# Instalação em um comando
curl -fsSL https://raw.githubusercontent.com/hugohvf/claude-code-statusline/main/install.sh | bash
```

Ou manualmente: baixe `statusline.sh` para `~/.claude/statusline.sh` e adicione ao seu `~/.claude/settings.json`:

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline.sh"
  }
}
```

Reinicie o Claude Code. Requer `bash`, `jq` e `git`.

---

## Progressão sugerida (sem ordem rígida)

```
Medir antes de otimizar
  └── AgentLens + Statusline para estabelecer baseline de custo

Instruções persistentes e hierarquia
  └── exemplos-agents/ (AGENTS_md_RUIM → AGENTS_md_BOM; RESUMO)
      comparação ao vivo, reescrita em tempo real

Sessão, prompts e continuidade
  └── exemplos-promps/ (pares ruim × bom por cenário)
  └── clear-compact-subagent/ (/compact vs nova sessão vs subagent)

Temas avançados (MCP, skills, subagents, evals, …)
  └── conteúdo costuma estar no LMS ou em materiais complementares do instrutor
  └── Statusline segue útil como painel contínuo de custo e cache hit
```

A ideia central: **medição primeiro** (AgentLens + statusline), depois **instruções e prompts** que o agent realmente segue, e por fim **decisões de sessão** quando o contexto pesa ou a tarefa muda de forma.
