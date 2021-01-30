---
title: Azure Red Hat OpenShift 4 için DNS Iletmeyi yapılandırma
description: Azure Red Hat OpenShift 4 için DNS Iletmeyi yapılandırma
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: aa18959d0dc78dedf8b57dc120ab6744afa9051f
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99070602"
---
# <a name="configure-dns-forwarding-on-an-azure-red-hat-openshift-4-cluster"></a>Azure Red Hat OpenShift 4 kümesinde DNS iletmeyi yapılandırma

Azure Red Hat OpenShift kümesinde DNS Iletmeyi yapılandırmak için DNS işlecini değiştirmeniz gerekir. Bu değişiklik, küme içinde çalışan uygulama yığınlarınızın, küme dışındaki özel bir DNS sunucusunda barındırılan adları çözümlemesine izin verir. Bu adımlar, OpenShift 4,6 için [burada](https://docs.openshift.com/container-platform/4.6/networking/dns-operator.html)belgelenmiştir.

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
Openshıft 4,6 için DNS iletimi hakkında daha fazla bilgi için [buraya](https://docs.openshift.com/container-platform/4.6/networking/dns-operator.html)bakın.