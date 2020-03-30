---
title: AKS bölmelerinize sağlık sondaları ekleyin
description: Bu makalede, uygulama ağ geçidi olan AKS bölmelerine sistem sondaları (hazır olma ve/veya canlılık) ekleme hakkında bilgi verilmektedir.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 5d0543a3a43d53e462a6406312faddf37d2653c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795589"
---
# <a name="add-health-probes-to-your-service"></a>Hizmetinize Sağlık Sondaları Ekleyin
Varsayılan olarak, Giriş denetleyicisi maruz kalan bölmeler için bir HTTP GET prob sağlayacaktır.
Sonda özellikleri `deployment` / `pod` spec bir [Hazırlık veya Canlılık Prob](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) ekleyerek özelleştirilebilir.

## <a name="with-readinessprobe-or-livenessprobe"></a>Veya `readinessProbe` ile`livenessProbe`
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aspnetapp
spec:
  replicas: 3
  template:
    metadata:
      labels:
        service: site
    spec:
      containers:
      - name: aspnetapp
        image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 3
          timeoutSeconds: 1
```

Kubernetes API Başvuru:
* [Konteyner Probları](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)
* [httpGet Eylem](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.14/#httpgetaction-v1-core)

> [!NOTE]
> * `readinessProbe`ve `livenessProbe` `httpGet`'ile yapılandırıldığında desteklenir.
> * Bölmede açığa çıkan bağlantı noktası dışında bir bağlantı noktası üzerinde sondalama şu anda desteklenmez.
> * `HttpHeaders`, `InitialDelaySeconds` `SuccessThreshold` , desteklenmez.

##  <a name="without-readinessprobe-or-livenessprobe"></a>Olmadan `readinessProbe` veya`livenessProbe`
Yukarıdaki problar sağlanmamışsa, Giriş Denetleyicisi hizmetin ek `Path` açıklama `backend-path-prefix` için belirtilen `path` veya hizmet `ingress` için tanımda belirtilen üzerinde ulaşılabilir olduğunu bir varsayımda bulunur.

## <a name="default-values-for-health-probe"></a>Sistem Durumu Sondası için Varsayılan Değerler
Hazırlık/canlılık sondası tarafından çıkarılamayan herhangi bir özellik için Varsayılan değerler ayarlanır.

| Uygulama Ağ Geçidi Sondası Özelliği | Varsayılan Değer |
|-|-|
| `Path` | / |
| `Host` | localhost |
| `Protocol` | HTTP |
| `Timeout` | 30 |
| `Interval` | 30 |
| `UnhealthyThreshold` | 3 |