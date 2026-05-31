# Deploy na Vercel 🚀

Há duas versões prontas para publicar. Escolha uma (ou publique as duas em projetos separados).

> Pré-requisito: `npm i -g vercel` e `vercel login` (uma vez).

---

## Opção A — HTML estático (mais rápido, sem build)

A pasta `deploy-static/` já tem o app num único `index.html`.

```bash
cd deploy-static
vercel        # primeira vez: responde às perguntas e cria o projeto
vercel --prod # publica em produção
```

Pronto. A Vercel serve o arquivo estático na hora. Nada de build.

---

## Opção B — App React (Vite)

Da raiz do projeto (onde está o `vercel.json` e o `package.json`):

```bash
vercel        # detecta o framework Vite automaticamente
vercel --prod
```

O `vercel.json` já define:
- `buildCommand`: `npm run build`
- `outputDirectory`: `dist`
- rewrite de SPA (todas as rotas → `index.html`)

### Variáveis de ambiente (React)

O Supabase já vem embutido via `.env`, mas em produção o ideal é cadastrar as chaves no painel da Vercel (Project → Settings → Environment Variables), ou por CLI:

```bash
vercel env add VITE_SUPABASE_URL
vercel env add VITE_SUPABASE_ANON_KEY
# opcionais:
vercel env add VITE_GOOGLE_MAPS_API_KEY
vercel env add VITE_FIREBASE_API_KEY
# ... demais VITE_FIREBASE_* e VITE_FIREBASE_VAPID_KEY
```

A chave `anon` do Supabase é pública por design (protegida por RLS), então pode ir para o build do cliente sem problema.

---

## Git integration (deploy automático)

Se preferir, conecte o repositório ao projeto na Vercel: cada `git push` na branch principal dispara um deploy. Nesse caso não precisa rodar `vercel` manualmente.

---

## Depois do deploy

- **Auth/OAuth**: no Supabase, em Authentication → URL Configuration, adicione a URL da Vercel (ex.: `https://primepet.vercel.app`) em *Site URL* e *Redirect URLs*, senão o login social falha.
- **Domínio**: Project → Settings → Domains para apontar um domínio próprio.
