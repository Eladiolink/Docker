## Volumes

Sintax do *-v*:

Bind-mount
```shell
-v [pasta_local]:[pasta_no_container]
```

Volume criado pelo o docker em um lugar qualquer na nossa máquina:
```shell
-v [pasta_do_container]
```

```shell
-v nome-de-um-volume:[pasta_do_container]
```

Cria o volume direto na memoria Ram
```shell
docker container run -it --tmpfs /app ubuntu:16.04
```

Recomendação do Docker:
```shell
docker container run -it \
--mount type=bind,source=$(pwd),target=/app \
ubuntu:16.04

docker container run -it \
--mount type=tmpfs,target=/app \
ubuntu:16.04

docker container run -it \
--mount source=my-vol,target=/app \
ubuntu:16.04
```