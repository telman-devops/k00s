# Harbor

## Redis (bitnami)

Ставим системный Redis для дальнейшего использования в Harbor.

https://github.com/bitnami/charts/tree/master/bitnami/redis

Готовим [Application](argo-app/01-redis-app.yaml) для ArgoCD

## Базы в PostgreSQL

| Название           | User   | Password       |
|--------------------|--------|----------------|
| harborcore         | harbor | harborpassword |
| harborclair        | harbor | harborpassword |
| harbornotary       | harbor | harborpassword |
| harbornotarysigner | harbor | harborpassword |

```
create database harborcore;
create user harbor with password 'harborpassword';
alter database harborcore owner to harbor;
grant all privileges on database harborcore to harbor;

create database harborclair;
alter database harborclair owner to harbor;
grant all privileges on database harborclair to harbor;

create database harbornotary;
alter database harbornotary owner to harbor;
grant all privileges on database harbornotary to harbor;

create database harbornotarysigner;
alter database harbornotarysigner owner to harbor;
grant all privileges on database harbornotarysigner to harbor;

```

## Harbor ((bitnami))

Готовим [Application](bitnami/01-bitnami-harbor-helm-app.yaml) для ArgoCD

Values берем из файла [02-values.yaml](bitnami/02-values.yaml)

## Используем template

    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm template system-harbor bitnami/harbor -f custom-values.yaml > app/01-harbor.yaml
