# Tutorial: Como instalar e utilizar o Memvid via Docker

O Memvid pode ser executado facilmente utilizando Docker. Existem duas imagens principais disponibilizadas: uma para a **CLI** (linha de comando) e outra para o **Core** (desenvolvimento da biblioteca Rust).

---

## 1. Utilizando a CLI via Docker

A imagem Docker da CLI permite utilizar todos os comandos do Memvid sem precisar instalar o Node.js no seu sistema.

### Pré-requisitos
- Docker instalado na sua máquina.

### Executando comandos básicos

Você pode executar o Memvid apontando o Docker para a pasta atual do seu computador, onde os arquivos `.mv2` serão criados e lidos.

Para ver a ajuda e os comandos disponíveis:
```bash
docker run --rm memvid/cli --help
```

Para criar uma memória (isso irá gerar um arquivo `minha-memoria.mv2` na pasta atual):
```bash
docker run --rm -v $(pwd):/data memvid/cli create minha-memoria.mv2
```

Para adicionar um arquivo à memória:
```bash
docker run --rm -v $(pwd):/data memvid/cli put minha-memoria.mv2 --input documento.pdf
```

Para realizar uma busca:
```bash
docker run --rm -v $(pwd):/data memvid/cli find minha-memoria.mv2 --query "termo de busca"
```

### Criando um Alias (Atalho) para facilitar
Para não precisar digitar todo o comando do Docker a cada vez, você pode criar um atalho no seu terminal (Linux/Mac). Adicione a seguinte linha ao seu arquivo `~/.bashrc` ou `~/.zshrc`:

```bash
alias memvid='docker run --rm -v $(pwd):/data -e MEMVID_API_KEY -e OPENAI_API_KEY memvid/cli'
```
Após recarregar seu terminal, você pode usar apenas o comando `memvid`:
```bash
memvid create minha-memoria.mv2
memvid put minha-memoria.mv2 --input docs/
memvid find minha-memoria.mv2 --query "ola"
```

---

## 2. Desenvolvendo o Core do Memvid via Docker

Se você deseja rodar, testar ou buildar a biblioteca Core do Memvid (feita em Rust), o projeto já possui um setup com `docker-compose`.

### Pré-requisitos
- Docker e Docker Compose instalados.

### Iniciando o ambiente de desenvolvimento

Acesse a pasta `docker/core` do projeto:
```bash
cd docker/core
```

Suba o container de desenvolvimento em segundo plano:
```bash
docker-compose up -d dev
```

Acesse o terminal do container para rodar comandos:
```bash
docker-compose exec dev bash
```

Dentro deste terminal do container, você pode rodar os comandos do Cargo normalmente:
```bash
cargo build
cargo test
cargo run --example basic_usage
```

### Executando testes e build via Docker Compose

Caso queira apenas rodar os testes sem entrar no container, na pasta `docker/core` execute:
```bash
docker-compose run --rm test
```

Para realizar a build da versão de release:
```bash
docker-compose run --rm build
```

E para parar e remover os containers após o uso:
```bash
docker-compose down
```
---

## 3. Tutorial para Windows (via PowerShell)

No Windows, a lógica para utilizar o Memvid em Docker é a mesma, com pequenas alterações nos comandos de terminal para montar os volumes usando o PowerShell.

### Pré-requisitos
- **Docker Desktop** instalado e rodando no seu Windows.

### Executando a CLI no Windows (PowerShell)

Abra o seu terminal **PowerShell** e navegue até a pasta onde deseja trabalhar.

Ao invés de `$(pwd)`, no PowerShell utilizamos `${PWD}` para referenciar o diretório atual.

Para ver a ajuda:
```powershell
docker run --rm memvid/cli --help
```

Para criar uma memória (gera o arquivo `minha-memoria.mv2` na pasta atual):
```powershell
docker run --rm -v "${PWD}:/data" memvid/cli create minha-memoria.mv2
```

Para adicionar um arquivo:
```powershell
docker run --rm -v "${PWD}:/data" memvid/cli put minha-memoria.mv2 --input documento.pdf
```

Para realizar uma busca:
```powershell
docker run --rm -v "${PWD}:/data" memvid/cli find minha-memoria.mv2 --query "termo de busca"
```

### Criando um Alias (Função) no PowerShell

Para simplificar a execução no Windows, você pode criar uma função no seu perfil do PowerShell (`$PROFILE`).

1. Abra o arquivo de perfil do PowerShell (se não existir, ele será criado):
```powershell
notepad $PROFILE
```

2. Adicione a seguinte função no final do arquivo:
```powershell
function memvid {
    $env:MEMVID_API_KEY = $env:MEMVID_API_KEY # (opcional) mantém chaves do ambiente
    $env:OPENAI_API_KEY = $env:OPENAI_API_KEY # (opcional) mantém chaves do ambiente
    docker run --rm -v "${PWD}:/data" -e MEMVID_API_KEY -e OPENAI_API_KEY memvid/cli $args
}
```

3. Salve, feche o bloco de notas e recarregue seu perfil:
```powershell
. $PROFILE
```

4. Agora você pode usar os comandos simplificados no PowerShell:
```powershell
memvid create minha-memoria.mv2
memvid put minha-memoria.mv2 --input docs/
memvid find minha-memoria.mv2 --query "ola"
```

### Desenvolvendo o Core no Windows

O processo para a biblioteca Core (`docker/core`) no Windows é idêntico ao do Linux/Mac, utilizando os mesmos comandos do `docker-compose`:

```powershell
cd docker\core
docker-compose up -d dev
docker-compose exec dev bash
```
