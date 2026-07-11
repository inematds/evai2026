# Objetivo — Conteúdo derivado do AIS Live

## O que é isto

Extraímos o conteúdo do evento **AIS Live — Real Projects. Real Revenue** (AI Automation Society, 11-12/jul/2026) como matéria-prima para decidir e produzir conteúdo próprio — não para reproduzir o evento, mas para usá-lo como mapa de temas relevantes no mercado de serviços de IA.

## Decisão de estruturação

O conteúdo do evento se divide em três blocos com destinos diferentes (ver `analise-estruturacao.md`):

1. **Bloco técnico (8 tópicos) → vira CURSO.** Conteúdo "mão na massa", ensinável em módulos/aulas.
2. **Bloco estratégico (7 tópicos) → vira PALESTRAS/vídeos avulsos.** Ponto de vista forte, reaproveitável em qualquer canal.
3. **Mecânica de formato do evento → inspiração para um EVENTO próprio**, quando fizer sentido organizar um ao vivo (não é conteúdo consumível, é know-how de estrutura de programação).

## Estado atual

Os **15 roteiros completos estão prontos** em `conteudo-final/` (arquivos `01-*.md` a `15-*.md`):

- **Bloco 1 (01–08):** aulas do curso "Como montar um negócio de serviços de IA", com pontes de sequência entre as aulas.
- **Bloco 2 (09–15):** palestras/vídeos avulsos, com cabeçalho padronizado (formato + tese central).

**Decisão editorial (2026-07-10): conteúdo despersonalizado.** Os roteiros foram reescritos para ficarem 100% standalone e autorais: sem nomes de pessoas reais (palestrantes do evento, fundadores, criadores), sem métricas/histórias pessoais, sem o aparato [VERIFICADO]/[SÍNTESE] e sem seções de fontes. Casos reais viraram casos anônimos; o que ficou é o conteúdo que ensina (pipelines, frameworks, prompts, práticas guiadas, exercícios). Nomes de ferramentas e produtos (n8n, Make, Claude Code, HeyGen etc.) permanecem. Os outlines antigos em `outlines-15-topicos.md` seguem como referência histórica de pesquisa (ainda contêm os nomes/fontes originais).

## Próximo passo (em aberto)

Decidir o formato de produção dos roteiros: vídeo (skills de vídeo INEMA), curso HTML (formato-curso) e/ou publicação no portal.

## Arquivos desta iniciativa

**Entregáveis (vão pro git):**
- `curso/` — curso HTML no formato INEMA v5, PRONTO: `index.html` (landing) + `curso.html` (página única com trilha + 8 aulas) + `assets/` (motor) + `fragments/` (fonte das views; `curso.html` = concatenação dos fragmentos) + `roteiros/` (roteiros 01–08 despersonalizados)
- `lives/` — os 7 roteiros de palestras/vídeos avulsos (09–15) despersonalizados
- `objetivo.md` — este arquivo

Descoberta do curso (v5): público 40+ liberal/escritório, leigo em programação; profissões-alvo contador(a)/advogado(a)/corretor(a)/assistente administrativa; ~20 min por aula; práticas em modo prompt/tarefa/análise (nunca código).

**Material interno (pasta `doc/`, fora do git):**
- `doc/curso-negocio-de-servicos-de-ia.md` — documento consolidado do curso (aulas 1–8, com índice)
- `doc/palestras-estrategicas.md` — documento consolidado das palestras (7 vídeos avulsos, com índice)
- `doc/ais-live-punisher.md` — conteúdo bruto extraído do evento (agenda, sessões, palestrantes)
- `doc/analise-estruturacao.md` — análise de como dividir o conteúdo (curso / palestra / evento)
- `doc/outlines-15-topicos.md` — outlines estruturados e pesquisados (contém nomes/fontes originais)

O `.gitignore` na raiz do projeto exclui `doc/`. (Pasta do projeto renomeada de `ais-live-2026` para `curso-e-live` em 2026-07-10.)
