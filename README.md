# Jogo da Velha Online (Android)

Jogo da velha multiplayer: um cria a sala, o outro entra com o código de 5 caracteres.
Conexão direta entre os celulares via WebRTC (PeerJS), sem servidor próprio.

## Baixar o APK
Aba **Actions** → último run de "Build APK" → artefato `jogo-da-velha-online-apk` (zip com o `app-debug.apk`).
Mande o `.apk` pro amigo e instale permitindo "fontes desconhecidas".

## Estrutura
- `app/src/main/assets/index.html` — o jogo inteiro (HTML/JS)
- `app/src/main/assets/peerjs.min.js` — PeerJS 1.5.4
- `app/src/main/java/.../MainActivity.java` — WebView que abre o jogo

## Sem GitHub Actions
- **Codemagic (grátis):** conecte este repositório em codemagic.io; o `codemagic.yaml` já compila o APK.
- **PWA:** a pasta `app/src/main/assets` é um site completo (manifest + service worker). Hospede em qualquer lugar (Netlify Drop, Cloudflare Pages…) e use "Adicionar à tela inicial".
