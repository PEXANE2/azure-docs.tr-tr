---
title: PowerShell ile Azure Uygulama Öngörülerini Otomatikleştirin | Microsoft Dokümanlar
description: Azure Kaynak Yöneticisi şablonu kullanarak PowerShell'de kaynak, uyarı ve kullanılabilirlik testlerini oluşturmayı ve yönetmeyi otomatikleştirin.
ms.topic: conceptual
ms.date: 10/17/2019
ms.openlocfilehash: 9494b659b5b4357f3190c45d8cc72c4e130f0ecc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275886"
---
#  <a name="manage-application-insights-resources-using-powershell"></a>PowerShell'i kullanarak Uygulama Öngörüleri kaynaklarını yönetme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, Azure Kaynak Yönetimi'ni kullanarak [Application Insights](../../azure-monitor/app/app-insights-overview.md) kaynaklarının oluşturulmasını ve güncelleştirilmeşeklini ve otomatik olarak nasıl güncelleştirilebildiğinizgösterilmektedir. Örneğin, bunu bir yapı işleminin parçası olarak yapabilirsiniz. Temel Application Insights kaynağının yanı sıra, [kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)oluşturabilir, [uyarıları](../../azure-monitor/app/alerts.md)ayarlayabilir, [fiyatlandırma düzenini](pricing.md)ayarlayabilir ve diğer Azure kaynaklarını oluşturabilirsiniz.

Bu kaynakları oluşturmanın anahtarı [Azure Kaynak Yöneticisi](../../azure-resource-manager/management/manage-resources-powershell.md)için JSON şablonlarıdır. Temel yordam: varolan kaynakların JSON tanımlarını indirin; adlar gibi belirli değerleri parametreize; ve ardından yeni bir kaynak oluşturmak istediğinizde şablonu çalıştırın. Birden fazla kaynağı tek bir denemede oluşturmak için birkaç kaynağı bir araya getirebilirsiniz - örneğin, sürekli dışa aktarma için kullanılabilirlik testleri, uyarıları ve depolama alanına sahip bir uygulama monitörü. Burada açıklayacağımız bazı parametrelemelerin bazı incelikleri vardır.

## <a name="one-time-setup"></a>Tek seferlik kurulum
PowerShell'i azure aboneliğinizle daha önce kullanmadıysanız:

Azure Powershell modüllerini komut dosyalarını çalıştırmak istediğiniz makineye yükleyin:

1. [Microsoft Web Platform Installer (v5 veya üstü)](https://www.microsoft.com/web/downloads/platform.aspx)yükleyin.
2. Microsoft Azure Powershell'i yüklemek için kullanın.

Kaynak Yöneticisi şablonlarını kullanmanın yanı sıra, Uygulama Öngörüleri kaynaklarını programlı olarak yapılandırmayı kolaylaştıran zengin bir [Uygulama Öngörüleri PowerShell cmdlets](https://docs.microsoft.com/powershell/module/az.applicationinsights)kümesi de vardır. Cmdletlerin sağladığı özellikler şunlardır:

* Application Insights kaynaklarını oluşturma ve silme
* Uygulama Öngörüleri kaynaklarının ve özelliklerinin listesini alın
* Sürekli Dışa Aktarma oluşturma ve yönetme
* Uygulama Anahtarları oluşturma ve yönetme
* Günlük Kapağı Nı Ayarla
* Fiyatlandırma Planını Ayarlama

## <a name="create-application-insights-resources-using-a-powershell-cmdlet"></a>PowerShell cmdlet kullanarak Uygulama Öngörüleri kaynakları oluşturma

[Yeni-AzApplicationInsights](https://docs.microsoft.com/powershell/module/az.applicationinsights/New-AzApplicationInsights) cmdlet'i kullanarak Azure Doğu ABD veri merkezinde yeni bir Uygulama Öngörüleri kaynağı nın nasıl oluşturulabildiğini aşağıda açıklayabilirsiniz:

```PS
New-AzApplicationInsights -ResourceGroupName <resource group> -Name <resource name> -location eastus
```


## <a name="create-application-insights-resources-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak Uygulama Öngörüleri kaynakları oluşturma

Kaynak Yöneticisi şablonu kullanarak yeni bir Uygulama Öngörüleri kaynağının nasıl oluşturulabildiğini aşağıda açıklayabilirsiniz.

### <a name="create-the-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu oluşturma

Yeni bir .json dosyası oluşturun - `template1.json` bu örnekte çağıralım. Bu içeriği kopyalayın:

```JSON
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "appName": {
                "type": "string",
                "metadata": {
                    "description": "Enter the name of your Application Insights resource."
                }
            },
            "appType": {
                "type": "string",
                "defaultValue": "web",
                "allowedValues": [
                    "web",
                    "java",
                    "other"
                ],
                "metadata": {
                    "description": "Enter the type of the monitored application."
                }
            },
            "appLocation": {
                "type": "string",
                "defaultValue": "eastus",
                "metadata": {
                    "description": "Enter the location of your Application Insights resource."
                }
            },
            "retentionInDays": {
                "type": "int",
                "defaultValue": 90,
                "allowedValues": [
                    30,
                    60,
                    90,
                    120,
                    180,
                    270,
                    365,
                    550,
                    730
                ],
                "metadata": {
                    "description": "Data retention in days"
                }
            },
            "ImmediatePurgeDataOn30Days": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. Use this with extreme caution. This only applies when retention is being set to 30 days."
                }
            },
            "priceCode": {
                "type": "int",
                "defaultValue": 1,
                "allowedValues": [
                    1,
                    2
                ],
                "metadata": {
                    "description": "Pricing plan: 1 = Per GB (or legacy Basic plan), 2 = Per Node (legacy Enterprise plan)"
                }
            },
            "dailyQuota": {
                "type": "int",
                "defaultValue": 100,
                "minValue": 1,
                "metadata": {
                    "description": "Enter daily quota in GB."
                }
            },
            "dailyQuotaResetTime": {
                "type": "int",
                "defaultValue": 0,
                "metadata": {
                    "description": "Enter daily quota reset hour in UTC (0 to 23). Values outside the range will get a random reset hour."
                }
            },
            "warningThreshold": {
                "type": "int",
                "defaultValue": 90,
                "minValue": 1,
                "maxValue": 100,
                "metadata": {
                    "description": "Enter the % value of daily quota after which warning mail to be sent. "
                }
            }
        },
        "variables": {
            "priceArray": [
                "Basic",
                "Application Insights Enterprise"
            ],
            "pricePlan": "[take(variables('priceArray'),parameters('priceCode'))]",
            "billingplan": "[concat(parameters('appName'),'/', variables('pricePlan')[0])]"
        },
        "resources": [
            {
                "type": "microsoft.insights/components",
                "kind": "[parameters('appType')]",
                "name": "[parameters('appName')]",
                "apiVersion": "2014-04-01",
                "location": "[parameters('appLocation')]",
                "tags": {},
                "properties": {
                    "ApplicationId": "[parameters('appName')]",
                    "retentionInDays": "[parameters('retentionInDays')]"
                },
                "dependsOn": []
            },
            {
                "name": "[variables('billingplan')]",
                "type": "microsoft.insights/components/CurrentBillingFeatures",
                "location": "[parameters('appLocation')]",
                "apiVersion": "2015-05-01",
                "dependsOn": [
                    "[resourceId('microsoft.insights/components', parameters('appName'))]"
                ],
                "properties": {
                    "CurrentBillingFeatures": "[variables('pricePlan')]",
                    "DataVolumeCap": {
                        "Cap": "[parameters('dailyQuota')]",
                        "WarningThreshold": "[parameters('warningThreshold')]",
                        "ResetTime": "[parameters('dailyQuotaResetTime')]"
                    }
                }
            }
        ]
    }
```

### <a name="use-the-resource-manager-template-to-create-a-new-application-insights-resource"></a>Yeni bir Uygulama Öngörüleri kaynağı oluşturmak için Kaynak Yöneticisi şablonunu kullanma

1. PowerShell'de, Azure'da oturum açarak oturum açın`$Connect-AzAccount`
2. Bağlamınızı bir aboneye ayarlama`Set-AzContext "<subscription ID>"`
2. Yeni bir Uygulama Öngörüleri kaynağı oluşturmak için yeni bir dağıtım çalıştırın:
   
    ```PS
        New-AzResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName`yeni kaynaklar oluşturmak istediğiniz gruptur.
   * `-TemplateFile`özel parametrelerden önce oluşmalıdır.
   * `-appName`Oluşturulacak kaynağın adı.

Diğer parametrelerekleyebilirsiniz - açıklamalarını şablonun parametreler bölümünde bulabilirsiniz.

## <a name="get-the-instrumentation-key"></a>Enstrümantasyon anahtarını alın

Bir uygulama kaynağı oluşturduktan sonra, enstrümantasyon anahtarını isteyeceksiniz: 

1. `$Connect-AzAccount`
2. `Set-AzContext "<subscription ID>"`
3. `$resource = Get-AzResource -Name "<resource name>" -ResourceType "Microsoft.Insights/components"`
4. `$details = Get-AzResource -ResourceId $resource.ResourceId`
5. `$details.Properties.InstrumentationKey`

Application Insights kaynağınızın diğer birçok özelliğinin listesini görmek için şunları kullanın:

```PS
Get-AzApplicationInsights -ResourceGroupName Fabrikam -Name FabrikamProd | Format-List
```

Ek özellikler cmdlets üzerinden mevcuttur:
* `Set-AzApplicationInsightsDailyCap`
* `Set-AzApplicationInsightsPricingPlan`
* `Get-AzApplicationInsightsApiKey`
* `Get-AzApplicationInsightsContinuousExport`

Bu cmdlets için parametreler için [ayrıntılı belgelere](https://docs.microsoft.com/powershell/module/az.applicationinsights) bakın.  

## <a name="set-the-data-retention"></a>Veri saklamayı ayarlama 

Application Insights kaynağınız için geçerli veri saklamayı almak için OSS aracı [ARMClient'ı](https://github.com/projectkudu/ARMClient)kullanabilirsiniz.  (David [Ebbo](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ve [Daniel Bowbyes'in](https://blog.bowbyes.co.nz/2016/11/02/using-armclient-to-directly-access-azure-arm-rest-apis-and-list-arm-policy-details/)makalelerinden ARMClient hakkında daha fazla bilgi edinin.)  Geçerli bekletme almak `ARMClient`için aşağıdaki leri kullanan bir örnek verilmiştir:

```PS
armclient GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/microsoft.insights/components/MyResourceName?api-version=2018-05-01-preview
```

Bekletme ayarlamak için komut benzer bir PUT'tur:

```PS
armclient PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/microsoft.insights/components/MyResourceName?api-version=2018-05-01-preview "{location: 'eastus', properties: {'retentionInDays': 365}}"
```

Yukarıdaki şablonu kullanarak veri saklamayı 365 güne ayarlamak için çalıştırın:

```PS
New-AzResourceGroupDeployment -ResourceGroupName "<resource group>" `
       -TemplateFile .\template1.json `
       -retentionInDays 365 `
       -appName myApp
```

Bekletme değiştirmek için aşağıdaki komut dosyası da kullanılabilir. Olarak `Set-ApplicationInsightsRetention.ps1`kaydetmek için bu komut dosyasını kopyalayın

```PS
Param(
    [Parameter(Mandatory = $True)]
    [string]$SubscriptionId,

    [Parameter(Mandatory = $True)]
    [string]$ResourceGroupName,

    [Parameter(Mandatory = $True)]
    [string]$Name,

    [Parameter(Mandatory = $True)]
    [string]$RetentionInDays
)
$ErrorActionPreference = 'Stop'
if (-not (Get-Module Az.Accounts)) {
    Import-Module Az.Accounts
}
$azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
if (-not $azProfile.Accounts.Count) {
    Write-Error "Ensure you have logged in before calling this function."    
}
$currentAzureContext = Get-AzContext
$profileClient = New-Object Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient($azProfile)
$token = $profileClient.AcquireAccessToken($currentAzureContext.Tenant.TenantId)
$UserToken = $token.AccessToken
$RequestUri = "https://management.azure.com/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Insights/components/$($Name)?api-version=2015-05-01"
$Headers = @{
    "Authorization"         = "Bearer $UserToken"
    "x-ms-client-tenant-id" = $currentAzureContext.Tenant.TenantId
}
## Get Component object via ARM
$GetResponse = Invoke-RestMethod -Method "GET" -Uri $RequestUri -Headers $Headers 

## Update RetentionInDays property
if($($GetResponse.properties | Get-Member "RetentionInDays"))
{
    $GetResponse.properties.RetentionInDays = $RetentionInDays
}
else
{
    $GetResponse.properties | Add-Member -Type NoteProperty -Name "RetentionInDays" -Value $RetentionInDays
}
## Upsert Component object via ARM
$PutResponse = Invoke-RestMethod -Method "PUT" -Uri "$($RequestUri)" -Headers $Headers -Body $($GetResponse | ConvertTo-Json) -ContentType "application/json"
$PutResponse
```

Bu komut dosyası daha sonra şu şekilde kullanılabilir:

```PS
Set-ApplicationInsightsRetention `
        [-SubscriptionId] <String> `
        [-ResourceGroupName] <String> `
        [-Name] <String> `
        [-RetentionInDays <Int>]
```

## <a name="set-the-daily-cap"></a>Günlük kapağı ayarlama

Günlük kapak özelliklerini almak için [Set-AzApplicationInsightsPricingPlan](https://docs.microsoft.com/powershell/module/az.applicationinsights/Set-AzApplicationInsightsPricingPlan) cmdlet'i kullanın: 

```PS
Set-AzApplicationInsightsDailyCap -ResourceGroupName <resource group> -Name <resource name> | Format-List
```

Günlük kapak özelliklerini ayarlamak için aynı cmdlet kullanın. Örneğin, kapağı 300 GB/gün olarak ayarlamak için,

```PS
Set-AzApplicationInsightsDailyCap -ResourceGroupName <resource group> -Name <resource name> -DailyCapGB 300
```

Günlük kapak parametrelerini almak ve ayarlamak için [ARMClient'ı](https://github.com/projectkudu/ARMClient) da kullanabilirsiniz.  Geçerli değerleri almak için şunları kullanın:

```PS
armclient GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/microsoft.insights/components/MyResourceName/CurrentBillingFeatures?api-version=2018-05-01-preview
```

## <a name="set-the-daily-cap-reset-time"></a>Günlük kapak sıfırlama süresini ayarlama

Günlük kapak sıfırlama süresini ayarlamak için [ARMClient'ı](https://github.com/projectkudu/ARMClient)kullanabilirsiniz. Sıfırlama saatini yeni `ARMClient`bir saate ayarlamak için bir örnek verilmiştir (bu örnekte 12:00 UTC):

```PS
armclient PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/microsoft.insights/components/MyResourceName/CurrentBillingFeatures?api-version=2018-05-01-preview "{'CurrentBillingFeatures':['Basic'],'DataVolumeCap':{'ResetTime':12}}"
```

<a id="price"></a>
## <a name="set-the-pricing-plan"></a>Fiyatlandırma planını ayarlama 

Geçerli fiyatlandırma planını almak için [Set-AzApplicationInsightsPricingPlan](https://docs.microsoft.com/powershell/module/az.applicationinsights/Set-AzApplicationInsightsPricingPlan) cmdlet'i kullanın:

```PS
Set-AzApplicationInsightsPricingPlan -ResourceGroupName <resource group> -Name <resource name> | Format-List
```

Fiyatlandırma planını ayarlamak `-PricingPlan` için, belirtilen ile aynı cmdlet kullanın:  

```PS
Set-AzApplicationInsightsPricingPlan -ResourceGroupName <resource group> -Name <resource name> -PricingPlan Basic
```

Ayrıca, "microsoft.insights/components" kaynağını ve `dependsOn` fatura kaynağından düğümü atlayarak, yukarıdaki Kaynak Yöneticisi şablonunu kullanarak varolan bir Application Insights kaynağının fiyatlandırma planını da ayarlayabilirsiniz. Örneğin, GB Başına plana ayarlamak için (eski adıyla Temel plan), çalıştırın:

```PS
        New-AzResourceGroupDeployment -ResourceGroupName "<resource group>" `
               -TemplateFile .\template1.json `
               -priceCode 1 `
               -appName myApp
```

Aşağıdaki `priceCode` ler olarak tanımlanır:

|priceCode|plan|
|---|---|
|1|GB başına (eski Temel planı olarak adlandırılır)|
|2|Düğüm Başına (eski adıyla Enterprise planı)|

Son olarak, fiyatlandırma planlarını ve günlük kap parametrelerini almak ve ayarlamak için [ARMClient'ı](https://github.com/projectkudu/ARMClient) kullanabilirsiniz.  Geçerli değerleri almak için şunları kullanın:

```PS
armclient GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/microsoft.insights/components/MyResourceName/CurrentBillingFeatures?api-version=2018-05-01-preview
```

Ve tüm bu parametreleri aşağıdakileri kullanarak ayarlayabilirsiniz:

```PS
armclient PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/microsoft.insights/components/MyResourceName/CurrentBillingFeatures?api-version=2018-05-01-preview
"{'CurrentBillingFeatures':['Basic'],'DataVolumeCap':{'Cap':200,'ResetTime':12,'StopSendNotificationWhenHitCap':true,'WarningThreshold':90,'StopSendNotificationWhenHitThreshold':true}}"
```

Bu, günlük kapağı 200 GB/gün olarak ayarlar, günlük kapak sıfırlama süresini 12:00 UTC olarak yapılandıracak, hem kapak vurulduğunda hem de uyarı seviyesi karşılandığında e-posta gönderir ve uyarı eşiğini kapağın %90'ına ayarlar.  

## <a name="add-a-metric-alert"></a>Metrik uyarı ekleme

Metrik uyarıların oluşturulmasını otomatikleştirmek için [metrik uyarılar şablonu makalesine](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-create-templates#template-for-a-simple-static-threshold-metric-alert) başvurun


## <a name="add-an-availability-test"></a>Kullanılabilirlik testi ekleme

Kullanılabilirlik testlerini otomatikleştirmek için [metrik uyarılar şablonu makalesine başvurun.](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-create-templates#template-for-an-availability-test-along-with-a-metric-alert)

## <a name="add-more-resources"></a>Daha fazla kaynak ekleme

Herhangi bir türde başka bir kaynağın oluşturulmasını otomatikleştirmek için, el ile bir örnek oluşturun ve ardından kodunu [Azure Kaynak Yöneticisi'nden](https://resources.azure.com/)kopyalayıp parametreleştirin. 

1. [Azure Kaynak Yöneticisi'ni](https://resources.azure.com/)aç. Uygulama kaynağınıza gidin. `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components` 
   
    ![Azure Kaynak Gezgini'nde gezinme](./media/powershell/01.png)
   
    *Bileşenler,* uygulamaları görüntülemek için temel Uygulama Öngörüleri kaynaklarıdır. İlişkili uyarı kuralları ve kullanılabilirlik web testleri için ayrı kaynaklar vardır.
2. Bileşenin JSON'ını uygun yere `template1.json`kopyalayın.
3. Bu özellikleri silin:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. `webtests` Ve `alertrules` bölümleri açın ve tek tek öğeler için JSON'u şablonunuza kopyalayın. (Düğümlerden kopya `webtests` `alertrules` yapmayın: altlarındaki öğelere girin.)
   
    Her web testinin ilişkili bir uyarı kuralı vardır, bu nedenle her ikisini de kopyalamanız gerekir.
   
    Ölçümlere uyarılar da ekleyebilirsiniz. [Metrik adlar](powershell-alerts.md#metric-names).
5. Bu satırı her kaynağa ekleyin:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-the-template"></a>Şablonu parametreye
Şimdi parametrelerile belirli adlar değiştirmeniz gerekir. [Şablonu parametrelendirmek](../../azure-resource-manager/templates/template-syntax.md)için, yardımcı [işlevleri kümesini](../../azure-resource-manager/templates/template-functions.md)kullanarak ifadeler yazarsınız. 

Bir dizenin sadece bir kısmını parametreleştiremezsiniz, bu nedenle dizeleri oluşturmak için kullanın. `concat()`

Aşağıda yapmak isteyeceğiniz ikamelerden örnekler verilmiştir. Her ikame çeşitli olaylar vardır. Şablonunuzda başkasına ihtiyacınız olabilir. Bu örnekler, şablonun üst kısmında tanımladığımız parametreleri ve değişkenleri kullanır.

| find | ile değiştirin |
| --- | --- |
| `"hidden-link:/subscriptions/.../../components/MyAppName"` |`"[concat('hidden-link:',`<br/>`resourceId('microsoft.insights/components',` <br/> `parameters('appName')))]"` |
| `"/subscriptions/.../../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"`(küçük harf) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>`Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`|

### <a name="set-dependencies-between-the-resources"></a>Kaynaklar arasındaki bağımlılıkları ayarlama
Azure kaynakları katı sırada ayarlamalıdır. Bir kurulumun bir sonraki başlamadan önce tamamlandığından emin olmak için bağımlılık satırları ekleyin:

* Kullanılabilirlik test kaynağında:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* Kullanılabilirlik testi için uyarı kaynağında:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Sonraki adımlar
Diğer otomasyon makaleleri:

* [Bir Uygulama Öngörüleri kaynağı oluşturun](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#creating-a-resource-automatically) - şablon kullanmadan hızlı yöntem.
* [Uyarıları Ayarlama](powershell-alerts.md)
* [Web testleri oluşturma](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Azure Tanılama verilerini Application Insights’a gönderme](powershell-azure-diagnostics.md)
* [GitHub'dan Azure'a dağıt](https://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Sürüm ek açıklamaları oluşturma](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)