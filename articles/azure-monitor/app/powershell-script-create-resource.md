---
title: Application Insights kaynağı oluşturmak için PowerShell betiği | Microsoft Docs
description: Application Insights kaynaklarının oluşturulmasını otomatikleştirin.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/19/2016
ms.openlocfilehash: 11245d0f9d6e6b86a5d0249df65b33f851bee9d7
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72820695"
---
# <a name="powershell-script-to-create-an-application-insights-resource"></a>Application Insights kaynağı oluşturmak için PowerShell betiği

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Yeni bir uygulamayı veya bir uygulamanın yeni bir sürümünü izlemek istediğinizde ( [Azure Application Insights](https://azure.microsoft.com/services/application-insights/)ile Microsoft Azure yeni bir kaynak ayarlarsınız. Bu kaynak, uygulamanızdaki Telemetri verilerinin çözümlenme ve görüntülendiği yerdir. 

PowerShell kullanarak yeni bir kaynağın oluşturulmasını otomatik hale getirebilirsiniz.

Örneğin, bir mobil cihaz uygulaması geliştiriyorsanız, bu durum büyük olasılıkla müşterilerinizin sizin tarafınızdan kullanılan birkaç yayımlanmış sürümü olacaktır. Farklı sürümlerden karma telemetri sonuçlarını almak istemezsiniz. Bu nedenle, derleme işleminizi her derleme için yeni bir kaynak oluşturacak şekilde alırsınız.

> [!NOTE]
> Aynı anda bir kaynak kümesi oluşturmak isterseniz, [kaynakları bir Azure şablonu kullanarak oluşturmayı](powershell.md)düşünün.
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma betiği
İlgili cmdlet özelliklerine bakın:

* [New-AzResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*PowerShell betiği*  

```powershell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Connect-AzAccount / Connect-AzAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

# Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource


$resource = New-AzResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access to the team

New-AzRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Ikey ile ne yapmalı
Her kaynak, izleme anahtarı (Ikey) tarafından tanımlanır. Ikey, kaynak oluşturma betiğinin bir çıktısından oluşur. Derleme betiğinizin, uygulamanıza katıştırılmış Application Insights SDK 'ya Ikey 'i sağlaması gerekir.

Ikey 'i SDK için kullanılabilir hale getirmek için iki yol vardır:

* [ApplicationInsights. config](../../azure-monitor/app/configuration-with-applicationinsights-config.md)dosyasında: 
  * `<instrumentationkey>`*Ikey*`</instrumentationkey>`
* Veya [başlatma kodu](../../azure-monitor/app/api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*Ikey*`";`

## <a name="see-also"></a>Ayrıca bkz.
* [Şablonlardan Application Insights ve Web testi kaynakları oluşturma](powershell.md)
* [PowerShell ile Azure tanılama 'nın izlenmesini ayarlama](powershell-azure-diagnostics.md) 
* [PowerShell kullanarak uyarıları ayarlama](powershell-alerts.md)

