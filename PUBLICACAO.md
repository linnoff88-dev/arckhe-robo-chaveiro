# PUBLICACAO — robo-chaveiro

Ordem do Nei (15/11/2026): "pode publicar no portal no inema.robot no lives6".

portal: feito
telegram: feito 3005
lives6: em produção (cadeia explicavideo; fase 2 pelo vigia continua, msg 4781 no v3)

## Guia (GitHub Pages, via Actions, branch main)

| Idioma | URL |
|---|---|
| PT | https://inematds.github.io/robo-chaveiro/guia/ |
| EN | https://inematds.github.io/robo-chaveiro/guia/en/ |
| ES | https://inematds.github.io/robo-chaveiro/guia/es/ |

- As três responderam 200 em 15/11/2026 (e a raiz, que redireciona para `guia/`).
- Repo: https://github.com/inematds/robo-chaveiro (público desde 15/11/2026).
- O guia tem a marca `<!-- VIDEO: entra o explicavideo do projeto quando publicado -->` nos 3 idiomas, logo
  depois do `#resumo`: o vídeo do lives6 entra ali (e uma linha "🎬 Vídeo:" nos 3 READMEs).
- Revisor independente: 1ª rodada reprovado (6), 2ª aprovado (8); as correções das duas entraram. Notas em
  `~/projetos/wifi/scripts/revisor-nota.py`.

## Portal (skill atualiza-portal)

| Repo | Commit |
|---|---|
| NeiMaldaner/portal (inema.club) | `4351030` |
| inematds/inemabuscas | `e99681c` |
| inematds/inemapro (inemapro-mono, catálogo PRO) | `61f0763` |

- Card em `communityProjects` (badge Guia), topo de `projectUpdatesData`:
  "robo-chaveiro — robozinho de bolso que conversa com seu filho, com você no controle".
- EN/ES em `translated-courses.ts` (id 10232). Enrichment `projeto:robo-chaveiro-guia` (robotica-ia-fisica).
- No inema.club aparece só no quadro "Últimas Atualizações de Projetos"; no PRO entra no catálogo e na busca.
- O teste `titulos-quadro` do portal já falhava antes desta publicação, por um título de vídeo de 13/11 com 101
  caracteres (não é deste projeto).

## Telegram (grupo INEMA.ROBOT)

- Tópico: **3005** — https://t.me/c/2412734678/3005
- Título: "Robô Chaveiro - Seu filho leva no bolso um robozinho que conversa, e você decide quando e sobre o quê"
- Mensagens 3006 a 3021; lançado no MENU (https://t.me/c/2412734678/2270, msg 3022).
- Texto: `~/projetos/output/posts-telegram/2026-11-15-robo-chaveiro.md`; capa:
  `~/projetos/output/posts-telegram/capas/robo-chaveiro.jpg` (a 1ª versão, com robô humanoide, ficou como `-v1` e não foi usada).
- Para responder no mesmo tópico: `postar_topico.py --grupo INEMA.ROBOT --topico 3005 ...` (skill `postar-telegram`).
- Nada no Telegram pode ser apagado sem aprovação individual do Nei.

## Vídeo no lives6 (em produção)

- Pasta: `~/projetos/explicavideos/textos/robo-chaveiro/` (pedido.txt, regras.md, FATOS.md, roda.sh, CONTINUAR.md).
- Cadeia `roda.sh` na unit `ev-robo-chaveiro` (iniciada 15/11/2026 02:50): ficha → avatar no estúdio HeyGen → visual →
  entrega (16:9 e, 1 h depois, o 9:16 no lives6). Andamento: `tail ~/projetos/explicavideos/textos/robo-chaveiro/cadeia.log`.
- Fase 2 (vigia `continua`, nome `ev2-robo-chaveiro`): põe o vídeo no guia (3 idiomas + READMEs), card de vídeo no
  portal e resposta no tópico 3005.
- lives6: feito 15/11/2026 — 16:9 https://www.youtube.com/watch?v=735cjrp4KiM · 9:16 https://youtube.com/shorts/sbQqWAy1ByE (vídeo no guia PT/EN/ES)
