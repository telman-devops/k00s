# Keycloak

https://www.keycloak.org/

helmchart https://github.com/codecentric/helm-charts/tree/master/charts/keycloak

## Установка keycloak

Подготовка манифестов

    helm repo add codecentric https://codecentric.github.io/helm-charts
    helm template kk codecentric/keycloak -f values.yaml | \
    sed '/^#/d' | \
    sed '/helm.sh\/chart/d' | \
    sed '/managed-by: Helm/d' | \
    sed '/serviceName: kk-headless/d' | \
    sed '/podManagementPolicy/d' | \
    sed '/updateStrategy/d' | \
    sed '/type: RollingUpdate/d' | \
    sed '/serviceName/d' | \
    sed '/kind: StatefulSet/c\kind: Deployment' > manifests/02-keycloak.yaml

Установка. Можно руками:

    kubectl -n keycloak apply -f manifests

Можно при помощи ArgoCD:
    
Пушим manifests/* в Git. Редактируем argo-app/{00-iam-project.yaml,01-keycloak-app.yaml}

    kubectl  apply -f argo-app

## Настройка ArgoCD

https://argo-cd.readthedocs.io/en/stable/operator-manual/user-management/keycloak/

