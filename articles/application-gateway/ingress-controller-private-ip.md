---
title: Bir giriş uç noktası için iç yönlendirme için özel IP adresi kullan
description: Bu makalede, iç yönlendirme için özel IP 'Lerin nasıl kullanılacağı ve bu nedenle bir küme içindeki giriş uç noktasının, VNet 'in geri kalanına nasıl yapılacağı hakkında bilgi sağlanır.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 570f28ce559ff1c1180ffaacb781b9120b1890a2
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "73795484"
---
# <a name="use-private-ip-for-internal-routing-for-an-ingress-endpoint"></a>Bir giriş uç noktası için iç yönlendirme için özel IP kullan 

Bu özellik özel bir IP `Virtual Network` kullanarak giriş uç noktasını kullanıma sunmasına olanak tanır.

## <a name="pre-requisites"></a>Ön koşullar  
[Özel BIR IP yapılandırmasıyla](https://docs.microsoft.com/azure/application-gateway/configure-application-gateway-with-private-frontend-ip) Application Gateway

Denetleyiciyi, giriş için özel IP kullanmak üzere yapılandırmanın iki yolu vardır,

## <a name="assign-to-a-particular-ingress"></a>Belirli bir giriş için ata
Özel IP üzerinden belirli bir girişi göstermek için, giriş bölümünde ek [`appgw.ingress.kubernetes.io/use-private-ip`](./ingress-controller-annotations.md#use-private-ip) açıklama kullanın.

### <a name="usage"></a>Kullanım
```yaml
appgw.ingress.kubernetes.io/use-private-ip: "true"
```

Özel IP olmayan uygulama ağ geçitleri için, ile `appgw.ingress.kubernetes.io/use-private-ip: "true"` ek açıklama eklenmiş olarak yoksayılır. Bu, giriş olayında ve AGIC Pod günlüğünde gösterilir.

* Giriş olayında gösterildiği gibi hata

    ```bash
    Events:
    Type     Reason       Age               From                                                                     Message
    ----     ------       ----              ----                                                                     -------
    Warning  NoPrivateIP  2m (x17 over 2m)  azure/application-gateway, prod-ingress-azure-5c9b6fcd4-bctcb  Ingress default/hello-world-ingress requires Application Gateway 
    applicationgateway3026 has a private IP address
    ```

* AGIC günlüklerinde gösterildiği gibi hata

    ```bash
    E0730 18:57:37.914749       1 prune.go:65] Ingress default/hello-world-ingress requires Application Gateway applicationgateway3026 has a private IP address
    ```


## <a name="assign-globally"></a>Küresel olarak ata
Bu durumda, tüm giriş ve özel IP üzerinden gösterilmesini sınırlamak için gereksinim, config içinde `appgw.usePrivateIP: true` `helm` kullanın.

### <a name="usage"></a>Kullanım
```yaml
appgw:
    subscriptionId: <subscriptionId>
    resourceGroup: <resourceGroupName>
    name: <applicationGatewayName>
    usePrivateIP: true
```

Bu, giriş denetleyicisini, Application Gateway ön uç dinleyicilerini yapılandırırken özel bir IP için IP adresi yapılandırmalarını filtreleyecek hale getirir.
AGIC, özel IP atanmadığı takdirde `usePrivateIP: true` , ve çöker.

> [!NOTE]
> Application Gateway v2 SKU 'SU için genel bir IP gerekir. Application Gateway özel olması gerekir, trafiği kısıtlamak için Application Gateway alt [`Network Security Group`](https://docs.microsoft.com/azure/virtual-network/security-overview) ağına bağlayın.
