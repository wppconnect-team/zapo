[English](./README.md) | **Português (Brasil)**

<p align="center">
  <a href="https://wppconnect.io/">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://wppconnect-team.github.io/brand/wppconnect-dark.png" />
      <img src="https://wppconnect-team.github.io/brand/wppconnect-light.png" alt="WPPConnect" width="220" height="121" />
    </picture>
  </a>
</p>

# Zapo

Desenvolva integrações do protocolo WhatsApp Web com um cliente socket em TypeScript.

Um fork da WPPConnect Team no ecossistema WPPConnect. Os créditos e a licença do projeto original são preservados.

[Primeiros passos](#primeiros-passos) | [Documentação](#documentação) | [Como contribuir](#como-contribuir) | [Apoie a WPPConnect](#apoie-o-projeto)

[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](./LICENSE) [![npm version](https://img.shields.io/npm/v/zapo-js)](https://www.npmjs.com/package/zapo-js) [![CI](https://github.com/wppconnect-team/zapo/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/wppconnect-team/zapo/actions/workflows/ci.yml) [![Open Collective](https://img.shields.io/badge/Open%20Collective-WPPConnect-blue)](https://opencollective.com/wppconnect)

## Visão geral

Para desenvolvedores que precisam de controle explícito de sessões, armazenamento e transporte. Este é o fork de integração da WPPConnect Team de [vinikjkkj/zapo](https://github.com/vinikjkkj/zapo). O pacote principal é `zapo-js`; módulos opcionais são publicados como `@zapo-js/*`. As versões npm e zapo.to são recursos do upstream e podem diferir deste fork.

## Funcionalidades

- Cliente principal, eventos tipados, mensagens, grupos, app-state e primitivas do protocolo.
- Stores opcionais SQLite, Redis, PostgreSQL, MySQL e MongoDB.
- Ferramentas opcionais de mídia, acelerador criptográfico nativo, chamadas, servidor de teste e servidor MCP de desenvolvimento.

## Primeiros passos

Exige Node.js **>=20.9.0** e npm **10** para desenvolver pelos fontes; os peers opcionais podem ter requisitos adicionais. O fluxo pelos pacotes upstream é:

```sh
npm install zapo-js @zapo-js/store-sqlite better-sqlite3 qrcode-terminal
```

Salve como `index.mjs`, execute `node index.mjs` e leia o QR no terminal em Aparelhos conectados do WhatsApp. Um contato de teste autorizado pode enviar `ping` para receber `pong`. O exemplo persiste todos os domínios obrigatórios de autenticação/Signal. Mantenha o banco `.auth` privado.

```javascript
import qrcode from 'qrcode-terminal'
import { ConsoleLogger, createStore, WaClient } from 'zapo-js'
import { createSqliteStore } from '@zapo-js/store-sqlite'

const store = createStore({
    backends: { sqlite: createSqliteStore({ path: '.auth/state.sqlite' }) },
    providers: {
        auth: 'sqlite',
        signal: 'sqlite',
        preKey: 'sqlite',
        session: 'sqlite',
        identity: 'sqlite',
        senderKey: 'sqlite',
        appState: 'sqlite',
        privacyToken: 'sqlite',
        messages: 'sqlite',
        threads: 'sqlite',
        contacts: 'sqlite'
    }
})
const client = new WaClient({ store, sessionId: 'default' }, new ConsoleLogger('info'))
client.on('auth_qr', ({ qr }) => qrcode.generate(qr, { small: true }))
client.on('message', async (event) => {
    if (event.message?.conversation === 'ping') {
        await client.message.send(event.key.remoteJid, 'pong')
    }
})
await client.connect()
```

Para exercitar este fork em vez das versões npm do upstream:

```sh
git clone https://github.com/wppconnect-team/zapo.git
cd zapo
npm ci
npm run build
npm run example
```

## Configuração

[examples/example.ts](examples/example.ts) oferece o fluxo executável. Cada módulo de armazenamento/mídia/chamadas documenta seus peers em [packages](packages/). O processamento de mídia pode exigir `sharp`, `ffmpeg` e `ffprobe`. `ZAPO_NATIVE_BACKEND=js` desativa o acelerador nativo opcional; o acelerador publicado é WASM, enquanto NAPI exige build local. `WaClient` não reconecta automaticamente: trate explicitamente os eventos de encerramento. O servidor MCP é para desenvolvimento, não para produção. O upstream documenta SemVer a partir de 1.0.0; revise as diferenças do fork ao fixar uma revisão.

## Documentação

- [API e guias do upstream](https://zapo.to/)
- [Fontes da documentação](https://github.com/vinikjkkj/zapo-docs)
- [Pacotes opcionais](./packages/)
- [Detalhes técnicos e agradecimentos preservados](documentation/readme-reference.md)

## Ecossistema WPPConnect

| Projeto                                                     | Finalidade                                                 | Quando usar                                      |
| ----------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------ |
| [WPPConnect](https://github.com/wppconnect-team/wppconnect) | Controle o WhatsApp Web em aplicações Node.js.             | Incorporar automação a uma aplicação Node.js.    |
| [Baileys](https://github.com/wppconnect-team/baileys)       | Use o protocolo WhatsApp Web em TypeScript por WebSockets. | Avaliar um fork de cliente WebSocket TypeScript. |

## Apoie o projeto

Apoie o desenvolvimento original pelo [GitHub Sponsors de vinikjkkj](https://github.com/sponsors/vinikjkkj), preservado de [.github/FUNDING.yml](.github/FUNDING.yml). O apoio à WPPConnect abaixo financia separadamente a manutenção deste fork no ecossistema.

A WPPConnect é mantida por uma comunidade de código aberto. Seu apoio ajuda a financiar a manutenção, a infraestrutura, a documentação e o trabalho das pessoas que desenvolvem o ecossistema.

Se a WPPConnect ajuda seu projeto ou sua empresa, considere fazer uma contribuição pontual ou apoiar de forma recorrente.

**[Apoie a WPPConnect no Open Collective](https://opencollective.com/wppconnect)**

As contribuições são voluntárias. Consulte a página oficial para acompanhar as informações financeiras atuais. Você também pode ajudar com código, testes, documentação, traduções e relatos de problemas bem descritos.

## Como contribuir

Contribuições de código, testes, exemplos, documentação e traduções são bem-vindas. Para mudanças maiores, discuta a proposta em uma issue primeiro. Ao relatar um problema, informe as versões do projeto/runtime, uma reprodução mínima e logs sem dados sensíveis. Atualize `README.md` e `README.pt-BR.md` na mesma alteração quando mudar instalação, configuração, funcionalidades, exemplos, licença ou arrecadação.

Leia [CONTRIBUTING.md](CONTRIBUTING.md).

Leia [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

Leia [AGENTS.md](AGENTS.md).

Após instalar as dependências de desenvolvimento, use as verificações pertinentes à sua alteração:

```sh
npm run lint
npm run format:check
npm test
npm run typecheck:all
```

Exemplos e testes que dependem de conta exigem validação separada com uma conta de teste autorizada. Nunca anexe credenciais ou arquivos de sessão a uma issue.

## Comunidade e suporte

Use as [issues do repositório](https://github.com/wppconnect-team/zapo/issues) para bugs reproduzíveis e propostas de funcionalidades. Para dúvidas de uso, participe pelos canais oficiais:

- [Discord](https://discord.gg/JU5JGGKGNG)
- [Telegram](https://t.me/wppconnect)
- [YouTube](https://www.youtube.com/c/wppconnect)

Use o [repositório de documentação do upstream](https://github.com/vinikjkkj/zapo-docs) para corrigir os guias. Siga [SECURITY.md](SECURITY.md) em particular para vulnerabilidades de criptografia/autenticação e o [Código de Conduta](CODE_OF_CONDUCT.md) para contribuições assistidas por IA.

Este projeto é independente e não é afiliado nem endossado pelo WhatsApp ou pela Meta. Siga os termos aplicáveis e obtenha autorização para operações de conta e mensagens.

## Licença

[MIT License (MIT)](./LICENSE).

Copyright (c) 2026 vinikjkkj <contact@vinicius.email>.

O documento jurídico original e os avisos de autoria existentes são preservados. Componentes de terceiros mantêm seus próprios termos.
