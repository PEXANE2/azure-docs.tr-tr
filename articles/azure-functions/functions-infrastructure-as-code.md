---
title: İşlev uygulaması kaynak dağıtımını Azure'a otomatikleştirin
description: İşlev uygulamanızı dağıtan bir Azure Kaynak Yöneticisi şablonu oluşturmayı öğrenin.
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.topic: conceptual
ms.date: 04/03/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 48d98d6fef896f9288be88824a62fa1c8179217f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276900"
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>Azure İşlevlerinde işlev uygulamanız için kaynak dağıtımını otomatikleştirin

Bir işlev uygulamasını dağıtmak için Azure Kaynak Yöneticisi şablonu kullanabilirsiniz. Bu makalede, bunu yapmak için gerekli kaynaklar ve parametreler özetler. İşlev uygulamanızdaki [tetikleyicilere ve bağlamalara](functions-triggers-bindings.md) bağlı olarak ek kaynaklar dağıtmanız gerekebilir.

Şablon oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/templates/template-syntax.md).

Örnek şablonlar için bkz:
- [Tüketim planında fonksiyon uygulaması]
- [Azure Uygulama Hizmeti planında işlev uygulaması]

## <a name="required-resources"></a>Gerekli kaynaklar

Azure İşlevler dağıtımı genellikle şu kaynaklardan oluşur:

| Kaynak                                                                           | Gereksinim | Sözdizimi ve özellikleri referans                                                         |   |
|------------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------------------------------|---|
| Bir işlev uygulaması                                                                     | Gerekli    | [Microsoft.Web/siteler](/azure/templates/microsoft.web/sites)                             |   |
| Azure [Depolama](../storage/index.yml) hesabı                                   | Gerekli    | [Microsoft.Storage/storageHesapları](/azure/templates/microsoft.storage/storageaccounts) |   |
| Uygulama [Öngörüleri](../azure-monitor/app/app-insights-overview.md) bileşeni | İsteğe bağlı    | [Microsoft.Insights/components](/azure/templates/microsoft.insights/components)         |   |
| Bir [barındırma planı](./functions-scale.md)                                             | İsteğe bağlı<sup>1</sup>    | [Microsoft.Web/serverfarms](/azure/templates/microsoft.web/serverfarms)                 |   |

<sup>1.1.2</sup> Barındırma planı yalnızca işlev uygulamanızı [Premium planda](./functions-premium-plan.md) (önizlemede) veya [Bir Uygulama Hizmeti planında](../app-service/overview-hosting-plans.md)çalıştırmayı seçtiğinizde gereklidir.

> [!TIP]
> Gerekli olmamakla birlikte, uygulamanız için Uygulama Öngörüleri yapılandırmanız önerilir.

<a name="storage"></a>
### <a name="storage-account"></a>Depolama hesabı

Bir işlev uygulaması için Azure depolama hesabı gereklidir. Lekeleri, tabloları, kuyrukları ve dosyaları destekleyen genel bir amaçlı hesaba ihtiyacınız vardır. Daha fazla bilgi için Azure [İşlevler depolama hesabı gereksinimlerine](storage-considerations.md#storage-account-requirements)bakın.

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2019-04-01",
    "location": "[resourceGroup().location]",
    "kind": "StorageV2",
    "sku": {
        "name": "[parameters('storageAccountType')]"
    }
}
```

Ayrıca, özellik `AzureWebJobsStorage` site yapılandırmasında bir uygulama ayarı olarak belirtilmelidir. İşlev uygulaması izleme için Application Insights kullanmıyorsa, `AzureWebJobsDashboard` uygulama ayarı olarak da belirtilmelidir.

Azure İşlevler çalışma `AzureWebJobsStorage` süresi, iç kuyruklar oluşturmak için bağlantı dizesini kullanır.  Uygulama Öngörüleri etkinleştirilemediğinde, çalışma `AzureWebJobsDashboard` zamanı, Azure Table depolamasına günlüğe kaydetmek ve portaldaki **Monitör** sekmesine güç vermek için bağlantı dizesini kullanır.

Bu özellikler `appSettings` `siteConfig` nesnedeki koleksiyonda belirtilir:

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
]
```

### <a name="application-insights"></a>Application Insights

Uygulama Öngörüleri, işlev uygulamalarınızı izlemek için önerilir. Application Insights kaynağı **Microsoft.Insights/components** türü ve **web**türü ile tanımlanır:

```json
        {
            "apiVersion": "2015-05-01",
            "name": "[variables('appInsightsName')]",
            "type": "Microsoft.Insights/components",
            "kind": "web",
            "location": "[resourceGroup().location]",
            "tags": {
                "[concat('hidden-link:', resourceGroup().id, '/providers/Microsoft.Web/sites/', variables('functionAppName'))]": "Resource"
            },
            "properties": {
                "Application_Type": "web",
                "ApplicationId": "[variables('appInsightsName')]"
            }
        },
```

Buna ek olarak, enstrümantasyon anahtarının `APPINSIGHTS_INSTRUMENTATIONKEY` uygulama ayarını kullanarak işlev uygulamasına sağlanması gerekir. Bu özellik `appSettings` `siteConfig` nesnedeki koleksiyonda belirtilir:

```json
"appSettings": [
    {
        "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
    }
]
```

### <a name="hosting-plan"></a>Hosting planı

Barındırma planının tanımı değişir ve aşağıdakilerden biri olabilir:
* [Tüketim planı](#consumption) (varsayılan)
* [Premium plan](#premium) (önizlemede)
* [Uygulama Hizmeti planı](#app-service-plan)

### <a name="function-app"></a>İşlev uygulaması

İşlev uygulaması kaynağı **microsoft.Web/sites** türünde bir kaynak ve tür **functionapp**kullanılarak tanımlanır:

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
    ]
}
```

> [!IMPORTANT]
> Bir barındırma planını açıkça tanımlıyorsanız, bağlı bağlı dizide ek bir öğe gerekir:`"[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"`

Bir işlev uygulaması şu uygulama ayarlarını içermelidir:

| Ayar adı                 | Açıklama                                                                               | Örnek değerler                        |
|------------------------------|-------------------------------------------------------------------------------------------|---------------------------------------|
| AzureWebJobsStorage          | İşlevler çalışma zamanı iç sıraya için kullandığı bir depolama hesabına bağlantı dizesi | [Bkz. Depolama hesabı](#storage)       |
| FUNCTIONS_EXTENSION_VERSION  | Azure İşlevleri çalışma zamanı sürümü                                                | `~2`                                  |
| FUNCTIONS_WORKER_RUNTIME     | Bu uygulamadaki işlevler için kullanılacak dil yığını                                   | `dotnet`, `node` `java`, `python`, veya`powershell` |
| WEBSITE_NODE_DEFAULT_VERSION | Yalnızca dil yığınını `node` kullanıyorsanız gerekli, kullanılacak sürümü belirtir              | `10.14.1`                             |

Bu `appSettings` `siteConfig` özellikler, özellikteki koleksiyonda belirtilir:

```json
"properties": {
    "siteConfig": {
        "appSettings": [
            {
                "name": "AzureWebJobsStorage",
                "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
            },
            {
                "name": "FUNCTIONS_WORKER_RUNTIME",
                "value": "node"
            },
            {
                "name": "WEBSITE_NODE_DEFAULT_VERSION",
                "value": "10.14.1"
            },
            {
                "name": "FUNCTIONS_EXTENSION_VERSION",
                "value": "~2"
            }
        ]
    }
}
```

<a name="consumption"></a>

## <a name="deploy-on-consumption-plan"></a>Tüketim planında dağıt

Tüketim planı, kodunuz çalışırken otomatik olarak işlem gücü ayırır, yükü işlemek için gerekli ölçeklendirilir ve kod çalışmadığında ölçeklenir. Boştaki VM'ler için ödeme yapmak zorunda değilsiniz ve kapasiteyi önceden rezerve etmek zorunda değilsiniz. Daha fazla bilgi edinmek için Azure [İşlevlerini ölçeklendirme ve barındırma](functions-scale.md#consumption-plan)'ya bakın.

Örnek bir Azure Kaynak Yöneticisi şablonu için [Tüketim planında İşlev uygulamasına]bakın.

### <a name="create-a-consumption-plan"></a>Tüketim planı oluşturma

Tüketim planının tanımlanması gerekmez. İşlev uygulama kaynağının kendisini oluşturduğunuzda, bölge başına olarak otomatik olarak oluşturulur veya seçilir.

Tüketim planı özel bir "serverfarm" kaynağı türüdür. Windows için, aşağıdaki özelliklerin `Dynamic` değerini `computeMode` `sku` kullanarak belirtebilirsiniz:

```json
{  
   "type":"Microsoft.Web/serverfarms",
   "apiVersion":"2016-09-01",
   "name":"[variables('hostingPlanName')]",
   "location":"[resourceGroup().location]",
   "properties":{  
      "name":"[variables('hostingPlanName')]",
      "computeMode":"Dynamic"
   },
   "sku":{  
      "name":"Y1",
      "tier":"Dynamic",
      "size":"Y1",
      "family":"Y",
      "capacity":0
   }
}
```

> [!NOTE]
> Tüketim planı Linux için açıkça tanımlanamaz. Otomatik olarak oluşturulur.

Tüketim planınızı açıkça tanımlarsanız, uygulamanın `serverFarmId` özelliğini planın kaynak kimliğini işaret edecek şekilde ayarlamanız gerekir. İşlev uygulamasının da plan `dependsOn` için bir ayar olduğundan emin olmalısınız.

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

#### <a name="windows"></a>Windows

Windows'da Tüketim planı, site yapılandırmasında iki `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` `WEBSITE_CONTENTSHARE`ek ayar gerektirir: ve. Bu özellikler, işlev uygulama kodu ve yapılandırmasının depolandığı depolama hesabını ve dosya yolunu yapılandırır.

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```

#### <a name="linux"></a>Linux

Linux'ta, işlev uygulaması `kind` kendi `functionapp,linux`ayarlamak gerekir , `reserved` ve özellik `true`için ayarlanmış olmalıdır:

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp,linux",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountName'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        },
        "reserved": true
    }
}
```



<a name="premium"></a>

## <a name="deploy-on-premium-plan"></a>Premium plana dağıtın

Premium plan, Tüketim planıyla aynı ölçekleme sunar, ancak özel kaynaklar ve ek özellikler içerir. Daha fazla bilgi için [Azure İşlevler Premium Planı'na](./functions-premium-plan.md)bakın.

### <a name="create-a-premium-plan"></a>Premium plan oluşturma

Premium plan, özel bir "serverfarm" kaynağı türüdür. `EP1`Bunu, `EP2`açıklama `EP3` `Name` `sku` [nesnesindeki](https://docs.microsoft.com/azure/templates/microsoft.web/2018-02-01/serverfarms#skudescription-object)özellik değerini kullanarak belirtebilirsiniz.

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2018-02-01",
    "name": "[parameters('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[parameters('hostingPlanName')]",
        "workerSize": "[parameters('workerSize')]",
        "workerSizeId": "[parameters('workerSizeId')]",
        "numberOfWorkers": "[parameters('numberOfWorkers')]",
        "hostingEnvironment": "[parameters('hostingEnvironment')]",
        "maximumElasticWorkerCount": "20"
    },
    "sku": {
        "Tier": "ElasticPremium",
        "Name": "EP1"
    }
}
```

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Premium plandaki bir işlev uygulaması, `serverFarmId` özelliğin daha önce oluşturulan planın kaynak kimliğine ayarlamış olması gerekir. Buna ek olarak, Premium plan site yapılandırmasında `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` `WEBSITE_CONTENTSHARE`iki ek ayar gerektirir: ve. Bu özellikler, işlev uygulama kodu ve yapılandırmasının depolandığı depolama hesabını ve dosya yolunu yapılandırır.

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```


<a name="app-service-plan"></a>

## <a name="deploy-on-app-service-plan"></a>Uygulama Hizmeti planında dağıt

Uygulama Hizmeti planında, işlev uygulamanız web uygulamalarına benzer şekilde Temel, Standart ve Premium SK'lerde özel VM'lerde çalışır. Uygulama Hizmeti planının nasıl çalıştığı hakkında ayrıntılı bilgi için [Azure Uygulama Hizmeti planlarına ayrıntılı bir bakış](../app-service/overview-hosting-plans.md)bakın.

Örnek bir Azure Kaynak Yöneticisi şablonu için [Azure Uygulama Hizmeti planında İşlev uygulamasına]bakın.

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

Uygulama Hizmeti planı bir "serverfarm" kaynağı tarafından tanımlanır.

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2018-02-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "S1",
        "tier": "Standard",
        "size": "S1",
        "family": "S",
        "capacity": 1
    }
}
```

Uygulamanızı Linux'ta çalıştırmak `kind` için aşağıdakileri `Linux`de ayarlamanız gerekir:

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2018-02-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "kind": "Linux",
    "sku": {
        "name": "S1",
        "tier": "Standard",
        "size": "S1",
        "family": "S",
        "capacity": 1
    }
}
```

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Uygulama Hizmeti planındaki bir işlev `serverFarmId` uygulaması, özelliğin daha önce oluşturulan planın kaynak kimliğine ayarlamış olması gerekir.

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```

Linux uygulamaları da `linuxFxVersion` altında `siteConfig`bir özellik içermelidir. Sadece kod dağıtıyorsanız, bunun değeri istediğiniz çalışma zamanı yığını tarafından belirlenir:

| Yığın            | Örnek değer                                         |
|------------------|-------------------------------------------------------|
| Python           | `DOCKER|microsoft/azure-functions-python3.6:2.0`      |
| JavaScript       | `DOCKER|microsoft/azure-functions-node8:2.0`          |
| .NET             | `DOCKER|microsoft/azure-functions-dotnet-core2.0:2.0` |

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ],
            "linuxFxVersion": "DOCKER|microsoft/azure-functions-node8:2.0"
        }
    }
}
```

[Özel bir kapsayıcı görüntüsü dağıtıyorsanız,](./functions-create-function-linux-custom-image.md) `linuxFxVersion` [kapsayıcılar için Web App'te](/azure/app-service/containers)olduğu gibi resminizin çekilmesine izin veren yapılandırmayı belirtmeniz ve eklemeniz gerekir. Ayrıca, `WEBSITES_ENABLE_APP_SERVICE_STORAGE` uygulama `false`içeriğiniz kapsayıcının kendisinde sağlandığı için aşağıdakileri ayarla:

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_URL",
                    "value": "[parameters('dockerRegistryUrl')]"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
                    "value": "[parameters('dockerRegistryUsername')]"
                },
                {
                    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
                    "value": "[parameters('dockerRegistryPassword')]"
                },
                {
                    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
                    "value": "false"
                }
            ],
            "linuxFxVersion": "DOCKER|myacr.azurecr.io/myimage:mytag"
        }
    }
}
```

## <a name="customizing-a-deployment"></a>Dağıtımözelleştirme

Bir işlev uygulaması, uygulama ayarları ve kaynak denetim seçenekleri de dahil olmak üzere dağıtımSırasında kullanabileceğiniz birçok alt kaynağa sahiptir. Kaynak **denetimleri** alt kaynağını kaldırmayı ve bunun yerine farklı bir [dağıtım seçeneği](functions-continuous-deployment.md) kullanmayı da seçebilirsiniz.

> [!IMPORTANT]
> Azure Kaynak Yöneticisi'ni kullanarak uygulamanızı başarıyla dağıtmak için, kaynakların Azure'da nasıl dağıtılandığını anlamak önemlidir. Aşağıdaki örnekte, üst düzey yapılandırmalar **siteConfig**kullanılarak uygulanır. Bu yapılandırmaları en üst düzeyde ayarlamak önemlidir, çünkü bu yapılandırmalar işlevleri çalışma zamanı ve dağıtım motoruna bilgi iletmektedir. Alt **kaynak denetimleri/web** kaynağı uygulanmadan önce üst düzey bilgiler gereklidir. Bu ayarları alt düzey **config/appSettings** kaynağında yapılandırmak mümkün olsa da, bazı durumlarda işlev uygulamanızın **config/appSettings** *uygulanmadan önce* dağıtılması gerekir. Örneğin, [Logic Apps](../logic-apps/index.yml)ile işlevleri kullanırken, işlevleriniz başka bir kaynağın bağımlılığıdır.

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('appName')]",
  "type": "Microsoft.Web/sites",
  "kind": "functionapp",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Web/serverfarms', parameters('appName'))]"
  ],
  "properties": {
     "serverFarmId": "[variables('appServicePlanName')]",
     "siteConfig": {
        "alwaysOn": true,
        "appSettings": [
            {
                "name": "FUNCTIONS_EXTENSION_VERSION",
                "value": "~2"
            },
            {
                "name": "Project",
                "value": "src"
            }
        ]
     }
  },
  "resources": [
     {
        "apiVersion": "2015-08-01",
        "name": "appsettings",
        "type": "config",
        "dependsOn": [
          "[resourceId('Microsoft.Web/Sites', parameters('appName'))]",
          "[resourceId('Microsoft.Web/Sites/sourcecontrols', parameters('appName'), 'web')]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ],
        "properties": {
          "AzureWebJobsStorage": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "FUNCTIONS_EXTENSION_VERSION": "~2",
          "FUNCTIONS_WORKER_RUNTIME": "dotnet",
          "Project": "src"
        }
     },
     {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites/', parameters('appName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('sourceCodeRepositoryURL')]",
            "branch": "[parameters('sourceCodeBranch')]",
            "IsManualIntegration": "[parameters('sourceCodeManualIntegration')]"
          }
     }
  ]
}
```
> [!TIP]
> Bu şablon, İşlevdağıtım altyapısının (Kudu) dağıtılabilir kodu aradığı temel dizini ayarlayan [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) uygulaması ayarları değerini kullanır. Depomuzda, işlevlerimiz **src** klasörünün bir alt klasöründedir. Bu nedenle, önceki örnekte, uygulama ayarları `src`değerini . İşlevleriniz deponuzun kökündeyse veya kaynak denetiminden dağıtım yapmadıysanız, bu uygulama ayarları değerini kaldırabilirsiniz.

## <a name="deploy-your-template"></a>Şablonunuzu dağıtma

Şablonunuzu dağıtmak için aşağıdaki yollardan birini kullanabilirsiniz:

* [Powershell](../azure-resource-manager/templates/deploy-powershell.md)
* [Azure CLI](../azure-resource-manager/templates/deploy-cli.md)
* [Azure portalında](../azure-resource-manager/templates/deploy-portal.md)
* [REST API](../azure-resource-manager/templates/deploy-rest.md)

### <a name="deploy-to-azure-button"></a>Azure düğmesine dağıt

GitHub'da dosyanızın ```<url-encoded-path-to-azuredeploy-json>``` `azuredeploy.json` ham yolunun URL [kodlanmış](https://www.bing.com/search?q=url+encode) bir sürümüyle değiştirin.

Aşağıda, işaretleme yi kullanan bir örnek verilmiştir:

```markdown
[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

Aşağıda HTML kullanan bir örnek verilmiştir:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"></a>
```

### <a name="deploy-using-powershell"></a>PowerShell kullanarak dağıtma

Aşağıdaki PowerShell komutları bir kaynak grubu oluşturur ve gerekli kaynakları olan bir işlev uygulaması oluşturan bir şablon dağıtır. Yerel olarak çalışmak için [Azure PowerShell](/powershell/azure/install-az-ps) yüklü olması gerekir. Oturum [`Connect-AzAccount`](/powershell/module/az.accounts/connect-azaccount) etmek için koş.

```powershell
# Register Resource Providers if they're not already registered
Register-AzResourceProvider -ProviderNamespace "microsoft.web"
Register-AzResourceProvider -ProviderNamespace "microsoft.storage"

# Create a resource group for the function app
New-AzResourceGroup -Name "MyResourceGroup" -Location 'West Europe'

# Create the parameters for the file, which for this template is the function app name.
$TemplateParams = @{"appName" = "<function-app-name>"}

# Deploy the template
New-AzResourceGroupDeployment -ResourceGroupName "MyResourceGroup" -TemplateFile template.json -TemplateParameterObject $TemplateParams -Verbose
```

Bu dağıtımı test etmek için, bir Tüketim planında Windows'da bir işlev uygulaması oluşturan [bunun gibi](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-function-app-create-dynamic/azuredeploy.json) bir şablon kullanabilirsiniz. İşlev uygulamanız için benzersiz bir adla değiştirin. `<function-app-name>`

## <a name="next-steps"></a>Sonraki adımlar

Azure İşlevlerini nasıl geliştirip yapılandırılabildiğini zedekçe öğrenin.

* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlev uygulama ayarlarını yapılandırma](functions-how-to-use-azure-function-app-settings.md)
* [İlk Azure işlevinizi oluşturun](functions-create-first-azure-function.md)

<!-- LINKS -->

[Tüketim planında fonksiyon uygulaması]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[Azure Uygulama Hizmeti planında işlev uygulaması]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
