---
title: Yeni bir Azure Uygulama Öngörüleri kaynağı oluşturun | Microsoft Dokümanlar
description: Yeni bir canlı uygulama için Uygulama Öngörüleri izlemeyi el ile ayarlayın.
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 0c8b9ccaa70a2fd1bf46c6f4537f54d702ecc48f
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537585"
---
# <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Azure Uygulama Öngörüleri, uygulamanızla ilgili verileri bir Microsoft Azure *kaynağında*görüntüler. Bu nedenle, yeni bir kaynak oluşturmak, [yeni bir uygulamayı izlemek için Uygulama Öngörüleri'ni ayarlamanın bir][start]parçasıdır. Yeni kaynağınızı oluşturduktan sonra enstrümantasyon anahtarını alabilir ve Bunu Application Insights SDK'yı yapılandırmak için kullanabilirsiniz. Enstrümantasyon anahtarı telemetrinizi kaynağa bağlar.

## <a name="sign-in-to-microsoft-azure"></a>Microsoft Azure'da oturum açın

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

[Azure portalında](https://portal.azure.com)oturum açın ve bir Application Insights kaynağı oluşturun:

![Sol üst köşedeki '+' işaretini tıklatın. Uygulama Öngörüleri'ni izleyen Geliştirici Araçlarını seçin](./media/create-new-resource/new-app-insights.png)

   | Ayarlar        |  Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Adı**      | Benzersiz değer | İzlediğiniz uygulamayı tanımlayan ad. |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verilerini barındıracak yeni veya varolan kaynak grubunun adı. |
   | **Konum** | Doğu ABD | Yakınınızda veya uygulamanızın barındırıldığı yere yakın bir konum seçin. |

> [!NOTE]
> Aynı kaynak adını farklı kaynak gruplarında kullanabiliyor olsanız da, genel olarak benzersiz bir ad kullanmak yararlı olabilir. Bu, gerekli sözdizimini basitleştirirken [çapraz kaynak sorguları gerçekleştirmeyi](https://docs.microsoft.com/azure/azure-monitor/log-query/cross-workspace-query#identifying-an-application) planlıyorsanız yararlı olabilir.

Gerekli alanlara uygun değerleri girin ve sonra **Gözden Geçir + oluştur'u**seçin.

![Gerekli alanlara değerleri girin ve ardından "gözden geçir + oluştur" seçeneğini belirleyin.](./media/create-new-resource/review-create.png)

Uygulamanız oluşturulduğunda, yeni bir bölme açılır. Bu bölme, izlenen uygulamanızla ilgili performans ve kullanım verilerini gördüğünüz yerdir. 

## <a name="copy-the-instrumentation-key"></a>Enstrümantasyon anahtarını kopyalama

Enstrümantasyon anahtarı, telemetri verilerinizi ilişkilendirmek istediğiniz kaynağı tanımlar. Enstrümantasyon anahtarını kopyalamanız ve uygulamanızın koduna eklemeniz gerekir.

![Enstrümantasyon tuşunu tıklatın ve kopyalayın](./media/create-new-resource/instrumentation-key.png)

## <a name="install-the-sdk-in-your-app"></a>Uygulamanızda SDK'yı yükleyin

Uygulama Öngörüleri SDK'yı uygulamanızda yükleyin. Bu adım, uygulamanızın türüne bağlıdır.

[Uygulamanızda yüklediğiniz SDK'yı][start]yapılandırmak için enstrümantasyon anahtarını kullanın.

SDK herhangi bir ek kod yazmak zorunda kalmadan telemetri göndermek standart modülleri içerir. Kullanıcı eylemlerini izlemek veya sorunları daha ayrıntılı olarak tanılamak için kendi telemetrinizi göndermek için [API'yi kullanın.][api]

## <a name="creating-a-resource-automatically"></a>Otomatik olarak kaynak oluşturma

### <a name="powershell"></a>PowerShell

Yeni bir Uygulama Öngörüleri kaynağı oluşturma

```powershell
New-AzApplicationInsights [-ResourceGroupName] <String> [-Name] <String> [-Location] <String> [-Kind <String>]
 [-Tag <Hashtable>] [-DefaultProfile <IAzureContextContainer>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="example"></a>Örnek

```powershell
New-AzApplicationInsights -Kind java -ResourceGroupName testgroup -Name test1027 -location eastus
```
#### <a name="results"></a>Sonuçlar

```powershell
Id                 : /subscriptions/{subid}/resourceGroups/testgroup/providers/microsoft.insights/components/test1027
ResourceGroupName  : testgroup
Name               : test1027
Kind               : web
Location           : eastus
Type               : microsoft.insights/components
AppId              : 8323fb13-32aa-46af-b467-8355cf4f8f98
ApplicationType    : web
Tags               : {}
CreationDate       : 10/27/2017 4:56:40 PM
FlowType           :
HockeyAppId        :
HockeyAppToken     :
InstrumentationKey : 00000000-aaaa-bbbb-cccc-dddddddddddd
ProvisioningState  : Succeeded
RequestSource      : AzurePowerShell
SamplingPercentage :
TenantId           : {subid}
```

Bu cmdlet için tam PowerShell belgeleri ve enstrümantasyon anahtarını nasıl alabildiğini öğrenmek için [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/module/az.applicationinsights/new-azapplicationinsights?view=azps-2.5.0)başvurun.

### <a name="azure-cli-preview"></a>Azure CLI (önizleme)

Önizleme Uygulama Öngörüleri Azure CLI komutlarına erişmek için öncelikle çalıştırmanız gerekir:

```azurecli
 az extension add -n application-insights
```

Komutu `az extension add` çalıştırmazsanız, aşağıdakileri belirten bir hata iletisi görürsünüz:`az : ERROR: az monitor: 'app-insights' is not in the 'az monitor' command group. See 'az monitor --help'.`

Artık Application Insights kaynağınızı oluşturmak için aşağıdakileri çalıştırabilirsiniz:

```azurecli
az monitor app-insights component create --app
                                         --location
                                         --resource-group
                                         [--application-type]
                                         [--kind]
                                         [--tags]
```

#### <a name="example"></a>Örnek

```azurecli
az monitor app-insights component create --app demoApp --location westus2 --kind web -g demoRg --application-type web
```

#### <a name="results"></a>Sonuçlar

```azurecli
az monitor app-insights component create --app demoApp --location eastus --kind web -g demoApp  --application-type web
{
  "appId": "87ba512c-e8c9-48d7-b6eb-118d4aee2697",
  "applicationId": "demoApp",
  "applicationType": "web",
  "creationDate": "2019-08-16T18:15:59.740014+00:00",
  "etag": "\"0300edb9-0000-0100-0000-5d56f2e00000\"",
  "flowType": "Bluefield",
  "hockeyAppId": null,
  "hockeyAppToken": null,
  "id": "/subscriptions/{subid}/resourceGroups/demoApp/providers/microsoft.insights/components/demoApp",
  "instrumentationKey": "00000000-aaaa-bbbb-cccc-dddddddddddd",
  "kind": "web",
  "location": "eastus",
  "name": "demoApp",
  "provisioningState": "Succeeded",
  "requestSource": "rest",
  "resourceGroup": "demoApp",
  "samplingPercentage": null,
  "tags": {},
  "tenantId": {tenantID},
  "type": "microsoft.insights/components"
}
```

Bu komut için tam Azure CLI belgeleri ve enstrümantasyon anahtarını nasıl alabildiğini öğrenmek için [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/ext/application-insights/monitor/app-insights/component?view=azure-cli-latest#ext-application-insights-az-monitor-app-insights-component-create)başvurun.

## <a name="next-steps"></a>Sonraki adımlar
* [Tanılama Araması](../../azure-monitor/app/diagnostic-search.md)
* [Ölçümleri keşfetme](../../azure-monitor/platform/metrics-charts.md)
* [Analytics sorguları yazma](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/platform/metrics-charts.md
[start]: ../../azure-monitor/app/app-insights-overview.md
