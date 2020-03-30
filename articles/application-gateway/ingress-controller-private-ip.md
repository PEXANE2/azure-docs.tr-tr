---
title: Giriş bitiş noktası için dahili yönlendirme için özel IP adresini kullanma
description: Bu makalede, iç yönlendirme için özel IP'lerin nasıl kullanılacağı ve böylece bir küme içindeki Giriş bitiş noktasının VNet'in geri kalanına nasıl teşhir edileceklerine ilişkin bilgiler verilmektedir.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 570f28ce559ff1c1180ffaacb781b9120b1890a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795484"
---
# <a name="use-private-ip-for-internal-routing-for-an-ingress-endpoint"></a>Giriş bitiş noktası için dahili yönlendirme için özel IP'yi kullanın 

Bu özellik, özel bir IP `Virtual Network` kullanarak içindeki giriş bitiş noktasını ortaya çıkarmanızı sağlar.

## <a name="pre-requisites"></a>Ön koşullar  
Özel IP [yapılandırması](https://docs.microsoft.com/azure/application-gateway/configure-application-gateway-with-private-frontend-ip) olan Uygulama Ağ Geçidi

Giriş için Özel IP kullanmak için denetleyiciyi yapılandırmanın iki yolu vardır,

## <a name="assign-to-a-particular-ingress"></a>Belirli bir girişe atama
Özel IP üzerinde belirli bir giriş ortaya [`appgw.ingress.kubernetes.io/use-private-ip`](./ingress-controller-annotations.md#use-private-ip) çıkarmak için Giriş ek açıklama kullanın.

### <a name="usage"></a>Kullanım
```yaml
appgw.ingress.kubernetes.io/use-private-ip: "true"
```

Özel IP'si olmayan Uygulama Ağ Geçitleri `appgw.ingress.kubernetes.io/use-private-ip: "true"` için açıklamalı Ingresses göz ardı edilecektir. Bu giriş olay ve AGIC pod günlüğü belirtilecektir.

* Giriş Olayında belirtildiği gibi hata

    ```bash
    Events:
    Type     Reason       Age               From                                                                     Message
    ----     ------       ----              ----                                                                     -------
    Warning  NoPrivateIP  2m (x17 over 2m)  azure/application-gateway, prod-ingress-azure-5c9b6fcd4-bctcb  Ingress default/hello-world-ingress requires Application Gateway 
    applicationgateway3026 has a private IP address
    ```

* AGIC Günlüklerinde belirtildiği gibi hata

    ```bash
    E0730 18:57:37.914749       1 prune.go:65] Ingress default/hello-world-ingress requires Application Gateway applicationgateway3026 has a private IP address
    ```


## <a name="assign-globally"></a>Genel Olarak Atama
Durumda, gereksinim, config kullanmak, `appgw.usePrivateIP: true` `helm` Özel IP üzerinde maruz kalmak için tüm Ingresses kısıtlamaktır.

### <a name="usage"></a>Kullanım
```yaml
appgw:
    subscriptionId: <subscriptionId>
    resourceGroup: <resourceGroupName>
    name: <applicationGatewayName>
    usePrivateIP: true
```

Bu, Giriş Denetleyicisi' nin Uygulama Ağ Geçidi'ndeki ön uç dinleyicilerini yapılandırırken Özel IP'nin IP adresi yapılandırmalarını filtrelemesini sağlar.
AGIC paniğe kapılacak ve özel IP atanmazsa çökecek. `usePrivateIP: true`

> [!NOTE]
> Uygulama Ağ Geçidi v2 SKU bir Kamu IP gerektirir. Uygulama Ağ Geçidi'nin özel olmasını [`Network Security Group`](https://docs.microsoft.com/azure/virtual-network/security-overview) istiyorsanız, trafiği kısıtlamak için Uygulama Ağ Geçidi'nin alt ağına bir ekleme.
