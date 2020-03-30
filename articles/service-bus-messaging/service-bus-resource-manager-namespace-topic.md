---
title: Şablon kullanarak Azure Hizmet Veri Çelim ad alanı konusunu oluşturma
description: 'Hızlı başlangıç: Azure Kaynak Yöneticisi şablonu kullanarak konu ve abonelik içeren bir Hizmet Veri Servisi ad alanı oluşturma'
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d3d55200-5c60-4b5f-822d-59974cafff0e
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: c94b670a33f7640d2d6f428287b3ba0fab766bc5
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384883"
---
# <a name="quickstart-create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Kaynak Yöneticisi şablonu kullanarak konu ve abonelikiçeren hizmet veri günü ad alanı oluşturma

Bu makalede, hizmet veri meskeni ad alanı ve bu ad alanı içinde bir konu ve abonelik oluşturan bir Azure Kaynak Yöneticisi şablonu nasıl kullanılacağı gösterilmektedir. Makalede, hangi kaynakların dağıtıldığı ve dağıtım yürütüldüğünde belirtilen parametrelerin nasıl tanımlanılığı açıklanmaktadır. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz

Şablon oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Şablonun tamamı [için, konu ve abonelik][Service Bus namespace with topic and subscription] şablonu içeren Servis Veri Servisi ad alanına bakın.

> [!NOTE]
> Aşağıdaki Azure Kaynak Yöneticisi şablonları karşıdan yükleme ve dağıtım için kullanılabilir.
> 
> * [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Sırayla hizmet veri günü ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
> * [Sıra ve yetkilendirme kuralı yla Hizmet Veri Servisi ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
> * [Konu, abonelik ve kural içeren bir Hizmet Veri Günü ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> En son şablonları denetlemek için [Azure Quickstart Şablonları][Azure Quickstart Templates] galerisini ziyaret edin ve **Hizmet Veri Servisi'ni**arayın.

## <a name="what-do-you-deploy"></a>Ne dağıtıyorsun?

Bu şablonla, konu ve abonelik içeren bir Hizmet Veri Servisi ad alanı dağıtırsınız.

[Servis Veri Aracı konuları ve abonelikleri,](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) *yayımlama/abone verme* deseninde bire bir iletişim biçimi sağlar.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure'a Dağıt](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon, tüm parametre değerlerini içeren bir `Parameters` bölüm içerir. Dağıtmakta olduğunuz projeye veya dağıtmakta olduğunuz ortama göre değişen değerler için bir parametre tanımlayın. Her zaman aynı kalan değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır.

Şablon aşağıdaki parametreleri tanımlar:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Oluşturulacak Servis Veri Günü ad alanının adı.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

Hizmet Veri Servisi ad alanında oluşturulan konunun adı.

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

Hizmet Veri Servisi ad alanında oluşturulan aboneliğin adı.

```json
"serviceBusSubscriptionName": {
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

Konu ve abonelik içeren tür **İletisi'nin**standart bir Servis Veri Mesle'si ad alanı oluşturur.

```json
"resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "Standard",
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

JSON sözdizimi ve özellikleri için [ad boşluklarına,](/azure/templates/microsoft.servicebus/namespaces) [konulara](/azure/templates/microsoft.servicebus/namespaces/topics)ve [aboneliklere](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions)bakın.

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```powershell-interactive
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az group deployment create \<my-resource-group\> --name \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Kaynak Yöneticisi'ni kullanarak kaynak oluşturduğunuzve dağıtdığınıza göre, bu makaleleri görüntüleyerek bu kaynakları nasıl yöneteceklerinizi öğrenin:

* [Service Bus’ı PowerShell ile yönetme](service-bus-manage-with-ps.md)
* [Servis Veri Servisi Gezgini ile Servis Veri Servisi kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/templates/template-syntax.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
