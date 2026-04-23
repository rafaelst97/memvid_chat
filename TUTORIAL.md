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