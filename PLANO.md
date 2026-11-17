# PLANO — robo-chaveiro

Cada fase tem um critério de pronto que se confere com um comando ou uma medida. O porquê de cada escolha está
na [DEFINICAO.md](DEFINICAO.md).

| Fase | O que entrega | Precisa de |
|---|---|---|
| F0 | Placa chegou, liga e carrega | A entrega |
| F1 | Firmware grava a fala e envia ao servidor do robo-cores3-lite | F0; aviso ao Nei antes de instalar biblioteca |
| F2 | O chaveiro toca a resposta e mostra o rosto | F1 |
| F2b | Aviso de bateria acabando, que funciona sem rede | F2; pino da bateria achado no esquema |
| F2c | Configuração pelo celular e painel da família (nome, limites, histórico) | F2; o painel pode começar antes, sem placa |
| F3 | Chaveiro de verdade: argola, capinha, uso por criança | F2 e decisões do Nei |
| F3b | Cérebro A (A1 direto ou A2 ponte), para funcionar longe de casa | Autorização escrita do Nei (serviço e finalidade) |
| F4 | Opcionais | Decisão do Nei, um por um |

Os três cérebros (B servidor, A1 direto, A2 ponte) e a bateria estão desenhados na DEFINICAO.md, seções 4, 5 e 7.
O firmware nasce com a camada "cérebro" separada desde a F1, mesmo que só a B exista.

## F0 — Placa chegou e liga

**Não ativar o XiaoZhi** (nuvem de fora). Ligar só para ver se está viva.

Pronto quando:
- Liga pelo botão e a tela acende.
- Com o cabo USB-C, o LED vermelho de carga acende e apaga quando a carga termina (wiki do muma).
- `arduino-cli board list` mostra uma porta nova com a placa ligada.
- Fotos da placa (frente, verso, etiqueta, bateria se houver) em `referencias/` e a variante anotada na
  DEFINICAO.md, seção 2 (Touch? bateria? tamanho medido?).

## F1 — Grava e envia para o servidor

O que fazer: ler os esquemas V1/V2 da wiki para achar os pinos do microfone, do falante **e da bateria** (divisor
de tensão? pino de "carregando"?); escolher a placa no compilador; escrever `firmware/chaveiro/` com o botão BOOT
como "segurar para falar", a camada `cerebro_b` separada do hardware e uma lista de redes Wi-Fi (casa, celular).

Pronto quando, com `python3 ~/projetos/robo-cores3-lite/servidor/servidor.py` no ar:

| # | Comando ou ação | Saída esperada |
|---|---|---|
| 1 | `arduino-cli compile --fqbn <placa escolhida> firmware/chaveiro` | Código 0 |
| 2 | Gravar e abrir o monitor serial | `PSRAM:` maior que zero; Wi-Fi conectado; `GET /saude` → 200 |
| 3 | Segurar o botão 3 s e dizer "um, dois, três, testando" a 30 cm, três vezes | O log do servidor mostra `OUVIDO:` com a frase certa nas três |
| 4 | `ffprobe` e `volumedetect` na gravação guardada pelo servidor | `pcm_s16le,16000,1`; volume médio acima de −35 dB |
| 5 | Segurar o botão 20 s | A gravação para sozinha em 15 s e é enviada; a placa não reinicia |

## F2 — Toca a resposta

Pronto quando:

| # | Ação | Saída esperada |
|---|---|---|
| 1 | Cinco perguntas seguidas, sem reiniciar | Cinco respostas tocadas até o fim, sem estalo |
| 2 | Ouvir a 50 cm numa sala comum | Dá para entender todas as palavras |
| 3 | Desligar o servidor e falar | Rosto de erro por 2 s e volta a pronto; não reinicia |
| 4 | 30 min parado e uma pergunta | Responde (Wi-Fi reconecta sozinho) |
| 5 | Bateria cheia, uma pergunta a cada 2 min, gravando a tensão a cada minuto | Minutos até desligar e a curva de tensão em `MEDIDAS.md`; daí saem os degraus da DEFINICAO.md, seção 7.1 |

O tempo do botão solto até o primeiro som vai para uma tabela `MEDIDAS.md` neste projeto.

## F2b — Aviso de bateria acabando

Antes: gerar uma vez as frases de bateria ("estou ficando com sono...", "tchau, vou dormir") pelo Chatterbox local
(inemavox, `/api/jobs/tts/upload`) e guardá-las no firmware.

Pronto quando:

| # | Ação | Saída esperada |
|---|---|---|
| 1 | Monitor serial a cada 30 s | Linha `BAT: <tensão> V <nível>%`; parado, a leitura não varia mais que o anotado na MEDIDAS.md |
| 2 | Descarregar até o degrau "cansado" | Ícone e olhos de sono na tela |
| 3 | Descarregar até o degrau "pedindo carga", **com o Wi-Fi desligado** | Fala a frase de bateria da memória dele (sem rede) |
| 4 | Descarregar até o degrau "dormindo" | Diz tchau, apaga e entra em sono profundo; não fica ligando e desligando |
| 5 | Ligar o cabo | Acorda; com pino de "carregando", mostra o raio e não avisa de bateria fraca |

Se o esquema não tiver pino de bateria: parar e decidir com o Nei (adaptação de hardware ou aviso por tempo).

## F2c — Configuração pelo celular e painel da família

Desenho na DEFINICAO.md, seção 13. O painel (`painel/`, porta 8771, só Python padrão) pode ser feito antes de a
placa chegar, testado com um chaveiro de mentira (`curl`).

Pronto quando:

| # | Ação | Saída esperada |
|---|---|---|
| 1 | Chaveiro novo, ligar | Abre a rede "Chaveiro-XXXX"; o celular entra, põe o Wi-Fi e o código de pareamento; o chaveiro reinicia já na rede de casa |
| 2 | BOOT seguro por 10 s | Volta ao modo de configuração sem apagar o nome |
| 3 | No celular, painel: trocar o nome para "Bolinha" | Na próxima conversa: "eu sou o Bolinha" |
| 4 | Horário permitido terminando 1 min à frente; esperar e falar | Rosto dormindo e "agora é hora de descansar"; nada vai ao servidor nem à API |
| 5 | Limite de 2 min por dia; conversar até passar | Aviso "só mais um pouquinho" e depois para |
| 6 | "Pausar agora" no painel | O chaveiro para em até 5 min (próxima consulta) |
| 7 | Cinco perguntas e abrir o histórico | As cinco aparecem em texto, com hora; nenhum áudio guardado |
| 8 | Guardar por "1 dia" e adiantar o relógio do teste | O painel apaga os turnos antigos sozinho |
| 9 | `curl` no painel sem senha e com token errado | 401 nos dois |
| 10 | Perguntar ao chaveiro "meus pais veem o que eu falo?" | Responde que sim, com palavras de criança |

A persona vinda do painel chegando ao servidor da B depende de mudança no `robo-cores3-lite`: combinar com o Nei
antes do teste 3 na B.

## F3 — Chaveiro e capinha

Pronto quando:
- O chaveiro fica preso numa argola ou mosquetão e aguenta 10 sacudidas sem soltar.
- Nenhuma peça sai com a mão (teste de puxar cada parte).
- Se houver capinha impressa: o botão e o USB-C ficam acessíveis, o falante não fica abafado (teste 2 da F2 passa
  com a capinha).
- Persona de criança combinada com o Nei e testada em 10 perguntas de criança (nenhuma resposta assusta ou pede
  dado pessoal; conferido pelo Nei).

## F3b — Cérebro A: direto numa API (A1) ou servidor como ponte (A2)

Só começa com a autorização escrita do Nei dizendo o serviço (provável: OpenRouter) e a finalidade, e a
escolha entre A1 e A2. A2 também exige mudança no servidor do `robo-cores3-lite`, combinada com ele.

Pronto quando:

| # | Ação | Saída esperada |
|---|---|---|
| 1 | Trocar o cérebro em `segredos.h` e compilar | Código 0; o mesmo firmware serve aos três cérebros |
| 2 | `grep -rni "api_key\|chave" firmware/chaveiro/*.ino firmware/chaveiro/*.cpp` | Nenhuma chave no código. Na A1, só a chave própria do chaveiro, no `segredos.h` (fora do git), com teto diário; na A2, nenhuma no chaveiro |
| 3 | Ponto de acesso do celular, longe do Wi-Fi de casa, cinco perguntas (A1) | Cinco respostas; o tempo até o primeiro som em `MEDIDAS.md`, ao lado do da B |
| 4 | Desligar o celular no meio de uma resposta | Rosto de "sem internet" e volta a PRONTO; não reinicia |
| 5 | Custo de uma conversa de 5 min, lido no painel da API | Anotado em `MEDIDAS.md` e mostrado ao Nei |
| 6 | 10 perguntas de criança com a persona do chaveiro | Nenhuma resposta assusta ou pede dado pessoal (conferido pelo Nei) |

## F4 — Opcionais (um por um, só com decisão do Nei)

| Opcional | Critério de pronto |
|---|---|
| Frases de espera tocadas pela placa | Toca uma frase da banca em menos de 1 s depois de soltar o botão; não repete as 8 últimas |
| Corpos de boneco e bicho (o chaveiro vira o rosto) | Ideias e imagens em `ideias/corpos/PROPOSTAS.md` (8 propostas, Codex, 15/11/2026). Pronto: um corpo feito na mão, testado com os 6 passos de "Antes de transformar ideia em brinquedo" de lá (encaixe, som dentro e fora, botões, temperatura, puxão) |
| Troca automática de cérebro | Rede de casa à vista → B; senão → A1. Cinco trocas de rede sem reiniciar |
| Cérebro B fora de casa por túnel | Ferramenta nova avisada ao Nei; 5 perguntas pelo celular respondidas pelo PC de casa |
| Palavra de ativação | Modelo local novo: só com aprovação; 9 de 10 a 50 cm |
| Guia e publicação | Só com a ordem do Nei: repo, guia em `guia/`, portal |

## Riscos

| Risco | Como se descobre | Saída |
|---|---|---|
| Placa não é o muma | F0, etiqueta e fotos | Achar a wiki certa antes da F1 |
| Mic e falante não funcionam juntos | F1, esquema | Meio a meio, como no robô de mesa |
| Demora de 10–14 s parece longa para criança | F2 | Frases de espera; rosto de "pensando" animado |
| Bateria fraca | F2, teste 5 | Avisar o Nei; aceitar uso curto |
| Placa sem pino de bateria | F1, esquema | Dois resistores num pino livre ou aviso por tempo (decidir com o Nei) |
| WebSocket da A1 pesado demais para o firmware | F3b | Ficar na A2 (o chaveiro só fala HTTP com o PC) |
| Chave do chaveiro vazada (perdeu o chaveiro) | — | Desativar só essa chave no OpenRouter pelo PC; o teto diário limita o prejuízo |
| Modelo de áudio do OpenRouter caro ou com voz ruim para criança | F3b, teste 5 e ouvir com o Nei | A2-texto: Whisper e Chatterbox locais, só a resposta no OpenRouter |
| Perda do chaveiro | — | Token trocável; nada guardado além do Wi-Fi e do token |
