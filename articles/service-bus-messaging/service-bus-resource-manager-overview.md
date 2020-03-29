---
title: Şablonları kullanarak Azure Hizmet Veri Servisi kaynakları oluşturma
description: Hizmet Veri Mes'leri kaynaklarının oluşturulmasını otomatikleştirmek için Azure Kaynak Yöneticisi şablonlarını kullanma
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
ms.openlocfilehash: 9bc784ee57b9bde393408cbefa9a197aebc59b08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76264467"
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak Hizmet Veri Servisi kaynakları oluşturma

Bu makalede, Azure Kaynak Yöneticisi şablonları, PowerShell ve Hizmet Veri Mes'i kaynak sağlayıcısını kullanarak Hizmet Veri Mes'leri kaynaklarının nasıl oluşturulup dağıtılanınca açıklanmaktadır.

Azure Kaynak Yöneticisi şablonları, bir çözüm için dağıtılacak kaynakları tanımlamanıza ve farklı ortamlar için değer oluşturmanıza olanak tanıyan parametreleri ve değişkenleri belirtmenize yardımcı olur. Şablon JSON'da yazılır ve dağıtımınız için değerler oluşturmak için kullanabileceğiniz ifadelerden oluşur. Azure Kaynak Yöneticisi şablonları yazma ve şablon biçimi yle ilgili bir tartışma hakkında ayrıntılı bilgi için [Azure Kaynak Yöneticisi şablonlarının yapısına ve sözdizimine](../azure-resource-manager/templates/template-syntax.md)bakın.

> [!NOTE]
> Bu makaledeki örnekler, Hizmet Veri Birimi ad alanı ve ileti varlığı (sıra) oluşturmak için Azure Kaynak Yöneticisi'nin nasıl kullanılacağını gösterir. Diğer şablon örnekleri için [Azure Hızlı Başlangıç Şablonları galerisini][Azure Quickstart Templates gallery] ziyaret edin ve Hizmet Veri **Servisi'ni**arayın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="service-bus-resource-manager-templates"></a>Servis Veri Servisi Kaynak Yöneticisi şablonları

Bu Hizmet Veri Mes'ü Azure Kaynak Yöneticisi şablonları karşıdan yükleme ve dağıtım için kullanılabilir. GitHub'daki şablonlara bağlantılar içeren her biri hakkında ayrıntılı bilgi için aşağıdaki bağlantıları tıklatın:

* [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
* [Sırayla hizmet veri günü ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
* [Konu ve abonelikle birlikte Hizmet Veri Günü ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
* [Sıra ve yetkilendirme kuralı yla Hizmet Veri Servisi ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
* [Konu, abonelik ve kural içeren bir Hizmet Veri Günü ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

Aşağıdaki yordam, Standart katman Hizmet Veri Mes'u ad alanı ve bu ad alanı içinde bir sıra oluşturan bir Azure Kaynak Yöneticisi şablonu dağıtmak için PowerShell'in nasıl kullanılacağını açıklar. Bu örnek, sıra şablonu [olan Hizmet Veri Servisi ad alanı oluşturma'yı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) temel alır. Yaklaşık iş akışı aşağıdaki gibidir:

1. PowerShell'i yükleyin.
2. Şablonu ve (isteğe bağlı olarak) bir parametre dosyasını oluşturun.
3. PowerShell'de Azure hesabınızda oturum açın.
4. Yoksa yeni bir kaynak grubu oluşturun.
5. Dağıtımı test edin.
6. İstenirse dağıtım modunu ayarlayın.
7. Şablonu dağıtın.

Azure Kaynak Yöneticisi şablonlarını dağıtma hakkında tam bilgi için [bkz.][Deploy resources with Azure Resource Manager templates]

### <a name="install-powershell"></a>PowerShell yükleme

Azure PowerShell ile başlarken yönergeleri izleyerek [Azure PowerShell'i yükleyin.](/powershell/azure/get-started-azureps)

### <a name="create-a-template"></a>Şablon oluşturma

Depoyu klonlayın veya GitHub'dan [201 servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) şablonunu kopyalayın:

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

### <a name="create-a-parameters-file-optional"></a>Bir parametre dosyası oluşturma (isteğe bağlı)

İsteğe bağlı parametreler dosyasını kullanmak için [201 servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) dosyasını kopyalayın. Bu dağıtımda `serviceBusNamespaceName` oluşturmak istediğiniz Hizmet Veri Kurumu ad alanının değerini değiştirin ve `serviceBusQueueName` oluşturmak istediğiniz kuyruğun adı ile değiştirin.

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

Daha fazla bilgi için [Parametreler](../azure-resource-manager/templates/parameter-files.md) makalesine bakın.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Azure'da oturum açın ve Azure aboneliğini ayarlayın

PowerShell komut isteminden aşağıdaki komutu çalıştırın:

```powershell
Connect-AzAccount
```

Azure hesabınızda oturum açmanız istenir. Oturum açtıktan sonra, kullanılabilir aboneliklerinizi görüntülemek için aşağıdaki komutu çalıştırın:

```powershell
Get-AzSubscription
```

Bu komut, kullanılabilir Azure aboneliklerinin bir listesini döndürür. Aşağıdaki komutu çalıştırarak geçerli oturum için bir abonelik seçin. Kullanmak `<YourSubscriptionId>` istediğiniz Azure aboneliği için GUID ile değiştirin:

```powershell
Set-AzContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Kaynak grubunu ayarlama

Varolan bir kaynak grubunuz yoksa, Yeni Kaynak **Grubu** komutunu içeren yeni bir kaynak grubu oluşturun. Kullanmak istediğiniz kaynak grubunun adını ve konumunu sağlayın. Örnek:

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

`Test-AzResourceGroupDeployment` Cmdlet çalıştırarak dağıtım doğrulayın. Dağıtımı sınarken, dağıtımı yürürken tam olarak yaptığınız parametreleri sağlayın.

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Dağıtımı oluşturma

Yeni dağıtım oluşturmak için `New-AzResourceGroupDeployment` cmdlet'i çalıştırın ve istendiğinde gerekli parametreleri sağlayın. Parametreler, dağıtımınız için bir ad, kaynak grubunuzun adını ve şablon dosyasının yolunu veya URL'sini içerir. **Mod** parametresi belirtilmemişse, **Artımlı'nın** varsayılan değeri kullanılır. Daha fazla bilgi için [bkz.](../azure-resource-manager/templates/deployment-modes.md)

Aşağıdaki komut, PowerShell penceresindeki üç parametre için sizi ister:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Bunun yerine bir parametre dosyası belirtmek için aşağıdaki komutu kullanın:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Dağıtım cmdlet'ini çalıştırdığınızda satır içinde parametrelerde de kullanabilirsiniz. Komut aşağıdaki gibidir:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

[Tam](../azure-resource-manager/templates/deployment-modes.md) bir dağıtım çalıştırmak için **Mod** parametresini **Tamamla**olarak ayarlayın:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="verify-the-deployment"></a>Dağıtımı doğrulama
Kaynaklar başarıyla dağıtılırsa, dağıtımın bir özeti PowerShell penceresinde görüntülenir:

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
Azure Kaynak Yöneticisi şablonunu dağıtmak için temel iş akışını ve komutlarını gördünüz. Daha ayrıntılı bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Azure Kaynak Yöneticisi'ne genel bakış][Azure Resource Manager overview]
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma][Deploy resources with Azure Resource Manager templates]
* [Azure Kaynak Yöneticisi şablonları yazma](../azure-resource-manager/templates/template-syntax.md)
* [Microsoft.ServiceBus kaynak türleri](/azure/templates/microsoft.servicebus/allversions)

[Azure Resource Manager overview]: ../azure-resource-manager/management/overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/templates/deploy-powershell.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
