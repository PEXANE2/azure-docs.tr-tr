---
title: AKS yığınlarınız için sistem durumu araştırmaları ekleyin
description: Bu makalede, aks Pod 'ye bir Application Gateway ile sistem durumu araştırmalarının (hazır olma ve/veya öğrenimlilik) nasıl ekleneceği hakkında bilgi verilmektedir.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 8c8b8b0090877db7abc8fae0e44f928e8b10dcf5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84808000"
---
# <a name="add-health-probes-to-your-service"></a>Hizmetinize sistem durumu araştırmaları ekleyin
Varsayılan olarak, giriş denetleyicisi, sunulan pods 'ler için bir HTTP GET araştırması sağlayacak.
Araştırma özellikleri, [belirtilinizin için bir hazırlık veya bir araştırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) eklenerek özelleştirilebilir `deployment` / `pod` .

## <a name="with-readinessprobe-or-livenessprobe"></a>`readinessProbe`Veya ile`livenessProbe`
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
* [HttpGet eylemi](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#httpgetaction-v1-core)

> [!NOTE]
> * `readinessProbe`ve `livenessProbe` ile yapılandırıldığında desteklenir `httpGet` .
> * Pod üzerinde açığa çıkarılan bir bağlantı noktasında araştırma Şu anda desteklenmiyor.
> * `HttpHeaders`, `InitialDelaySeconds` ,, `SuccessThreshold` desteklenmez.

##  <a name="without-readinessprobe-or-livenessprobe"></a>`readinessProbe`Veya olmadan`livenessProbe`
Yukarıdaki yoklamalar sağlanmamışsa, giriş denetleyicisi, hizmetin `Path` `backend-path-prefix` ek açıklama için belirtilen veya `path` `ingress` hizmet tanımında belirtilen bir varsayımını yapar.

## <a name="default-values-for-health-probe"></a>Sistem durumu araştırması için varsayılan değerler
Hazırlık/lizlilik araştırması tarafından çıkarsanmayan tüm özellikler için varsayılan değerler ayarlanır.

| Application Gateway araştırma özelliği | Varsayılan değer |
|-|-|
| `Path` | / |
| `Host` | localhost |
| `Protocol` | HTTP |
| `Timeout` | 30 |
| `Interval` | 30 |
| `UnhealthyThreshold` | 3 |