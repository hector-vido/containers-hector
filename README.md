# Containers - Hector

## Aplicação

### Banco de Dados

```bash
docker container run -dti --name mysql -p 3306:3306 \
-e MYSQL_ROOT_PASSWORD='!Abc123' \
-e MYSQL_USER=lua \
-e MYSQL_PASSWORD=4linux \
-e MYSQL_DATABASE=lua mariadb
```

### Contêiner com Dependências

Iniciar o contêiner base da aplicação:

```bash
docker container run -dti -v $PWD:/opt/app -p 8080:8080 --name lapis alpine
docker container exec -ti lapis sh
```

Entrar no contêiner e adicionar as dependências para o **OpenResty**:

```bash
KEY='openresty.com-5ea678a6.rsa.pub'
wget "http://openresty.org/package/$KEY" -O "/etc/apk/keys/$KEY"
echo "http://openresty.org/package/alpine/v3.12/main" >> /etc/apk/repositories
apk update
apk add openresty
```

Instalar as dependências do Lua, para cada passo após o build rodar novamente `luarocks-5.1` para acompanhar os erros:

```bash
cd /opt/app
apk add lua5.1 luarocks5.1
luarocks-5.1 build --only-deps app-0.1-1.rockspec
# ops...
apk add lua5.1-dev
apk add gcc
apk add musl-dev
apk add openssl-dev
apk add mariadb-dev
luarocks-5.1 build --only-deps app-0.1-1.rockspec MYSQL_INCDIR=/usr/include/mysql
# ops...
apk add git
```

Preparar a aplicação e iniciar o servidor:

```bash
lapis new --lua
lapis server
```

## Testar a Aplicação

Expor uma porta via "play with docker" e fazer o login na aplicação com uma destas opções:

| E-mail                    | Senha |
|---------------------------|-------|
| paramahansa@yogananda.in  | 123   |
| victor@frankenstein.co.uk | 123   |

Ao fazer o login, um erro aparecerá, será necessário rodar a **migration**.

## Migration - Iniciar o Banco

Para a aplicação com `CTRL + C` e executar no diretório `/opt/app`:

```bash
lapis migrate
```

Testar a aplicação novamente.
