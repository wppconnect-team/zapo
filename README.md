**English** | [Português (Brasil)](./README.pt-BR.md)

<p align="center">
  <a href="https://wppconnect.io/">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://wppconnect-team.github.io/brand/wppconnect-dark.png" />
      <img src="https://wppconnect-team.github.io/brand/wppconnect-light.png" alt="WPPConnect" width="220" height="121" />
    </picture>
  </a>
</p>

# Zapo

Build WhatsApp Web protocol integrations with a TypeScript socket client.

A WPPConnect Team fork in the WPPConnect ecosystem. Original project credits and license are preserved.

[Getting started](#getting-started) | [Documentation](#documentation) | [Contributing](#contributing) | [Support WPPConnect](#support-the-project)

[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](./LICENSE) [![npm version](https://img.shields.io/npm/v/zapo-js)](https://www.npmjs.com/package/zapo-js) [![CI](https://github.com/wppconnect-team/zapo/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/wppconnect-team/zapo/actions/workflows/ci.yml) [![Open Collective](https://img.shields.io/badge/Open%20Collective-WPPConnect-blue)](https://opencollective.com/wppconnect)

## Overview

For developers who need explicit session, storage and transport control. This is the WPPConnect Team integration fork of [vinikjkkj/zapo](https://github.com/vinikjkkj/zapo). The core package is `zapo-js`; optional modules are published as `@zapo-js/*`. npm releases and zapo.to are upstream resources and may differ from this fork.

## Features

- Core client, typed events, messages, groups, app-state and protocol primitives.
- Optional SQLite, Redis, PostgreSQL, MySQL and MongoDB stores.
- Optional media tools, native crypto accelerator, calls, test server and development MCP server.

## Getting started

Requires Node.js **>=20.9.0** and npm **10** for source development; optional peers may have additional requirements. The upstream package flow is:

```sh
npm install zapo-js @zapo-js/store-sqlite better-sqlite3 qrcode-terminal
```

Save as `index.mjs`, run `node index.mjs` and scan the terminal QR from WhatsApp Linked devices. An authorized test contact can send `ping` to receive `pong`. The example persists all required auth/Signal domains. Keep the `.auth` database private.

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

To exercise this fork instead of upstream npm releases:

```sh
git clone https://github.com/wppconnect-team/zapo.git
cd zapo
npm ci
npm run build
npm run example
```

## Configuration

[examples/example.ts](examples/example.ts) provides the runnable flow. Each store/media/call module documents its own peers in [packages](packages/). Media processing may need `sharp`, `ffmpeg` and `ffprobe`. `ZAPO_NATIVE_BACKEND=js` disables the optional native accelerator; the published accelerator is WASM, while NAPI needs a local build. `WaClient` has no automatic reconnect: handle connection-close events explicitly. The MCP server is for development, not production. Upstream documents SemVer from 1.0.0; review fork diffs when pinning a revision.

## Documentation

- [Upstream API and guides](https://zapo.to/)
- [Documentation source](https://github.com/vinikjkkj/zapo-docs)
- [Optional packages](./packages/)
- [Retained technical details and acknowledgments](documentation/readme-reference.md)

## WPPConnect ecosystem

| Project                                                     | Purpose                                                        | When to use it                               |
| ----------------------------------------------------------- | -------------------------------------------------------------- | -------------------------------------------- |
| [WPPConnect](https://github.com/wppconnect-team/wppconnect) | Control WhatsApp Web from Node.js applications.                | Embed automation in a Node.js application.   |
| [Baileys](https://github.com/wppconnect-team/baileys)       | Use the WhatsApp Web protocol from TypeScript over WebSockets. | Evaluate a TypeScript WebSocket client fork. |

## Support the project

Support original development through [vinikjkkj on GitHub Sponsors](https://github.com/sponsors/vinikjkkj), preserved from [.github/FUNDING.yml](.github/FUNDING.yml). Support for WPPConnect below funds this fork's ecosystem maintenance separately.

WPPConnect is maintained by an open-source community. Your support helps fund maintenance, infrastructure, documentation, and the people working on the ecosystem.

If WPPConnect helps your project or business, consider making a one-time contribution or becoming a recurring supporter.

**[Support WPPConnect on Open Collective](https://opencollective.com/wppconnect)**

Contributions are voluntary. See the official page for current financial information. You can also help with code, tests, documentation, translations and well-described issue reports.

## Contributing

Contributions of code, tests, examples, documentation and translations are welcome. For larger changes, discuss the approach in an issue first. Include the project/runtime versions, a minimal reproduction and sanitized logs when reporting a problem. Update both `README.md` and `README.pt-BR.md` in the same change when installation, configuration, features, examples, licensing or funding changes.

Read [CONTRIBUTING.md](CONTRIBUTING.md).

Read [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

Read [AGENTS.md](AGENTS.md).

After installing the development dependencies, use the repository checks relevant to your change:

```sh
npm run lint
npm run format:check
npm test
npm run typecheck:all
```

Account-dependent examples and tests require separate validation with an authorized test account. Never attach credentials or session files to an issue.

## Community and support

Use [repository issues](https://github.com/wppconnect-team/zapo/issues) for reproducible bugs and feature proposals. For usage questions, join the community through the official channels:

- [Discord](https://discord.gg/JU5JGGKGNG)
- [Telegram](https://t.me/wppconnect)
- [YouTube](https://www.youtube.com/c/wppconnect)

Use the upstream [documentation repository](https://github.com/vinikjkkj/zapo-docs) for guide fixes. Follow [SECURITY.md](SECURITY.md) privately for crypto/auth vulnerabilities and the [Code of Conduct](CODE_OF_CONDUCT.md) for AI-assisted contributions.

This project is independent and is not affiliated with or endorsed by WhatsApp or Meta. Follow applicable terms and obtain authorization for account and messaging operations.

## License

[MIT License (MIT)](./LICENSE).

Copyright (c) 2026 vinikjkkj <contact@vinicius.email>.

The original legal document and existing copyright notices are preserved. Third-party components retain their own terms.
