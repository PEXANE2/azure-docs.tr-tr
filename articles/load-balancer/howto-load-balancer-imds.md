---
title: Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma
titleSuffix: Azure Load Balancer
description: Azure Instance Metadata Service kullanarak yük dengeleyici meta verilerini nasıl alacağınızı öğrenmeye başlayın.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: 9ec217cefb05929ed6f5c7395df5e68891e823ac
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101739789"
---
# <a name="retrieve-load-balancer-metadata-using-the-azure-instance-metadata-service-imds"></a>Azure Instance Metadata Service (ıMDS) kullanarak yük dengeleyici meta verilerini alma

## <a name="prerequisites"></a>Önkoşullar

* İsteğiniz için [en son API sürümünü](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#supported-api-versions) kullanın.

## <a name="sample-request-and-response"></a>Örnek istek ve yanıt
> [!IMPORTANT]
> Bu örnek proxy 'leri atlar. IDS 'yi sorgularken proxy 'leri **atmalısınız** . Daha fazla bilgi için bkz. [proxy 'ler](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#proxies).
### <a name="windows"></a>[Windows](#tab/windows/)

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254:80/metadata/loadbalancer?api-version=2020-10-01" | ConvertTo-Json
```
> [!NOTE]
> -NoProxy parametresi, PowerShell 6,0 ' de tanıtılmıştı. PowerShell 'in eski bir sürümünü kullanıyorsanız, istek gövdesinde-NoProxy 'yi kaldırın ve ıMDS bilgilerini alırken proxy kullanmadığınız emin olun. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service?tabs=windows#proxies).
> 
### <a name="linux"></a>[Linux](#tab/linux/)

```bash
curl -H "Metadata:true" --noproxy "*" "http://169.254.169.254:80/metadata/loadbalancer?api-version=2020-10-01"
```

---
### <a name="sample-response"></a>Örnek yanıt

```json
{
   "loadbalancer": {
    "publicIpAddresses":[
      {
         "frontendIpAddress":"51.0.0.1",
         "privateIpAddress":"10.1.0.4"
      }
   ],
   "inboundRules":[
      {
         "frontendIpAddress":"50.0.0.1",
         "protocol":"tcp",
         "frontendPort":80,
         "backendPort":443,
         "privateIpAddress":"10.1.0.4"
      },
      {
         "frontendIpAddress":"2603:10e1:100:2::1:1",
         "protocol":"tcp",
         "frontendPort":80,
         "backendPort":443,
         "privateIpAddress":"ace:cab:deca:deed::1"
      }
   ],
   "outboundRules":[
      {
         "frontendIpAddress":"50.0.0.1",
         "privateIpAddress":"10.1.0.4"
      },
      {
         "frotendIpAddress":"2603:10e1:100:2::1:1",
         "privateIpAddress":"ace:cab:deca:deed::1"
      }
    ]
   }
}

```

## <a name="next-steps"></a>Sonraki adımlar
[Ortak hata kodları ve sorun giderme adımları](troubleshoot-load-balancer-imds.md)

[Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md) hakkında daha fazla bilgi

[Örnek için tüm meta verileri alma](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#access-azure-instance-metadata-service)

[Standart yük dengeleyici dağıtma](quickstart-load-balancer-standard-public-portal.md)

