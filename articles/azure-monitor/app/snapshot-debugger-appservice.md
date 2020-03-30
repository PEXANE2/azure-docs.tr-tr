---
title: Azure Uygulama Hizmeti'ndeki .NET uygulamaları için Anlık Görüntü Hata Ayıklama'yı etkinleştirin | Microsoft Dokümanlar
description: Azure Uygulama Hizmeti'ndeki .NET uygulamaları için Anlık Görüntü Hata Ayıklama'yı etkinleştirme
ms.topic: conceptual
author: brahmnes
ms.author: bfung
ms.date: 03/26/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 8af688e38003e0613a06d7d8622ce279a3838589
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298264"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Azure Uygulama Hizmeti'ndeki .NET uygulamaları için Anlık Görüntü Hata Ayıklama'yı etkinleştirme

Anlık Hata Ayıklama şu anda Windows hizmet planlarında Azure Uygulama Hizmeti'nde çalışan ASP.NET ve ASP.NET Core uygulamaları için çalışmaktadır.

## <a name="enable-snapshot-debugger"></a><a id="installation"></a>Anlık Görüntü Hata Ayıklama etkinleştirme
Bir uygulama için Anlık Görüntü Hata Ayıklama'yı etkinleştirmek için aşağıdaki yönergeleri izleyin. Farklı bir Azure hizmeti çalıştırıyorsanız, anlık görüntü hata ayıklamasını diğer desteklenen platformlarda etkinleştirme yönergeleri aşağıda veda edilmiştir:
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Hizmet Kumaşı hizmetleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Sanal Makineler ve sanal makine ölçek setleri](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Şirket içi sanal veya fiziksel makineler](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

.NET Core'un önizleme sürümünü kullanıyorsanız, lütfen önce [microsoft.applicationinsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini uygulamayla birlikte eklemek ve ardından aşağıdaki talimatların geri kalanını tamamlamak [için diğer ortamlar için Anlık Görüntü Hata Ayıklayıcısını Etkinleştir](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) yönergelerini izleyin. 

Uygulama Öngörüleri Anlık Hata Ayıklama, Uygulama Hizmetleri çalışma zamanının bir parçası olarak önceden yüklenir, ancak Uygulama Hizmeti uygulamanız için anlık görüntü almak için açmanız gerekir. Bir uygulamayı dağıttıktan sonra, Kaynak koduna Application Insights SDK'yı eklenmiş olsanız bile, anlık görüntü hata ayıklayıcısını etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure portalındaki **Uygulama Hizmetleri** bölmesine gidin.
2. Ayarlar **> Uygulama Öngörüleri** bölmesine gidin.

   ![Uygulama Hizmetleri portalında App Insights'ı etkinleştirin](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Yeni bir kaynak oluşturmak için bölmedeki yönergeleri izleyin veya uygulamanızı izlemek için mevcut bir App Insights kaynağını seçin. Ayrıca Snapshot Debugger için her iki **anahtarın**da Üzerinde olduğundan emin olun.

   ![App Insights site uzantısı ekle][Enablement UI]

4. Anlık Görüntü Hata Ayıklama artık bir Uygulama Hizmetleri Uygulama Ayarı kullanılarak etkinleştirildi.

    ![Anlık Görüntü Hata Ayıklama için Uygulama Ayarı][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Anlık Görüntü Hata Ayıklama'yı devre dışı

Anlık Görüntü Hata **Ayıklama etkinleştir**için aynı adımları izleyin, ancak Anlık Görüntü Debugger için her iki anahtarı da **Kapatın'a**geçin.
Uygulama özel durumlarının tanısını kolaylaştırmak için tüm uygulamalarınızda Anlık Görüntü Hata Ayıklama özelliğini etkinleştirmenizi öneririz.

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Azure Uygulama Hizmeti için, Anlık Görüntü Hata Ayıklayıcı ve Profil Oluşturucusu'nun etkinolmasını sağlamak için uygulama ayarlarını bir Azure Kaynak Yöneticisi şablonunda ayarlayabilirsiniz. Web sitesinin alt kaynağı olarak uygulama ayarlarını içeren bir config kaynağı eklersiniz:

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

- Uygulamanızda bir özel durum tetikleyebilen trafik oluşturun. Ardından, anlık görüntünün Uygulama Öngörüleri örneğine gönderilmesi için 10 ila 15 dakika bekleyin.
- Azure portalındaki [anlık görüntülere](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) bakın.
- Anlık Görüntü Hata Ayıklama sorunlarıyla ilgili yardım için Bkz. Anlık Hata [Ayıklama sorun giderme.](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json)

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

