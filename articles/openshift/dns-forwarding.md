---
title: Azure Red Hat OpenShift 4 için DNS Iletmeyi yapılandırma
description: Azure Red Hat OpenShift 4 için DNS Iletmeyi yapılandırma
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 6784c69c049945f3bd4977ecd647d57ec59bf9f1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82232641"
---
# <a name="configure-dns-forwarding-on-an-azure-red-hat-openshift-4-cluster"></a>Azure Red Hat OpenShift 4 kümesinde DNS iletmeyi yapılandırma

Azure Red Hat OpenShift kümesinde DNS Iletmeyi yapılandırmak için DNS işlecini değiştirmeniz gerekir. Bu değişiklik, küme içinde çalışan uygulama yığınlarınızın, küme dışındaki özel bir DNS sunucusunda barındırılan adları çözümlemesine izin verir. Bu adımlar, OpenShift 4,3 için [burada](https://docs.openshift.com/container-platform/4.3/networking/dns-operator.html)belgelenmiştir.

Örneğin, *. example.com için tüm DNS isteklerini bir DNS sunucusu 192.168.100.10 tarafından çözümlenecek şekilde iletmek istiyorsanız, şunu çalıştırarak operatör yapılandırmasını düzenleyebilirsiniz:
 
```bash
oc edit dns.operator/default
```
 
Bu, bir düzenleyici başlatacaktır ve şu şekilde değiştirebilirsiniz `spec: {}` :
 
 ```yaml
spec:
  servers:
  - forwardPlugin:
      upstreams:
      - 192.168.100.10
    name: example-dns
    zones:
    - example.com
```

Dosyayı kaydedin ve düzenleyiciden çıkın.

## <a name="next-steps"></a>Sonraki adımlar
Openshıft 4,3 için DNS iletimi hakkında daha fazla bilgi için [buraya](https://docs.openshift.com/container-platform/4.3/networking/dns-operator.html)bakın.