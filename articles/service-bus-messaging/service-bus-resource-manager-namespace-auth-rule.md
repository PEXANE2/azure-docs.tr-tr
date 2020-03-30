---
title: Azure şablonu kullanarak Hizmet Veri Çelim yetkilendirme kuralı oluşturma
description: Azure Kaynak Yöneticisi şablonu kullanarak ad alanı ve sıra için Hizmet Veri Çiş yetkilendirme kuralı oluşturma
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 7f1443a0-5fa8-4d90-8637-1a977ef0b1f0
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 12/20/2019
ms.author: aschhab
ms.openlocfilehash: 1bfb2d2d946a85c1d051315fb29a5a63f7a00871
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384934"
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak ad alanı ve sıra için Hizmet Veri Çiş yetkilendirme kuralı oluşturma

Bu makalede, Hizmet Veri Servisi ad alanı ve sırası için [yetkilendirme kuralı](service-bus-authentication-and-authorization.md#shared-access-signature) oluşturan bir Azure Kaynak Yöneticisi şablonu nasıl kullanılacağı gösterilmektedir. Makalede, hangi kaynakların dağıtıldığı ve dağıtım yürütüldüğünde belirtilen parametrelerin nasıl tanımlanılığı açıklanmaktadır. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şablon oluşturma hakkında daha fazla bilgi için lütfen [Azure Kaynak Yöneticisi şablonları yazma][Authoring Azure Resource Manager templates]'ya bakın.

Şablonun tamamı için GitHub'daki [Servis Veri Yolunda yetkilendirme kuralı şablonuna][Service Bus auth rule template] bakın.

> [!NOTE]
> Aşağıdaki Azure Kaynak Yöneticisi şablonları karşıdan yükleme ve dağıtım için kullanılabilir.
> 
> * [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Sırayla hizmet veri günü ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
> * [Konu ve abonelikle birlikte Hizmet Veri Günü ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
> * [Konu, abonelik ve kural içeren bir Hizmet Veri Günü ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> En son şablonları denetlemek için [Azure Quickstart Şablonları][Azure Quickstart Templates] galerisini ziyaret edin ve **Hizmet Veri Servisi'ni**arayın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-will-you-deploy"></a>Ne dağıtacaksınız?

Bu şablonla, ad alanı ve ileti varlığı (bu durumda, bir sıra) için bir Hizmet Veri Çiş yetkilendirme kuralı dağıtMış sınız.

Bu şablon, kimlik doğrulaması için [Paylaşılan Erişim İmzası 'nı (SAS)](service-bus-sas.md) kullanır. SAS, uygulamaların ad alanında veya belirli hakların ilişkili olduğu ileti varlığında (sıra veya konu) yapılandırılan bir erişim anahtarını kullanarak Hizmet Veri Yolunda kimlik doğrulaması yapılmasını sağlar. Daha sonra bu anahtarı, istemcilerin sırayla Servis Veri Yoluna'nın kimliğini doğrulamak için kullanabilecekleri bir SAS belirteci oluşturmak için kullanabilirsiniz.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure'a Dağıt](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon, tüm parametre değerlerini içeren bir `Parameters` bölüm içerir. Dağıttığınız projeye veya dağıtmakta olduğunuz ortama göre değişen değerler için bir parametre tanımlamanız gerekir. Her zaman aynı kalacak değerler için parametreleri tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır.

Şablon aşağıdaki parametreleri tanımlar.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Oluşturulacak Servis Veri Günü ad alanının adı.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName

Ad alanı için yetkilendirme kuralının adı.

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Hizmet Veri Günü ad alanında sıranın adı.

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Şablonun Servis Veri Servisi API sürümü.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2017-04-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       }
```

## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar

Tür İletisi'nin standart bir **Messaging**Servis Veri Mesle'si ad alanı ve ad alanı ve varlık için Bir Hizmet Veri Servisi yetkilendirme kuralı oluşturur.

```json
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "Standard",
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

JSON sözdizimi ve özellikleri için [ad boşluklarına,](/azure/templates/microsoft.servicebus/namespaces) [kuyruklara](/azure/templates/microsoft.servicebus/namespaces/queues)ve [Yetkilendirme Kuralları'na](/azure/templates/microsoft.servicebus/namespaces/authorizationrules)bakın.

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```powershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Kaynak Yöneticisi'ni kullanarak kaynak oluşturduğunuzve dağıtdığınıza göre, bu makaleleri görüntüleyerek bu kaynakları nasıl yöneteceklerinizi öğrenin:

* [Service Bus’ı PowerShell ile yönetme](service-bus-powershell-how-to-provision.md)
* [Servis Veri Servisi Gezgini ile Servis Veri Servisi kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [Service Bus kimlik doğrulaması ve yetkilendirme](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/templates/template-syntax.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
