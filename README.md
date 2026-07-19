# 🌲 UIANA LOSER PRO MAX

Corrida de tabuleiro multiplayer (2 a 4 jogadores) pra jogar com os amigos direto no navegador do celular. Sem app, sem cadastro — só nome e código de sala.

## 1. Configurar o Firebase (obrigatório, grátis, ~5 minutos)

1. Acesse **https://console.firebase.google.com/** e crie um projeto novo.
2. No menu lateral: **Compilação → Realtime Database → Criar banco de dados**.
   - Escolha a região mais próxima de você/seus amigos.
   - Comece em **modo de teste** (funciona na hora; depois você pode
     colar o conteúdo do arquivo `database.rules.json` deste projeto
     na aba "Regras" pra deixar mais seguro).
3. No menu lateral: **Compilação → Authentication → Sign-in method →
   ative "Anônimo"**. É assim que cada jogador recebe uma identidade
   sem precisar de senha.
4. No menu lateral: **⚙️ Configurações do projeto → Seus aplicativos
   → ícone "</>"** para registrar um app Web. O Firebase vai te
   mostrar um objeto `firebaseConfig`.
5. Abra `js/firebase-config.js` neste projeto e substitua os valores
   de exemplo pelos que o Firebase te deu.

## 2. Hospedar

Qualquer hospedagem estática funciona, porque o jogo é só
HTML/CSS/JS puro (sem servidor próprio). Sugestões gratuitas:

- **Vercel** (vercel.com) — arraste a pasta do projeto.
- **Netlify** (netlify.com) — arraste a pasta do projeto (drag & drop).
- **GitHub Pages** — suba os arquivos num repositório e ative o Pages.
- Ou o seu domínio/hospedagem comum: basta subir os arquivos por FTP.

Não precisa de build, Node, nem instalar nada — é só subir os
arquivos como estão.

## 3. Jogar

1. Abra o link no celular.
2. Um jogador clica em **Criar sala**, escolhe nome/boneco/cor e
   recebe um código de 5 letras.
3. Os outros (até 3) clicam em **Entrar em uma sala** e digitam o
   código.
4. O anfitrião (quem criou a sala) toca em **Iniciar partida**.

## Estrutura do projeto

```
index.html              → estrutura de todas as telas
css/style.css            → identidade visual (tema floresta)
js/firebase-config.js    → suas credenciais do Firebase (edite aqui)
js/board-data.js         → catálogo de eventos + gerador do tabuleiro (BOARD_SIZE casas, hoje 50)
js/sound.js               → sistema de som (sintetizado; troque por arquivos reais se quiser)
js/game.js                → motor do jogo (salas, turnos, dado, eventos, pódio)
database.rules.json      → regras de segurança sugeridas do Realtime Database
sounds/, img/             → pastas vazias, prontas para você colocar arquivos reais
```

## Como funciona a sincronização

O jogador que **cria a sala** atua como "anfitrião": é o navegador
dele que sorteia o dado, calcula os efeitos das casas especiais e
grava o novo estado da partida no Firebase. Os outros celulares
apenas leem esse estado em tempo real e mostram na tela — isso evita
que alguém altere o próprio resultado do dado ou "trapaceie". Se o
anfitrião fechar o app no meio da partida, a sala fica pausada até
ele voltar (o Firebase mantém tudo salvo).

## Personalizando

- **Trocar sons**: preencha o objeto `SOUND_FILES` no topo de
  `js/sound.js` com caminhos para arquivos em `/sounds`.
- **Trocar bonecos por imagens**: os personagens hoje são emojis
  (`AVATARS` em `js/game.js`). Troque `emoji: '🧭'` por um caminho de
  imagem e ajuste o CSS de `.pawn`/`.podium-avatar` pra usar
  `background-image` em vez de emoji.
- **Ajustar eventos/pegadinhas**: edite o array `EVENTS` em
  `js/board-data.js`. A quantidade de casas especiais (hoje ~30% a 40%
  do tabuleiro) e as regras de distribuição estão em `generateBoard()`.
- **Mudar o tamanho do tabuleiro**: altere a constante `BOARD_SIZE`
  no topo de `js/board-data.js` (hoje 50 casas). Todo o resto do jogo
  se ajusta automaticamente a partir dela.

## Limitações conhecidas (transparência)

- É um jogo pensado pra "jogar com os amigos", não pra escala de
  milhares de salas simultâneas — o plano gratuito do Firebase dá
  conta tranquilamente disso.
- A segurança é "anfitrião confiável": não há um servidor separado
  validando cada jogada (isso exigiria Firebase Cloud Functions,
  que é pago a partir de um certo volume). Para um jogo casual entre
  amigos, isso não costuma ser problema.
- Sons são sintetizados (bipes) por padrão — funcionam sem nenhum
  arquivo externo, mas você pode trocar por efeitos reais.
