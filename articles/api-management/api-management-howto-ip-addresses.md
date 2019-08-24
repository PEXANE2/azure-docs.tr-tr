---
title: Azure API Management hizmeti 'nin IP adresleri | Microsoft Docs
description: Azure API Management hizmetinin IP adreslerini nasıl alacağınızı ve ne zaman değişeceğinizi öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/26/2019
ms.author: apimpm
ms.openlocfilehash: c54acde03f2652465ca0947d81f6c6ac4fb98934
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70014470"
---
# <a name="ip-addresses-of-azure-api-management"></a>Azure API Management IP adresleri

Bu makalede, Azure API Management hizmeti 'nin IP adreslerini alma anlatılmaktadır. Hizmet bir sanal ağda ise, IP adresleri genel veya özel olabilir.

Güvenlik duvarı kuralları oluşturmak, arka uç hizmetlerine gelen trafiği filtrelemek veya giden trafiği kısıtlamak için IP adreslerini kullanabilirsiniz.

## <a name="ip-addresses-of-api-management-service"></a>API Management hizmetinin IP adresleri

API Management hizmetiniz bir geliştirici, temel, standart veya Premium katman hizmeti ise, IP adreslerini Azure portal kaynağınızın genel bakış panosundan elde edebilirsiniz.

![API Management IP adresi](media/api-management-howto-ip-addresses/public-ip.png)

Bunları, aşağıdaki API çağrısıyla programlı bir şekilde de getirebilirsiniz:

```
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<service-name>?api-version=<api-version>
```

Genel IP adresleri yanıtın bir parçası olacaktır:

```json
{
  ...
  "properties": {
    ...
    "publicIPAddresses": [
      "13.77.143.53"
    ],
    ...
  }
  ...
}
```

[Çoklu bölgesel dağıtımlarda](api-management-howto-deploy-multi-region.md), her bölgesel dağıtımın tek BIR genel IP adresi vardır.

## <a name="ip-addresses-of-api-management-service-in-vnet"></a>VNET 'teki API Management hizmetinin IP adresleri

API Management hizmetiniz bir sanal ağın içindeyse, iki tür IP adresine sahip olur-genel ve özel.

Genel IP adresleri, bağlantı noktası `3443` üzerinde dahili iletişim için kullanılır (örneğin, Azure Resource Manager aracılığıyla). Ayrıca, API Management bir istek, herkese açık (Internet 'e yönelik) arka uca gönderildiğinde, bir genel IP adresi isteğin kaynağı olarak görünür olur.

Özel sanal IP (VIP) adresleri, ağ içinden API Management uç noktalarına, geliştirici portalına ve doğrudan API erişimi için yönetim düzlemini bağlamak için kullanılır. Ağ içinde DNS kayıtlarını ayarlamak için bunları kullanabilirsiniz.

Azure portal her iki türün adreslerini ve API çağrısının yanıtını görürsünüz:

![VNET IP adresinde API Management](media/api-management-howto-ip-addresses/vnet-ip.png)


```json
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<service-name>?api-version=<api-version>

{
  ...
  "properties": {
    ...
    "publicIPAddresses": [
      "13.85.20.170"
    ],
    "privateIPAddresses": [
      "192.168.1.5"
    ],
    ...
  },
  ...
}
```

## <a name="ip-addresses-of-consumption-tier-api-management-service"></a>Tüketim katmanının IP adresleri API Management hizmeti

API Management hizmetiniz bir tüketim katmanı hizmeti ise, ayrılmış bir IP adresi yoktur. Tüketim katmanı hizmeti, paylaşılan bir altyapıda ve belirleyici bir IP adresi olmadan çalışır. 

Trafik kısıtlama amaçları için, Azure veri merkezlerinin IP adresleri aralığını kullanabilirsiniz. Kesin adımlar için [Azure işlevleri belge makalesine](../azure-functions/ip-addresses.md#data-center-outbound-ip-addresses) başvurun.

## <a name="changes-to-the-ip-addresses"></a>IP adreslerinde yapılan değişiklikler

API Management geliştirici, temel, standart ve Premium katmanlarında genel IP adresleri (VIP), bir hizmetin kullanım ömrü için aşağıdaki özel durumlarla statiktir:

* Hizmet silinir ve sonra yeniden oluşturulur.
* Hizmet aboneliği [askıya alındı](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) veya [Uyarı](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) verebilir (örneğin, ödeme dışı) ve ardından yeniden belirtilir.
* Azure sanal ağı, hizmetine eklenir veya hizmete kaldırılır.

[Çoklu bölgesel dağıtımlarda](api-management-howto-deploy-multi-region.md), bölge yeniden BELIRTILMIŞSE bölgesel IP adresi değişir.
