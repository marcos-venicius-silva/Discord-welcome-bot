# 🤖 Bot de Boas-vindas e Saída — Discord.js v14 + MongoDB

Bot profissional para Discord, totalmente configurável através de um **Painel** com botões, menus (canal/cargo) e modais — **sem precisar editar nenhum arquivo**.

## ✨ Funcionalidades

- **Boas-vindas**: mensagem em canal, DM opcional, embed 100% customizável (título, descrição, cor, banner, thumbnail, rodapé), botões de link, ping do usuário e imagem gerada dinamicamente (cartão com avatar).
- **Saída**: mensagem em canal com embed customizável e botões, ao estilo da boas-vindas.
- **Variáveis dinâmicas**: `{user}`, `{username}`, `{mention}`, `{server}`, `{membercount}`, `{date}`.
- **Painel de configuração** (`/painel`): criar, editar, excluir, visualizar e ativar/desativar cada sistema, tudo via botões, selects e modais.
- **Permissões**: apenas administradores ou cargos autorizados (configuráveis pelo próprio painel) podem usar o painel.
- **Logs de alterações**: registradas no console e, opcionalmente, em um canal de logs do servidor.
- **Backup**: exporta a configuração atual em JSON e permite restaurar a partir de um backup salvo.
- **Multi-servidor**: cada servidor tem sua própria configuração isolada no MongoDB.
- **Slash Commands** e código 100% em português.

## 📦 Requisitos

- Node.js 18 ou superior
- Uma instância MongoDB (local ou Atlas)
- Uma aplicação/bot criada no [Discord Developer Portal](https://discord.com/developers/applications)
  - Ative os **Privileged Gateway Intents**: `SERVER MEMBERS INTENT`

## 🚀 Instalação

```bash
git clone <este-repositorio>
cd discord-welcome-bot
npm install
cp .env.example .env
```

Edite o arquivo `.env` com seus dados:

```env
DISCORD_TOKEN=seu_token_aqui
CLIENT_ID=seu_client_id_aqui
MONGODB_URI=mongodb://localhost:27017/welcome-bot
DEV_GUILD_ID=            # opcional, para testar comandos instantaneamente em 1 servidor
BOT_OWNERS=               # IDs de usuários com acesso total, separados por vírgula
```

Registre os slash commands:

```bash
npm run register
```

Inicie o bot:

```bash
npm start
```

## 🖥️ Como usar

1. Convide o bot para o servidor com os escopos `bot` e `applications.commands` e as permissões necessárias (Enviar Mensagens, Inserir Links, Anexar Arquivos, Gerenciar Cargos se for usar cargos autorizados, etc.).
2. Um administrador executa `/painel`.
3. No painel principal, use os botões:
   - 🟢 **Boas-vindas** — define canal, ativa/desativa, DM, ping, imagem, embed, banner/thumbnail e botões.
   - 🔴 **Saída** — define canal, embed, banner/thumbnail e botões.
   - 🔐 **Permissões** — define cargos autorizados e o canal de logs.
   - 📋 **Ver Configuração** — mostra um resumo de tudo que está configurado.
   - 🧪 **Testar Mensagens** — envia as mensagens de boas-vindas/saída de teste para você mesmo.
   - 💾 **Backup** — exporta a configuração em JSON ou restaura um backup colado em um modal.

Todas as ações ficam registradas no console e, se configurado, em um canal de logs.

## 🖼️ Cartão de imagem de boas-vindas

O cartão de imagem usa `@napi-rs/canvas` (não exige compilação nativa). Caso a dependência não esteja disponível no ambiente, o bot simplesmente ignora a geração de imagem sem quebrar o restante do sistema.

## 🗂️ Estrutura do projeto

```
src/
├── index.js                # Ponto de entrada
├── deploy-commands.js       # Registro dos slash commands
├── config/database.js       # Conexão com MongoDB
├── models/                  # Schemas Mongoose (GuildConfig, Backup)
├── events/                  # ready, guildMemberAdd, guildMemberRemove, interactionCreate
├── commands/painel.js       # Slash command /painel
├── handlers/                # Carregadores de comandos e eventos
├── panel/                   # Telas do painel (embeds + componentes) e modais
└── utils/                   # Variáveis, embeds, imagem, permissões, logs, backup
```

## 🔐 Permissões do painel

Podem gerenciar o painel:
- Usuários com permissão de **Administrador**;
- Usuários listados em `BOT_OWNERS` no `.env`;
- Membros com algum dos **cargos autorizados** definidos em 🔐 Permissões.

## 🛠️ Solução de problemas

- **Membros não entram/saem e nada acontece**: confirme que o intent `SERVER MEMBERS INTENT` está ativado no Developer Portal e no código (`GatewayIntentBits.GuildMembers`).
- **Bot não responde a `/painel`**: rode `npm run register` novamente e aguarde alguns minutos (comandos globais podem demorar até 1h para propagar; use `DEV_GUILD_ID` para testes instantâneos).
- **Imagem de boas-vindas não aparece**: verifique se `@napi-rs/canvas` foi instalado corretamente (`npm install`) e se a URL de fundo é válida.
