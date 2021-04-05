# Containers - Hector

## Aplicação

### Banco de Dados

```bash
docker container run -dti --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD='!Abc123' -e MYSQL_USER=lua -e MYSQL_PASSWORD=4linux -e MYSQL_DATABASE=lua mariadb
```

### Contêiner com Dependências

Iniciar o contêiner base:

```bash
docker container run -dti -v $PWD:/opt/app -p 8080:8080 --name lapis alpine
docker container exec -ti lapis sh
```

Entrar no contêiner e adicionar as dependências para o **OpenResty**:

```bash
wget 'http://openresty.org/package/admin@openresty.com-5ea678a6.rsa.pub' -O /etc/apk/keys/admin@openresty.com-5ea678a6.rsa.pub
echo "http://openresty.org/package/alpine/v3.12/main" >> /etc/apk/repositories
apk update
apk add openresty
```

Instalar as dependências do Lua:

```bash
cd /opt/app
apk add lua5.1 luarocks5.1
luarocks-5.1 build --only-deps app-0.1-1.rockspec
apk add lua5.1-dev
apk add gcc
apk add musl-dev
apk add openssl-dev
apk add mariadb-dev
luarocks-5.1 build --only-deps app-0.1-1.rockspec MYSQL_INCDIR=/usr/include/mysql
apk add git
```
