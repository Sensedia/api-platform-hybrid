<!-- TOC -->

- [Contribuindo](#contribuindo)
  - [Requisitos](#requisitos)
- [How to](#how-to)

<!-- TOC -->

# Contribuindo

## Requisitos

* Instale os seguintes pacotes: ``git`` e um editor de texto de sua preferência. Por padrão, é recomendado o uso do [VSCode](https://code.visualstudio.com).
  * O VSCode (https://code.visualstudio.com), combinado com os plugins a seguir, auxilia o processo de edição/revisão, principalmente permitindo a pré-visualização do conteúdo antes do commit, analisando a sintaxe do Markdown e gerando o sumário automático, à medida que os títulos das seções vão sendo criados/alterados.

    * Markdown-lint: https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint
    * Markdown-toc: https://marketplace.visualstudio.com/items?itemName=AlanWalk.markdown-toc
    * Markdown-all-in-one: https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one

* Configure a autenticação na sua conta do Github para utilizar o protocolo SSH ao invés do HTTP. Veja este tutorial para saber como configurar: https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account


# How to

Quando alguém do time da Sensedia quiser contribuir com melhorias neste repositório, os seguintes passos devem ser executados.

* Clone o repositório para o seu computador, com o seguinte comando.

```bash
git clone git@github.com:Sensedia/api-platform-hybrid.git
```

* Crie uma branch usando o seguinte comando: 

```bash
git checkout -b BRANCH_NAME
```

* Certifique-se de que está branch correta, usando o comando a seguir.

```bash
git branch
```

* Estará sendo utilizada a branch que estiver um '*' antes do nome.
* Faça as alterações necessárias.
* Teste suas alterações.
* Faça commit das suas alterações na branch recém criada.
* Envie os commits para o repositório remoto com o seguinte comando:

```bash
git push --set-upstream origin BRANCH_NAME
```

* Crie um Pull Request (PR) para a branch `master` do repositório. Veja este [tutorial](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork)
* Atualize o conteúdo com as sugestões dos revisores (se necessário).
* Depois de aprovado e realizado o merge do seu PR, atualize as mudanças no seu repositório local com os comandos a seguir.

```bash
git checkout master
git pull upstream master
```

* Remova a branch local após a aprovação e merge do seu PR, usando o seguinte comando:

```bash
git branch -d BRANCH_NAME
```

Referência:
* https://blog.scottlowe.org/2015/01/27/using-fork-branch-git-workflow/