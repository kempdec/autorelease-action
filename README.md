# Auto Release Action

Gere automaticamente releases do seu projeto no GitHub usando o Auto Release para GitHub Actions.

## Como usar

Aprenda como usar o **Auto Release** [clicando aqui.](https://github.com/kempdec/AutoRelease)

## Entradas

As entradas estão disponíveis somente a partir da versão 2 do Auto Release Action (`kempdec/autorelease-action`).

| **Entrada**       | **Tipo**  | **Padrão** | **Descrição** |
| ----------------- | --------- | ---------- | ------------- |
| `version`         | `string`  | `0.*`      | A versão do Auto Release. |
| `skip-need-steps` | `boolean` | `false`    | Um sinalizador indicando se as etapas necessárias devem ser puladas.<br><br>Use essa opção se você deseja que `actions/checkout` e `actions/setup-dotnet` sejam executados por você e não por `kempdec/autorelease-action`. |

## Exemplos simples de uso

Criar uma release após um `push` no branch `main`.

``` yml
# .github/workflows/main.yml

name: Criar Release

on:
  push:
    branches:
      - main

jobs:
  create-release:
    name: Criar Release
    runs-on: ubuntu-latest
    permissions: write-all

    steps:
      - name: Criar Release
        uses: kempdec/autorelease-action@v2
        env:
          AutoRelease_Token: ${{ secrets.GITHUB_TOKEN }} # Este token é fornecido pelo GitHub Actions, você não precisa criar seu próprio token.
```

Criar uma release após um `push` de uma `tag` de versão.

``` yml
# .github/workflows/main.yml

name: Criar Release

on:
  push:
    tags:
      - "v*" # Tags que iniciarem com v, como v1.0.0, v2.3.5-beta.1.

jobs:
  create-release:
    name: Criar Release
    runs-on: ubuntu-latest
    permissions: write-all

    steps:
      - name: Criar Release
        uses: kempdec/autorelease-action@v2
        env:
          AutoRelease_Token: ${{ secrets.GITHUB_TOKEN }} # Este token é fornecido pelo GitHub Actions, você não precisa criar seu próprio token.
          AutoRelease_Version: ${{ github.ref_name }}
```

## Autores

- [KempDec](https://kempdec.com) - Mantedora do projeto de código aberto.
- [Vinícius Lima](https://github.com/viniciusxdl) - Desenvolvedor .NET C#.

## Licença

[MIT](https://github.com/kempdec/autorelease-action/blob/main/LICENSE.txt)