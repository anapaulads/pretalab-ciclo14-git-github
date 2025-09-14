# Resumo da Oficina: Git e Github - PretaLab

Este documento resume os principais conceitos e comandos apresentados na oficina de Git e Github do Ciclo Formativo Intermediário da PretaLab. 

O objetivo é fundamentar o conhecimento teórico e prático sobre essas ferramentas.

---

## Semana 1: Fundamentos

### Aula 1: Introdução ao Git

#### O que é Controle de Versão?
É um sistema que permite gerenciar e acompanhar as alterações em arquivos ao longo do tempo. Ele facilita o trabalho colaborativo, permitindo que várias pessoas trabalhem em um mesmo projeto simultaneamente, evitando perda de código.

#### O que é Git?
Git é um sistema de controle de versão distribuído, gratuito e de código aberto. Ele permite que cada desenvolvedor tenha uma cópia local (clone) do projeto e, posteriormente, junte (merge) suas alterações na versão principal remota. Foi criado em 2005 por Linus Torvalds, o mesmo criador do Linux.

#### Instalação e Configuração

1.  **Instalação**: Baixar e instalar a partir do site oficial: `https://git-scm.com/downloads`.  

2.  **Configuração Inicial**: Após a instalação, configure seu nome e e-mail.
    ```bash
    # Verifica a versão do Git instalada
    git --version

    # Configura o nome de usuário globalmente
    git config --global user.name "Seu Nome"

    # Configura o e-mail globalmente
   git config --global user.email "seuemail@email.com"
    ```

#### Repositórios Locais e Ciclo de Vida

**Inicializando um Repositório**: Para começar a versionar um projeto, use o comando `git init` dentro da pasta do projeto. Isso cria uma pasta oculta chamada `.git`, que armazena todo o histórico e as configurações.

**Ciclo de Vida dos Arquivos**: Os arquivos em um repositório Git passam por três estágios principais:
    1.  **Untracked**: Arquivo novo, ainda não rastreado pelo Git.
    2.  **Staged**: Arquivo preparado para ser salvo no histórico (commit).
    3.  **Committed**: O arquivo foi salvo de forma segura no histórico do repositório local.

   O fluxo é: `Untracked` --(git add)--> `Staged` --(git commit)--> `Committed`.

#### Comandos Essenciais
* `git status`: Mostra o estado atual dos arquivos no repositório.
* `git add <nome-do-arquivo>`: Adiciona um arquivo à área de Staging.
* `git commit -m "mensagem descritiva"`: Salva as alterações da área de Staging no histórico local.
* `git log`: Exibe o histórico de commits.
* `git diff`: Mostra as diferenças entre o estado atual e o último commit.

---

### Aula 2: Conexão com GitHub

#### O que é o GitHub?
O GitHub é uma plataforma online para hospedar códigos e arquivos usando Git. Ele facilita o trabalho colaborativo e o versionamento de projetos.

#### Chave SSH
A chave SSH é uma forma segura de autenticação para se conectar ao GitHub sem precisar digitar usuário e senha a cada vez.
* É composta por uma chave privada (que fica no seu computador) e uma chave pública (que você adiciona ao seu perfil no GitHub).
* **Gerar chave**: `ssh-keygen -t ed25519 -C "seu-email@exemplo.com"`.
* **Adicionar ao GitHub**: Vá em `Settings` > `SSH and GPG keys` > `New SSH key` e cole o conteúdo da sua chave pública (`id_ed25519.pub`).
* **Testar conexão**: `ssh -T git@github.com`.

#### Repositórios Remotos
1.  Crie um novo repositório no GitHub.
2.  Copie a URL SSH do repositório.
3.  Conecte seu repositório local ao remoto:
    ```bash
    git remote add origin git@github.com:seu-usuario/seu-repositorio.git 
    ```
4.  Envie suas alterações (commits) para o GitHub:
    ```bash
    # Na primeira vez, para estabelecer a conexão
    git push -u origin main

    # Nas próximas vezes
    git push
    ```

#### Desfazendo Mudanças
* `git restore <arquivo>`: Descarta alterações feitas em um arquivo no seu diretório de trabalho.
* `git restore --staged <arquivo>`: Tira um arquivo da área de Staging, mas mantém as modificações.
* `git reset --soft HEAD~1`: Desfaz o último commit, mas mantém as alterações na área de Staging.

---

## Semana 2: Git Avançado

### Aula 1: Branches

#### O que são Branches?
Branches (ramos) são ramificações independentes de um projet. Elas permitem desenvolver novas funcionalidades ou corrigir bugs sem afetar a linha principal de desenvolvimento (chamada de `main`).

#### Comandos de Branch
* `git branch`: Lista todas as branches locais.
* `git switch -c <nome-da-branch>`: Cria uma nova branch e já muda para ela.
* `git switch <nome-da-branch>`: Troca para uma branch existente.
* `git branch -d <nome-da-branch>`: Deleta uma branch de forma segura (apenas se já foi mesclada).

#### Integrando Alterações
* **`git merge <nome-da-branch>`**: Combina o histórico de duas branches. Cria um "commit de merge" extra para unir as histórias. É mais simples e ideal para equipes.
    * Para atualizar sua branch com as novidades da `main`: `git merge main`.
    * Para levar suas alterações para a `main`: `git checkout main` e depois `git merge minha-branch`.
* **`git rebase <nome-da-branch>`**: Reaplica os commits de uma branch "no topo" de outra, criando um histórico linear e limpo, sem commits de merge. É mais complexo e ideal para uso individual antes de compartilhar o código.

#### Fluxo de Trabalho com GitHub
* **Fork**: Cria uma cópia de um repositório remoto na sua própria conta do GitHub, permitindo que você trabalhe livremente nele.
* **Clone**: Baixa uma cópia de um repositório remoto para a sua máquina local.
* **Pull Request (PR)**: É uma solicitação feita no GitHub para que o dono do repositório original aceite as alterações que você fez no seu fork/branch.

* **`git revert <id-do-commit>`**: Desfaz um commit que já foi enviado para o repositório remoto de forma segura. Ele não apaga o histórico, mas cria um novo commit que anula as alterações do commit especificado.

---

### Aula 2: Ferramentas e Boas Práticas

#### Git Stash
* `git stash`: Salva temporariamente as alterações que você ainda não está pronto para commitar, limpando seu diretório de trabalho.
* `git stash pop`: Reaplica as últimas alterações salvas com `stash`.

#### .gitignore
É um arquivo de texto onde você lista arquivos e pastas que o Git deve ignorar (ex: arquivos de configuração, dependências) e não incluir no versionamento.

#### Tags
São marcadores usados para identificar pontos específicos no histórico, como versões de lançamento (`v1.0.0`).
* **Leve**: `git tag v1.0.0`.
* **Anotada** (com mais detalhes): `git tag -a v1.0.0 -m "Mensagem"`.
* **Enviar para o remoto**: `git push origin --tags`.

#### Alias
Permite criar atalhos para comandos longos.
```bash
# Exemplo: criar um atalho 'st' para 'status'
git config --global alias.st status]

# Uso:
git st # Equivale a 'git status'