---
title: Redis için Azure Önbelleği ile Web Uygulaması Sağlama
description: Redis için Azure Önbelleği ile web uygulamasını dağıtmak için Azure Kaynak Yöneticisi şablonunu kullanın.
services: app-service
author: yegu-ms
ms.service: app-service
ms.topic: conceptual
ms.date: 01/06/2017
ms.author: yegu
ms.openlocfilehash: 11c854491ab030394eb61964979cb04a5a4b489b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75433388"
---
# <a name="create-a-web-app-plus-azure-cache-for-redis-using-a-template"></a>Şablon kullanarak Redis için Bir Web Uygulaması ve Azure Önbelleği oluşturma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu konuda, Redis için Azure Önbelleği ile bir Azure Web Uygulaması dağıtan bir Azure Kaynak Yöneticisi şablonu oluşturmayı öğreneceksiniz. Hangi kaynakların dağıtılanını nasıl tanımladığınızı ve dağıtım yürütüldüğünde belirtilen parametreleri nasıl tanımladığınızı öğreneceksiniz. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şablon oluşturma hakkında daha fazla bilgi için [bkz.](../azure-resource-manager/templates/template-syntax.md) Önbellek kaynak türleri için JSON sözdizimi ve özellikleri hakkında bilgi edinmek için [Bkz. Microsoft.Cache kaynak türleri.](/azure/templates/microsoft.cache/allversions)

Şablonun tamamı için [Redis için Azure Önbelleği içeren Web Uygulaması'na](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json)bakın.

## <a name="what-you-will-deploy"></a>Ne dağıtacağınız
Bu şablonda:

* Azure Web App
* Redis için Azure Önbelleği

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure'a Dağıt](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)

## <a name="parameters-to-specify"></a>Belirtecek parametreler
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[!INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a>Adlar için değişkenler
Bu şablon, kaynakların adlarını oluşturmak için değişkenler kullanır. Kaynak grubu kimliğine dayalı bir değer oluşturmak için [uniqueString](../azure-resource-manager/templates/template-functions-string.md#uniquestring) işlevini kullanır.

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="azure-cache-for-redis"></a>Redis için Azure Önbelleği
Web uygulamasıyla birlikte kullanılan Redis için Azure Önbelleği oluşturur. Önbelleğin adı **önbellek Adı** değişkeninde belirtilir.

Şablon önbelleği kaynak grubuyla aynı konumda oluşturur.

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### <a name="web-app"></a>Web uygulaması
**WebSiteName** değişkeninde belirtilen adla web uygulaması oluşturur.

Web uygulamasının Redis için Azure Önbelleği ile çalışmasını sağlayan uygulama ayar özellikleriyle yapılandırıldığına dikkat edin. Bu uygulama ayarları dağıtım sırasında sağlanan değerlere göre dinamik olarak oluşturulur.

    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup
