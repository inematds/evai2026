
# Aula 6 — Como rodar uma agência inteira dentro do Claude Code

**Curso:** Como montar um negócio de serviços de IA
**Bloco:** Curso técnico — Trilha "mão na massa"
**Duração estimada:** 55 minutos (± 5 min conforme ritmo de gravação)

---

## Abertura / gancho (0:00 – 3:00)

Fala direto pra câmera, sem rodeio:

"Deixa eu te perguntar uma coisa. Quantas abas você tem abertas agora pra rodar sua agência? CRM numa aba. Trello ou ClickUp noutra. Notion pra base de conhecimento. Calendário. Uma ferramenta de automação tipo n8n. Slack. Email. Uma pasta de Drive com os briefings dos clientes espalhada sabe-se lá onde. Se você contou mais de quatro, você tem o que essa aula vai chamar de **tool sprawl** — uma agência que existe espalhada em dez lugares diferentes, e que só funciona porque *você* é a cola que conecta tudo isso na cabeça.

Isso não escala. E o pior: cada ferramenta nova que você assina não te dá mais controle — te dá mais um lugar pra esquecer de olhar.

Existe um jeito oposto de operar, e ele já está sendo usado por agências de automação de IA reais: **a agência inteira rodando de dentro de um workspace só, no Claude Code.** CRM, gestão de projeto, base de conhecimento, calendário de conteúdo, pesquisa, entrega pro cliente, vendas, conteúdo, planejamento do dia e o próprio playbook de retainer — tudo dentro da mesma pasta, do mesmo terminal, da mesma conversa com o agente. E a mesma stack que a agência usa internamente é a que ela entrega pros clientes de serviço.

O papel de hoje é desmontar como isso é montado de verdade — não é mágica, é estrutura de arquivo e disciplina de rotina. E pra isso a gente vai usar dois frameworks: os **Quatro Cs** (a arquitetura do workspace) e os **3Ms** (a cabeça de quem opera). No fim da aula você sai com um mini sistema operacional de agência seu, de verdade, rodando."

---

## 1. Por que "um workspace" e não dez ferramentas — a tese do tool sprawl (3:00 – 8:00)

A ideia central: parar de "colar" ferramentas soltas e passar a operar como **um sistema único**.

Isso não é purismo estético. É um argumento de custo cognitivo. Cada ferramenta separada tem: login próprio, estrutura de dados própria, e — o pior de tudo — **nenhuma delas sabe o que a outra sabe**. Seu CRM não sabe o que está no Notion. Seu Notion não sabe o que foi decidido no Slack ontem. Você é o único lugar onde essas informações se encontram. Isso significa que a sua agência só escala até onde a sua memória escala.

Um workspace de Claude Code — uma pasta de projeto com um `CLAUDE.md`, pastas de contexto, skills e subagents — inverte isso. A "memória" da agência vira um arquivo versionado, pesquisável, que qualquer sessão nova do Claude consegue ler do zero e responder "o que essa empresa faz e quem trabalha aqui" sem precisar de você explicar de novo. Guarda essa frase entre aspas — ela vai voltar como o teste de aceitação da camada de Contexto daqui a pouco.

Repara que a tese não é "use só uma ferramenta e ponto" — conectar sistemas externos continua fazendo parte do jogo (CRM, calendário, financeiro continuam existindo como sistemas). A diferença é *onde fica o comando central*. Em vez de você pular entre dez painéis, o Claude Code vira o hub que **lê e escreve** nesses sistemas — e a "cola" que hoje mora só na sua cabeça vira um arquivo que qualquer pessoa da equipe, ou qualquer sessão futura, consegue herdar.

---

## 2. Anatomia do workspace: CLAUDE.md, pastas de contexto, skills e subagents (8:00 – 15:00)

Vamos abrir a estrutura de pastas de verdade. Um workspace de agência bem montado no Claude Code se organiza mais ou menos assim:

```
minha-agencia/
├── CLAUDE.md                        ← seu manual de operação (preenchido pelo /onboard)
├── EXPANSIONS.md                    ← o que adicionar conforme você cresce
├── intake.md                        ← fonte da verdade do /onboard — edite e rode de novo quando quiser
├── connections.md                   ← registro de todo sistema que sua IA consegue alcançar
├── context/                         ← sobre você, sobre o seu negócio (preenchido pelo /onboard)
├── references/                      ← guias de API e de ferramenta, pesquisados uma vez, salvos pra sempre
├── decisions/
│   └── log.md                       ← registro append-only do que foi decidido e por quê
├── archives/                        ← coisa antiga. Não apaga. Move pra cá.
└── .claude/
    └── skills/
        ├── onboard/SKILL.md
        ├── audit/SKILL.md
        └── level-up/SKILL.md
```

Cada peça tem um papel específico — e é importante narrar isso peça por peça, porque é aqui que mora a diferença entre "um Claude Code com uma pasta bagunçada" e "uma agência de verdade rodando dentro dele":

- **`CLAUDE.md`** é o manual de operação. É o arquivo que qualquer sessão nova do Claude lê primeiro — quem você é, o que vende, pra quem vende, como fala. Não é preenchido à mão do zero: é gerado por uma entrevista guiada (o `/onboard`, que a gente detalha na seção 5).
- **`context/`** é a pasta que guarda o "quem somos" em profundidade — separada do `CLAUDE.md` porque o manual de operação precisa ser enxuto (é lido em toda sessão), enquanto o contexto profundo só é puxado quando necessário.
- **`connections.md`** é o registro — uma tabela — de cada sistema externo que o workspace consegue alcançar: CRM, calendário, financeiro, comunicação, gestão de projeto, inteligência de reunião, conhecimento. Isso é o "wiring" que conecta o workspace ao mundo real (mais na seção 7).
- **`.claude/skills/`** é onde moram as **skills** — comandos reutilizáveis, tipo `/onboard`, que empacotam um fluxo de trabalho inteiro num único gatilho de texto.
- **`decisions/log.md`** é um log append-only — cada decisão relevante vira uma linha datada. Isso é o que transforma "eu lembro que a gente decidiu isso" (que só existe na sua cabeça) em algo que qualquer pessoa, incluindo você em três meses, consegue recuperar.
- **`.claude/agents/`** — não precisa existir no dia 1, mas é o próximo passo natural (por isso o `EXPANSIONS.md` existe): **subagents** são processos especializados que o Claude Code pode disparar pra tarefas específicas (por exemplo, um subagent dedicado a "testar isso num navegador de verdade", rodando Playwright por baixo — voltamos nisso na seção 7). A regra de ouro aqui é clara: **não adicione pasta nova cedo demais** — o `EXPANSIONS.md` existe justamente pra separar "o que já está pronto" do "o que você adiciona conforme sente a dor", pra não recriar o mesmo tool sprawl que a gente acabou de criticar, só que dentro da própria pasta do Claude Code.

O ponto pedagógico aqui: isso **não é código**. É estrutura de arquivo markdown, texto simples, versionável em git. A "engenharia" da coisa toda é decidir o que vai em cada camada — e é exatamente isso que os dois frameworks a seguir formalizam.

---

## 3. Framework de arquitetura: os Quatro Cs — Context, Connections, Capabilities, Cadence (15:00 – 24:00)

Este é o framework de **arquitetura** — o que você constrói. Quatro camadas: *Context. Connections. Capabilities. Cadence.*

| # | Camada | Em uma frase | Teste de "essa camada está pronta" |
|---|--------|--------------|--------------------------------------|
| 1 | **Context** | Conhece o seu negócio | Uma sessão nova do Claude, do zero, responde "o que esse negócio faz e quem trabalha aqui" sem precisar navegar em nada |
| 2 | **Connections** | Alcança suas coisas | "O que tem na minha agenda amanhã e quais tarefas vencem?" → dado ao vivo, sem você colar nada |
| 3 | **Capabilities** | Sabe fazer o trabalho | Uma frase curta dispara um fluxo de vários passos que produz um artefato de verdade |
| 4 | **Cadence** | Roda sem ser pedido | Notebook fechado. Um briefing cai na caixa de entrada. Um colega de equipe manda mensagem pro sistema e recebe resposta real |

Repara na ordem — ela não é decorativa, é uma **cadeia de dependência**: *Context é inegociável* (não dá pra pular). *Connections e Capabilities podem ser construídas em paralelo* — uma não trava a outra. *Cadence vem por último* — e a regra explícita é: **não automatize um fluxo que ainda não funciona feito à mão.** Isso vira o erro comum #2 da seção 8, então guarda essa frase.

Vamos concretizar cada C com exemplo real de como isso aparece dentro de um `CLAUDE.md`:

- **Context** na prática não é um parágrafo de missão vaga. É o tipo de coisa que sai de uma entrevista estruturada — quem você é, o que vende, pra quem, suas prioridades dos próximos 90 dias, e até **amostras de escrita sua coladas cruas** (sem editar), pra IA aprender sua voz de verdade e não uma versão performada da sua voz. Voltamos nisso na seção 5.
- **Connections** é a tabela de sistemas — o `connections.md`. Existem **7 domínios universais de dado, "Tier-1"**, que qualquer negócio tem: Receita/Financeiro (Stripe, QuickBooks, gateway de pagamento), Interação com cliente (HubSpot, Salesforce, até Gmail-como-CRM), Calendário, Comunicação (Gmail, Slack, Teams), Gestão de projeto/tarefa (ClickUp, Asana, Linear, Notion), Inteligência de reunião (Granola, Otter, Fireflies, Zoom) e Conhecimento/arquivos (Notion, Drive, Confluence). O mecanismo de conexão pode ser MCP, um script batendo numa API, um pipeline de exportação, ou simplesmente uma chave de API guardada com um guia de referência — **a abordagem é "API-first", sem preferir MCP sobre as outras opções.**
- **Capabilities** é a camada de skills e subagents — "uma frase curta dispara um workflow de vários passos" é, literalmente, o que uma skill em `.claude/skills/` faz.
- **Cadence** é o gatilho recorrente — um hook em `.claude/settings.json`, uma skill nomeada tipo `daily-*`/`weekly-*`, ou simplesmente o hábito semanal de rodar um comando.

---

## 4. Framework de operação: os 3Ms — Mindset, Method, Machine (24:00 – 34:00)

Se os Quatro Cs são a **arquitetura** (o que você constrói), os **3Ms** são o **cérebro operador** (como você pensa). E a ordem de aprendizado recomendada importa: **3Ms primeiro, Quatro Cs depois** — sem a reformatação de cabeça, a arquitetura é só uma estrutura de pasta. Ou seja: o framework de mentalidade vem antes do framework de estrutura, mesmo que a gente tenha apresentado os Cs primeiro nesta aula por razão didática (a estrutura de arquivo é mais fácil de visualizar primeiro).

| M | Conteúdo | Ideia-chave |
|---|----------|-------------|
| **Mindset** | Default Shift, Function Breakdown, Curiosity Rule, "Expect the Dip" | Até que ponto dá pra usar IA aqui? |
| **Method** | Find the Constraint → EAD (Eliminate, Automate, Delegate) → Map the Process → Autonomy Spectrum → Tie to KPI | Do achado ao plano de execução |
| **Machine** | Lego Principle, Assembly Line, Validation Chain, Iteration Mindset, Bike Method, Intern Rule, Kill Switch | Chato é bonito. Workflow ganha de agente |

Vamos abrir cada camada com o conteúdo real, porque tem coisa muito boa aqui pra usar quase literalmente na aula:

**Mindset.** O hábito central chama **Default Shift**: antes de fazer qualquer tarefa do jeito velho, pergunte "como a IA faria isso?". Se a resposta for "não dá pra fazer tudo", a pergunta seguinte é "como a IA ajudaria nos primeiros 30%?". Nunca é binário. Um exemplo concreto: atualizar um link de rastreamento em centenas de descrições de vídeo do YouTube — do jeito antigo, são horas abrindo vídeo por vídeo; do jeito novo, você descreve o problema pro Claude Code e vai tomar água — quando volta, ele já pesquisou a API, escreveu o script, e está esperando aprovação. Isso puxa pro segundo princípio, **Function Breakdown**: seu cargo é um punhado de funções, cada uma quebrada em dezenas de tarefas pequenas — você não automatiza o cargo inteiro, automatiza uma peça de cada vez e encadeia. E o terceiro, **Curiosity Rule**: nunca aceite output de IA sem perguntar por quê — peça três alternativas, veja qual ela acha melhor e por quê. A frase forte aqui, pra citar direto: *"se você construiu algo e não consegue explicar como funciona, você construiu um passivo, não um ativo."* Some a isso o aviso de **"Expect the Dip"** — produtividade cai uns 20% nas primeiras semanas enquanto você reaprende o fluxo; isso é esperado, não é sinal de que não está funcionando.

**Method.** É o pipeline de decisão, cinco passos. Primeiro, **Find the Constraint** — qual gargalo isso resolve. Segundo, o mais citável do framework inteiro: **EAD — Eliminate, Automate, Delegate**, sempre nessa ordem. Elimine primeiro: "o que acontece se a gente simplesmente parar de fazer isso?" — se ninguém notaria a falta, mate o processo, não automatize desperdício. Automatize segundo, seguindo a **regra de ouro 60/30/10**: cerca de 60% totalmente automatizado, 30% assistido por IA (a IA faz, humano revisa antes de sair), 10% fica manual porque é nuance ou risco demais. E aqui vale ser direto: se alguém prometer 100% de automação em qualquer coisa que importa, estão te vendendo alguma coisa. Delegue terceiro, se nem isso resolver — passe pra uma pessoa. Terceiro passo do Method: **Map the Process**, em cinco elementos — gatilho, fontes de dado, transformações de dado, pontos de decisão, destino. A regra aqui: "se você não consegue explicar pra uma pessoa, não consegue explicar pra uma IA" — desenhe no papel antes de tocar em qualquer ferramenta. Quarto passo: o **Autonomy Spectrum**, de L0 a L4 — Manual, Sugerido, Rascunho, Supervisionado, Autônomo — com a regra de ouro sendo **sempre a escolha do nível mais baixo que resolve o problema**, e um empurrão explícito contra pular direto pro L4. Quinto e último passo: **Tie to a KPI** — mais clientes, mais valor por cliente, ou menos custo, com uma métrica específica. Se você não consegue nomear o balde e a métrica, o processo para aqui e não te deixa seguir.

**Machine.** É a camada de construção e operação. **Lego Principle**: passos menores possíveis, comece pelos passos sem IA (busca de dado, formatação, roteamento) e só depois camada IA onde realmente precisa. **Assembly Line**: cada etapa de IA faz um trabalho especializado só — não construa um generalista, uma chamada pra copy, outra pra raciocínio, outra pra classificação, cada uma isolada, fácil de debugar e trocar de modelo. **Validation Chain**: valide cada passo antes de encadear o próximo — não construa o pipeline inteiro e teste tudo de uma vez, isso é receita pra "não funciona e eu não sei por quê". **Iteration Mindset**: não existe produto pronto, principalmente com IA — o prompt ótimo de seis meses atrás já é caro e prolixo hoje; suba o protótipo, colha uso real, itere; perfeccionismo é o inimigo do deploy. **Bike Method**: lance em fases, como ensinar uma criança a andar de bicicleta — Fase 1 rodinhas (roda manual, você olha tudo), Fase 2 guiado (roda mas você revisa cada output antes de sair), Fase 3 vigiado (roda sozinho, você monitora com alerta pra anomalia), Fase 4 sem mãos. Mesmo com 90% de confiança, solte 10% do volume primeiro, observe uma semana, depois aumente — como teste de remédio, não dose cheia pra todo mundo no dia um. **Intern Rule**: trate a IA como um contratado novo no primeiro dia — identidade própria (email e credenciais dela, nunca os seus), somente-leitura por padrão até provar que precisa escrever, nunca se passa por você (assina como "assistente de IA de [seu nome]"), zero credencial pessoal, trilha de auditoria completa, permissões mínimas. A frase-âncora: *"você não confiaria numa pessoa que acabou de conhecer com sua conta bancária."* E por último, **Kill Switch**: monitore o que está rodando — se uma automação precisa de patch toda hora, entrega output ruim, ou custa mais pra manter do que economiza, **derrube**. Sem cair na armadilha do custo afundado ("mas eu passei três semanas construindo isso" não é motivo pra manter algo que não funciona).

O princípio guarda-chuva do Machine inteiro, quase um slogan: **"Chato é bonito. Workflow ganha de agente."** Nem tudo precisa de um agente decidindo — a maior parte da sua agência deveria ser fluxo determinístico, chato, previsível.

---

## 5. Rotina recorrente: /onboard, /audit, /level-up (34:00 – 41:00)

Aqui a gente sai da teoria e mostra o mecanismo real — três skills, cada uma com um papel específico e um dia certo pra rodar pela primeira vez:

| Skill | Tipo | Quando roda pela 1ª vez |
|-------|------|--------------------------|
| `/onboard` | Assistente de configuração (uma vez só) | Dia 1, logo após criar o workspace. Entrevista de 7 perguntas. Gera o conjunto de arquivos do Dia 1 e preenche o `CLAUDE.md`. |
| `/audit` | Skill de pensamento recorrente | Dia 7, depois semanal. Relatório de lacuna dos Quatro Cs. Só leitura. |
| `/level-up` | Skill de pensamento recorrente | Dia 14, depois semanal. Entrevista dos 3Ms (Mindset → Method → Machine). Um run = um artefato entregue. |

A relação entre as duas recorrentes: *`/audit` pergunta "o sistema está bem construído?" (forma). `/level-up` pergunta "que alavancagem de negócio eu estou perdendo?" (função). Elas trabalham em série — conserte a estrutura primeiro, depois o planejamento de capacidade passa a fazer sentido.*

**`/onboard` na prática** — sete perguntas, teto rígido, uma de cada vez, e a resposta vai sendo escrita no `intake.md` conforme a entrevista anda (pra dar pra retomar se cair a sessão): quem você é / o que vende / pra quem (Q1); cole 1-2 textos que você escreveu recentemente, sem editar — essa é a única pergunta com regra dura: **tem que ser colado cru**, porque se você digitar na hora, a amostra já sai moldada pela conversa (Q2); suas 2-3 prioridades dos próximos 90 dias, com a IA pressionando se a resposta for vaga tipo "crescer o negócio" (Q3); onde a receita realmente cai e é rastreada (Q4); onde você fala com cliente, equipe e o mundo lá fora no dia a dia (Q5); onde moram gravação de reunião, nota, documento importante (Q6); e qual tarefa come sua semana e onde você rastreia trabalho hoje (Q7). No fim, a skill sugere um "momento uau": pedir "me diga no que eu deveria focar essa semana" — não precisa de skill `/today` separada pra isso, o próprio prompt já planta o princípio do Default Shift.

**`/audit` na prática** — lê (nunca escreve) o projeto inteiro e pontua cada um dos Quatro Cs em até 25 pontos, total 100, com estágios: 0-39 Fundação, 40-69 Construído, 70-89 Compondo, 90-100 Autônomo. Sai um placar visual (barra por 5 pontos), pontos fortes, e os **top 3 gaps ranqueados por alavancagem**, cada um com um próximo passo concreto — não "melhore seu contexto", e sim uma ação executável.

**`/level-up` na prática** — a entrevista dos 3Ms em três fases dentro de uma única execução. Fase 1 (Mindset): pergunta tipo "me conta sua semana, o que você fez três vezes ou mais?", "algo que parecia manual, chato, copiar-colar?", "algo em que você pensou 'um estagiário esperto daria conta'?" — sai daí uma lista de 1 a 3 candidatos ranqueados. Fase 2 (Method): você escolhe um candidato e roda o pipeline EAD → Map the Process → Autonomy Spectrum → Tie to KPI, tudo isso vira uma entrada datada no `decisions/log.md`. Fase 3 (Machine): pergunta "como você quer entregar isso?", com opções em ordem de "chato é bonito" — só prompt salvo, skill determinística sem IA, skill assistida por IA com uma chamada, ou subagent (último recurso, só se realmente precisar de raciocínio + uso de ferramenta). O artefato final sai com um cabeçalho registrando em que fase do Bike Method ele nasce (sempre Fase 1, rodinhas — força você a validar manualmente antes de qualquer coisa).

Recapitulando a cadência inteira: Dia 1 `/onboard`. Uma semana usando de verdade, registrando decisão real. Dia 7 `/audit`, escolhe um gap pra fechar. Dia 14 `/level-up`, constrói o primeiro automatismo real. Semana 3 em diante: `/level-up` semanal, ritual de sexta-feira — um artefato entregue por semana.

---

## 6. Cobrindo os "10 chapéus" da agência num só lugar (41:00 – 46:00)

Volta na lista de funções que uma agência de automação típica acumula no dia a dia: CRM, gestão de projetos, base de conhecimento, calendário de conteúdo, pesquisa, entrega ao cliente, vendas, conteúdo, planejamento diário e o playbook de retainer. São dez funções distintas — a gente vai chamar de "10 chapéus" pra fins didáticos.

O ponto que fecha essa seção: cada "chapéu" não é uma ferramenta separada — é uma combinação de **Contexto** (o workspace sabe quem é o cliente e o histórico dele), **Connections** (o workspace alcança o sistema real — CRM de verdade, calendário de verdade) e **Capabilities** (uma skill nomeada que produz um artefato daquele chapéu específico). Por exemplo:

- **CRM** vira `connections.md` linha "Interação com cliente" + uma skill tipo `client-brief` que puxa histórico e monta resumo antes de uma call.
- **Gestão de projeto** vira a linha "Projeto/tarefa" do `connections.md`, escrevendo de volta em ClickUp/Asana/Notion via script ou MCP.
- **Base de conhecimento** é literalmente a pasta `context/` + `references/` — cada ferramenta nova que você conecta ganha seu próprio `references/{ferramenta}-api.md`: pesquisado uma vez, salvo pra sempre.
- **Calendário de conteúdo** e **planejamento diário** são skills de Cadence — o `/level-up` de sexta-feira é literalmente isso, aplicado à sua semana.
- **Playbook de retainer** é o `decisions/log.md` acumulado virando, com o tempo, um manual replicável de como você entrega.

Nenhum desses "chapéus" precisa de uma conta SaaS nova. Precisa de uma linha na tabela de conexões e, na maioria das vezes, de uma skill de dez ou vinte linhas de markdown.

---

## 7. Conectando o mundo externo: MCP, ou Playwright quando não há API (46:00 – 50:00)

O `connections.md` deve ser explícito sobre mecanismo: **MCP** (servidor MCP), **script** (Python/Bash batendo numa API, guardado em `scripts/`), **export** (pipeline de exportação CSV/JSON), ou **chave + referência** (uma chave de API no `.env` mais um guia salvo em `references/`). E vale reforçar: **é API-first — o audit não prefere MCP sobre as outras opções**, elas contam igual desde que o sistema seja de fato alcançável.

E quando o sistema do cliente não tem API nenhuma? Acontece bastante com ferramentas legadas, painéis internos, ou SaaS pequeno sem integração. A saída prática é um **subagent com Playwright**, automatizando o navegador de verdade como se fosse um humano clicando. A documentação de subagents do Claude Code documenta esse padrão explicitamente: um agente com `mcpServers` inline apontando pro `@playwright/mcp`, dedicado a "testar funcionalidades num navegador real". O mesmo padrão vale pra extrair ou inserir dado num sistema sem API — não é o caminho preferido (é mais lento e mais frágil que uma API), mas é o caminho que existe quando não há alternativa, e evita que você volte a fazer aquilo manualmente. Regra prática: **API/MCP primeiro, sempre. Playwright é o plano B quando não existe porta de entrada nenhuma.**

O critério de decisão volta pro Method dos 3Ms: antes de escolher mecanismo, pergunte se automatizar aquele sistema específico realmente move um KPI (mais cliente, mais valor por cliente, menos custo). Conectar tudo que existe não é o objetivo — conectar o que sustenta um "chapéu" real da agência, é.

---

## 8. Erros comuns (50:00 – 53:00)

Dois erros concentram praticamente todo o risco aqui, e os dois já estão sinalizados dentro dos próprios frameworks:

**Erro 1 — pular o Context.** É tentador ir direto pra parte "legal" (conectar sistema, criar skill vistosa), mas o grafo de dependência dos Quatro Cs marca Context como **inegociável** — sem ele, Connections e Capabilities ficam construindo em cima de um workspace que não sabe nem o que é o próprio negócio. O sintoma prático: você faz uma pergunta simples numa sessão nova ("o que essa empresa faz?") e o Claude não sabe responder sem você colar contexto de novo. Isso é o teste de aceitação falhando ao vivo.

**Erro 2 — automatizar a Cadência cedo demais.** A regra é direta: **não automatize um workflow que ainda não roda bem manual**. Cadence é a última camada por design — ela pressupõe que Capabilities já funciona sozinha quando disparada por você, e que Connections já é estável. Automatizar antes disso é construir um sistema que roda sozinho errado, sem ninguém olhando, na velocidade de "sem ser pedido" — que é exatamente a pior hora pra um erro silencioso aparecer.

Um terceiro erro, menor mas recorrente na prática: **pular direto pro L4 (autônomo) no Autonomy Spectrum** só porque "IA automação" soa como sinônimo de "deixa tudo no automático". O Method é explícito: o padrão certo é sempre o nível mais baixo que resolve, e você só sobe de nível depois de provar que o de baixo funciona.

---

## Tabelas de referência rápida (pra deixar fixadas na tela ou no material de apoio)

### Os Quatro Cs — arquitetura

| # | Camada | Teste de "pronto" |
|---|--------|--------------------|
| 1 | Context | Sessão nova responde "o que esse negócio faz" sem navegar |
| 2 | Connections | "O que tem na minha agenda amanhã" → dado ao vivo |
| 3 | Capabilities | Uma frase dispara workflow que produz artefato |
| 4 | Cadence | Roda sozinho, com o notebook fechado |

### Os 3Ms — cérebro operador

| M | Componentes |
|---|-------------|
| Mindset | Default Shift · Function Breakdown · Curiosity Rule · Expect the Dip |
| Method | Find the Constraint → EAD (60/30/10) → Map the Process → Autonomy Spectrum (L0-L4) → Tie to KPI |
| Machine | Lego Principle · Assembly Line · Validation Chain · Iteration Mindset · Bike Method · Intern Rule · Kill Switch |

### As três skills recorrentes

| Skill | Pergunta que responde | Frequência |
|-------|------------------------|------------|
| `/onboard` | Quem somos e o que fazemos? | Uma vez, Dia 1 |
| `/audit` | A estrutura está bem construída? | Semanal, a partir do Dia 7 |
| `/level-up` | Que alavancagem estou perdendo? | Semanal, a partir do Dia 14 |

---

## Exercício: montar seu mini sistema operacional de agência (53:00 – 55:00 na aula, execução real leva de 2 a 4 horas fora da gravação)

Objetivo: sair da aula com um workspace de Claude Code real, mínimo, mas funcionando — não uma simulação.

**Passo 1 — Crie a pasta e o `CLAUDE.md` real (30-45 min).**
Crie uma pasta de projeto nova (pode ser a pasta real da sua agência ou uma pasta de teste). Dentro dela, escreva um `CLAUDE.md` de verdade respondendo, no mínimo:
- Quem você é, o que vende, pra quem vende (equivalente à Q1 do `/onboard`).
- Cole ali dentro, sem editar, um trecho real de algo que você escreveu recentemente pra um cliente (email ou post) — isso ensina sua voz de verdade pro Claude.
- Suas 2-3 prioridades dos próximos 90 dias, com número ou prazo, não vago.
Se você preferir, monte a skill `/onboard` primeiro (seguindo a seção 5) e responda as 7 perguntas dentro dela — é o caminho mais estruturado pra chegar num `CLAUDE.md` completo.

**Passo 2 — Construa 1 skill real (30-45 min).**
Escolha **um** dos "10 chapéus" da seção 6 que hoje te consome tempo manual. Escreva uma skill simples em `.claude/skills/{nome}/SKILL.md` com frontmatter (`name`, `description`) e o passo a passo do que ela deve fazer. Regra do Machine: comece pela versão **sem IA** se der (deterministic skill) — só suba pra "IA-assistida" se realmente precisar de julgamento. Exemplo de nome de skill: `client-brief` (monta resumo de cliente antes de uma call), `weekly-recap` (resume decisões da semana do `decisions/log.md`), ou `content-slot` (organiza a pauta de conteúdo da semana).

**Passo 3 — Abra o log de decisão e registre uma entrada real (10 min).**
Crie `decisions/log.md` e escreva uma entrada datada com uma decisão real que você tomou essa semana — o quê, por quê, e qual alternativa você descartou. Isso não é exercício de faz-de-conta: é o primeiro tijolo do seu "playbook de retainer" pessoal.

**Passo 4 — Rode seu próprio mini-audit dos Quatro Cs (15-20 min).**
Sem precisar da skill `/audit` pronta, avalie você mesmo, com nota de 0 a 25 em cada camada, honestamente:
- Context: uma sessão nova do Claude, lendo só o seu `CLAUDE.md`, consegue responder "o que esse negócio faz"?
- Connections: existe pelo menos um sistema real (calendário, CRM, o que for) que o workspace alcança de verdade, não só em teoria?
- Capabilities: a skill que você criou no Passo 2 realmente dispara e produz um artefato quando você invoca ela?
- Cadence: existe algum gatilho recorrente (nem que seja "eu mesmo rodando isso toda sexta"), ou tudo ainda depende de você lembrar?
Some as quatro notas. Isso é o seu ponto de partida real — o número não importa tanto quanto identificar **qual das quatro camadas está mais fraca**, porque é ali que você foca na próxima semana.

**Entregável do exercício:** uma pasta de projeto com `CLAUDE.md` preenchido de verdade, 1 skill funcional, 1 entrada real no log de decisão, e sua autoavaliação dos Quatro Cs com a camada mais fraca identificada.

---

## Fechamento e ponte pra próxima aula (55:00)

"Recapitulando rápido: uma agência inteira rodando de um workspace só não é sobre ter uma ferramenta mágica — é sobre estrutura de arquivo (Context, Connections, Capabilities, Cadence) e disciplina de mentalidade (Mindset, Method, Machine), com uma rotina recorrente de três comandos: `/onboard` uma vez, `/audit` e `/level-up` toda semana. É um padrão que agências de automação de IA reais já operam no dia a dia — e que qualquer pessoa consegue replicar, porque no fim é markdown, git e disciplina.

Só que um workspace bem construído levanta uma pergunta natural: e se, em vez de você abrir esse workspace toda vez que precisa de alguma coisa, ele simplesmente **ficasse rodando sozinho, o tempo todo**, reagindo a gatilho sem você precisar disparar nada? É exatamente aí que a próxima aula entra — frotas de agentes sempre ativos, o modelo do Hyperagent da Airtable, e o checklist de seis perguntas pra saber se o que você construiu é de fato um 'agente always-on' ou só uma automação bonita disfarçada de agente. Cadence, que hoje a gente tratou como 'a última camada, com cuidado' — na próxima aula vira o assunto inteiro."
