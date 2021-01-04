---
title: Azure App Service .NET uygulamaları için Snapshot Debugger etkinleştirme | Microsoft Docs
description: Azure App Service .NET uygulamaları için Snapshot Debugger etkinleştirme
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 03/26/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 421f80493a9cb88e8bbbddc06aa9a24042b64b17
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2020
ms.locfileid: "97695466"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Azure App Service .NET uygulamaları için Snapshot Debugger etkinleştirme

Snapshot Debugger Şu anda Windows hizmet planlarına Azure App Service çalışan ASP.NET ve ASP.NET Core uygulamalar için çalışır. Anlık görüntü hata ayıklayıcısı 'nı kullanırken uygulamanızı temel hizmet katmanında veya daha yüksek bir sürüme çalıştırmanızı öneririz. Çoğu uygulama için, ücretsiz ve paylaşılan hizmet katmanlarında anlık görüntüleri kaydetmek için yeterli bellek veya disk alanı yoktur.

## <a name="enable-snapshot-debugger"></a><a id="installation"></a> Snapshot Debugger etkinleştir
Bir uygulama için Snapshot Debugger etkinleştirmek üzere aşağıdaki yönergeleri izleyin.

Farklı bir Azure hizmeti türü çalıştırıyorsanız, desteklenen diğer platformlarda Snapshot Debugger etkinleştirme talimatları aşağıda verilmiştir:
* [Azure İşlevi](snapshot-debugger-function-app.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric Hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure sanal makineleri ve sanal makine ölçek kümeleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Şirket içi sanal veya fiziksel makineler](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

> [!NOTE]
> .NET Core 'un önizleme sürümünü kullanıyorsanız veya uygulamanız Application Insights SDK 'ya bir bağımlı derleme aracılığıyla doğrudan veya dolaylı olarak başvuruyorsa, [diğer ortamlar için](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) , uygulamaya sahip [Microsoft. ApplicationInsights. snapshotcollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini dahil etmek için Snapshot Debugger etkinleştirme yönergelerini izleyin ve ardından aşağıdaki yönergelerin geri kalanını tamamlayın. 

Snapshot Debugger, App Services çalışma zamanının bir parçası olarak önceden yüklenir, ancak App Service uygulamanız için anlık görüntüler almak üzere açmanız gerekir.

Bir uygulamayı dağıttıktan sonra, anlık görüntü hata ayıklayıcısını etkinleştirmek için aşağıdaki adımları izleyin:

1. App Service için Azure Denetim Masası ' na gidin.
2. **Ayarlar > Application Insights** sayfasına gidin.

   ![App Services portalında App Insights 'ı etkinleştirme](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Uygulamanızı izlemek üzere yeni bir kaynak oluşturmak veya var olan bir Application Insights kaynağını seçmek için sayfadaki yönergeleri izleyin. Ayrıca Snapshot Debugger için her iki **anahtar de bulunduğundan emin olun.**

   ![App Insights site uzantısı Ekle][Enablement UI]

4. Snapshot Debugger artık bir App Services uygulama ayarı kullanılarak etkinleştirildi.

    ![Snapshot Debugger için uygulama ayarı][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Snapshot Debugger devre dışı bırak

**Etkinleştirme Snapshot Debugger** ile aynı adımları izleyin, ancak her iki anahtarı da Snapshot Debugger için **kapalı** olarak değiştirin.

Uygulama özel durumlarının tanılamayı kolaylaştırmak için tüm uygulamalarınızda Snapshot Debugger etkinleştirilmiş olması önerilir.

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Azure App Service için, Azure Resource Manager şablonu içinde uygulama ayarlarını Snapshot Debugger ve profil oluşturucuyu etkinleştirmek üzere ayarlayabilirsiniz ve aşağıdaki şablon kod parçacığına bakın:

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('webSiteName')]",
  "type": "Microsoft.Web/sites",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  "tags": { 
    "[concat('hidden-related:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "empty",
    "displayName": "Website"
  },
  "properties": {
    "name": "[parameters('webSiteName')]",
    "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "name": "appsettings",
      "type": "config",
      "dependsOn": [
        "[parameters('webSiteName')]",
        "[concat('AppInsights', parameters('webSiteName'))]"
      ],
      "properties": {
        "APPINSIGHTS_INSTRUMENTATIONKEY": "[reference(resourceId('Microsoft.Insights/components', concat('AppInsights', parameters('webSiteName'))), '2014-04-01').InstrumentationKey]",
        "APPINSIGHTS_PROFILERFEATURE_VERSION": "1.0.0",
        "APPINSIGHTS_SNAPSHOTFEATURE_VERSION": "1.0.0",
        "DiagnosticServices_EXTENSION_VERSION": "~3",
        "ApplicationInsightsAgent_EXTENSION_VERSION": "~2"
      }
    }
  ]
},
```

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanıza bir özel durum tetikleyebilmesi için trafik oluşturun. Ardından, anlık görüntülerin Application Insights örneğine gönderilmesi için 10 ila 15 dakika bekleyin.
- Azure portal [anlık görüntüleri](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) görüntüleyin.
- Sorunları giderme Snapshot Debugger konusunda yardım için bkz. [Snapshot Debugger sorun giderme](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

