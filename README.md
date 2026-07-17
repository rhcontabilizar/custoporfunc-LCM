# Avaliação de Desempenho · Contabilizare

Sistema interno de avaliação de desempenho por competências, conforme
**POL RH 0001**, **POP RH 0001**, **MAN RH 0001** e o Formulário de Avaliação
por Competências.

Responsável: Supervisão de RH.

---

## Como as peças se encaixam

```
GitHub            guarda os arquivos e o histórico de versões
   ↓ (publica sozinho a cada alteração)
Vercel            entrega o endereço na internet
   ↓ (o sistema conversa com o banco pelo navegador)
Supabase          guarda os dados que todos compartilham
```

Não existe programa para instalar. O sistema é **um arquivo HTML só**
(`index.html`), com tudo dentro: telas, cálculos, logo e relatórios.

---

## As pastas

```
avaliacao-desempenho-contabilizare/
│
├── index.html          O sistema inteiro. É este arquivo que abre no navegador.
│
├── vercel.json         Regras do Vercel: esconde o sistema de buscadores,
│                       bloqueia que ele seja aberto dentro de outro site e
│                       garante que todo mundo receba sempre a versão nova.
│
├── robots.txt          Reforça o pedido para buscadores não indexarem.
│
├── .gitignore          Impede que cópias de segurança (com dados de
│                       avaliação) subam para o GitHub por engano.
│
├── README.md           Este arquivo.
│
└── supabase/
    └── schema.sql      Script do banco. Roda uma vez, no começo.
```

---

## Publicação — passo a passo

### 1. Banco de dados (Supabase) — 5 minutos

1. Entre em `supabase.com` e crie um projeto.
   Guarde a senha do banco num lugar seguro; ela não é usada no sistema,
   mas o Supabase pede se você precisar de suporte.
2. Menu lateral → **SQL Editor** → **New query**.
3. Abra o arquivo `supabase/schema.sql`, copie tudo, cole e clique em **Run**.
   No fim deve aparecer uma linha chamada `principal`. É o sinal de que deu certo.
4. Menu lateral → **Project Settings** → **API**. Anote dois valores:
   - **Project URL** — algo como `https://abcdefgh.supabase.co`
   - **anon public** — uma chave longa começando com `eyJ...`

### 2. Repositório (GitHub)

1. Crie um repositório novo. Sugestão de nome: `avaliacao-desempenho-contabilizare`.
2. **Deixe como privado.** O Vercel consegue publicar de repositório privado
   sem custo — é o GitHub Pages que não deixa, e não vamos usar ele.
3. Suba esta pasta inteira. Pelo site: **Add file → Upload files**, arraste
   os arquivos e a pasta `supabase`, e confirme em **Commit changes**.

### 3. Endereço (Vercel)

1. Entre em `vercel.com` com a sua conta do GitHub.
2. **Add New → Project** → escolha o repositório.
3. Não mexa em nada nas opções: é site estático, o Vercel reconhece sozinho.
   Framework Preset deve ficar em **Other**.
4. **Deploy**. Em menos de um minuto sai o endereço, algo como
   `avaliacao-desempenho-contabilizare.vercel.app`.

### 4. Ligar o sistema ao banco

1. Abra o endereço e entre com `eloisa` / `rh2026`.
2. **Configurações → Nuvem e dados**.
3. Cole a **Project URL** e a **anon public** que você anotou no passo 1.
4. Tabela: `avaliacao_desempenho`. Situação: **Ligada**.
5. **Salvar e conectar** e depois **Testar conexão**.
6. No topo da tela, o indicador deve mudar de "local" para **"na nuvem"**.

### 5. Primeiras providências no RH

1. Troque a sua senha e a de todos, em **Configurações → Usuários e acessos**.
   As senhas que vieram de fábrica (`rh2026`, `ger2026`, `coord2026`, `ceo2026`)
   são provisórias.
2. Corrija o nome do CEO e os sobrenomes dos gestores.
3. Confira o mapa de avaliação, no fim da mesma tela.
4. Confirme o ciclo em **Configurações → Ciclo avaliativo**.

---

## Como atualizar o sistema depois

Toda vez que o `index.html` mudar no GitHub, o Vercel republica sozinho
em segundos. Não existe botão de publicar.

Pelo site do GitHub: abra o `index.html` → ícone de lápis → cole a versão
nova → **Commit changes**. Pronto.

Peça para as pessoas darem **Ctrl + Shift + R** na primeira vez, para o
navegador buscar a versão nova.

---

## Cópia de segurança

O sistema tem **Configurações → Nuvem e dados → Baixar cópia (.json)**.

Recomendação: baixar ao fim de cada ciclo trimestral, depois de homologar
tudo, e guardar na pasta do RH. Esse arquivo tem as avaliações de todo mundo —
**não suba ele para o GitHub** (o `.gitignore` já bloqueia, mas vale saber por quê).

O Supabase também faz cópia automática do banco no plano gratuito.

---

## O que este arranjo protege — e o que não protege

**Protege:**

- A chave pública do Supabase só consegue ler, gravar e atualizar a linha
  `principal`. Não consegue apagar nada nem criar outras linhas.
- Buscadores não indexam o endereço.
- O sistema não pode ser aberto dentro de outro site.
- As senhas separam o que cada pessoa enxerga, conforme a hierarquia.

**Não protege:**

- O endereço do Vercel é público. Quem chegar nele vê a tela de login e,
  no código-fonte da página, a chave do Supabase. Com essa chave, dá para
  ler os dados sem passar pela tela de login.
- O plano gratuito do Vercel (Hobby) é para uso pessoal, não comercial.
  Para uso do escritório, o plano previsto nos termos é o Pro.

**Para fechar isso**, existem dois caminhos:

1. **Vercel Pro** (US$ 20/mês): libera uso comercial e permite proteger o
   endereço de produção com login antes de a página carregar.
2. **Migrar o login para o Supabase Auth**: as senhas passam a ser validadas
   pelo próprio banco, e as políticas do `schema.sql` trocam `true` por
   `auth.role() = 'authenticated'`. Aí a chave sozinha não abre nada.
   Resolve de graça, mas exige alteração no sistema.

Os dados de avaliação são confidenciais pelo item 12 do MAN RH 0001 —
por isso o assunto está registrado aqui em vez de ficar só no e-mail.

---

## Sinais de que algo não está certo

| O que você vê | O que costuma ser |
|---|---|
| Indicador no topo diz "local" | Nuvem desligada em Configurações, ou URL/chave em branco |
| Indicador diz "erro de envio" | URL ou chave errada, ou o `schema.sql` não foi rodado |
| Gestor não vê alguém que deveria | Escopo de setores dele em Configurações → Usuários |
| Coordenador não consegue avaliar | O campo "Avaliado por" da pessoa aponta para outro |
| Alguém não consegue entrar | Sem senha definida, ou usuário marcado como inativo |
| Mudei o arquivo e não aparece | Ctrl + Shift + R; se persistir, veja se o Vercel republicou |
