---
title: Azure API Yönetimi hizmetinin IP adresleri | Microsoft Dokümanlar
description: Azure API Yönetimi hizmetinin IP adreslerini nasıl ve ne zaman değiştireceğinizi öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/26/2019
ms.author: apimpm
ms.openlocfilehash: 45501fee9ae6ff47643a1ed197a07c4ba598e981
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80047739"
---
# <a name="ip-addresses-of-azure-api-management"></a>Azure API Yönetimi'nin IP adresleri

Bu makalede, Azure API Yönetimi hizmetinin IP adreslerinin nasıl alındığını açıklıyoruz. Hizmet sanal ağdaysa IP adresleri herkese açık veya özel olabilir.

Güvenlik duvarı kuralları oluşturmak, gelen trafiği arka uç hizmetlerine filtrelemek veya giden trafiği kısıtlamak için IP adreslerini kullanabilirsiniz.

## <a name="ip-addresses-of-api-management-service"></a>API Yönetimi hizmetinin IP adresleri

Geliştirici, Temel, Standart veya Premium katmandaki her API Yönetimi hizmeti örneğinde, yalnızca bu hizmet örneğine özel olan ortak IP adresleri bulunur (diğer kaynaklarla paylaşılmaz). 

IP adreslerini Azure portalındaki kaynağınızın genel bakış panosundan alabilirsiniz.

![API Yönetimi IP adresi](media/api-management-howto-ip-addresses/public-ip.png)

Ayrıca, aşağıdaki API çağrısıyla bunları programlı bir şekilde getirebilirsiniz:

```
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<service-name>?api-version=<api-version>
```

Ortak IP adresleri yanıtın bir parçası olacaktır:

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

[Çok bölgesel dağıtımlarda,](api-management-howto-deploy-multi-region.md)her bölgesel dağıtımın bir genel IP adresi vardır.

## <a name="ip-addresses-of-api-management-service-in-vnet"></a>VNet'teki API Yönetimi hizmetinin IP adresleri

API Yönetimi hizmetiniz sanal bir ağ içindeyse, genel ve özel olmak üzere iki tür IP adresi ne olur.

Ortak IP adresleri bağlantı noktasındaki `3443` dahili iletişim için kullanılır - yapılandırmayı yönetmek için (örneğin, Azure Kaynak Yöneticisi aracılığıyla). Harici VNet yapılandırmasında, çalışma zamanı API trafiği için de kullanılır. API Yönetimi'nden halka açık (Internet'e bakan) bir arka uça bir istek gönderildiğinde, isteğin kaynağı olarak herkese açık bir IP adresi görünür.

Özel sanal IP (VIP) adresleri, **yalnızca** [dahili VNet modunda](api-management-using-with-internal-vnet.md)kullanılabilir, ağ içinden API Management uç noktalarına bağlanmak için kullanılır - ağ geçitleri, geliştirici portalı ve doğrudan API erişimi için yönetim düzlemi. Bunları ağ içinde DNS kayıtlarını ayarlamak için kullanabilirsiniz.

Azure portalında ve API çağrısının yanıtında her iki türdeki adresleri görürsünüz:

![VNet IP adresinde API Yönetimi](media/api-management-howto-ip-addresses/vnet-ip.png)


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

API Yönetimi, VNet dışındaki bağlantılar için genel bir IP adresi ve VNet içindeki bağlantılar için özel bir IP adresi kullanır.

## <a name="ip-addresses-of-consumption-tier-api-management-service"></a>Tüketim katmanı API Yönetimi hizmetinin IP adresleri

API Yönetimi hizmetiniz bir Tüketim katmanı hizmetiyse, özel bir IP adresi yoktur. Tüketim katmanı hizmeti, ortak bir altyapıda ve deterministik bir IP adresi olmadan çalışır. 

Trafik kısıtlaması amacıyla, Azure veri merkezlerinin IP adresleri aralığını kullanabilirsiniz. Kesin adımlar için [Azure Fonksiyonları dokümantasyon makalesine](../azure-functions/ip-addresses.md#data-center-outbound-ip-addresses) bakın.

## <a name="changes-to-the-ip-addresses"></a>IP adreslerinde yapılan değişiklikler

API Yönetimi'nin Geliştirici, Temel, Standart ve Premium katmanlarında, genel IP adresleri (VIP) aşağıdaki istisnalar dışında hizmetin kullanım ömrü boyunca durağandır:

* Hizmet silinir ve sonra yeniden oluşturulur.
* Hizmet aboneliği [askıya](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) alınır veya [uyarılır](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) (örneğin, ödeme meden) ve sonra eski haline getirilmiştir.
* Azure Sanal Ağı hizmete eklenir veya hizmetten kaldırılır.
* API Yönetimi hizmeti, Harici ve Dahili VNet dağıtım modu arasında değiştirilir.

[Çok bölgesel dağıtımlarda,](api-management-howto-deploy-multi-region.md)bölge boşaltılırsa ve daha sonra eski haline getirilirse bölgesel IP adresi değişir.
