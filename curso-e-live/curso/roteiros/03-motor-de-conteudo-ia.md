# Aula 3 — Como montar um motor de conteúdo de IA

**Curso:** Como montar um negócio de serviços de IA
**Trilha:** Bloco técnico / Foundation
**Duração estimada:** 70–85 minutos (aula expositiva + prática guiada)

---

## 0. Abertura / gancho (3–4 min)

Comece direto, sem rodeio:

> "Eu quero que vocês guardem uma imagem antes de mais nada: uma criadora solo. Sem equipe. Sem investidor. Sem agência de conteúdo contratada. Sem gastar um centavo em anúncio. E essa pessoa opera, hoje, na casa de milhões de seguidores conquistados organicamente, com dezenas de milhões de visualizações por mês, publicando centenas de posts por semana em oito plataformas diferentes. O motivo de ela conseguir fazer isso sozinha não é sorte, nem carisma sobre-humano. É que ela construiu uma **máquina**. Um pipeline. Um motor de conteúdo. E é exatamente esse motor que vocês vão aprender a montar hoje — em escala menor, mas com a mesma lógica."

Plante a tese que vai guiar a aula inteira: **uma pessoa com as ferramentas certas deve superar uma agência de 10 pessoas**. Diga isso em voz alta pro aluno: não é uma promessa de curso de infoproduto — é uma consequência direta de trocar produção artesanal por um sistema. Zero verba, zero equipe, zero anúncio: o que substitui tudo isso é arquitetura.

Feche o gancho com a promessa da aula: ao final, o aluno sai com um diagrama de 5 caixas e um protótipo mínimo funcionando — não um slide bonito, uma automação que ele testou de verdade.

---

## 1. Modelo mental: "motor" vs. "post avulso" (6–8 min)

A maior parte de quem tenta fazer conteúdo trabalha no modo "post avulso": abre o Canva, escreve uma legenda, sobe no Instagram, torce. É trabalho artesanal, não escalável, e para no dia em que a pessoa não tem inspiração ou tempo.

Um **motor de conteúdo** é outra categoria de coisa. É um sistema com entrada e saída bem definidas: você alimenta uma fonte (um vídeo, um artigo, uma ideia), e do outro lado sai um conjunto de formatos prontos para publicar em múltiplos canais, sem que uma pessoa precise reescrever cada peça manualmente.

A diferença central, que você deve deixar bem marcada na aula, é esta: **post avulso escala linearmente com o seu tempo. Motor de conteúdo escala com o número de fontes que você alimenta nele.** Enquanto uma agência tradicional de conteúdo precisa contratar mais gente pra produzir mais posts (redator, designer, editor de vídeo, social media — cada um cobrando hora), o motor faz o trabalho de transformação e distribuição de forma automatizada. É por isso que a tese "uma pessoa com as ferramentas certas supera uma agência de 10" não é discurso motivacional vazio — é uma consequência direta de trocar "produção artesanal por peça" por "pipeline programado uma vez, rodando toda semana".

Um jeito prático de fixar isso na cabeça do aluno: pergunte "quantas pessoas você acha que trabalham na equipe de conteúdo de uma operação que publica centenas de posts por semana em oito redes?". Deixe a resposta suspensa por 2 segundos. Nos casos reais mais impressionantes desse modelo, a resposta é: **uma pessoa, sem equipe de conteúdo, usando um agente de código mais uma ferramenta de distribuição como motor**. Isso é o contraste que a aula inteira vai explorar.

---

## 2. A arquitetura clássica de um motor de conteúdo (12–15 min)

Esta é a seção mais importante da aula — é o "como". A arquitetura clássica de um motor de conteúdo é um fluxo de 5 passos, e vale a pena narrar caixa por caixa:

1. **Google Sheets — Watch Rows.** Uma planilha do Google Sheets funciona como fila de entrada. Cada linha nova é uma URL de conteúdo-fonte (um artigo, por exemplo). Uma automação monitora a planilha e dispara o processo assim que uma linha nova é adicionada.
2. **HTTP Module — Fetch Content.** A automação busca o conteúdo bruto (HTML) da URL registrada na planilha.
3. **OpenAI — Extract Article.** Um modelo de linguagem limpa esse HTML bruto e extrai só o texto do artigo — isso existe porque HTML cru tem menu, rodapé, anúncio, e você não quer isso "contaminando" o prompt de geração.
4. **OpenAI — Create Content.** Aqui mora o cérebro do motor: um (ou mais) prompt gera, a partir do artigo limpo, simultaneamente: posts escritos para LinkedIn, Twitter/X, Threads, Facebook e Instagram; um prompt de imagem de IA (gerado depois via DALL-E 3); e um roteiro de vídeo + legenda para um vídeo de avatar de IA voltado a TikTok.
5. **Blotato — Automated Posting.** A publicação em todas as plataformas acontece via chamada de API da Blotato — um único ponto de saída que distribui pra tudo.

Narre isso deixando claro o insight central: **o gargalo de conteúdo nunca foi a criatividade, foi a transformação de um formato em vários outros — e é exatamente esse gargalo que o motor ataca.** Uma fonte de conteúdo entra, cinco ou seis formatos diferentes saem, e nenhuma etapa depois da linha 1 exige uma pessoa sentada reescrevendo.

Vale apresentar também uma variação mais simples, útil como exemplo de "motor mínimo" pra quem está começando — o **repost engine**: um feed RSS (via RSS.app) monitora os vídeos novos postados no seu TikTok; um Google Drive com pasta pública recebe o vídeo baixado; e a API da Blotato republica esse vídeo automaticamente nas outras redes. Três etapas de configuração, zero trabalho manual depois disso. É um ótimo "motor de 3 caixas" pra contrastar com o motor completo de 5 passos acima — mesma lógica, menor escopo.

Uma camada extra que vale citar como evolução do stack: a Blotato hoje oferece integração nativa com **Claude Code** (documentada em `help.blotato.com/api/claude-code`). Isso mostra que o mesmo motor pode ser orquestrado tanto por automação low-code (Make/n8n) quanto por um agente de código rodando localmente — dois caminhos válidos pra montar a mesma máquina, e o aluno pode escolher o que já domina.

---

## 3. Prompt engineering aplicado a conteúdo (8–10 min)

A técnica de organização de prompts que sustenta um motor de conteúdo é simples e poderosa: **um prompt por formato de saída, nomeado pelo output que produz**. O padrão de nomenclatura é **verbo + objeto** — `create_algumacoisa.md` para tarefas de geração, `summarize_algumacoisa.md` para tarefas de síntese. Exemplos do tipo de biblioteca que se forma com o tempo:

- `create_linkedin_post.md`
- `create_tiktok_script.md`
- `create_article_structure.md`
- `create_video_prompts.md`
- `summarize_paper.md`

O motivo pelo qual isso importa pra quem monta um motor de conteúdo: cada etapa de transformação do seu pipeline (etapa 4 do pipeline acima — "Create Content") deveria ser **um prompt nomeado por output**, não um prompt genérico "escreva um post sobre isso". Um prompt por formato de saída, testado isoladamente, é mais fácil de debugar, versionar e melhorar do que um prompt monolítico tentando gerar seis coisas de uma vez. Quando o post de LinkedIn sai ruim, você abre `create_linkedin_post.md` e mexe só ali — sem risco de quebrar o roteiro de TikTok.

**Dois exemplos de prompt nomeados por output**, escritos pra esta aula e testados de fato como parte do protótipo mínimo:

**`create_linkedin_post.md`** (input: artigo/transcrição bruta → output: post de LinkedIn)
```
Você é um editor de conteúdo B2B. Receberá um texto bruto (artigo, transcrição ou notas).
Gere um post de LinkedIn de até 1.300 caracteres com:
- Gancho de 1 linha (sem emoji, sem "você sabia que")
- 3 a 5 parágrafos curtos (máx. 2 linhas cada), quebra de linha entre eles
- Um exemplo concreto extraído do texto-fonte
- Fechamento com uma pergunta que convide comentário
- Tom: direto, sem jargão de guru, sem hashtag em excesso (máx. 3 no fim)
Texto-fonte:
{{artigo_limpo}}
```

**`create_tiktok_script.md`** (input: artigo/transcrição bruta → output: roteiro de vídeo curto + legenda)
```
Você escreve roteiros para vídeos verticais de 30 a 45 segundos com avatar de IA falando direto pra câmera.
A partir do texto-fonte, gere:
1. ROTEIRO (texto corrido, tom de conversa, sem indicação técnica de câmera):
   - 3 primeiros segundos: gancho que gera "espera, o quê?" (contraintuitivo ou número chocante)
   - Corpo: 1 ideia central, 1 exemplo, sem enrolar
   - Fechamento: 1 frase de takeaway (não peça "siga o perfil" — isso vai em outro lugar)
2. LEGENDA para postar junto do vídeo (2-3 linhas + até 5 hashtags relevantes ao tema)
Texto-fonte:
{{artigo_limpo}}
```

Frise pro aluno: o valor não está no prompt em si (esses dois exemplos foram testados nesta preparação e funcionam, mas são simples de propósito). O valor está no **hábito de nomear o prompt pelo output que ele produz** e mantê-lo isolado — isso é o que faz o motor ser fácil de manter e trocar de peça depois.

---

## 4. Repurposing: 1 vídeo longo + o resto da semana automatizado (8–10 min)

Este é talvez o insight de rotina mais replicável da aula inteira. O ritual semanal de quem opera um motor maduro é assim: concentrar a criação de conteúdo original num único dia — gravar um tutorial em vídeo longo para YouTube, gravar um lote de vídeos curtos (algo em torno de 20), e escrever a newsletter. Depois disso, a ferramenta de distribuição reaproveita, agenda e publica tudo ao longo dos sete dias seguintes, nas plataformas: YouTube, TikTok, Instagram, LinkedIn, X, Threads, Facebook e Substack. Um sábado de produção vira centenas de posts agendados pra semana — e a operação inteira, sustentando milhões de seguidores e dezenas de milhões de visualizações orgânicas mensais, roda com um custo de ferramenta na casa de poucas centenas de dólares por mês. Compare isso com o payroll de uma agência de 10 pessoas e a tese da aula para de ser frase de efeito.

A lógica de repurposing que sustenta isso: escolher **1 ou 2 plataformas** onde você produz conteúdo original de alta qualidade, e reaproveitar esse conteúdo em todas as outras. Reaproveitar não é "postar a mesma coisa igual em todo canto" — é converter formato: um vídeo do YouTube vira newsletter e post social; um vídeo do TikTok vira quote card e carrossel pro Instagram, Pinterest ou LinkedIn.

Para ir além do que a ferramenta de distribuição faz nativamente, entra o **n8n** com automações mais avançadas — por exemplo, depois de postar um vídeo no TikTok, uma automação de n8n baixa esse vídeo automaticamente e faz o crosspost pras outras plataformas, já com legenda e edição customizada por canal. Outra automação útil no mesmo espírito: raspar (scrape) TikToks e Reels virais de referência e jogar as ideias numa base do Airtable, como banco de inspiração pra próximos roteiros.

O ponto pedagógico aqui: **a régua de "quanto produzir" não é "produza mais todo santo dia"** — é "produza bem uma vez, distribua automaticamente sete vezes". Isso é o oposto do burnout de criador de conteúdo tradicional, e é a peça que faz a tese "uma pessoa supera uma agência" fazer sentido operacionalmente, não só como frase de efeito.

---

## 5. Vídeo com IA/avatar como camada de escala (6–8 min)

A camada de vídeo com avatar de IA é onde o motor ganha "corpo" — em vez de só texto e imagem, o pipeline também produz vídeo com uma versão sintética do próprio criador falando.

A combinação que se consolidou na prática: **HeyGen** (geração de vídeo de avatar) com **ElevenLabs** (clonagem de voz profissional), porque a qualidade de voz clonada da ElevenLabs é substancialmente superior à voz padrão nativa do HeyGen. Um detalhe editorial que vale ensinar junto: usar um elemento visual fixo no avatar (um chapéu, um óculos, um fundo específico) propositalmente, para que quem assiste consiga identificar rapidamente que aquele vídeo específico é a versão "clone de IA", não a pessoa gravando ao vivo — um detalhe pequeno, mas que sinaliza uma preocupação real com transparência e protege a confiança da audiência.

Em termos de arquitetura, esse pedaço do sistema combina: **Make (ou automação equivalente) + Perplexity (pesquisa) + ChatGPT (roteiro) + HeyGen (vídeo) + ElevenLabs (voz) + Blotato (distribuição)**, permitindo pesquisar, escrever, gerar e distribuir vídeos de avatar de IA para as principais redes de forma automatizada e recorrente.

Trate essa camada como **opcional e avançada** na aula — é o topo do pipeline, não o ponto de partida. O aluno que está montando o motor pela primeira vez deve dominar as camadas 2, 3 e 4 (texto, imagem, distribuição) antes de entrar em avatar de vídeo, porque essa camada tem custo de ferramenta mais alto e uma curva de qualidade mais sensível (voz ruim ou avatar com uncanny valley prejudica a marca mais do que ajuda).

---

## 6. Tabela de stack de referência

| Camada do pipeline | Stack de referência (caso real) | Alternativa acessível pro aluno |
|---|---|---|
| Fila de entrada / captação de fonte | Google Sheets (linha nova = gatilho) | Google Sheets (grátis) ou Airtable |
| Orquestração / automação | Make (automação original); n8n (automações avançadas de crosspost e scraping) | n8n self-hosted (grátis) ou Make (plano free/starter) |
| Extração de conteúdo bruto | HTTP Module + OpenAI (limpeza do HTML) | HTTP Request node + qualquer LLM (GPT, Claude, Gemini) |
| Geração de texto multi-formato | OpenAI ("Create Content": posts + prompt de imagem + roteiro de vídeo) | Claude ou GPT via API, com prompts nomeados por output |
| Geração de imagem | DALL·E 3 | Ideogram, Flux (inclusive flux2-klein), Midjourney |
| Geração de vídeo com avatar | HeyGen + ElevenLabs (voz clonada) | HeyGen (plano menor) + ElevenLabs, ou D-ID/Synthesia como alternativa de avatar |
| Publicação/distribuição multi-canal | Blotato API | Buffer, Publer, ou construir publish direto via APIs nativas de cada rede (mais trabalho) |
| Orquestração agente/código | Claude Code (integração nativa da Blotato) | Claude Code, ou qualquer runner de agente com acesso a API |
| Banco de inspiração/curadoria | Airtable (alimentado por scraping via n8n) | Airtable, Notion, ou até planilha simples |

Deixe claro em aula: a coluna da direita não é "pior" — é o ponto de entrada acessível. O aluno não precisa nascer pagando Blotato + HeyGen + ElevenLabs junto; a arquitetura (5 caixas) é o que importa, as ferramentas são substituíveis.

---

## 7. Prática guiada em aula — montar a versão mínima do pipeline (18–22 min)

Aqui a aula vira mão na massa. Guie o aluno passo a passo, ao vivo, montando uma versão mínima com o que ele já tem disponível (não precisa ser Blotato pago — pode ser um Google Sheets + um LLM + publicação manual como "saída" provisória, se o aluno ainda não tem conta de API de rede social).

**Passo 1 — Defina a fonte (5 min).** Escolha 1 tipo de fonte fixo: pode ser um vídeo do YouTube (via transcrição), um artigo, ou uma nota de voz transcrita. Crie uma planilha Google Sheets com 2 colunas: `url_ou_texto_fonte` e `status` (pendente/processado).

**Passo 2 — Escreva o prompt de extração (3 min).** Um prompt simples: "extraia apenas o conteúdo substantivo deste texto, removendo menu, propaganda e elementos de navegação; devolva só o texto limpo". Teste com 1 exemplo real na mão (cole no ChatGPT/Claude e rode).

**Passo 3 — Escreva 2 prompts nomeados por output (5 min).** Reaproveite ou adapte os dois exemplos da seção 3 (`create_linkedin_post` e `create_tiktok_script`). Rode os dois manualmente sobre o texto extraído no passo 2. Isso já é o "motor" rodando manualmente — a automação (Sheets → n8n/Make → LLM → publish) vem depois, mas a lógica de transformação já está provada.

**Passo 4 — Desenhe o diagrama de 5 caixas (4 min).** Peça que o aluno desenhe (papel, Excalidraw, ou o que tiver): [1] Fonte → [2] Extração → [3] Geração multi-formato → [4] (opcional) Geração de mídia (imagem/vídeo) → [5] Publicação. Cada caixa recebe o nome da ferramenta escolhida por ele (pode ser manual nesta fase).

**Passo 5 — Rode o pipeline uma vez, de ponta a ponta, com saída real (5 min).** Da fonte até o texto final de pelo menos 1 formato publicado (mesmo que manualmente colado na rede social). O critério de sucesso da prática não é "ter automação perfeita" — é ter as 5 caixas desenhadas e pelo menos 2 prompts validados com output real na mão.

---

## 8. Erros comuns (5 min)

Narre estes com exemplos, não só como lista:

1. **Prompt genérico demais tentando gerar tudo de uma vez.** "Escreva posts pra todas as redes sobre isso" produz output raso em todas. Prompt nomeado por output, um de cada vez, é sempre melhor no início.
2. **Automatizar antes de validar manualmente.** Quem monta o Make/n8n antes de rodar o prompt manualmente 5 vezes gasta hora debugando automação em vez de prompt. Valide a mão primeiro.
3. **Confundir "motor" com "gerar lixo em massa".** O objetivo não é postar mais, é transformar bem uma fonte de qualidade em múltiplos formatos — sem qualidade na fonte e no prompt, o motor só escala erro.
4. **Pular direto pra avatar de vídeo com IA antes de ter texto/imagem redondo.** É a camada mais cara e mais sensível a qualidade ruim; comece pelas camadas de texto e imagem.
5. **Não isolar prompts por arquivo/nome.** Um prompt monolítico é impossível de debugar quando um formato específico sai ruim — nomear cada prompt pelo output que ele produz resolve isso.
6. **Procurar um "framework secreto" pra copiar.** Não existe atalho escondido — o que existe é disciplina de pipeline: fonte boa, prompts isolados, distribuição automatizada, e a rotina de rodar toda semana. Reforce isso de novo aqui, é importante.

---

## Exercício para entregar (recapitulação + critério de avaliação)

**Entregável:** diagrama do próprio motor de conteúdo (5 caixas: fonte → extração → geração multi-formato → [mídia opcional] → publicação) + 2 prompts nomeados por output, testados de fato (com pelo menos 1 rodada de output real anexada, print ou texto colado).

**Critério de "pronto":**
- O diagrama nomeia a ferramenta (ou "manual" se ainda não automatizado) em cada uma das 5 caixas.
- Os 2 prompts têm nome de arquivo que descreve o output (`create_algumacoisa.md`), não um nome genérico como "prompt1".
- Existe pelo menos 1 execução real documentada — não vale entregar só o prompt teórico sem rodar.
- O aluno consegue explicar em 1 frase por que separou os prompts por output (a resposta esperada: mais fácil de debugar e melhorar isoladamente).

---

## Fechamento + ponte pra próxima aula (2–3 min)

Feche assim:

> "Hoje vocês montaram a espinha dorsal de um motor de conteúdo: fonte, extração, geração multi-formato, distribuição — e viram como isso sustenta operações de milhões de seguidores rodadas por uma pessoa só. Só que conteúdo é metade do jogo de construir um negócio de serviços de IA. A outra metade é conversa — é vender. Na próxima aula, a gente sai do motor que produz conteúdo pra montar um motor que **atende e vende**: um vendedor de IA por voz, com pipeline de fala em tempo real, memória de conversa entre canais, e — o mais importante — o que fazer quando a IA erra na frente do cliente. Se hoje foi sobre escala de produção, a próxima é sobre escala de conversa. Vejo vocês lá."
