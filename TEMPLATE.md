# Template TypeScript monorepo

Documentação do template [gfceccon/template-test](https://github.com/gfceccon/template-test).
Este arquivo pertence ao template e é atualizado pela sincronização; o `README.md`
do projeto é seu.

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

## Atualizando a partir do template

Repositórios criados com "Use this template" recebem `packages/a`, `packages/b`
e as configurações de tooling (ESLint, Prettier, tsconfig, `.nvmrc`), mas o GitHub
não mantém nenhum vínculo depois disso. O workflow
[template-sync.yml](.github/workflows/template-sync.yml) recria esse vínculo:
toda segunda-feira (ou manualmente, em Actions → Template Sync → Run workflow) ele
abre um PR com as mudanças do template, usando a action
[actions-template-sync](https://github.com/AndreasAugustin/actions-template-sync).

### Configuração (uma vez por repositório derivado)

1. Crie um fine-grained personal access token restrito **a este repositório**,
   com as permissões `Contents`, `Pull requests` e `Workflows` em _Read and write_.
   O `GITHUB_TOKEN` padrão não serve porque o template também altera arquivos em
   `.github/workflows/`, e só um token com escopo de workflow pode fazê-lo.
2. Salve o token como secret `TEMPLATE_SYNC_TOKEN` em _Settings → Secrets and
   variables → Actions_.
3. Rode o workflow uma vez manualmente para confirmar. Se o repositório já
   estiver igual ao template, ele termina com "nothing to commit".

Sem o secret o workflow falha de propósito, com uma mensagem apontando para cá.
Se não quiser sincronizar, desative o workflow em vez de deixá-lo falhando.

### O que é sincronizado

A sincronização é um retrato completo do template: em caso de conflito, **o template
vence**. Por isso a divisão de responsabilidades precisa estar explícita no
[.templatesyncignore](.templatesyncignore).

| Dono         | Arquivos                                                                                                                                                  |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Template** | `packages/**`, `eslint.config.js`, `.prettierrc.json`, `.prettierignore`, `tsconfig.json`, `.nvmrc`, `TEMPLATE.md`, `.github/workflows/template-sync.yml` |
| **Projeto**  | `README.md`, `LICENSE`, `package.json`, `package-lock.json`, `src/`, `.gitignore`, `.github/workflows/ci.yml`                                             |

Consequências práticas:

- **Não edite `packages/a` nem `packages/b`** no derivado; use-os como dependência.
  Qualquer alteração local é revertida no próximo PR de sincronização.
- Se você personalizar um arquivo do template (por exemplo `eslint.config.js`),
  adicione-o ao `.templatesyncignore` ou revise o PR e descarte essas mudanças.
  O `.templatesyncignore` é seu e nunca é sobrescrito pelo template.
- Mudanças do template em `package.json` (scripts, devDependencies) **não** chegam
  ao derivado, porque o arquivo tem nome, repositório e dependências do projeto.
  Quando o template alterar o tooling, compare com o `package.json` do template e
  aplique à mão.
- O `package-lock.json` também é do projeto. Depois de mergear um PR que altere
  dependências de `packages/*`, rode `npm install` e commite o lockfile.
