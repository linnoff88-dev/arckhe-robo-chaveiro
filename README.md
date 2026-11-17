# robo-chaveiro

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

[![RobÃ´ Chaveiro](guia/assets/banner.jpg)](https://inematds.github.io/robo-chaveiro/guia/)

## O que Ã©

Este Ã© um projeto aberto para transformar uma plaquinha com tela de 1,54" numa espÃ©cie de bichinho de bolso que
conversa por voz: a crianÃ§a segura um botÃ£o, fala, e ele responde em voz alta, com um rosto de olhos grandes na
tela. Ele "pensa" de um de trÃªs jeitos: pelo computador de casa, direto num serviÃ§o de IA na internet (OpenRouter),
ou pelo computador de casa repassando para esse serviÃ§o. Os pais vÃ£o configurar tudo pelo celular. Hoje existe o
projeto no papel e o servidor de casa, que jÃ¡ funciona num projeto irmÃ£o; a placa e o programa dela ainda nÃ£o.

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/robo-chaveiro/guia/**

ðŸŽ¬ VÃ­deo: https://www.youtube.com/watch?v=735cjrp4KiM Â· Short: https://youtube.com/shorts/sbQqWAy1ByE

## Mais detalhes

A placa Ã© a SpotPear ESP32-S3 1,54" (provÃ¡vel "muma", versÃ£o "emo"), comprada no AliExpress em 15/11/2026 por
R$ 132,14. O "cÃ©rebro" de casa Ã© o servidor que jÃ¡ funciona no
[robo-cores3-lite](https://github.com/inematds/robo-cores3-lite) (opÃ§Ã£o B): Whisper â†’ `claude -p` pela
assinatura â†’ voz Chatterbox. Desenhados tambÃ©m: cÃ©rebro A1 (direto no OpenRouter) e A2 (servidor como ponte),
para usar longe de casa, o aviso de bateria acabando, o painel dos pais e corpos de bicho com o chaveiro como
rosto. Nada disso estÃ¡ implementado, e nenhuma API foi chamada.

**Estado em 15/11/2026: sÃ³ a definiÃ§Ã£o. Placa a caminho, nenhum firmware.**

| Para | Onde |
|---|---|
| O que Ã©, a placa, seguranÃ§a, ideias | [DEFINICAO.md](DEFINICAO.md) |
| Fases e critÃ©rio de pronto | [PLANO.md](PLANO.md) |
| Regras do projeto | [CLAUDE.md](CLAUDE.md) |
| Retomar o trabalho | [handoffs/latest.md](handoffs/latest.md) |
| Fotos do anÃºncio e ficha da placa | [referencias/](referencias/) |
| Ideia para depois: corpos de boneco e bicho com o chaveiro como rosto | [ideias/corpos/PROPOSTAS.md](ideias/corpos/PROPOSTAS.md) |

## O processo, do inÃ­cio ao fim

Legenda: ðŸ§  sessÃ£o com o Nei Â· âš™ï¸ comando/script Â· ðŸ¤– `claude -p`

| # | Passo | Comando | Quem | Existe hoje? |
|---|---|---|---|---|
| 1 | Ler a definiÃ§Ã£o e responder as perguntas abertas | [handoffs/latest.md](handoffs/latest.md) | ðŸ§  | sim |
| 2 | Receber a placa e ligar, sem ativar o XiaoZhi | [PLANO.md](PLANO.md), F0 | ðŸ§  | nÃ£o: placa a caminho |
| 3 | Ver a porta da placa | `arduino-cli board list` | âš™ï¸ | nÃ£o testado: sem placa |
| 4 | Subir o servidor (no outro projeto) | `python3 ~/projetos/robo-cores3-lite/servidor/servidor.py` | âš™ï¸ | sim |
| 5 | Dentro de cada turno: escrever a resposta | `claude -p --model haiku --tools "" --strict-mcp-config --no-session-persistence â€¦` (feito pelo servidor) | ðŸ¤– | sim (no robo-cores3-lite) |
| 6 | Achar pinos de Ã¡udio nos esquemas e escolher a placa no compilador | [PLANO.md](PLANO.md), F1 | ðŸ§  | nÃ£o |
| 7 | Instalar biblioteca de tela (avisar o Nei antes) | a definir | âš™ï¸ | nÃ£o |
| 8 | Escrever, compilar e gravar o firmware | `arduino-cli compile/upload --fqbn <a definir> firmware/chaveiro` | âš™ï¸ | nÃ£o |
| 9 | Conversar na bancada: 5 perguntas seguidas (cÃ©rebro B) | [PLANO.md](PLANO.md), F2 | ðŸ§  | nÃ£o |
| 10 | Gravar as frases de bateria (Chatterbox local) e testar o aviso sem rede | [PLANO.md](PLANO.md), F2b | âš™ï¸ ðŸ§  | nÃ£o |
| 11 | Configurar pelo celular (nome, Wi-Fi) e painel da famÃ­lia (limites, histÃ³rico) | `python3 painel/painel.py` (a criar), porta 8771; [PLANO.md](PLANO.md), F2c | âš™ï¸ ðŸ§  | nÃ£o: sÃ³ desenho |
| 12 | Argola, capinha, persona de crianÃ§a | [PLANO.md](PLANO.md), F3 | ðŸ§  | nÃ£o |
| 13 | CÃ©rebro A (A1 direto ou A2 ponte), sÃ³ com autorizaÃ§Ã£o escrita do Nei | [PLANO.md](PLANO.md), F3b | ðŸ§  | nÃ£o: sÃ³ desenho |
| 14 | Opcionais (troca automÃ¡tica, tÃºnel, espera, palavra de ativaÃ§Ã£o, guia) | [PLANO.md](PLANO.md), F4 | ðŸ§  | nÃ£o |
| 15 | Guia PT/EN/ES no GitHub Pages | skill `projetos-landing-guia` | ðŸ§  | sim â€” [PUBLICACAO.md](PUBLICACAO.md) |
| 16 | Portal inema.club, inemabuscas e catÃ¡logo PRO | skill `atualiza-portal` | ðŸ§  | ver [PUBLICACAO.md](PUBLICACAO.md) |
| 17 | TÃ³pico no grupo INEMA.ROBOT do Telegram + MENU | skill `postar-telegram` | ðŸ§  | ver [PUBLICACAO.md](PUBLICACAO.md) |
| 18 | VÃ­deo no canal lives6 e no guia | marca `<!-- VIDEO: -->` no guia | ðŸ§  | ver [PUBLICACAO.md](PUBLICACAO.md) |

O Ãºnico passo com `claude -p` Ã© o 5, feito pelo servidor do robo-cores3-lite, pela assinatura, sem ferramentas.

## Regras

- Nenhuma API externa sem autorizaÃ§Ã£o explÃ­cita do Nei. Isso inclui a nuvem do XiaoZhi do firmware de fÃ¡brica.
- Claude sÃ³ pela assinatura (`claude -p`).
- Repo pÃºblico `inematds/robo-chaveiro` desde 15/11/2026 (ordem do Nei: publicar no portal, no INEMA.ROBOT e no lives6).

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe