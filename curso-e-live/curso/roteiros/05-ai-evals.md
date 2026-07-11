# Aula 5 — AI Evals: como testar e validar agentes antes do cliente

**Curso:** Como montar um negócio de serviços de IA
**Duração estimada:** 60–65 minutos (desenvolvimento ~45 min + exercício ~15–20 min)
**Formato:** roteiro corrido, pronto pra narrar/gravar

---

## ABERTURA / GANCHO (0:00–2:30)

*(narrar em tom direto, sem rodeio)*

Deixa eu te contar uma situação que separa quem cobra R$ 500 por mês de automação de quem cobra R$ 15 mil por um agente de IA em produção.

Você construiu um agente. Ele responde e-mail de cliente, consulta um CRM, agenda reunião, manda proposta. Você testou umas cinco vezes na sua máquina, ficou bonito, mandou pro cliente. Duas semanas depois o cliente te liga: o agente prometeu um desconto que não existe, ou agendou reunião no fuso errado, ou — pior — respondeu de um jeito agressivo pra um lead importante. Você não sabia que isso podia acontecer porque **você nunca testou de verdade**. Testou uma vez, no feliz caminho, com os dados que você mesmo escolheu.

Isso não é falta de sorte. É estrutural. LLM não é função determinística — o mesmo prompt, o mesmo agente, pode produzir três respostas diferentes em três execuções, porque o modelo amostra o output de uma distribuição de probabilidade. Um sistema tradicional você testa uma vez e ele se comporta assim pra sempre. Um agente de IA você testa uma vez e não sabe nada sobre a próxima execução.

A definição de trabalho é simples: **uma avaliação (eval) é um teste para um sistema de IA — você dá um input pra IA, depois aplica uma lógica de correção no output pra medir sucesso.** Parece óbvio. Mas a maioria de quem vende automação de IA pula essa etapa inteira — e é exatamente aí que mora a diferença entre "fiz um MVP que funcionou uma vez" e "entrego um produto que eu consigo defender se o cliente perguntar: como você sabe que isso funciona?".

Essa é a aula 5. Hoje você sai daqui com: o que é um eval na prática, os três tipos de eval que você vai usar em qualquer projeto de agente, a diferença entre avaliar o caminho que o agente percorreu e avaliar só o resultado final, como montar a suíte mínima de teste sem virar um projeto de pesquisa acadêmica, o que fazer com o resultado de um eval, e um checklist que você literalmente pode usar antes de entregar qualquer agente a um cliente pagante.

---

## SEÇÃO 1 — Por que evals importam (2:30–7:00)

Duas coisas te obrigam a levar isso a sério: o não-determinismo dos LLMs, e o custo de não testar.

**Não-determinismo.** Um LLM não executa um algoritmo fixo — ele amostra a próxima palavra a partir de uma distribuição de probabilidade. Isso significa que o mesmo agente, com a mesma pergunta, pode tomar caminhos diferentes: chamar uma ferramenta diferente, interpretar uma instrução ambígua de outro jeito, ou simplesmente formular a resposta com uma nuance que muda o sentido. Em um sistema de regras tradicional (se X então Y), você escreve o teste uma vez e ele vale para sempre. Em um agente de IA, o comportamento é uma distribuição — e você só enxerga essa distribuição rodando muitos casos, não um.

**Custo de não testar.** Pense no que está em jogo quando você entrega um agente a um cliente de serviços de IA:
- O agente conversa direto com o cliente final dele (lead, paciente, comprador) — um erro vira um problema de reputação do *seu cliente*, não só seu.
- O agente pode ter ação no mundo real: mandar e-mail, agendar, cobrar, atualizar um registro. Um erro não é só "resposta feia", é uma ação errada que precisa ser desfeita manualmente.
- Você, prestador de serviço, é quem vai receber a ligação de "por que isso aconteceu?" — e "não sei, ele geralmente funciona" é a pior resposta possível pra manter um contrato.

O objetivo de um eval automatizado é exatamente este: rodar testes **durante o desenvolvimento, sem usuários reais** — ou seja, pegar o erro *antes* que o cliente final pegue. A regra vale como mantra: **seu produto de IA precisa de evals.** Não é sugestão. Sem eval, você não tem como saber se uma mudança de prompt te deixou melhor ou pior — você só tem opinião, não dado.

E aqui vai o ponto que mais importa pra quem monta negócio de serviços de IA: eval não é luxo de laboratório de pesquisa nem exclusividade de big tech. Eval é o que separa consultoria amadora de operação profissional. É o artefato que você usa pra justificar preço mais alto, pra provar handoff de qualidade, e pra se proteger quando (não se, quando) o agente errar em produção — porque você vai ter um histórico de "isso aqui eu testei, isso aqui eu vi funcionando em X% dos casos, isso aqui é o limite conhecido".

---

## SEÇÃO 2 — O que é um eval na prática: análise de erro primeiro, critério depois (7:00–13:00)

Erro comum de quem começa a testar agente: sentar e escrever uma "rubrica de qualidade" antes de ver um único exemplo de erro real. Resultado: você escreve critérios genéricos ("resposta deve ser clara e profissional") que não capturam o que realmente quebra no seu agente.

A abordagem que funciona é ao contrário: você constrói os testes **observando falhas reais**, não imaginando hipóteses. Um caso ilustrativo: um assistente de IA para o mercado imobiliário, com acesso a um CRM. O processo de eval foi assim — pegar uma funcionalidade (ex.: "buscar imóveis"), quebrar em cenários de uso real ("encontre imóveis com mais de 3 quartos abaixo de 2 milhões em tal cidade"), rodar, ver onde quebra, e só então escrever a asserção que verifica aquele comportamento específico. Operações maduras acumulam centenas desses testes unitários e os atualizam continuamente com base em falhas novas observadas nos dados, conforme os usuários desafiam a IA e o produto evolui. Ou seja: o critério nasce do erro observado, não o contrário.

Um exemplo concreto e bem específico do mesmo caso: o CRM retornava campos internos (como UUID) que não deveriam aparecer pro usuário final. Em vez de escrever uma regra vaga tipo "não vaze dados internos", o time escreveu uma asserção técnica exata — uma regex que verifica se o padrão de UUID (`[0-9a-f]{8}-[0-9a-f]{4}-...`) aparece na resposta do LLM, e falha o teste se aparecer. Esse é o nível de precisão que "análise de erro primeiro" produz: um teste nasce de um erro real que você viu acontecer, não de uma preocupação abstrata.

E não é por acaso que os melhores treinamentos de mercado sobre evals dedicam horas inteiras só a análise de erro, antes de qualquer conversa sobre métrica ou ferramenta. No campo que mais estuda esse problema, análise de erro é tratada como a *primeira* competência a desenvolver — antes de "escrever rubrica bonita".

Na prática, pra você que vai aplicar isso em cliente: antes de escrever qualquer critério de avaliação, rode o agente em 20–30 casos realistas (reais se tiver, sintéticos se não tiver ainda) e **leia cada transcript**. Anote toda vez que algo te incomodar — mesmo que você não saiba ainda nomear o critério. Só depois de ter uns 8–10 incômodos anotados, você agrupa em categorias e transforma cada categoria num teste. Essa ordem — olhar dado real primeiro, batizar critério depois — é o que separa eval que pega o que importa de eval que só documenta o óbvio.

---

## SEÇÃO 3 — Três tipos de eval: checks automáticos, revisão humana, LLM-as-judge com rubrica (13:00–20:00)

A regra de decisão é direta: **escolha graders determinísticos onde for possível, graders de LLM onde for necessário ou para flexibilidade adicional, e use graders humanos com critério, para validação adicional.** Vamos destrinchar os três.

**1. Checks automáticos (determinísticos).** São regras de código — sem ambiguidade, sem custo de rodar LLM extra, resultado binário. O exemplo do CRM que já vimos (regex de UUID) é check automático. Outro do mesmo caso: criar um contato via linguagem natural e depois consultar aquele contato — se o CRM não retornar exatamente 1 resultado, o teste falha. Use check automático sempre que o critério de certo/errado for objetivo: formato de saída, presença/ausência de um dado, número de resultados, chamada da ferramenta certa, resposta dentro de um schema.

**2. Revisão humana.** Ter humanos avaliando periodicamente pelo menos uma amostra dos traces é indispensável — na prática, "correção" costuma ser mais subjetiva do que parece, e você precisa alinhar o julgamento do modelo com o julgamento humano. Um formato prático, documentado num caso de gerador de queries em linguagem natural: uma planilha com a resposta do modelo, uma crítica gerada por outro modelo mais forte, e um veredito binário bom/ruim — e, ao lado, as mesmas três colunas preenchidas por um humano especialista no domínio, pra comparar se o julgamento automático está alinhado com o julgamento humano. Revisão humana é cara e não escala — por isso ela deve ser usada em amostra, não em cada execução, e principalmente para **calibrar** o próximo tipo.

**3. LLM-as-judge com rubrica.** É usar um segundo LLM (geralmente mais forte, ou o mesmo com um prompt diferente) pra julgar a saída do primeiro contra critérios explícitos. O ponto técnico mais importante: rubricas itemizadas — dezenas de critérios avaliados um a um — produzem julgamento muito mais confiável do que uma nota geral. Decompor a avaliação em muitos checks detalhados, julgados separadamente, melhora a confiabilidade do score. Ou seja: não peça pro juiz LLM dar uma nota de 1 a 10 pra "qualidade geral" — isso é ruído. Decomponha em itens binários e específicos.

**Exemplo de rubrica de LLM-as-judge**, pro caso de um agente de atendimento que responde e-mail de leads (aplicável direto em cliente de automação):

| # | Critério (avaliação binária: sim/não) |
|---|---|
| 1 | A resposta usa o nome do lead corretamente (não inventou, não trocou) |
| 2 | A resposta não promete preço, prazo ou condição que não está na base de conhecimento fornecida |
| 3 | Se o lead fez uma pergunta objetiva, a resposta responde essa pergunta antes de qualquer coisa |
| 4 | A resposta não usa tom que contradiz o guia de voz da marca (ex.: informal demais, agressivo, genérico de robô) |
| 5 | Se o agente chamou uma ferramenta (ex.: agenda, CRM), o resultado da ferramenta é refletido corretamente na resposta final |
| 6 | A resposta não contém informação confidencial de outro cliente (vazamento de contexto) |
| 7 | Se o caso exige escalonar pra humano, o agente escalona (não tenta resolver algo fora do escopo dele) |

Cada linha é julgada separadamente, sim ou não, pelo LLM-juiz — e depois você soma ou pondera. Isso é muito mais confiável do que pedir "dê uma nota de 0 a 10 pra essa resposta", porque reduz a subjetividade de cada julgamento a uma pergunta objetiva.

Regra prática de decisão: comece sempre tentando check automático. Se o critério for genuinamente subjetivo (tom, adequação, coerência), use LLM-as-judge com rubrica granular como a de cima. Reserve revisão humana pra amostragem periódica e, principalmente, pra checar se o seu juiz LLM está calibrado. Combinando os três, você chega no processo maduro: **evals automatizados pra iterar rápido, monitoramento de produção pra ter o ground truth, e revisão humana periódica pra calibração.**

---

## SEÇÃO 4 — Avaliação de trajetória vs. resultado final (20:00–27:00)

Aqui está uma armadilha que praticamente todo mundo que começa a testar agente cai: só olhar se a resposta final ficou boa, e ignorar *como* o agente chegou lá. E o oposto também é armadilha: exigir que o agente siga uma sequência exata de passos, o que trava o sistema desnecessariamente.

Vale a pena adotar o vocabulário padrão da área: cada tentativa do agente é um **trial**; o registro completo do que o agente fez durante o trial — saídas, chamadas de ferramenta, passos de raciocínio — é o **transcript**, também chamado de **trace** ou **trajectory**; o estado final do ambiente depois que o trial termina é o **outcome**; e o **grader** é quem aplica os checks em cima disso. A avaliação **orientada a outcome** foca em saber se a resposta final está correta, em vez de vigiar quais ferramentas o agente usou pra chegar nela.

Sobre o extremo oposto, o alerta dos times que constroem e testam agentes em escala é claro: existe um instinto comum de checar se o agente seguiu passos muito específicos — uma sequência de tool calls na ordem certa — e essa abordagem se mostra rígida demais na prática. O motivo: agentes bons encontram caminhos criativos e válidos que você não previu. Se o seu grader trava numa sequência fixa de tool calls, você vai reprovar soluções corretas só porque o agente resolveu de um jeito diferente do que você imaginou — há casos documentados em benchmarks públicos de agentes resolvendo a tarefa por um caminho fora do script esperado, mas ainda assim correto.

A recomendação prática, juntando os dois lados: **avalie o outcome como critério principal (é o que o cliente sente), mas grave e leia a trajetória sempre que o outcome falhar ou quando o outcome sozinho não for suficiente pra confiar no sistema.** No caso de um agente de código, por exemplo: você tem testes automáticos que checam só o resultado (o código passa nos testes?), mas depois disso é útil também avaliar a transcrição — heurísticas de qualidade de código e graders baseados em modelo, com rubricas claras, podem avaliar comportamentos como o jeito que o agente chama ferramentas ou interage com o usuário.

Traduzindo pro seu dia a dia com cliente: se você entrega um agente que agenda reuniões, o outcome é "a reunião foi marcada no horário certo?" — isso você checa automático, direto no calendário. Mas se o outcome falhar (reunião no horário errado), você precisa da trajetória pra saber *por quê*: o agente leu o fuso horário errado? Chamou a ferramenta certa mas com parâmetro errado? Interpretou "terça que vem" de um jeito ambíguo? Sem o transcript, você só sabe que quebrou — não sabe o que consertar. E essa é literalmente a ponte pra próxima seção.

---

## SEÇÃO 5 — Montando a suíte mínima: golden set validado por humano, e o "oracle problem" (27:00–33:00)

Você não precisa (e não deve, no início) de milhares de casos de teste. Precisa de um **golden set**: um conjunto pequeno, mas validado, de casos representativos, com resposta esperada conhecida — pra rodar toda vez que mudar algo.

Como montar isso sem dado de produção (situação comum em projeto novo de cliente): você não precisa esperar dados reais pra testar o sistema. Dá pra fazer apostas informadas sobre como os usuários vão usar o produto e gerar dados sintéticos a partir disso — e depois deixar um grupo pequeno de usuários reais usar o produto, e usar esse uso pra refinar a sua estratégia de geração sintética. Ou seja: comece com casos sintéticos plausíveis (você mesmo escreve, baseado no que sabe do negócio do cliente), rode um piloto pequeno com usuário real, e refine o golden set com o que você observar.

Um ponto que quebra uma expectativa comum: diferente de teste unitário tradicional, **você não precisa necessariamente de 100% de aprovação.** A taxa de acerto aceitável é uma decisão de produto — depende de quais falhas você está disposto a tolerar. Isso é decisão de negócio, não só técnica — combine com o cliente qual taxa de acerto é aceitável pra cada categoria de caso antes de ir pro ar.

Um requisito de infraestrutura que muita gente esquece: **cada trial precisa rodar num ambiente limpo e isolado.** Estado compartilhado entre execuções (arquivo que sobrou, cache, recurso esgotado) pode inflar ou derrubar artificialmente o resultado — há relato documentado de um agente ganhando vantagem injusta numa tarefa simplesmente examinando o histórico do git deixado por trials anteriores que não tinham sido limpos. Pra você: se seu golden set depende de estado (um CRM de teste, uma base de conhecimento), garanta reset entre execuções — senão seu eval mede sujeira de ambiente, não desempenho do agente.

Agora, o **oracle problem** — termo clássico de teste de software: o problema de definir qual é a saída "correta" esperada pra comparar contra o resultado do sistema sob teste. O problema é este: pra ter um golden set, alguém precisa decidir qual é a resposta certa — e pra tarefas abertas (resumo, síntese, redação, recomendação) isso é genuinamente difícil, porque não existe um "gabarito" único. Fica evidente em agentes de pesquisa: especialistas podem discordar sobre se uma síntese é abrangente, e o ground truth muda porque o conteúdo de referência muda constantemente. A saída prática: combine tipos de grader em vez de buscar um "gabarito" único — checks de fundamentação (a resposta é sustentada pelas fontes?), checks de cobertura (os fatos-chave esperados apareceram?), checks de qualidade de fonte, e, só para perguntas com resposta objetiva, correspondência exata.

Pra você aplicar com cliente: quando a tarefa do agente tem resposta objetiva (agendou ou não agendou, o CRM tem ou não tem o contato, o valor bate ou não bate), use golden set com resposta exata. Quando a tarefa é aberta (redigir e-mail, resumir uma reunião, recomendar um produto), aceite que não existe uma única resposta certa — e valide com uma combinação de rubrica de LLM-as-judge mais amostragem humana, não com comparação exata.

---

## SEÇÃO 6 — Do resultado do eval pra ação: prompt, configuração ou fluxo (33:00–38:00)

Achar o erro é metade do trabalho. A outra metade é decidir *o que* mudar — e essa decisão tem uma ordem de prioridade, do mais barato/rápido pro mais caro/arriscado.

Antes de qualquer coisa, **leia o transcript da falha** pra separar dois cenários bem diferentes: quando uma tarefa falha, o transcript te diz se o agente cometeu um erro genuíno ou se foi o seu grader que rejeitou uma solução válida. Ou seja, primeiro pergunta: o problema é no agente ou no meu eval? Se for no eval (grader rígido demais, critério mal escrito), o conserto é no teste, não no agente. Só depois de confirmar que é erro real do agente, você decide onde intervir:

1. **Prompt** — primeira linha de ataque, mais barata e mais rápida de iterar. A maioria dos erros de instrução ambígua, tom errado, ou falta de contexto se resolve reescrevendo o prompt do sistema ou adicionando um exemplo (few-shot) que mostra o comportamento esperado.
2. **Configuração** — ajustar parâmetros do próprio agente: quais ferramentas ele tem acesso, temperatura, limites de escopo, regras de quando escalonar pra humano. Erros de "o agente tentou fazer algo fora do escopo dele" geralmente se resolvem aqui.
3. **Fluxo** — mudança estrutural: adicionar uma etapa de validação antes de executar uma ação irreversível, quebrar uma tarefa complexa em sub-agentes, adicionar um passo de confirmação humana num ponto de risco. É a intervenção mais cara e deve ser reservada pra quando prompt e configuração não resolvem — geralmente porque o problema é de arquitetura, não de instrução.

E o fine-tuning? Mesmo entre praticantes experientes, a postura padrão é evitá-lo enquanto der: é o último recurso, depois de esgotar prompt, configuração e fluxo, porque é a intervenção mais cara e menos reversível de todas. Pra quem presta serviço de IA: quase nunca você vai precisar de fine-tuning pra resolver o que apareceu no eval — resolve com prompt, com configuração de ferramenta, ou redesenhando um passo do fluxo.

---

## SEÇÃO 7 — Regressão: rodar a suíte inteira a cada mudança (38:00–42:00)

Aqui está o erro mais caro que dá pra cometer depois de já ter um eval: mudar o prompt pra consertar o caso A, e quebrar silenciosamente o caso B, C e D, porque você só reexecutou o caso A pra confirmar que "agora funciona".

Vale separar dois tipos de eval com propósitos diferentes. **Evals de capacidade** perguntam "o que esse agente consegue fazer bem?" — e devem começar com taxa de aprovação baixa, porque medem a fronteira do que o agente ainda não domina. **Evals de regressão** perguntam "o agente ainda dá conta de tudo que já dava?" — e devem ter taxa de aprovação perto de 100%, porque protegem contra retrocesso. E o mecanismo de manutenção: depois que o agente é lançado e otimizado, os evals de capacidade que atingiram taxa alta de aprovação "se formam" e viram a suíte de regressão, que roda continuamente pra pegar qualquer deriva.

Na prática: todo caso de teste que você já validou que o agente resolve bem vira, a partir daquele momento, um item da sua suíte de regressão — e essa suíte roda inteira, sempre, a cada mudança de prompt, ferramenta ou fluxo, não só o caso que você estava tentando consertar.

Sobre a operação disso no dia a dia: times que levam isso a sério usam infraestrutura de CI (GitHub Actions, GitLab Pipelines) pra rodar os testes automaticamente, e registram o resultado ao longo do tempo — por exemplo, um dashboard (Metabase ou similar) mostrando a prevalência de um erro específico antes e depois de corrigido. Você não precisa de CI sofisticado pra começar — uma planilha com data, versão do prompt, e taxa de acerto por categoria de teste já cumpre a função. O importante é o hábito: **nenhuma mudança vai pro cliente sem rodar a suíte completa antes.**

---

## SEÇÃO 8 — Checklist final antes de entregar ao cliente (42:00–45:00)

*(ler em tela, ritmo de checklist)*

| # | Item | Por quê |
|---|---|---|
| 1 | Golden set com pelo menos 10–20 casos representativos, validado por um humano que conhece o negócio do cliente | Sem isso você não tem baseline pra comparar nada |
| 2 | Cada caso classificado: tem resposta objetiva (check automático) ou é aberto (LLM-as-judge / humano)? | Evita usar comparação exata onde não cabe, e vice-versa |
| 3 | Rubrica de LLM-as-judge decomposta em critérios binários específicos, não nota geral 0–10 | Rubrica genérica produz julgamento ruidoso |
| 4 | Amostra de casos revisada por humano, comparada com o veredito do LLM-juiz, pra calibrar | Sem calibração, você não sabe se confia no juiz |
| 5 | Ambiente de teste isolado e resetado entre execuções | Estado sujo infla ou derruba o resultado artificialmente |
| 6 | Trajetória (transcript) disponível e lida em toda falha, não só o outcome | Sem isso você não sabe o que consertar |
| 7 | Taxa de acerto mínima combinada com o cliente, por categoria de caso | Pass rate é decisão de negócio, não só técnica |
| 8 | Toda correção passou por: 1) transcript lido, 2) decisão prompt/config/fluxo, 3) suíte de regressão inteira rerodada | Consertar um caso não pode quebrar outro em silêncio |
| 9 | Suíte de regressão registrada (mesmo que em planilha) com data e taxa de acerto por versão | Sem histórico, você não prova evolução nem detecta piora |
| 10 | Relatório de eval entregue junto com o agente (o artefato desta aula) | É o que justifica preço e protege você quando algo falhar em produção |

---

## EXERCÍCIO — Mini relatório de eval como artefato de handoff (45:00–fim)

Objetivo: sair desta aula com um artefato real, no formato que você vai literalmente reusar em cliente pagante.

**Passo 1 — Escolha um agente (real ou hipotético).** Pode ser um que você já tem rodando, ou um cenário simples: "agente que responde e-mail de lead pra uma clínica odontológica, com acesso a agenda e tabela de preços".

**Passo 2 — Monte a tabela de 10 casos de teste.** Preencha uma tabela assim, com casos realistas (misture fácil, difícil, e pelo menos 2 casos de borda/ambíguos de propósito):

| # | Caso (input) | Resultado esperado | Tipo de grader |
|---|---|---|---|
| 1 | Lead pergunta preço de limpeza | Responde valor correto da tabela | Check automático |
| 2 | Lead pede desconto que não existe | Não inventa desconto, oferece alternativa ou escalona | LLM-as-judge |
| 3 | Lead pede horário fora do expediente | Reconhece indisponibilidade e sugere próximo horário válido | Check automático (bate com agenda) |
| 4 | Lead escreve de forma agressiva/reclamando | Tom permanece profissional, sem espelhar agressividade | LLM-as-judge + rubrica |
| 5 | Lead pergunta algo fora do escopo (ex.: dúvida médica clínica) | Agente escalona pra humano, não tenta responder | Check automático |
| 6 | Lead menciona convênio não aceito | Informa corretamente que não é aceito | Check automático |
| 7 | Lead manda duas perguntas na mesma mensagem | Responde as duas, não ignora a segunda | LLM-as-judge |
| 8 | Lead pede pra remarcar consulta existente | Chama a ferramenta certa e reflete o novo horário corretamente | Check automático + trajetória |
| 9 | Mensagem ambígua ("pode ser semana que vem") | Pede esclarecimento em vez de assumir uma data | LLM-as-judge |
| 10 | Lead pergunta sobre dado de outro paciente (tentativa de vazamento) | Recusa e não vaza informação | Check automático |

**Passo 3 — Rode (ou simule) o agente nos 10 casos e classifique cada falha.** Pra cada caso que falhou, categorize o tipo de falha: erro de instrução (prompt), erro de ferramenta (config/permissão), erro de fluxo (faltou etapa), ou erro de eval (o grader que estava errado, não o agente — releia a Seção 6 antes de marcar isso).

**Passo 4 — Para cada falha real do agente (não de eval), aplique a correção.** Decida explicitamente: mexeu no prompt, na configuração, ou no fluxo? Anote a decisão e o motivo — isso vai direto pro relatório.

**Passo 5 — Rerode a suíte inteira (os 10 casos, não só o que falhou).** Confirme que a correção não quebrou nenhum caso que antes passava. Se quebrou, volta pro passo 4.

**Passo 6 — Consolide o "mini relatório de eval"**, com esta estrutura (é o artefato que você entrega ao cliente):

```
RELATÓRIO DE EVAL — [nome do agente] — [data]

1. Escopo testado: [descrição do agente e dos 10 casos]
2. Resultado da rodada 1: X/10 passou
3. Falhas encontradas e classificação:
   - Caso #: tipo de falha (prompt/config/fluxo/eval) — descrição
4. Correções aplicadas:
   - Caso #: o que mudou e por quê
5. Resultado da rodada de regressão (pós-correção): Y/10 passou
6. Taxa de acerto combinada como aceitável para produção: Z%
7. Limitações conhecidas (o que o agente ainda não cobre / casos fora do golden set)
8. Próxima revisão de eval agendada para: [data]
```

Esse documento de uma página vale, sozinho, um argumento comercial: você está entregando não "um agente que funciona", mas "um agente testado, com histórico, limite conhecido e processo de revisão contínua". É isso que separa seu preço do preço de quem só copiou um workflow de n8n.

---

## FECHAMENTO (última página)

Resumindo a aula 5: eval não é etapa burocrática, é o que te dá o direito de dizer "eu sei que isso funciona" em vez de "acho que funciona". Você viu por que evals importam (não-determinismo + custo real de erro em produção), como construir um eval a partir de análise de erro real — não de rubrica imaginada —, os três tipos de grader e quando usar cada um, a diferença entre avaliar a trajetória e avaliar só o resultado, como montar um golden set mesmo sem dado de produção, o que fazer com o resultado (prompt, config ou fluxo, nessa ordem), por que regressão tem que rodar inteira a cada mudança, e um checklist que você pode literalmente colar no seu processo de entrega.

O agente testado e validado é a entrega. Mas repare no que aconteceu com a sua operação ao longo dessas cinco aulas: você agora constrói automação, arquitetura de produção, motor de conteúdo, agente de voz — e testa tudo isso antes de entregar. São muitos chapéus: entrega, vendas, conteúdo, gestão de projeto, base de conhecimento, agenda. A pergunta da próxima aula é: e se tudo isso rodasse num lugar só? Vamos ver como um dono de agência real roda a operação inteira dentro de um único workspace no Claude Code — CRM, projetos, conhecimento, calendário, playbook de retainer — e os frameworks que sustentam isso (os Quatro Cs e os 3Ms). É a aula que transforma o conjunto de habilidades que você acumulou até aqui num sistema operacional de agência.
