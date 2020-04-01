---
title: Logic Apps REST API ile entegrasyon hizmeti ortamları (ISS' ler) oluşturun
description: Azure Logic Apps Apps Uygulamalarından Azure sanal ağlarına (VNETs) erişebilmeniz için Logic Apps REST API'sini kullanarak bir entegrasyon hizmeti ortamı (İmKB) oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: 0670331d2338b4b6419ffbff1452b5fbac91029f
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478841"
---
# <a name="create-an-integration-service-environment-ise-by-using-the-logic-apps-rest-api"></a>Logic Apps REST API'yi kullanarak bir entegrasyon hizmet ortamı (Ise) oluşturun

Bu makalede, mantık uygulamalarınızın ve tümleştirme hesaplarınızın bir [Azure sanal ağına](../virtual-network/virtual-networks-overview.md)erişmeye ihtiyaç duyduğu senaryolar için Logic Apps REST API aracılığıyla bir [ *tümleştirme hizmet ortamının* (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) nasıl oluşturulacağı gösterilmektedir. Ise, "genel" çok kiracılı Logic Apps hizmetinden ayrı tutulan özel depolama ve diğer kaynakları kullanan yalıtılmış bir ortamdır. Bu ayrım, diğer Azure kiracılarının uygulamalarınızın performansı üzerindeki etkisini de azaltır. Bir ISE aynı zamanda kendi statik IP adreslerinizi de sağlar. Bu IP adresleri, genel, çok kiracılı hizmetteki mantık uygulamaları tarafından paylaşılan statik IP adreslerinden ayrıdır.

Ayrıca, örnek Azure Kaynak [Yöneticisi hızlı başlatma şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-integration-service-environment) kullanarak veya [Azure portalını](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)kullanarak bir İmKB oluşturabilirsiniz.

> [!IMPORTANT]
> Logic uygulamaları, yerleşik tetikleyiciler, yerleşik eylemler ve İmKB'nizde çalışan bağlayıcılar, tüketim tabanlı fiyatlandırma planından farklı bir fiyatlandırma planı kullanır. ISE'ler için fiyatlandırma ve faturalandırmanın nasıl çalıştığını öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma oranları için [Logic Apps fiyatlandırması'na](../logic-apps/logic-apps-pricing.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure portalında Bir İmKB oluşturduğunuzda olduğu [gibi İmKB'nize erişimi etkinleştirmek için](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access) aynı [ön koşullar](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#prerequisites) ve gereksinimler

* Https PUT isteğiyle Logic Apps REST API'sini arayarak İmKB'nizi oluşturmak için kullanabileceğiniz bir araçtır. Örneğin, [Postacı'yı](https://www.getpostman.com/downloads/)kullanabilir veya bu görevi gerçekleştiren bir mantık uygulaması oluşturabilirsiniz.

## <a name="send-the-request"></a>İsteği gönderme

Logic Apps REST API'yi arayarak İmKB'nizi oluşturmak için şu HTTPS PUT isteğini yapın:

`PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/integrationServiceEnvironments/{integrationServiceEnvironmentName}?api-version=2019-05-01`

> [!IMPORTANT]
> Logic Apps REST API 2019-05-01 sürümü, İmKB konektörleri için kendi HTTP PUT isteğinizi oluşturmanızı gerektirir.

Dağıtımın tamamlanması genellikle iki saat içinde gerçekleşir. Bazen, dağıtım dört saat kadar sürebilir. Dağıtım durumunu denetlemek için, Azure araç çubuğunuzdaki [Azure portalında,](https://portal.azure.com)bildirimler bölmesini açan bildirimler simgesini seçin.

> [!NOTE]
> Dağıtım başarısız olursa veya İmKB'nizi silerseniz, Azure'un alt ağlarınızı serbest bırakması bir saat kadar sürebilir. Bu gecikme, bu alt ağları başka bir ISE'de yeniden kullanmadan önce beklemeniz anlamına gelir.
>
> Sanal ağınızı silerseniz, Azure'un alt ağlarınızı serbest bırakması genellikle iki saat kadar sürer, ancak bu işlem daha uzun sürebilir. 
> Sanal ağları silerken, hiçbir kaynağın hala bağlı olmadığından emin olun. 
> Bkz. [Sanal ağı sil.](../virtual-network/manage-virtual-network.md#delete-a-virtual-network)

## <a name="request-header"></a>İstek üst bilgisi

İstek üstbilgisinde şu özellikleri ekleyin:

* `Content-type`: Bu özellik `application/json`değerini .

* `Authorization`: Bu özellik değerini, kullanmak istediğiniz Azure aboneliğine veya kaynak grubuna erişimi olan müşteri için taşıyıcı belirteci olarak ayarlayın.

### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi

İmKB'nizi oluştururken kullanılacak özellikleri açıklayan istek gövdesi sözdizimi aşağıda verilmiştir:

```json
{
   "id": "/subscriptions/{Azure-subscription-ID/resourceGroups/{Azure-resource-group}/providers/Microsoft.Logic/integrationServiceEnvironments/{ISE-name}",
   "name": "{ISE-name}",
   "type": "Microsoft.Logic/integrationServiceEnvironments",
   "location": "{Azure-region}",
   "sku": {
      "name": "Premium",
      "capacity": 1
   },
   "properties": {
      "networkConfiguration": {
         "accessEndpoint": {
            // Your ISE can use the "External" or "Internal" endpoint. This example uses "External".
            "type": "External"
         },
         "subnets": [
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-1}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-2}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-3}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-4}",
            }
         ]
      }
   }
}
```

### <a name="request-body-example"></a>İstek gövdesi örneği

Bu örnek istek gövdesi örnek değerleri gösterir:

```json
{
   "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Logic/integrationServiceEnvironments/Fabrikam-ISE",
   "name": "Fabrikam-ISE",
   "type": "Microsoft.Logic/integrationServiceEnvironments",
   "location": "WestUS2",
   "sku": {
      "name": "Premium",
      "capacity": 1
   },
   "properties": {
      "networkConfiguration": {
         "accessEndpoint": {
            // Your ISE can use the "External" or "Internal" endpoint. This example uses "External".
            "type": "External"
         },
         "subnets": [
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-1",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-2",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-3",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-4",
            }
         ]
      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Entegrasyon hizmeti ortamlarına kaynak ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Tümleştirme hizmeti ortamlarını yönetme](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)

