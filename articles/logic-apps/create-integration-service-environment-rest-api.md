---
title: Logic Apps REST API tümleştirme hizmeti ortamları (sesleri) oluşturun
description: Azure Logic Apps Azure sanal ağlarına (VNet) erişebilmek için Logic Apps REST API kullanarak bir tümleştirme hizmeti ortamı (ıSE) oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: rarayudu, logicappspm
ms.topic: conceptual
ms.date: 12/05/2020
ms.openlocfilehash: 783431c4888a68e24cf3d2603c541c4797ea65d8
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2020
ms.locfileid: "96741108"
---
# <a name="create-an-integration-service-environment-ise-by-using-the-logic-apps-rest-api"></a>Logic Apps REST API'sini kullanarak tümleştirme hizmeti ortamı (ISE) oluşturma

Mantıksal uygulamalarınızın ve tümleştirme hesaplarınızın bir [Azure sanal ağına](../virtual-network/virtual-networks-overview.md)erişmesi gereken senaryolarda, Logic Apps REST API kullanarak bir [ *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturabilirsiniz. Sesleri hakkında daha fazla bilgi edinmek için bkz. [Azure Logic Apps Azure sanal ağ kaynaklarına erişim](connect-virtual-network-vnet-isolated-environment-overview.md).

Bu makalede, genel olarak Logic Apps REST API kullanarak bir ıSE oluşturma gösterilmektedir. İsteğe bağlı olarak, yalnızca şu anda Logic Apps REST API kullanarak, [sistem tarafından atanan veya Kullanıcı tarafından atanan bir yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) de etkinleştirebilirsiniz. Bu kimlik, ıSE 'nin sanal makineler ve bir Azure sanal ağı 'nda bulunan ya da bağlı olan sanal makineler ve diğer sistemler veya hizmetler gibi güvenli kaynaklara erişimi kimlik doğrulamasına olanak tanır. Bu şekilde, kimlik bilgilerinizle oturum açmanız gerekmez.

ISE oluşturmanın diğer yolları hakkında daha fazla bilgi için şu makalelere bakın:

* [Azure portal kullanarak bir ıSE oluşturun](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* [Örnek Azure Resource Manager hızlı başlangıç şablonunu kullanarak bir ıSE oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-integration-service-environment)
* [Bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmayı destekleyen bir ıSE oluşturun](customer-managed-keys-integration-service-environment.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure portal bir ıSE oluştururken aynı [ön koşullar](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#prerequisites) ve [erişim gereksinimleri](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access)

* Bilgilerini ıSE tanımına eklemek istediğiniz ek kaynaklar, örneğin, bu bilgileri ıSE tanımına dahil edebilmeniz için: 

  * Otomatik olarak imzalanan sertifika desteğini etkinleştirmek için, bu sertifikayla ilgili bilgileri ıSE tanımına dahil etmeniz gerekir.

  * Kullanıcı tarafından atanan yönetilen kimliği etkinleştirmek için bu kimliği önceden oluşturmanız ve `objectId` , `principalId` ve `clientId` özelliklerini ve değerlerini ve değerlerini Daha fazla bilgi için, bkz. [Azure Portal Kullanıcı tarafından atanan yönetilen kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

* HTTPS PUT isteğiyle Logic Apps REST API çağırarak ıSE oluşturmak için kullanabileceğiniz bir araç. Örneğin [Postman](https://www.getpostman.com/downloads/)'ı kullanabilir veya bu görevi gerçekleştiren bir mantıksal uygulama oluşturabilirsiniz.

## <a name="create-the-ise"></a>ISE oluşturma

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

* `Content-type`: Bu özellik değerini olarak ayarlayın `application/json` .

* `Authorization`: Bu özellik değerini, kullanmak istediğiniz Azure aboneliğine veya kaynak grubuna erişimi olan müşterinin taşıyıcı belirtecine ayarlayın.

<a name="request-body"></a>

## <a name="request-body"></a>İstek gövdesi

İstek gövdesinde, Ise 'de etkinleştirmek istediğiniz ek yetenekler hakkında bilgiler de dahil olmak üzere ıSE 'yi oluşturmak için kullanılacak kaynak tanımını sağlayın, örneğin:

* Konumda yüklü olan otomatik olarak imzalanan bir sertifikanın kullanılmasına izin veren bir ıSE oluşturmak için `TrustedRoot` , `certificates` `properties` Bu makalede daha sonra açıklanan şekilde, o zaman Ise tanımının bölümünün içine nesnesini ekleyin.

  Mevcut bir ıSE üzerinde bu özelliği etkinleştirmek için, yalnızca nesnesi için bir yama isteği gönderebilirsiniz `certificates` . Otomatik olarak imzalanan sertifikaları kullanma hakkında daha fazla bilgi için bkz. [diğer hizmetlere ve sistemlere giden çağrılar Için güvenli erişim ve veri erişimi](../logic-apps/logic-apps-securing-a-logic-app.md#secure-outbound-requests).

* Sistem tarafından atanan veya Kullanıcı tarafından atanan yönetilen kimlik kullanan bir ıSE oluşturmak için, `identity` Bu makalede daha sonra açıklandığı gibi, yönetilen kimlik türüne sahip ve Ise tanımında gereken diğer bilgileri içeren bir nesne ekleyin.

* Müşteri tarafından yönetilen anahtarlar kullanan bir ıSE oluşturmak ve bekleyen verileri şifrelemek için Azure Key Vault, [müşteri tarafından yönetilen anahtar desteğini sağlayan bilgileri](customer-managed-keys-integration-service-environment.md)ekleyin. Müşteri tarafından yönetilen anahtarları, daha sonra değil *yalnızca oluşturma sırasında* ayarlayabilirsiniz.

### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi

İşte, ıSE 'nizi oluştururken kullanılacak özellikleri açıklayan istek gövdesi sözdizimi şöyledir:

```json
{
   "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Logic/integrationServiceEnvironments/{ISE-name}",
   "name": "{ISE-name}",
   "type": "Microsoft.Logic/integrationServiceEnvironments",
   "location": "{Azure-region}",
   "sku": {
      "name": "Premium",
      "capacity": 1
   },
   // Include the `identity` object to enable the system-assigned identity or user-assigned identity
   "identity": {
      "type": <"SystemAssigned" | "UserAssigned">,
      // When type is "UserAssigned", include the following "userAssignedIdentities" object:
      "userAssignedIdentities": {
         "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{user-assigned-managed-identity-object-ID}": {
            "principalId": "{principal-ID}",
            "clientId": "{client-ID}"
         }
      }
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
      },
      // Include `certificates` object to enable self-signed certificate support
      "certificates": {
         "testCertificate": {
            "publicCertificate": "{base64-encoded-certificate}",
            "kind": "TrustedRoot"
         }
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
   "identity": {
      "type": "UserAssigned",
      "userAssignedIdentities": {
         "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/*********************************": {
            "principalId": "*********************************",
            "clientId": "*********************************"
         }
      }
   },
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
      },
      "certificates": {
         "testCertificate": {
            "publicCertificate": "LS0tLS1CRUdJTiBDRV...",
            "kind": "TrustedRoot"
         }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tümleştirme hizmeti ortamlarına kaynaklar ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Tümleştirme hizmeti ortamlarını yönetme](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)
