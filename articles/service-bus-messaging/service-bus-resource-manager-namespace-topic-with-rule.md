---
title: Azure şablonu kullanarak Hizmet Veri Çelim konu aboneliği oluşturma ve kural oluşturma
description: Azure Kaynak Yöneticisi şablonu kullanarak konu, abonelik ve kural içeren bir Hizmet Veri Servisi ad alanı oluşturma
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 9e0aaf58-0214-4bca-bd00-d29c08f9b1bc
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/27/2019
ms.author: spelluru
ms.openlocfilehash: 6cbaf447dfcf06ae11f2282d7d847978297af8b8
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384900"
---
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak konu, abonelik ve kural içeren bir Hizmet Veri Servisi ad alanı oluşturma

Bu makalede, bir konu, abonelik ve kural (filtre) içeren bir Hizmet Veri Servisi ad alanı oluşturan bir Azure Kaynak Yöneticisi şablonu nasıl kullanılacağı nı gösterir. Makalede, hangi kaynakların dağıtıldığı ve dağıtım yürütüldüğünde belirtilen parametrelerin nasıl tanımlanılığı açıklanmaktadır. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz

Şablon oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma][Authoring Azure Resource Manager templates].

Azure kaynakları adlandırma kurallarıyla ilgili uygulama ve desenler hakkında daha fazla bilgi için, [Azure kaynakları için Önerilen adlandırma kuralları'na][Recommended naming conventions for Azure resources]bakın.

Şablonun tamamı [için, konu, abonelik ve kural][Service Bus namespace with topic, subscription, and rule] şablonu içeren Servis Veri Servisi ad alanına bakın.

> [!NOTE]
> Aşağıdaki Azure Kaynak Yöneticisi şablonları karşıdan yükleme ve dağıtım için kullanılabilir.
> 
> * [Sıra ve yetkilendirme kuralı yla Hizmet Veri Servisi ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
> * [Sırayla hizmet veri günü ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
> * [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Konu ve abonelikle birlikte Hizmet Veri Günü ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
> 
> En son şablonları kontrol etmek için [Azure Quickstart Şablonları][Azure Quickstart Templates] galerisini ziyaret edin ve Hizmet Veri Servisi'ni arayın.

## <a name="what-do-you-deploy"></a>Ne dağıtıyorsun?

Bu şablonla, konu, abonelik ve kural (filtre) içeren bir Hizmet Veri Servisi ad alanı dağıtırsınız.

[Servis Veri Aracı konuları ve abonelikleri,](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) *yayımlama/abone verme* deseninde bire bir iletişim biçimi sağlar. Konuları ve abonelikleri kullanırken, dağıtılmış bir uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmaz, bunun yerine aracı görevi gören bir konu üzerinden ileti alışverişinde bulunulr. Bir konuya abonelik, konuya gönderilen iletilerin kopyalarını alan sanal bir kuyruğa benzer. Abonelik filtresi, bir konuya gönderilen iletilerin belirli bir konu aboneliğinde görünmesi gerektiğini belirtmenize olanak tanır.

## <a name="what-are-rules-filters"></a>Kurallar (filtreler) nedir?

Birçok senaryoda, belirli özelliklere sahip iletiler farklı şekillerde işlenmelidir. Bu özel işlemeyi etkinleştirmek için, abonelikleri belirli özelliklere sahip iletileri bulmak ve sonra bu özelliklerde değişiklikler gerçekleştirecek şekilde yapılandırabilirsiniz. Hizmet Veri Kurumu abonelikleri konuya gönderilen tüm iletileri görse de, bu iletilerin bir alt kümesini yalnızca sanal abonelik kuyruğuna kopyalayabilirsiniz. Abonelik filtreleri kullanılarak gerçekleştirilir. Kurallar (filtreler) hakkında daha fazla bilgi edinmek için [Kurallar ve eylemlere](service-bus-queues-topics-subscriptions.md#rules-and-actions)bakın.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure'a Dağıt](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler

Azure Kaynak Yöneticisi ile şablonun dağıtıldığında belirtmek istediğiniz değerler için parametreler tanımlayın. Şablon, tüm parametre değerlerini içeren `Parameters` adlı bir bölüm içerir. Dağıtmakta olduğunuz projeye veya dağıtmakta olduğunuz ortama göre değişen değerler için bir parametre tanımlayın. Her zaman aynı kalan değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır.

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

### <a name="servicebusrulename"></a>serviceBusRuleName

Hizmet Veri Servisi ad alanında oluşturulan kural(filtre) adı.

```json
   "serviceBusRuleName": {
   "type": "string",
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

Konu, abonelik ve kurallariçeren, tür **İletisi'nin**standart bir Servis Veri Mesle'si ad alanı oluşturur.

```json
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
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
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filterType": "SqlFilter",
                        "sqlFilter": {
                            "sqlExpression": "StoreName = 'Store1'",
                            "requiresPreprocessing": "false"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

JSON sözdizimi ve özellikleri için [ad boşluklarına,](/azure/templates/microsoft.servicebus/namespaces) [konulara,](/azure/templates/microsoft.servicebus/namespaces/topics) [aboneliklere](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions)ve [kurallara](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions/rules)bakın.

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```powershell-interactive
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleleri görüntüleyerek bu kaynakları nasıl yöneteceklerini öğrenin:

* [Azure Servis Veri Toplarını Yönetme](service-bus-management-libraries.md)
* [Service Bus’ı PowerShell ile yönetme](service-bus-manage-with-ps.md)
* [Servis Veri Servisi Gezgini ile Servis Veri Servisi kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/templates/template-syntax.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: /azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
