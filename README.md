# template-test

Monorepo TypeScript baseado em npm workspaces, com project references do
TypeScript ligando o pacote raiz aos pacotes internos.

## Estrutura

```
.
├── src/               # pacote raiz (consome os pacotes internos)
├── packages/
│   ├── a/              # biblioteca TypeScript "a"
│   └── b/              # biblioteca TypeScript "b"
└── tsconfig.json       # referencias para packages/a e packages/b
```

Cada pacote em `packages/*` é uma biblioteca TypeScript simples (sem React/JSX),
publicada localmente via npm workspaces e consumida pelo pacote raiz através
dos nomes `a` e `b`.

## Requisitos

- Node.js conforme `.nvmrc` (usar `nvm use`)
- npm 10+

## Scripts

| Script                 | Descrição                                               |
| ---------------------- | ------------------------------------------------------- |
| `npm run build`        | Compila todos os workspaces via `tsc -b`                |
| `npm run clean`        | Limpa os outputs de build (`tsc -b --clean`)            |
| `npm run lint`         | Roda o ESLint no monorepo                               |
| `npm run format`       | Formata os arquivos com Prettier                        |
| `npm run format:check` | Verifica formatação sem alterar arquivos                |
| `npm test`             | Placeholder — nenhuma suíte de testes configurada ainda |

## CI

Existe um workflow de exemplo em `.github/workflows/ci.yml`, mas ele está
totalmente comentado (não dispara no GitHub Actions). Os comentários no
próprio arquivo explicam o passo a passo para habilitá-lo quando fizer
sentido para o projeto.

## Licença

Apache-2.0 — veja [LICENSE](./LICENSE).
