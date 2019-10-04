---
title: Azure Application Insights PowerShell ile otomatikleştirin | Microsoft Docs
description: Azure Resource Manager şablonu kullanarak PowerShell 'de kaynak, uyarı ve kullanılabilirlik testleri oluşturmayı otomatikleştirin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: mbullwin
ms.openlocfilehash: b4f3d2eba70be39b23e86ebde3c71dfc7c19a374
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936714"
---
#  <a name="create-application-insights-resources-using-powershell"></a>PowerShell kullanarak Application Insights kaynakları oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, Azure Kaynak Yönetimi kullanılarak [Application Insights](../../azure-monitor/app/app-insights-overview.md) kaynaklarının otomatik olarak oluşturulmasını ve güncelleştirilmesini nasıl otomatikleştirebileceğiniz gösterilmektedir. Örneğin, bir yapı sürecinin bir parçası olarak bunu yapabilirsiniz. Temel Application Insights kaynağıyla birlikte, [kullanılabilirlik Web testleri](../../azure-monitor/app/monitor-web-app-availability.md)oluşturabilir, [uyarıları](../../azure-monitor/app/alerts.md)ayarlayabilir, [fiyatlandırma şemasını](pricing.md)ayarlayabilir ve diğer Azure kaynaklarını oluşturabilirsiniz.

Bu kaynakları oluşturmaya yönelik anahtar, [Azure Resource Manager](../../azure-resource-manager/manage-resources-powershell.md)için JSON şablonlarıdır. Bir Nutshell 'de, yordam: mevcut kaynakların JSON tanımlarını indirin; adlar gibi bazı değerleri parametreleştirin; ardından, yeni bir kaynak oluşturmak istediğiniz her seferinde şablonu çalıştırın. Çeşitli kaynakları tek bir şekilde paketleyebilir, örneğin, sürekli dışa aktarma için kullanılabilirlik testleri, uyarılar ve depolama ile bir uygulama izleyicisine sahip olabilirsiniz. Burada açıklayacağımız bazı parametreler için bazı alt değişkenler vardır.

## <a name="one-time-setup"></a>Tek seferlik kurulum
Daha önce Azure aboneliğinizle PowerShell kullanmadıysanız:

Azure PowerShell modülünü, betikleri çalıştırmak istediğiniz makineye yükleyebilirsiniz:

1. [Microsoft Web Platformu Yükleyicisi (V5 veya üzeri)](https://www.microsoft.com/web/downloads/platform.aspx)yükler.
2. PowerShell 'i Microsoft Azure yüklemek için kullanın.

## <a name="create-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu oluşturma
Yeni bir. JSON dosyası oluşturun-Bu örnekte `template1.json` olarak çağrım. Bu içeriği buraya kopyalayın:

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
                "defaultValue": 24,
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
                    "ApplicationId": "[parameters('appName')]"
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
                    "retentionInDays": "[variables('retentionInDays')]",
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



## <a name="create-application-insights-resources"></a>Application Insights kaynakları oluşturma
1. PowerShell 'de Azure 'da oturum açın:
   
    `Connect-AzAccount`
2. Şöyle bir komut çalıştırın:
   
    ```PS
   
        New-AzResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName`, yeni kaynakları oluşturmak istediğiniz gruptur.
   * `-TemplateFile` özel parametrelerden önce gelmelidir.
   * `-appName` oluşturulacak kaynağın adı.

Başka parametreler ekleyebilirsiniz. bu kişilerin açıklamalarını, şablonun parametreler bölümünde bulacaksınız.

## <a name="to-get-the-instrumentation-key"></a>İzleme anahtarını almak için
Uygulama kaynağı oluşturduktan sonra, izleme anahtarını isteyeceksiniz: 

```PS
    $resource = Find-AzResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-the-price-plan"></a>Fiyat planını ayarlama

[Fiyat planını](pricing.md)ayarlayabilirsiniz.

Kurumsal fiyat planıyla, yukarıdaki şablonu kullanarak bir uygulama kaynağı oluşturmak için:

```PS
        New-AzResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|priceCode|planınızın|
|---|---|
|1\.|Temel|
|2|Enterprise|

* Yalnızca varsayılan temel fiyat planını kullanmak istiyorsanız, şablondan CurrentBillingFeatures kaynağını atlayabilirsiniz.
* Bileşen kaynağı oluşturulduktan sonra fiyat planını değiştirmek istiyorsanız, "Microsoft. Insights/Components" kaynağını kullanmayan bir şablon kullanabilirsiniz. Ayrıca faturalandırma kaynağından `dependsOn` düğümünü atlayın. 

Güncelleştirilmiş fiyat planını doğrulamak için tarayıcıdaki **kullanım ve tahmini maliyetler sayfa** dikey penceresine bakın. En son durumu görtığınızdan emin olmak için **tarayıcı görünümünü yenileyin** .



## <a name="add-a-metric-alert"></a>Ölçüm uyarısı Ekle

Bir ölçüm uyarısını uygulama kaynağınız ile aynı anda ayarlamak için aşağıdaki kodu şablon dosyası ile birleştirin:

```JSON
{
    parameters: { ... // existing parameters ...
            "responseTime": {
              "type": "int",
              "defaultValue": 3,
              "minValue": 1,
              "metadata": {
                "description": "Enter response time threshold in seconds."
              }
    },
    variables: { ... // existing variables ...
      // Alert names must be unique within resource group.
      "responseAlertName": "[concat('ResponseTime-', toLower(parameters('appName')))]"
    }, 
    resources: { ... // existing resources ...
     {
      //
      // Metric alert on response time
      //
      "name": "[variables('responseAlertName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this resource is created after the app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('responseAlertName')]",
        "description": "response time alert",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/components', parameters('appName'))]",
            "metricName": "request.duration"
          },
          "threshold": "[parameters('responseTime')]", //seconds
          "windowSize": "PT15M" // Take action if changed state for 15 minutes
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

Şablonu çağırdığınızda, isteğe bağlı olarak şu parametreyi ekleyebilirsiniz:

    `-responseTime 2`

Tabii ki diğer alanları parametreleştirebilirsiniz. 

Diğer uyarı kurallarının tür adlarını ve yapılandırma ayrıntılarını bulmak için, el ile bir kural oluşturun ve sonra [Azure Resource Manager](https://resources.azure.com/)inceleyin. 


## <a name="add-an-availability-test"></a>Bir kullanılabilirlik testi ekleyin

Bu örnek, ping testi için (tek bir sayfayı test etmek için).  

Bir kullanılabilirlik testinde **iki bölüm vardır** : test kendisi ve hatalara bildirim veren uyarı.

Aşağıdaki kodu, uygulamayı oluşturan şablon dosyasında birleştirin.

```JSON
{
    parameters: { ... // existing parameters here ...
      "pingURL": { "type": "string" },
      "pingText": { "type": "string" , defaultValue: ""}
    },
    variables: { ... // existing variables here ...
      "pingTestName":"[concat('PingTest-', toLower(parameters('appName')))]",
      "pingAlertRuleName": "[concat('PingAlert-', toLower(parameters('appName')), '-', subscription().subscriptionId)]"
    },
    resources: { ... // existing resources here ...
    { //
      // Availability test: part 1 configures the test
      //
      "name": "[variables('pingTestName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this is created after the app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "Name": "[variables('pingTestName')]",
        "Description": "Basic ping test",
        "Enabled": true,
        "Frequency": 900, // 15 minutes
        "Timeout": 120, // 2 minutes
        "Kind": "ping", // single URL test
        "RetryEnabled": true,
        "Locations": [
          {
            "Id": "us-va-ash-azr"
          },
          {
            "Id": "emea-nl-ams-azr"
          },
          {
            "Id": "apac-jp-kaw-edge"
          }
        ],
        "Configuration": {
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExecutionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('pingTestName')]"
      }
    },

    {
      //
      // Availability test: part 2, the alert rule
      //
      "name": "[variables('pingAlertRuleName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]", 
      "dependsOn": [
        "[resourceId('Microsoft.Insights/webtests', variables('pingTestName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource",
        "[concat('hidden-link:', resourceId('Microsoft.Insights/webtests', variables('pingTestName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('pingAlertRuleName')]",
        "description": "alert for web test",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.LocationThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.LocationThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/webtests', variables('pingTestName'))]",
            "metricName": "GSMT_AvRaW"
          },
          "windowSize": "PT15M", // Take action if changed state for 15 minutes
          "failedLocationCount": 2
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

Diğer test konumlarına ait kodları keşfetme veya daha karmaşık Web testi oluşturmayı otomatikleştirin, el ile bir örnek oluşturun ve sonra [Azure Resource Manager](https://resources.azure.com/)kodu parametreleştirin.

## <a name="add-more-resources"></a>Daha fazla kaynak ekleyin

Herhangi bir türdeki başka bir kaynağı oluşturmayı otomatikleştirin, bir örneği el ile oluşturun ve ardından [Azure Resource Manager](https://resources.azure.com/)kodunu kopyalayın ve parametreleştirin. 

1. [Azure Resource Manager](https://resources.azure.com/)açın. Uygulama kaynağınız için `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components` ' a gidin. 
   
    ![Azure Kaynak Gezgini gezinti](./media/powershell/01.png)
   
    *Bileşenler* , uygulamaları görüntülemek için temel Application Insights kaynaklarıdır. İlişkili uyarı kuralları ve kullanılabilirlik Web testleri için ayrı kaynaklar vardır.
2. Bileşenin JSON 'sini `template1.json` ' daki uygun yere kopyalayın.
3. Bu özellikleri Sil:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Web testleri ve alertrules bölümlerini açın ve tek tek öğeler için JSON 'u şablonunuza kopyalayın. (Web testleri veya alertrules düğümlerinden kopyalamayın: altındaki öğelere gidin.)
   
    Her Web testinin ilişkili bir uyarı kuralı vardır, bu nedenle her ikisini de kopyalamanız gerekir.
   
    Ölçümlere uyarıları da dahil edebilirsiniz. [Ölçüm adları](powershell-alerts.md#metric-names).
5. Her kaynağa bu satırı ekleyin:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-the-template"></a>Şablonu Parametreleştirme
Artık belirli adları parametrelerle değiştirmeniz gerekir. [Bir şablonu parametreleştirmek](../../azure-resource-manager/resource-group-authoring-templates.md)için, bir [dizi yardımcı işlevi](../../azure-resource-manager/resource-group-template-functions.md)kullanarak ifadeleri yazarsınız. 

Bir dizenin yalnızca bir kısmını parametreleştirebilirsiniz, bu nedenle dizeleri derlemek için `concat()` kullanın.

Yapmak istediğiniz değişimlerin örnekleri aşağıda verilmiştir. Her değiştirmenin çeşitli oluşumları vardır. Şablonunuzda başkalarının olması gerekebilir. Bu örnekler, şablonun üst kısmında tanımladığımız parametreleri ve değişkenleri kullanır.

| find | Değiştir |
| --- | --- |
| `"hidden-link:/subscriptions/.../../components/MyAppName"` |`"[concat('hidden-link:',`<br/>`resourceId('microsoft.insights/components',` <br/> `parameters('appName')))]"` |
| `"/subscriptions/.../../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"` (küçük harf) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>`Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>GUID ve ID 'yi silin. |

### <a name="set-dependencies-between-the-resources"></a>Kaynaklar arasında bağımlılıkları ayarlama
Azure, kaynakları katı sırayla ayarlamalıdır. Bir kurulumun bir sonraki başlamadan önce tamamlandığından emin olmak için, bağımlılık satırlarını ekleyin:

* Kullanılabilirlik testi kaynağında:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* Bir kullanılabilirlik testinin uyarı kaynağında:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Sonraki adımlar
Diğer otomasyon makaleleri:

* Şablon kullanmadan hızlı Yöntem [Application Insights oluşturun](powershell-script-create-resource.md) .
* [Uyarıları ayarlama](powershell-alerts.md)
* [Web testleri oluştur](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Application Insights Azure Tanılama gönder](powershell-azure-diagnostics.md)
* [GitHub 'dan Azure 'a dağıtma](https://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Sürüm ek açıklamaları oluştur](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)