# Прием логов через MetalLB  в Kubernetes
## Архитектура
- Kubernetes
- MetalLB (Layer2)
- Service типа LoadBalancer
- Vector
- Внешний IP


## Схема 
 ```  Источник событий
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
