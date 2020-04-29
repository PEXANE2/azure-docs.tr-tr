---
title: Logic Apps REST API tümleştirme hizmeti ortamları (sesleri) oluşturun
description: Azure Logic Apps Azure sanal ağlarına (VNet) erişebilmek için Logic Apps REST API kullanarak bir tümleştirme hizmeti ortamı (ıSE) oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: 0670331d2338b4b6419ffbff1452b5fbac91029f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80478841"
---
# <a name="create-an-integration-service-environment-ise-by-using-the-logic-apps-rest-api"></a>Logic Apps kullanarak bir tümleştirme hizmeti ortamı (ıSE) oluşturun REST API

Bu makalede, mantıksal uygulamalarınızın ve tümleştirme hesaplarınızın bir [Azure sanal ağına](../virtual-network/virtual-networks-overview.md)erişmesi gereken senaryolar için Logic Apps REST API aracılığıyla bir [ *tümleştirme hizmeti ortamının* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) nasıl oluşturulacağı gösterilmektedir. ISE, "Global" çok kiracılı Logic Apps hizmetinden ayrı tutulan adanmış depolama ve diğer kaynakları kullanan yalıtılmış bir ortamdır. Bu ayrım Ayrıca diğer Azure kiracılarının uygulamalarınızın performansı üzerinde sahip olabileceği etkileri azaltır. ISE, size kendi statik IP adreslerinizi de sağlar. Bu IP adresleri, genel, çok kiracılı hizmette Logic Apps tarafından paylaşılan statik IP adreslerinden ayrıdır.

Ayrıca, [örnek Azure Resource Manager hızlı başlangıç şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-integration-service-environment) kullanarak veya [Azure Portal](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)kullanarak bir Ise oluşturabilirsiniz.

> [!IMPORTANT]
> Logic Apps, yerleşik Tetikleyiciler, yerleşik Eylemler ve ıSE 'de çalışan bağlayıcılar, tüketim tabanlı fiyatlandırma planından farklı bir fiyatlandırma planı kullanır. Fiyatlandırma ve faturalandırma işinin nasıl sesleri olduğunu öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma fiyatları için bkz. [Logic Apps fiyatlandırması](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure portal bir ıSE oluşturduğunuzda, [Ise için erişimi etkinleştirmek için](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access) aynı [Önkoşullar](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#prerequisites) ve gereksinimler

* HTTPS PUT isteğiyle Logic Apps REST API çağırarak ıSE oluşturmak için kullanabileceğiniz bir araç. Örneğin [Postman](https://www.getpostman.com/downloads/)'ı kullanabilir veya bu görevi gerçekleştiren bir mantıksal uygulama oluşturabilirsiniz.

## <a name="send-the-request"></a>İsteği gönder

Logic Apps REST API çağırarak ıSE oluşturmak için, bu HTTPS PUT isteğini yapın:

`PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/integrationServiceEnvironments/{integrationServiceEnvironmentName}?api-version=2019-05-01`

> [!IMPORTANT]
> Logic Apps REST API 2019-05-01 sürümü, ıSE bağlayıcıları için kendi HTTP PUT isteğinizi yapmanızı gerektirir.

Dağıtımın tamamlanabilmesi için genellikle iki saat içinde sürer. Bazen dağıtım dört saate kadar sürebilir. Dağıtım durumunu denetlemek için, Azure araç çubuğinizdeki [Azure Portal](https://portal.azure.com)bildirimler bölmesini açan Bildirimler simgesini seçin.

> [!NOTE]
> Dağıtım başarısız olursa veya ıSE 'yi silerseniz, Azure, alt ağlarınızı serbest bırakmadan önce bir saat kadar sürebilir. Bu gecikme, başka bir ıSE içinde bu alt ağları yeniden kullanmadan önce beklemeniz gerekebilecek anlamına gelir.
>
> Sanal ağınızı silerseniz Azure, alt ağlarınızı serbest bırakmadan genellikle iki saate kadar sürer, ancak bu işlem daha uzun sürebilir. 
> Sanal ağları silerken, hala bağlı hiçbir kaynak bulunmadığından emin olun. 
> Bkz. [sanal ağı silme](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

## <a name="request-header"></a>İstek üst bilgisi

İstek üstbilgisinde şu özellikleri ekleyin:

* `Content-type`: Bu özellik değerini olarak `application/json`ayarlayın.

* `Authorization`: Bu özellik değerini, kullanmak istediğiniz Azure aboneliğine veya kaynak grubuna erişimi olan müşterinin taşıyıcı belirtecine ayarlayın.

### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi

İşte, ıSE 'nizi oluştururken kullanılacak özellikleri açıklayan istek gövdesi sözdizimi şöyledir:

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

Bu örnek istek gövdesinde örnek değerler gösterilmektedir:

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

* [Tümleştirme hizmeti ortamlarına kaynaklar ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Tümleştirme hizmeti ortamlarını yönetme](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)

