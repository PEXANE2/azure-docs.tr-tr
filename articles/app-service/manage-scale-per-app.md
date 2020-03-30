---
title: Yüksek yoğunluklu barındırma için uygulama başına ölçekleme
description: Uygulamaları Uygulama Hizmeti planlarından bağımsız olarak ölçeklendirin ve planınızdaki ölçeklenmiş örnekleri optimize edin.
author: btardif
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.topic: article
ms.date: 05/13/2019
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: f1ca4958fe2608d0c040ef5b93827a7e71a4151c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74672344"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>Uygulama başına ölçekleme kullanarak Azure Uygulama Hizmeti'nde yüksek yoğunluklu barındırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Uygulama Hizmeti'ni kullanırken, uygulamalarınızı çalıştırdıkları [Uygulama Hizmeti planını](overview-hosting-plans.md) ölçeklendirerek ölçeklendirebilirsiniz. Aynı Uygulama Hizmeti planında birden çok uygulama çalıştırıldığında, her ölçeklenmiş örnek plandaki tüm uygulamaları çalıştırılır.

Uygulama *başına ölçekleme,* bir uygulamayı barındıran Uygulama Hizmeti planından bağımsız olarak ölçeklemesine olanak sağlamak için App Service planı düzeyinde etkinleştirilebilir. Bu şekilde, bir Uygulama Hizmeti planı 10 örneğine ölçeklenebilir, ancak bir uygulama yalnızca beş tane kullanacak şekilde ayarlanabilir.

> [!NOTE]
> Uygulama başına ölçeklendirme yalnızca **Standart,** **Premium, Premium** **V2** ve **İzole** fiyatlandırma katmanları için kullanılabilir.
>

Uygulamalar, örnekler arasında eşit bir dağıtım için en iyi çaba yaklaşımını kullanarak kullanılabilir Uygulama Hizmeti planına tahsis edilir. Eşit bir dağıtım garanti edilmese de, platform aynı uygulamanın iki örneğinin aynı Uygulama Hizmeti planı örneğinde barındırılmayan olmasını sağlayacaktır.

Platform, işçi dağılımına karar vermek için ölçümlere güvenmez. Uygulamalar yalnızca örnekler Eklendiğinde veya Uygulama Hizmeti planından kaldırıldığında yeniden dengelenir.

## <a name="per-app-scaling-using-powershell"></a>PowerShell kullanarak uygulama başına ölçekleme

```-PerSiteScaling $true``` Cmdlet'e ```New-AzAppServicePlan``` parametre geçirerek uygulama başına ölçekleme içeren bir plan oluşturun.

```powershell
New-AzAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Parametreyi `-PerSiteScaling $true` ```Set-AzAppServicePlan``` cmdlet'e geçirerek mevcut bir Uygulama Hizmet Planı ile uygulama başına ölçekleme olanağı sağlar.

```powershell
# Enable per-app scaling for the App Service Plan using the "PerSiteScaling" parameter.
Set-AzAppServicePlan -ResourceGroupName $ResourceGroup `
   -Name $AppServicePlan -PerSiteScaling $true
```

Uygulama düzeyinde, uygulamanın App Service planında kullanabileceği örnek sayısını yapılandırın.

Aşağıdaki örnekte, uygulama, temel uygulama hizmet planının kaç örnekle ölçeklendirildiklerine bakılmaksızın iki örnekle sınırlıdır.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzWebApp -ResourceGroupName $ResourceGroup -Name $webapp

# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2

# Post updated app back to azure
Set-AzWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers`dan `$newapp.MaxNumberOfWorkers`farklıdır. Uygulama başına ölçeklendirme, `$newapp.SiteConfig.NumberOfWorkers` uygulamanın ölçek özelliklerini belirlemek için kullanır.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak uygulama başına ölçekleme

Aşağıdaki Azure Kaynak Yöneticisi şablonu oluşturur:

- 10 örneğe ölçeklenen bir Uygulama Hizmeti planı
- en fazla beş örnekle ölçeklendirecek şekilde yapılandırılan bir uygulama.

Uygulama Hizmeti planı **PerSiteScaling** özelliğini `"perSiteScaling": true`doğru şekilde ayarlıyor. Uygulama, kullanılacak **işçi sayısını** 5'e `"properties": { "numberOfWorkers": "5" }`ayarlıyor.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a>Yüksek yoğunluklu barındırma için önerilen yapılandırma

Uygulama başına ölçekleme, hem genel Azure bölgelerinde hem de [Uygulama Hizmet Ortamlarında](environment/app-service-app-service-environment-intro.md)etkinleştirilen bir özelliktir. Ancak, önerilen strateji, gelişmiş özelliklerinden ve daha büyük App Service planı kapasitesinden yararlanmak için Uygulama Hizmet Ortamlarını kullanmaktır.  

Uygulamalarınız için yüksek yoğunluklu barındırmayı yapılandırmak için aşağıdaki adımları izleyin:

1. Bir Uygulama Hizmeti planını yüksek yoğunluklu plan olarak belirleyin ve istediğiniz kapasiteye ölçeklendirin.
1. `PerSiteScaling` Bayrağı Uygulama Hizmeti planında doğru şekilde ayarlayın.
1. Yeni uygulamalar oluşturulur ve bu App Service planına atanan **numberOfWorkers** özelliği **1**olarak ayarlanır.
   - Bu yapılandırmayı kullanmak mümkün olan en yüksek yoğunluğu sağlar.
1. Çalışan sayısı, gerektiğinde ek kaynak sağlamak için uygulama başına bağımsız olarak yapılandırılabilir. Örnek:
   - Yüksek kullanımlı bir uygulama, bu uygulama için daha fazla işlem kapasitesine sahip olmak için **numberOfWorkers'ı** **3** olarak ayarlayabilir.
   - Düşük kullanımlı uygulamalar **ofworkers sayısını** **1**olarak ayarlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Uygulama Hizmeti'ne derinlemesine genel bakış planları](overview-hosting-plans.md)
- [App Service Ortamı’na giriş](environment/app-service-app-service-environment-intro.md)
