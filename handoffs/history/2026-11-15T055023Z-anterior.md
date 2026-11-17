# Handoff — robo-chaveiro — 15/11/2026

## Objetivo
Robozinho de bolso, chaveiro, que conversa por voz, para crianças e pessoas. Pedido literal do Nei:
"quero que crie um outro projeto, criando uma pasta para este projeto, pega as ideias daqui, depois vamos
construir este projeto para as crianças e pessoas levarem como chaveiro". Reaproveita o servidor (opção B) do
`~/projetos/robo-cores3-lite`.

## Feito nesta sessão
- Placa identificada pelo anúncio (fotos com a marca SpotPear e "emo Xiaozhi") e pela wiki oficial da SpotPear:
  provável **SpotPear ESP32-S3 1.54" "muma"**, firmware "emo". Ficha na DEFINICAO.md, seção 2.
- Pasta criada com DEFINICAO, PLANO, README, CLAUDE, FALHAS e este handoff. Fotos em `referencias/`.
- Repo git local, sem remoto.

## Feito depois (15/11/2026, 2ª rodada)
- Pedido do Nei: desenhar o uso "direto numa API e também através de um servidor", sem câmera, e o aviso de bateria
  acabando, porque o chaveiro é móvel. Feito só no papel: DEFINICAO seções 4 (cérebros B, A1, A2), 5 (fora de casa,
  várias redes Wi-Fi) e 7.1 (bateria em três degraus, frases gravadas no flash, que funcionam sem rede).
  PLANO: novas fases F2b (bateria) e F3b (cérebro A).
- A wiki do muma não lista pino de bateria. Conferir no esquema V1/V2 na F1.

- 3ª rodada: desenho da configuração pelo celular (rede própria do chaveiro na 1ª vez + painel da família no PC,
  porta 8771) e do controle dos pais (horário, minutos por dia, pausar, assuntos, gasto, histórico só em texto,
  retenção, a criança sabe que os pais veem). DEFINICAO seção 13, PLANO fase F2c. O painel pode ser feito antes
  da placa chegar.

- 4ª rodada (15/11, ordem do Nei "pode publicar no portal no inema.robot no lives6"): repo público
  inematds/robo-chaveiro, guia PT/EN/ES no Pages, portal (4351030/e99681c/61f0763), tópico 3005 no INEMA.ROBOT, vídeo
  lives6 em produção (fase 2 pelo vigia continua). Tudo em PUBLICACAO.md.

## O que NÃO fazer
- Não ativar o XiaoZhi (nuvem de terceiros) nem configurar o firmware de fábrica no Wi-Fi sem o Nei.
- Não chamar nenhuma API externa. Opção A só com autorização escrita do Nei (serviço e finalidade).
- Não editar o `~/projetos/robo-cores3-lite` nem o `~/projetos/inemavox`.
- Não instalar biblioteca, `esptool`, PlatformIO, ESP-IDF ou modelo sem avisar o Nei.

## Decisões
- Cérebro: servidor da opção B do robo-cores3-lite, usado onde está, com `X-Robo-Id: chaveiro-1`.
- Ativação: segurar o botão BOOT para falar (proposta; confirmar na F1).
- Sem câmera: a placa não tem.

## Não verificado
- Se a placa é mesmo o muma (o anúncio não mostra o SKU).
- Bateria da versão de chaveiro (tem? quantos mAh?), tamanho e peso.
- Chip de áudio, pinos do microfone e do falante, número de microfones, se mic e falante funcionam juntos.
- Se a unidade é a versão "Touch".
- Placa no compilador (FQBN) e modo da PSRAM.
- Frete e imposto da compra.

## Fica com o Nei (perguntas abertas)
1. Que variante veio (Touch? com bateria? capinha verde?) — conferir quando chegar.
2. A nuvem do XiaoZhi está fora de cogitação, ou pode testar uma vez o firmware de fábrica?
3. Fora de casa: aceitar "só no Wi-Fi de casa", celular como ponte, ou estudar a opção A com API?
4. Persona, nome e voz do chaveiro (voz de criança? nome próprio?). Isso muda o `persona.txt` do outro projeto,
   ou o servidor passa a escolher a persona pelo `X-Robo-Id`.
5. Idade das crianças e se as gravações da conversa ficam guardadas no PC (e por quanto tempo).
6. Cérebro A: o Nei disse "provavelmente usaremos openrouter" (15/11) — intenção, não autorização. Falta: autorizar por
   escrito (serviço e finalidade) e escolher A1 (direto), A2-texto ou A2-áudio. DEFINICAO 4.2 e 4.3.
7. Painel: quem são os pais que usam (só a família do Nei ou outras famílias)? Alerta aos pais por mensagem
   (Telegram/e-mail) entra? Histórico guardado por quanto tempo por padrão?

## Próxima ação exata
Quando a placa chegar: F0 do PLANO.md (ligar sem ativar o XiaoZhi, `arduino-cli board list`, fotografar a placa
e a etiqueta, anotar a variante). Antes disso, se o Nei quiser adiantar: baixar e ler os esquemas V1/V2 da wiki
do muma para achar os pinos de áudio (só leitura, sem instalar nada).
