---
title: AKS yığınlarınız için sistem durumu araştırmaları ekleyin
description: Bu makalede, aks Pod 'ye bir Application Gateway ile sistem durumu araştırmalarının (hazır olma ve/veya öğrenimlilik) nasıl ekleneceği hakkında bilgi verilmektedir.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 5d0543a3a43d53e462a6406312faddf37d2653c6
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "73795589"
---
# <a name="add-health-probes-to-your-service"></a>Hizmetinize sistem durumu araştırmaları ekleyin
Varsayılan olarak, giriş denetleyicisi, sunulan pods 'ler için bir HTTP GET araştırması sağlayacak.
Araştırma özellikleri `deployment` / `pod` , [belirtilinizin için bir hazırlık veya bir araştırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) eklenerek özelleştirilebilir.

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

Kubernetes API başvurusu:
* [Kapsayıcı araştırmaları](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)
* [HttpGet eylemi](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.14/#httpgetaction-v1-core)

> [!NOTE]
> * `readinessProbe`ve `livenessProbe` ile `httpGet`yapılandırıldığında desteklenir.
> * Pod üzerinde açığa çıkarılan bir bağlantı noktasında araştırma Şu anda desteklenmiyor.
> * `HttpHeaders`, `InitialDelaySeconds`, `SuccessThreshold` , desteklenmez.

##  <a name="without-readinessprobe-or-livenessprobe"></a>Veya `readinessProbe` olmadan`livenessProbe`
Yukarıdaki yoklamalar sağlanmamışsa, giriş denetleyicisi, hizmetin `Path` ek açıklama için `backend-path-prefix` belirtilen veya hizmet `path` `ingress` tanımında belirtilen bir varsayımını yapar.

## <a name="default-values-for-health-probe"></a>Sistem durumu araştırması için varsayılan değerler
Hazırlık/lizlilik araştırması tarafından çıkarsanmayan tüm özellikler için varsayılan değerler ayarlanır.

| Application Gateway araştırma özelliği | Varsayılan Değer |
|-|-|
| `Path` | / |
| `Host` | localhost |
| `Protocol` | HTTP |
| `Timeout` | 30 |
| `Interval` | 30 |
| `UnhealthyThreshold` | 3 |