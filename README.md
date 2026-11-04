# Auto Release Action

Gere as releases do seu projeto no GitHub a partir das mensagens de commit, com o
[Auto Release](https://github.com/kempdec/AutoRelease): a versão sai dos tipos dos commits, e as notas, das descrições.

A documentação completa (como escrever os commits, a configuração e a conferência das mensagens) está no
[README do Auto Release](https://github.com/kempdec/AutoRelease#readme).

## Como usar

```yml
# .github/workflows/release.yml
name: Criar release

on:
  push:
    branches:
      - main

jobs:
  release:
    name: Criar release
    runs-on: ubuntu-latest
    permissions:
      contents: write # Criar a tag e a release; nada além disso.

    steps:
      - name: Criar release
        uses: kempdec/autorelease-action@v3
        env:
          AutoRelease_Token: ${{ secrets.GITHUB_TOKEN }}
```

A release sai do commit do push, com os commits desde a última tag de versão. Se algo falhar (o token recusado, a
release que já existe), o passo falha e diz o que houve e o que fazer.

### Ver a release antes de criar

```yml
      - name: Ver a release que seria criada
        uses: kempdec/autorelease-action@v3
        with:
          arguments: --dry-run
        env:
          AutoRelease_Token: ${{ secrets.GITHUB_TOKEN }}
```

### Criar a release de uma tag

```yml
on:
  push:
    tags:
      - "v*"

# ...
        env:
          AutoRelease_Token: ${{ secrets.GITHUB_TOKEN }}
          AutoRelease_Version: ${{ github.ref_name }}
```

### Conferir as mensagens de um pull request

```yml
on:
  pull_request:

jobs:
  commits:
    runs-on: ubuntu-latest
    steps:
      - name: Conferir as mensagens de commit
        uses: kempdec/autorelease-action@v3
        with:
          command: lint
          arguments: --from ${{ github.event.pull_request.base.sha }} --to ${{ github.event.pull_request.head.sha }}
```

### Usar a saída

```yml
      - name: Criar release
        id: release
        uses: kempdec/autorelease-action@v3
        env:
          AutoRelease_Token: ${{ secrets.GITHUB_TOKEN }}
          AutoRelease_CreateCommand__OutputType: Version

      - name: Gerar o pacote
        run: dotnet pack -p:Version=${{ steps.release.outputs.output }}
```

## Entradas

| Entrada | Padrão | O que faz |
| ------- | ------ | --------- |
| `version` | `2.*` | A versão do Auto Release. |
| `command` | `create` | O comando: `create`, `note`, `version` ou `lint`. |
| `arguments` | nenhum | Os argumentos extras do comando, separados por espaço, como `--dry-run`. |
| `skip-need-steps` | `false` | Pula o checkout e a instalação do .NET, quando o seu fluxo já faz as duas. |

## Saída

| Saída | O que é |
| ----- | ------- |
| `output` | O que o comando escreveu: o identificador da release (padrão), a versão, as notas ou o que `OutputType` pede. |

## Versões

| Ação | Auto Release | .NET |
| ---- | ------------ | ---- |
| `@v3` | 2.x | 10 |
| `@v2` | 0.x | 8 (sem suporte desde 10/11/2026) |

Na `@v2`, o passo passava como se tivesse dado certo mesmo quando o Auto Release falhava. Para migrar, troque `@v2`
por `@v3` e `permissions: write-all` por `permissions: contents: write`; a configuração continua valendo. O que mudou
está em [Migrando da versão 1](https://github.com/kempdec/AutoRelease#migrando-da-versão-1).

## Autores

- [KempDec](https://kempdec.com), mantenedora do projeto de código aberto.
- [Vinícius Lima](https://github.com/viniciusxdl).

## Licença

[MIT](LICENSE.TXT)
