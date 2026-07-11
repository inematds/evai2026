# Aula 4 — Como Montar um Vendedor de IA por Voz (Voice AI Sales Rep)

**Curso:** Como montar um negócio de serviços de IA
**Trilha:** Advanced — Voice AI Sales Rep
**Duração estimada:** 85-90 minutos (7 seções) + exercício prático à parte (~80-90 minutos)

---

## Abertura — gancho (0-5 min)

Começa com um problema que qualquer pessoa que já ligou pra uma empresa reconhece na hora.

Um lead liga pra um negócio às 2h da manhã. É atendido por um agente de IA, conversa, demonstra interesse, desliga. Uma hora depois, manda uma mensagem de texto. E do outro lado, o "atendente de IA" responde: "Olá, como posso ajudar?" — como se nada tivesse acontecido. A ligação, a intenção, o contexto: tudo evaporou. Isso não é um vendedor; é um formulário com voz.

Existe hoje no mercado um tipo de produto que resolve exatamente isso — e que agências compram, colocam a própria marca e revendem como se fosse delas. Um exemplo real: um produto de agendamento por voz, construído por uma equipe mínima em questão de semanas usando ferramentas prontas de mercado, que é vendido por assinatura na casa de algumas centenas de dólares por mês, com plano white-label pra agências revenderem com marca, domínio e preço próprios.

A tese da aula de hoje é simples: **voice AI parou de ser demo de feira e virou produto que se vende e se opera com SLA**. E a diferença entre um protótipo bonito que quebra na primeira ligação de verdade e um produto que uma agência consegue revender com a cara dela é **arquitetura de produção** — não é o modelo de linguagem, é o pipeline em volta dele. É isso que a gente vai desmontar hoje: como ele é montado, onde ele quebra, e como você entrega isso sem virar suporte técnico 24/7 do seu cliente.

---

## 1. Por que voice AI é a "onda" agora (10 min)

Por que voz e não mais um chatbot de texto? Três motivos práticos, não hype:

1. **O canal mais antigo do mundo ainda é o mais eficaz pra vender.** Ligação continua sendo o canal de maior taxa de conversão em vendas B2B e B2C de ticket médio, porque é síncrono e de alta largura de banda emocional — só que ninguém quer contratar e treinar gente pra atender 24h.
2. **A barreira técnica caiu.** Até 2023, montar um pipeline de voz decente exigia equipe de ML. Hoje existem plataformas gerenciadas (Vapi, Retell, Bland e similares) que abstraem streaming de áudio, transporte de telefonia e orquestração — um desenvolvedor sozinho monta um agente de voz em produção em dias, não meses.
3. **O modelo de negócio de revenda white-label já existe e já fatura.** Não é só "eu, agência, uso IA de voz pro meu cliente" — é "eu compro a plataforma, coloco minha marca, e revendo pros meus clientes como se fosse meu produto próprio". Produtos de agendamento por voz no mercado já operam exatamente assim, com planos de operação direta e planos de agência.

O que os produtos que dão certo nesse mercado fazem de diferente não é inventar tecnologia nova — é resolver **um problema de produto muito específico**: a maioria das ferramentas de IA trata cada canal (ligação, SMS, e-mail, Instagram, WhatsApp) como uma conversa isolada, sem memória entre eles. O lead ligou, disse que tem interesse, desliga; manda mensagem de texto depois; e do outro lado, o "atendente de IA" recomeça do zero. O gancho de produto vencedor é o oposto: **uma conversa contínua, todo canal, sem buracos**. Isso vai voltar na seção 3.

Vale registrar pra onde esse mercado está indo: o agente que hoje atende ligação e agenda reunião, vendido por algumas centenas de dólares por mês, é a porta de entrada. A evolução natural — e é assim que os próprios produtos do setor desenham seu roadmap — é o agente de prospecção ativa completa: encontra o lead, pesquisa, aborda, nutre por semanas, agenda a reunião e ainda orienta o time de vendas de como fechar, num contrato de valor muito maior. Ou seja: o "vendedor de IA por voz" de hoje é a porta de entrada pra um "departamento de vendas de IA" amanhã — e isso muda o tamanho do contrato que você consegue vender.

---

## 2. Arquitetura de produção: o pipeline STT → LLM → TTS (20 min)

Aqui é o coração técnico da aula. Um vendedor de IA por voz não é "um chatbot com uma vozinha em cima". É um pipeline de três estágios rodando sob orçamento de latência apertadíssimo, porque o ouvido humano não perdoa pausa.

### 2.1 Os três estágios

1. **STT (Speech-to-Text):** transforma o áudio da pessoa ligando em texto, em tempo real, enquanto ela ainda está falando (streaming, não espera ela terminar a frase inteira).
2. **LLM (o "cérebro"):** recebe o texto parcial/final, decide o que responder, decide se precisa chamar uma ferramenta (agendar, consultar CRM, transferir), e começa a gerar a resposta token a token.
3. **TTS (Text-to-Speech):** converte os primeiros tokens da resposta em áudio e já começa a tocar, sem esperar a resposta inteira estar pronta.

O ponto crítico da aula, que costuma passar batido: **isso não pode ser sequencial**. Se você espera o STT terminar 100%, depois manda pro LLM esperar a resposta completa, depois manda pro TTS gerar o áudio inteiro — você já perdeu a conversa. Cada estágio tem que **rodar em streaming e sobrepor com o próximo**: o TTS já começa a falar a primeira frase enquanto o LLM ainda está gerando a segunda. É esse desenho concorrente, e não o modelo em si, que decide se a conversa soa humana ou soa "walkie-talkie". A documentação técnica das próprias plataformas de voz (Vapi, Pipecat) trata esse desenho concorrente como requisito, não otimização.

### 2.2 Por que <500ms importa (e de onde vem esse número)

Isso não é regra arbitrária de produto — é psicolinguística. Pesquisas em conversação humana mostram que o intervalo médio de resposta entre falantes fica perto de 200ms, e até 500ms ainda soa natural. Passado isso, a pessoa do outro lado **registra a pausa como estranha**; depois de 1.500ms, ela já volta a falar por cima ("será que caiu a ligação?") ou desliga.

O orçamento de latência de ponta a ponta (da pessoa parar de falar até o primeiro som de resposta) se divide, em uma arquitetura moderna otimizada, aproximadamente assim:

| Estágio | Faixa típica (2026) | O que está acontecendo |
|---|---|---|
| STT — finalização da transcrição | 50–100ms | Confirmar o texto final depois que a pessoa parou de falar |
| Detecção de fim de turno (VAD + semântico) | até ~75ms (p99, modelos dedicados) | Decidir *se* a pessoa realmente terminou de falar (não é só silêncio — é entender se a frase está completa) |
| LLM — primeiro token | 100–200ms | Modelo processa o prompt e começa a gerar a resposta |
| TTS — primeiro byte de áudio | 50–80ms | Converter o primeiro pedaço de texto em som |
| Transporte de rede | 20–50ms (WebRTC) / 150–700ms (telefonia/PSTN) | Ida e volta de rede — é aqui que uma ligação por telefone perde a corrida |
| **Total ponta a ponta** | **~220–430ms (WebRTC)** / **~350ms a mais de 1s (telefone)** | — |

Duas coisas pra grifar aqui:

- **Uma chamada por WebRTC (navegador) tem folga; uma chamada por telefone via PSTN/Twilio já nasce perto do limite do orçamento antes mesmo do LLM gerar um token.** Isso muda a decisão de qual canal de telefonia usar quando o cliente quer atender ligação de celular de verdade.
- **A maior fonte de atraso hoje não é mais o modelo — é a detecção de turno.** Decidir "a pessoa terminou de falar ou só fez uma pausa no meio da frase" é o gargalo real de 2026. É por isso que existe detecção semântica de turno (não só silêncio/VAD): um modelo leve avalia se a transcrição parcial representa um pensamento completo antes de disparar a resposta — a LiveKit, por exemplo, roda esse detector em menos de 75ms no p99.

### 2.3 Um stack de referência comprovado em produção

Um stack que produtos reais desse mercado usam hoje — e que qualquer agência consegue montar:

- **Vapi + LLM comercial (OpenAI ou Claude)** como camada de voz + cérebro. É comum, inclusive, o produto permitir que o cliente "traga sua própria chave" de API do provedor de LLM, pagando o consumo direto.
- **Supabase como banco** — em particular como banco vetorial (via extensão pgvector do Postgres), pra sustentar a memória entre canais que vamos detalhar na seção 3.
- **MCP + Composio pra conexão de ferramentas** — calendário, CRM, transferência de chamada. É o que transformou "conectar ferramentas" de um pesadelo de integração num fluxo de clicar e conectar.

E uma lição de produto que vale mais que qualquer benchmark: o cliente final não liga se você usa banco vetorial ou se a resposta sai em frações de segundo — ele só liga se **funciona**. A latência e a arquitetura são meios; a métrica do cliente é "resolveu ou não resolveu".

O recado pra passar em aula: **o valor de mercado desses produtos não está em tecnologia exótica — está no stack padrão do setor (Vapi + LLM comercial + banco vetorial + MCP) bem montado e embrulhado pra não-técnico revender.** Isso é replicável por qualquer agência que monte hoje o mesmo protótipo do exercício desta aula.

---

## 3. Memória e contexto entre canais — o diferencial de produto real (10 min)

Essa é a parte mais importante do desenho de produto, porque é literalmente o que separa os produtos que vendem dos que não vendem nesse mercado. O diagnóstico é direto: quase todo produto de IA trata cada conversa como amnésia. Alguém liga, depois manda mensagem, e a IA age como se nunca tivesse conhecido a pessoa. Isso não é vendedor — é lixo.

A solução, na prática: **um único fio de contexto compartilhado entre canais** — ligação, SMS, e-mail, Instagram, WhatsApp — de forma que se o lead ligou às 2h da manhã, foi atendido, depois manda um SMS uma hora depois, a IA responde algo como "Oi, seguindo aquela conversa sobre seu financiamento" em vez de recomeçar do zero. Uma conversa. Todo canal. Sem buracos.

Do ponto de vista de arquitetura, isso implica:

- **Um identificador único de "lead" ou "conversa"** que persiste entre canal de voz, canal de texto e CRM — normalmente por telefone/e-mail como chave.
- **Uma camada de memória de longo prazo** (tipicamente busca vetorial sobre um resumo ou embeddings de conversas anteriores) que é consultada no início de cada nova interação, injetando contexto relevante no prompt do LLM antes dele responder.
- **Um resumo estruturado gerado a cada interação** (não o transcript bruto inteiro, que estouraria a janela de contexto e o orçamento de latência) — isso é prática de mercado padrão em sistemas de memória de agente (Mem0, Zep e Letta são exemplos de ferramentas dedicadas a isso).

O ponto pedagógico: **memória entre canais é a diferença entre "chatbot de voz" e "vendedor de IA"**, e é a parte do produto que mais justifica cobrar recorrência — porque quanto mais tempo o lead conversa com aquele agente, mais contexto acumulado ele carrega, e mais caro fica pro cliente final trocar de fornecedor.

---

## 4. Modos de falha comuns (12 min)

Aqui a aula muda de tom: até agora foi "como funciona quando dá certo"; agora é "onde quebra na primeira semana com cliente de verdade". E quebra — todo produto desse tipo passa pela fase em que o primeiro usuário beta destrói o protótipo em cinco minutos de uso real. A lista abaixo é conhecimento consolidado de mercado sobre por que voice AI falha em produção:

1. **Dead-end escalation.** O agente entra num loop ou trava numa intenção que não sabe resolver, e não existe uma saída definida — o lead fica preso numa conversa sem rumo até desligar frustrado. A causa raiz quase sempre é *design*, não modelo: ninguém definiu os gatilhos de escalonamento antes de ir ao ar.
2. **Handoff sem contexto (cold transfer).** Quando o agente transfere pra um humano, o humano atende "cego" e o cliente tem que reexplicar tudo do zero — o padrão clássico de URA (IVR) ruim, só que com um verniz de IA em cima. Isso é hoje considerado o ponto de falha de maior visibilidade em produção, porque o cliente já tinha investido tempo numa conversa "substantiva" com a IA.
3. **STT errando vocabulário de nicho.** Termos técnicos do setor do cliente (nomes de produto, siglas, jargão médico/jurídico/imobiliário), sotaques regionais e nomes próprios têm taxa de erro de transcrição (WER) desproporcionalmente mais alta — e isso se traduz direto em tarefa não concluída, porque o LLM está raciocinando em cima de um texto errado.
4. **Latência acumulada.** Cada componente individualmente parece rápido, mas se STT, detecção de turno, LLM e TTS não estão rodando em streaming sobreposto (ver seção 2), os atrasos se somam e o total estoura os 500ms — e ninguém percebeu porque cada peça, isolada, "passou no teste".
5. **Falta de guardrails.** Sem trilhos definidos, o agente pode ser levado (por engano do lead ou por ataque deliberado) a prometer coisas fora do escopo, autorizar transações que não devia, ou vazar dado sensível puxado de um CRM. Isso inclui dois vetores de ataque específicos de voz: **clonagem de voz** e **injeção de prompt** — inclusive injeção indireta, quando uma instrução maliciosa está escondida dentro de um dado do próprio CRM que é lido pelo LLM sem checagem.

O dado que amarra tudo isso: pesquisas de implantação enterprise apontam que **menos de 1 em cada 8 pilotos de voice AI corporativo chega a produção** — a maioria trava exatamente nesses cinco pontos.

---

## 5. Fallback humano, guardrails e testes de estresse antes do ar (10 min)

Se a seção 4 foi "onde quebra", esta é "como você evita que quebre na cara do cliente".

### 5.1 Fallback humano bem desenhado (não é "transferir", é "transferir com bagagem")

Defina **antes de ir ao ar** — não em produção — os gatilhos de escalonamento:

- Confiança do modelo abaixo de um limiar mínimo.
- Categorias de intenção sempre escaladas (reclamação, jurídico, questão de segurança/saúde).
- Limite de duração/tamanho da sessão sem resolução.
- Pedido explícito do lead por um humano.

E o transfer em si precisa carregar, obrigatoriamente: (1) detecção do gatilho, (2) captura do estado inteiro da conversa, (3) geração de um resumo estruturado, (4) roteamento de telefonia, (5) entrega desse resumo ao humano **no momento ou antes** da conexão — nunca depois. Se a sua arquitetura de telefonia não consegue carregar esse payload de contexto junto com a ligação, o problema é de desenho, e precisa ser resolvido antes de vender o produto como "com fallback humano".

### 5.2 Guardrails explícitos

Mínimo defensável pra qualquer vendedor de IA por voz que vai ao ar:

- **Separação entre raciocínio e autorização.** O LLM decide a *intenção* (o que o lead quer), mas uma camada separada e determinística decide o que ele *pode de fato executar* (agendar, cancelar, autorizar reembolso). Isso neutraliza boa parte do risco de injeção de prompt, direta ou vinda de dado externo (CRM, e-mail).
- **Escopo de tópico explícito no prompt de sistema** — o que o agente pode e não pode discutir, com instrução clara de redirecionar (não improvisar) fora do escopo.
- **Log e replay de toda chamada** pra auditoria e regressão.

### 5.3 Teste de estresse antes de ir ao ar

Prática de mercado consolidada pra qualificar um agente antes de expô-lo a lead real: suíte de regressão cobrindo intenções de borda, chamadas com ruído injetado em diferentes níveis (por exemplo, 45dB, 65dB, 75dB, simulando ambiente silencioso, escritório e rua), diversidade de sotaque/dialeto, todos os fluxos de escalonamento, e tentativas adversariais de injeção de prompt pra confirmar que o guardrail aguenta. Qualquer mudança de prompt, motor de STT ou provedor de TTS deveria disparar essa suíte de novo antes de voltar ao ar — porque uma mudança que parece segura em ambiente de teste pode quebrar um fluxo específico que só aparece em volume real.

Métricas mínimas de monitoramento em produção, com meta de referência de mercado:

| Métrica | Meta de referência | O que revela |
|---|---|---|
| Tempo até a primeira resposta (TTFR) | <300ms (WebRTC) / <800ms (telefone) | Experiência real do usuário |
| Taxa de erro de transcrição (WER) | <5–10%, alerta em 8% | Saúde do STT/vocabulário |
| Taxa de conclusão de tarefa | >85–90% | O agente está resolvendo, não só conversando |
| Taxa de interrupção (barge-in) | <15% dos turnos | Qualidade da detecção de fim de turno |

---

## 6. Como empacotar e vender (12 min)

Aqui a aula sai da engenharia e entra no comercial — porque um pipeline perfeito que ninguém sabe precificar não vira negócio.

### 6.1 A métrica que abre a conversa de venda: speed-to-lead

Este é o argumento comercial mais forte que existe pra vender um vendedor de IA por voz, porque tem décadas de dado por trás, não é hype:

- Responder um lead em até 5 minutos torna o contato **até 100 vezes mais provável** e a qualificação **21 vezes mais provável**, comparado a esperar 30 minutos (estudo clássico de gestão de resposta a leads, ainda citado como referência em 2026).
- Pesquisas do setor registram aumento de **391% na conversão** ao responder no primeiro minuto.
- Passados 5 minutos, a qualidade percebida do lead cai cerca de **80%**.
- **78% dos compradores fecham com a primeira empresa que responde.**

Isso é ouro pro pitch: nenhum time de vendas humano atende em menos de 5 minutos, 24 horas por dia, todo santo dia. Um vendedor de IA por voz, sim — e é exatamente esse o argumento, não "economizar salário de SDR".

### 6.2 Estrutura de SLA e precificação

Elementos mínimos de um SLA defensável pra vender esse tipo de produto:

- **Meta de latência contratual** (ex.: p95 abaixo de X segundos) e **taxa de conclusão de tarefa mínima**, com relatório periódico — não fique só na promessa, meça e mostre.
- **Modelo de custo transparente por minuto/uso.** Referência de mercado (documentação de precificação do GoHighLevel Voice AI, uma plataforma do setor): custo composto de tarifa por minuto de voz (na faixa de centavos, ex. ~US$0,045-0,06/min) mais custo de tokens de LLM mais custo de TTS (de ~US$0,015/min em voz padrão a ~US$0,17/min em voz premium), resultando em algo como US$0,16/minuto em média. Outro modelo comum no mercado: créditos-bônus inclusos no plano, depois repasse do custo do provedor mais uma margem fixa (ex.: 20%) — ou o cliente traz a própria chave de API e paga direto, sem markup.
- **Integração a CRM como parte do produto, não extra.** O ideal é que a ligação vire automaticamente um contato/atividade no CRM do cliente (GoHighLevel, HubSpot ou equivalente) — contato criado, nota registrada, tag aplicada, reunião marcada direto na agenda —, porque é isso que faz o cliente final enxergar valor sem precisar entender o pipeline por trás.
- **Dois níveis de plano fazem sentido comercial:** um plano "operador" pra pequeno negócio usar direto, e um plano "agência/white-label" mais caro pra quem revende, com marca própria e acesso a chaves próprias. É o padrão que os produtos de agendamento por voz do mercado praticam, com mensalidades na casa de dezenas a algumas centenas de dólares.

---

## 7. Roteiro de construção mão-na-massa (8 min)

Passo a passo de como você monta a primeira versão disso, hoje, sem equipe de ML:

1. **Escolha a plataforma gerenciada de voz.** Vapi (ou equivalente como Retell/Bland) — você não escreve pipeline de áudio do zero; você configura provedor de STT, LLM e TTS via painel/API.
2. **Escolha o LLM e escreva o prompt de sistema com escopo explícito** (o que o agente pode e não pode fazer, quando escalar).
3. **Configure a camada de memória** — mínimo viável: registrar cada conversa (resumo, não transcript bruto) associado a um identificador único do lead (telefone/e-mail), consultável no início de cada nova interação.
4. **Conecte as ferramentas via MCP/Composio** (ou webhooks diretos) — calendário pra agendar, CRM pra registrar, e a rota de transferência para humano.
5. **Defina os gatilhos de escalonamento e o guardrail de autorização** antes de testar com lead real.
6. **Rode a suíte de teste de estresse** (seção 5.3) antes de expor a um número de telefone real.
7. **Meça TTFR, WER e taxa de conclusão desde o primeiro dia em produção** — não espere reclamação de cliente pra descobrir que está fora da meta.

---

## Exercício prático — protótipo de voice sales rep com guardrail e fallback

**Entregável final:** um protótipo funcional (mesmo que mínimo) de vendedor de IA por voz, com guardrail explícito, rota de fallback definida, uma gravação de teste, e um diagrama de arquitetura.

### Passo a passo

1. **Defina o cenário de negócio (10 min).** Escolha um nicho concreto (ex.: imobiliária, clínica odontológica, escritório de advocacia) e escreva em 3 frases: o que o agente vende/qualifica, o que ele NUNCA pode fazer sozinho (ex.: confirmar valor de contrato, dar diagnóstico), e qual é o gatilho óbvio de escalonamento pra esse nicho específico.

2. **Monte o protótipo de voz (30-40 min).**
   - Crie uma conta na plataforma de voz escolhida (Vapi ou equivalente).
   - Escreva o prompt de sistema com: persona, escopo de tópico permitido, e instrução explícita de guardrail (ex.: *"Nunca confirme valores de contrato ou datas de fechamento sem consultar a ferramenta de CRM. Se o lead pedir para falar com um humano, ou mencionar reclamação/processo jurídico, ou a conversa passar de 6 minutos sem resolução, transfira imediatamente com o resumo estruturado da conversa."*).
   - Configure ao menos uma ferramenta real (agenda ou webhook simulando CRM).

3. **Implemente a rota de fallback (15 min).** Configure a ação de transferência (mesmo que simulada — pode ser um webhook que registra "seria transferido agora + payload de contexto") e escreva manualmente o formato do resumo estruturado que seria entregue ao humano (nome do lead, intenção detectada, o que já foi dito, motivo do escalonamento).

4. **Grave um teste de verdade (15 min).** Faça pelo menos duas ligações de teste:
   - Uma ligação "feliz", dentro do escopo, que o agente deve resolver sozinho.
   - Uma ligação adversarial, tentando propositalmente tirar o agente do escopo ou disparar o guardrail (ex.: pedir pra ele confirmar um desconto que ele não tem autorização de dar, ou simular frustração pra forçar o escalonamento).
   - Salve as duas gravações/transcrições como evidência.

5. **Desenhe o diagrama de arquitetura (10 min).** Um diagrama simples (pode ser caixas e setas, à mão ou em qualquer ferramenta) mostrando: Chamada → STT → LLM (+ ferramentas: memória, CRM, agenda) → TTS → Resposta, com um ramal separado saindo do LLM apontando pra "Guardrail/Autorização" e outro apontando pra "Fallback humano com contexto".

**Critério de entrega:** as 4 evidências (prompt com guardrail escrito, payload de fallback, as duas gravações, o diagrama) juntas — não vale só o protótipo "funcionando na demonstração feliz"; o que prova que você entendeu a aula é a gravação adversarial e o fallback com contexto.

---

## Fechamento e ponte para a próxima aula

Hoje a gente desmontou o que separa um "chatbot com voz" de um vendedor de IA de verdade: pipeline em streaming sob orçamento de latência apertado, memória contínua entre canais, modos de falha previsíveis e guardrails desenhados antes — não depois — de ir ao ar. E os casos reais desse mercado mostram que isso não exige um laboratório de pesquisa: exige algumas semanas de foco, um problema real resolvido primeiro (memória entre canais) e disciplina de teste antes do primeiro cliente de verdade.

Mas tudo o que vimos hoje levanta uma pergunta que ainda não respondemos: **como você sabe, com confiança, que seu agente está pronto antes de expô-lo a um lead real — sem depender de "parece que está funcionando na minha demonstração"?** Isso não é exclusivo de voice AI: vale pra qualquer agente de IA que você entrega pra cliente. Na próxima aula — **AI Evals: como testar e validar agentes antes do cliente** — a gente monta exatamente essa suíte mínima de avaliação, com casos de teste reais, classificação de falha e um mini relatório de eval que você entrega como artefato de handoff. É o complemento direto do teste de estresse que fizemos na seção 5 desta aula, só que generalizado pra qualquer tipo de agente, não só voz.
