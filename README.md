# Прием логов через MetalLB  в Kubernetes
## Архитектура
- Kubernetes
- MetalLB (Layer2)
- Service типа LoadBalancer
- Vector
- Внешний IP


## Схема 
 ```  
     Источник событий
           ↓
       X.X.X.X:514
           ↓
        MetalLB
           ↓
        kube-proxy
           ↓
        Service
           ↓
        Pod Vector
           ↓
        Backend (SIEM)
```
MetalLB в режиме Layer2 следит за Service (LoadBalancer), проверяет IP адрес в address-pool, выбирает ноду, speaker отвечает за этот IP. 
Когда пакет пришел на ноду, далее kube-proxy: перехватывает трафик, делает DNAT, балансирует между репликами. 

## Что происходит при падении ноды? 
Если нода с IP падает: 
- другой speaker начинает отвечать ARP
- IP "перезжает"
- сервис продолжает работать

Failover происходит без внешнего load balancer. 

## Установка
#### 1. MetalLB установлен в кластере
Это базовая установка из официального манифеста MetalLB. 

Должны быть: 
- controller 
- speaker

### 2. Настройка IP (пул IP), которые MetalLB может раздать (metallb-conf.yaml)
`kubectl apply -f metallb-conf.yaml`

