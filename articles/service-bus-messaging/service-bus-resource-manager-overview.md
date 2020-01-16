---
title: Kaynak Yöneticisi şablonları kullanarak Azure Service Bus kaynakları oluşturma | Microsoft Docs
description: Service Bus kaynaklarının oluşturulmasını otomatikleştirmek için Azure Resource Manager şablonları kullanma
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 09/11/2018
ms.author: spelluru
ms.openlocfilehash: d473cf2a79fb86028797205ee7885b51169b10bf
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75978229"
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak Service Bus kaynakları oluşturma

Bu makalede Azure Resource Manager şablonları, PowerShell ve Service Bus kaynak sağlayıcısı kullanılarak Service Bus kaynaklarının nasıl oluşturulacağı ve dağıtılacağı açıklanır.

Azure Resource Manager şablonlar, bir çözüm için dağıtılacak kaynakları tanımlamanıza ve farklı ortamların değerlerini giretkinleştirmenizi sağlayan parametreleri ve değişkenleri belirtmenize yardımcı olur. Şablon JSON dilinde yazılır ve dağıtımınızın değerlerini oluşturmak için kullanabileceğiniz ifadelerden oluşur. Azure Resource Manager şablonları yazma hakkında ayrıntılı bilgi ve şablon biçiminin bir açıklaması için, bkz. [Azure Resource Manager şablonlarının yapısı ve sözdizimi](../azure-resource-manager/templates/template-syntax.md).

> [!NOTE]
> Bu makaledeki örneklerde, Service Bus bir ad alanı ve mesajlaşma varlığı (sıra) oluşturmak için Azure Resource Manager nasıl kullanılacağı gösterilmektedir. Diğer şablon örnekleri için [Azure hızlı başlangıç şablonları Galerisi][Azure Quickstart Templates gallery] ' ni ziyaret edin ve **Service Bus**arayın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="service-bus-resource-manager-templates"></a>Service Bus Kaynak Yöneticisi şablonları

Bu Service Bus Azure Resource Manager şablonları indirme ve dağıtım için kullanılabilir. GitHub 'daki şablonların bağlantılarıyla birlikte her biri hakkındaki ayrıntılar için aşağıdaki bağlantılara tıklayın:

* [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
* [Sıraya sahip bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
* [Konu ve abonelikle bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
* [Kuyruk ve yetkilendirme kuralıyla Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
* [Konu, abonelik ve kuralla Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

Aşağıdaki yordamda, PowerShell kullanarak standart bir katman Service Bus ad alanı ve bu ad alanı içinde bir sıra oluşturan bir Azure Resource Manager şablonu dağıtma işlemi açıklanmaktadır. Bu örnek, [kuyruk şablonuyla Service Bus ad alanı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) ' yı temel alır. Yaklaşık olarak iş akışı şöyledir:

1. PowerShell 'i yükler.
2. Şablonu ve (isteğe bağlı olarak) bir parametre dosyası oluşturun.
3. PowerShell 'de Azure hesabınızda oturum açın.
4. Mevcut değilse yeni bir kaynak grubu oluşturun.
5. Dağıtımı test edin.
6. İsterseniz Dağıtım modunu ayarlayın.
7. Şablonu dağıtın.

Azure Resource Manager şablonlarını dağıtma hakkında ayrıntılı bilgi için bkz. [Azure Resource Manager şablonlarla kaynakları dağıtma][Deploy resources with Azure Resource Manager templates].

### <a name="install-powershell"></a>PowerShell yükleme

[Azure PowerShell kullanmaya](/powershell/azure/get-started-azureps)başlama yönergelerini izleyerek Azure PowerShell ' i yükler.

### <a name="create-a-template"></a>Şablon oluşturma

Depoyu kopyalama veya [201-ServiceBus-Create-Queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) şablonunu GitHub 'dan kopyalama:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "serviceBusNamespaceName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Service Bus namespace"
      }
    },
    "serviceBusQueueName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Queue"
      }
    }
  },
  "variables": {
    "defaultSASKeyName": "RootManageSharedAccessKey",
    "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]",
    "sbVersion": "2017-04-01"
  },
  "resources": [
    {
      "apiVersion": "2017-04-01",
      "name": "[parameters('serviceBusNamespaceName')]",
      "type": "Microsoft.ServiceBus/Namespaces",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard"
      },
      "properties": {},
      "resources": [
        {
          "apiVersion": "2017-04-01",
          "name": "[parameters('serviceBusQueueName')]",
          "type": "Queues",
          "dependsOn": [
            "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
          ],
          "properties": {
            "lockDuration": "PT5M",
            "maxSizeInMegabytes": "1024",
            "requiresDuplicateDetection": "false",
            "requiresSession": "false",
            "defaultMessageTimeToLive": "P10675199DT2H48M5.4775807S",
            "deadLetteringOnMessageExpiration": "false",
            "duplicateDetectionHistoryTimeWindow": "PT10M",
            "maxDeliveryCount": "10",
            "autoDeleteOnIdle": "P10675199DT2H48M5.4775807S",
            "enablePartitioning": "false",
            "enableExpress": "false"
          }
        }
      ]
    }
  ],
  "outputs": {
    "NamespaceConnectionString": {
      "type": "string",
      "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
    },
    "SharedAccessPolicyPrimaryKey": {
      "type": "string",
      "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
    }
  }
}
```

### <a name="create-a-parameters-file-optional"></a>Parametre dosyası oluşturma (isteğe bağlı)

İsteğe bağlı parametreler dosyası kullanmak için [201-ServiceBus-Create-Queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) dosyasını kopyalayın. `serviceBusNamespaceName` değerini, bu dağıtımda oluşturmak istediğiniz Service Bus ad alanı adıyla değiştirin ve `serviceBusQueueName` değerini oluşturmak istediğiniz sıranın adıyla değiştirin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2017-04-01"
        }
    }
}
```

Daha fazla bilgi için bkz. [Parametreler](../azure-resource-manager/templates/parameter-files.md) makalesi.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Azure 'da oturum açma ve Azure aboneliğini ayarlama

Bir PowerShell isteminden aşağıdaki komutu çalıştırın:

```powershell
Connect-AzAccount
```

Azure hesabınızda oturum açmanız istenir. Oturum açtıktan sonra, kullanılabilir aboneliklerinizi görüntülemek için aşağıdaki komutu çalıştırın:

```powershell
Get-AzSubscription
```

Bu komut, kullanılabilir Azure aboneliklerinin bir listesini döndürür. Aşağıdaki komutu çalıştırarak geçerli oturum için bir abonelik seçin. `<YourSubscriptionId>`, kullanmak istediğiniz Azure aboneliğinin GUID 'SI ile değiştirin:

```powershell
Set-AzContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Kaynak grubunu ayarlama

Mevcut bir kaynak grubunuz yoksa, **New-AzResourceGroup** komutuyla yeni bir kaynak grubu oluşturun. Kaynak grubunun ve kullanmak istediğiniz konumun adını belirtin. Örneğin:

```powershell
New-AzResourceGroup -Name MyDemoRG -Location "West US"
```

Başarılı olursa, yeni kaynak grubunun bir özeti görüntülenir.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Dağıtımı test etme

`Test-AzResourceGroupDeployment` cmdlet 'ini çalıştırarak dağıtımınızı doğrulayın. Dağıtımı sınarken, dağıtımı yürütürken yaptığınız gibi parametreleri tam olarak sağlayın.

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Dağıtımı oluşturma

Yeni dağıtımı oluşturmak için `New-AzResourceGroupDeployment` cmdlet 'ini çalıştırın ve istendiğinde gerekli parametreleri sağlayın. Parametreler dağıtımınız için bir ad, kaynak grubunuzun adı ve şablon dosyasının yolunu veya URL 'sini içerir. **Mode** parametresi belirtilmemişse, varsayılan **artımlı** değeri kullanılır. Daha fazla bilgi için bkz. [artımlı ve tamamlanmış dağıtımlar](../azure-resource-manager/templates/deployment-modes.md).

Aşağıdaki komut, PowerShell penceresindeki üç parametreyi ister:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Bunun yerine bir parametre dosyası belirtmek için aşağıdaki komutu kullanın:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Ayrıca, dağıtım cmdlet 'ini çalıştırdığınızda satır içi parametreleri de kullanabilirsiniz. Komut aşağıdaki gibidir:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

[Tüm](../azure-resource-manager/templates/deployment-modes.md) bir dağıtımı çalıştırmak Için, **Mode** parametresini **Tamam**olarak ayarlayın:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="verify-the-deployment"></a>Dağıtımı doğrulama
Kaynaklar başarıyla dağıtılırsa, PowerShell penceresinde dağıtımın bir özeti görüntülenir:

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2017 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2017-04-01

```

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure Resource Manager şablonu dağıtmaya yönelik temel iş akışını ve komutları gördünüz. Daha ayrıntılı bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Azure Resource Manager’a genel bakış][Azure Resource Manager overview]
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma][Deploy resources with Azure Resource Manager templates]
* [Azure Resource Manager şablonları yazma](../azure-resource-manager/templates/template-syntax.md)
* [Microsoft. ServiceBus kaynak türleri](/azure/templates/microsoft.servicebus/allversions)

[Azure Resource Manager overview]: ../azure-resource-manager/management/overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/templates/deploy-powershell.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
