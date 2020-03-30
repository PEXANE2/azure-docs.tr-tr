---
title: Azure Kaynak Yöneticisi ile Redis için Azure Önbelleği Dağıtma
description: Redis için bir Azure Önbelleği dağıtmak için Azure Kaynak Yöneticisi şablonu nasıl kullanacağınızı öğrenin. Şablonlar sık karşılaşılan senaryolar için sağlanır.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 01/23/2017
ms.openlocfilehash: 787edf662aa3a34e167db61b0a89dfc5c2944219
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75412401"
---
# <a name="create-an-azure-cache-for-redis-using-a-template"></a>Şablon kullanarak Redis için Azure Önbelleği oluşturma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu konuda, Redis için bir Azure Önbelleği dağıtan bir Azure Kaynak Yöneticisi şablonu oluşturmayı öğrenirsiniz. Önbellek, tanılama verilerini tutmak için varolan bir depolama hesabıyla kullanılabilir. Ayrıca, hangi kaynakların dağıtılanını nasıl tanımlayacağını ve dağıtım sürürken belirtilen parametrelerin nasıl tanınırılacağını da öğrenirsiniz. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şu anda, tanılama ayarları bir abonelik için aynı bölgedeki tüm önbellekler için paylaşılır. Bölgedeki bir önbelleğe güncelleştirin, bölgedeki diğer tüm önbellekleri etkiler.

Şablon oluşturma hakkında daha fazla bilgi için [bkz.](../azure-resource-manager/templates/template-syntax.md) Önbellek kaynak türleri için JSON sözdizimi ve özellikleri hakkında bilgi edinmek için [Bkz. Microsoft.Cache kaynak türleri.](/azure/templates/microsoft.cache/allversions)

Şablonun tamamı [için Redis için Azure Önbelleği şablonuna](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json)bakın.

> [!NOTE]
> Yeni [Premium katmanı](cache-premium-tier-intro.md) için Kaynak Yöneticisi şablonları kullanılabilir. 
> 
> * [Kümeleme ile Redis için Premium Azure Önbelleği Oluşturma](https://azure.microsoft.com/resources/templates/201-redis-premium-cluster-diagnostics/)
> * [Veri kalıcılığıyla Redis için Premium Azure Önbelleği oluşturun](https://azure.microsoft.com/resources/templates/201-redis-premium-persistence/)
> * [Sanal Ağa dağıtılan Premium Redis Önbelleği oluşturma](https://azure.microsoft.com/resources/templates/201-redis-premium-vnet/)
> 
> En son şablonları denetlemek için [Azure Quickstart Şablonları'na](https://azure.microsoft.com/documentation/templates/) bakın ve ' ı arayın. `Azure Cache for Redis`
> 
> 

## <a name="what-you-will-deploy"></a>Ne dağıtacağınız
Bu şablonda, tanılama verileri için varolan bir depolama hesabı kullanan Redis için bir Azure Önbelleği dağıtırsınız.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure'a Dağıt](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler
Azure Resource Manager sayesinde, şablon dağıtıldığında belirtmek istediğiniz değerlerin parametrelerini siz tanımlarsınız. Şablon, parametre değerlerinin tümünün bulunduğu Parametreler adlı bir bölüm içerir.
Dağıtmakta olduğunuz projeye veya dağıtım yaptığınız ortama göre değişen değerler için bir parametre tanımlamanız gerekir. Her zaman aynı kalan değerler için parametre tanımlamayın. Her parametre değeri, dağıtılan kaynakları tanımlamak için şablonda kullanılır. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
Redis için Azure Önbelleğinin konumu. En iyi performans için, önbellekte kullanılacak uygulamayla aynı konumu kullanın.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>mevcutDiagnosticsStorageAccountName
Tanılama için kullanılacak varolan depolama hesabının adı. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort
SSL olmayan bağlantı noktaları üzerinden erişime izin verip vermemeyi gösteren bir boolean değeri.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>tanılamaDurum
Tanılamanın etkin olup olmadığını gösteren bir değer. AVEYA KAPALI olarak kullanın.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
### <a name="azure-cache-for-redis"></a>Redis için Azure Önbelleği
Redis için Azure Önbelleği oluşturur.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2017-05-01-preview",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup
