# CLAUDE.md — robo-chaveiro

Robozinho de bolso, para levar como chaveiro, que conversa por voz. Placa: SpotPear ESP32-S3 1,54" (provável
"muma", versão "emo"; confirmar na F0). Cérebro: o servidor da opção B do `~/projetos/robo-cores3-lite`.
Pedido literal do Nei na DEFINICAO.md, seção 1.

- Repo: `inematds/robo-chaveiro`, público desde 15/11/2026 (ordem do Nei: "pode publicar no portal no inema.robot no
  lives6"). Guia em `guia/` (PT/EN/ES), publicação registrada em `PUBLICACAO.md`.
- Autor dos commits: `inematds <inematds@gmail.com>` (configurado só neste repo).
- O `~/projetos/robo-cores3-lite` é só leitura daqui. Mudar o servidor, a persona ou o `.env` de lá: só com o Nei.

## Ao começar uma sessão
1. Ler `handoffs/latest.md`.
2. Ler `DEFINICAO.md` e a fase atual no `PLANO.md`.

## Comandos
- Servidor (no outro projeto): `python3 ~/projetos/robo-cores3-lite/servidor/servidor.py` (porta 8770).
  Validar: `python3 ~/projetos/robo-cores3-lite/servidor/testar.py ~/projetos/robo-cores3-lite/servidor/teste/pergunta.wav`.
- Placa: `arduino-cli board list`. Placa no compilador (FQBN): a definir na F1.

## Regras do projeto
- **Nenhuma API externa sem autorização explícita do Nei** (Gemini, OpenAI, Groq, ElevenLabs, HeyGen, Edge TTS,
  qualquer nuvem). Inclui **ativar o XiaoZhi** do firmware de fábrica: é nuvem de terceiros e levaria a voz de
  crianças para fora. Chave em `.env` não é autorização.
- **Claude só pela assinatura:** `claude -p`, nunca `--bare`, sempre `env -u ANTHROPIC_API_KEY`, sempre
  `--tools "" --strict-mcp-config --no-session-persistence`.
- **Voz só pela rota `/api/jobs/tts/upload` do inemavox** (Chatterbox local). Não mexer no `~/projetos/inemavox`.
- **Nenhum modelo local novo** sem aprovação do Nei (palavra de ativação, outro Whisper, outro TTS).
- **Biblioteca nova** (Arduino ou Python) e ferramenta nova (`esptool`): avisar o Nei antes de instalar.
- Não instalar PlatformIO nem ESP-IDF sem o Nei.
- Antes de gravar firmware nosso: guardar os links dos `.bin` de fábrica (wiki do muma) para poder voltar.
- Segredos fora do git: `firmware/chaveiro/segredos.h` (Wi-Fi e token).
- Crianças: nada de cordão no pescoço sem fecho que se solta; nenhuma peça pequena solta; persona não pede dado
  pessoal; decidir com o Nei se as gravações ficam guardadas.
- Número sem fonte não entra em documento. O que não foi conferido leva "não verificado".
- Commit a cada pacote verificado. Mudou o fluxo: atualizar "O processo, do início ao fim" no README no mesmo commit.
- Falha corrigida: uma linha no `FALHAS.md`. Limite de ambiente: uma linha em `~/projetos/wifi/LIMITES.md`.
- Perguntas ao Nei em texto livre (nunca menu).

## Self-learning
Quando o Nei corrigir ou você notar um erro: antes de seguir, escreva a lição como uma linha no TOPO da seção
Lessons abaixo.

## Lessons
- 15/11/2026: existem duas placas ESP32-S3 de 1,54" na SpotPear: o "muma" (nossa, provável) e a Waveshare
  ESP32-S3-Touch-LCD-1.54 (ES8311, ES7210, giroscópio, três botões). Não misturar as fichas.
