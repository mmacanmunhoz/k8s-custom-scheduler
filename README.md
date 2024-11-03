## Create Logic Custom Scheduler
https://overcast.blog/creating-a-custom-scheduler-in-kubernetes-a-practical-guide-2d9f9254f3b5


## Create Kind Cluster
kind create cluster --config cluster.yaml


## Informações de Regras de Scheduler

- filter: Determinam quais nós são considerados viáveis para um pod.
- score: atribui uma pontuação ao nós 
- bind: associa o pod ao nó selecionado


```
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
  - schedulerName: custom-scheduler
    plugins:
      # Plugins de Pontuação (Score Plugins)
      score:
        enabled:
          - name: "NodeResourcesFit"    # Pontua nós com base nos recursos solicitados
          - name: "ImageLocality"       # Prioriza nós que já possuem a imagem do pod
        disabled:
          - name: "ServiceAffinity"     # Desabilita afinidade com serviços
      filter:
        enabled:
          - name: "NodeUnschedulable"   # Filtra nós que estão marcados como indisponíveis
          - name: "TaintToleration"     # Respeita as tolerâncias de taints do pod
      bind:
        enabled:
          - name: "DefaultBinder"       # Usa o vinculador padrão para associar o pod ao nó
```
