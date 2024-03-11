# Auto Release Action

Gere automaticamente releases do seu projeto no GitHub usando o Auto Release para GitHub Actions.

## Como usar

Aprenda como usar o **Auto Release** [clicando aqui.](https://github.com/kempdec/AutoRelease)

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
        uses: kempdec/autorelease-action@v1
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
        uses: kempdec/autorelease-action@v1
        env:
          AutoRelease_Token: ${{ secrets.GITHUB_TOKEN }} # Este token é fornecido pelo GitHub Actions, você não precisa criar seu próprio token.
          AutoRelease_Version: ${{ github.ref_name }}
```

## Autores

- [KempDec](https://kempdec.com) - Mantedora do projeto de código aberto.
- [Vinícius Lima](https://github.com/viniciusxdl) - Desenvolvedor .NET C#.

## Licença

[MIT](https://github.com/kempdec/autorelease-action/blob/main/LICENSE.txt)