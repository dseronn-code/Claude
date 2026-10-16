# Jogo da Velha Online (Android + navegador)

Multiplayer online: sala com código, matchmaking ranqueado e ranking de vitórias.
**App e navegador jogam um contra o outro** (mesmo código, mesmo protocolo).

- Partida em si: conexão direta entre os dois jogadores (WebRTC / PeerJS).
- Ranking e matchmaking: Firebase Realtime Database (plano grátis, sem cartão).
- Sem o Firebase configurado, tudo continua funcionando **menos** ranking/matchmaking (sala por código segue ok).

## 1. Configurar o Firebase (uma vez só, dá pra fazer pelo celular)
1. Entre em <https://console.firebase.google.com> → **Adicionar projeto** (pode desligar o Analytics).
2. **Build → Authentication → Vamos começar → Anônimo → Ativar → Salvar.**
3. **Build → Realtime Database → Criar banco de dados** (qualquer região; modo bloqueado).
   Depois abra a aba **Regras**, apague tudo, cole o conteúdo de [`database.rules.json`](database.rules.json) e toque em **Publicar**.
4. Engrenagem ⚙️ → **Configurações do projeto → Seus apps → ícone `</>` (Web)** → registre o app → copie o `firebaseConfig`.
   (Se não tiver `databaseURL`, copie o endereço que aparece no topo da página do Realtime Database, tipo `https://xxxx-default-rtdb.firebaseio.com`.)
5. No GitHub, edite `app/src/main/assets/firebase-config.js` e troque `window.FIREBASE_CONFIG = null;` por:
   ```js
   window.FIREBASE_CONFIG = {
     apiKey: "...", authDomain: "...", databaseURL: "...", projectId: "...", appId: "..."
   };
   ```
   A `apiKey` do Firebase não é segredo; quem protege os dados são as regras do passo 3.

## 2. APK (Android)
Codemagic (grátis): conecte o repositório e rode o workflow "Jogo da Velha APK" (`codemagic.yaml`). Baixe o `app-debug.apk` em Artifacts.
(Alternativa: workflow "Build APK" do GitHub Actions, se sua conta permitir.)

## 3. Versão para navegador
A pasta `app/src/main/assets` é um site pronto (instalável como PWA). Hospede grátis, por exemplo no **Cloudflare Pages**:
*Workers & Pages → Create → Pages → Connect to Git →* escolha o repositório e a branch,
**Build command:** vazio · **Build output directory:** `app/src/main/assets`.
Se o Cloudflare criar um projeto do tipo *Workers* (deploy command `npx wrangler deploy`), o arquivo `wrangler.jsonc` já aponta a pasta do site.
Cada commit na branch republica o site. Depois é só mandar o link; no Chrome dá pra "Adicionar à tela inicial".

## Como funciona o ranking
- **Só partidas do matchmaking ("Jogar online") contam**; sala por código não vale ponto (evita jogar contra si mesmo).
- Cada jogador registra o próprio resultado (V/D/E). As regras do banco limitam a +1 por vez, mas é um jogo casual: não há anti-trapaça forte.
- A identidade é um login anônimo do Firebase: limpar os dados do app/navegador começa um novo perfil.

## Estrutura
- `app/src/main/assets/` — o jogo inteiro (HTML/JS), PeerJS 1.5.4 e Firebase 10.14.1 (compat), tudo local
- `app/src/main/java/.../MainActivity.java` — WebView que abre o jogo
- `database.rules.json` — regras do Firebase
