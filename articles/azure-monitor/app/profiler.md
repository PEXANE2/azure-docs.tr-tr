---
title: Uygulama Öngörüleri ile canlı Azure Uygulama Hizmeti uygulamalarını profille | Microsoft Dokümanlar
description: Application Insights Profiler ile Azure Uygulama Hizmeti'ndeki canlı uygulamaları profilleyin.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: ba9a2aca73dbdb8de298b68670fd6ab16f810a4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275782"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Application Insights ile canlı Azure Uygulama Hizmeti uygulamalarını profille

Profiler'ı, Temel hizmet katmanını veya daha yüksek bir sürümü kullanarak Azure Uygulama Hizmeti'nde çalışan ASP.NET ve ASP.NET Core uygulamalarında çalıştırabilirsiniz. Linux'ta Profiler'ı etkinleştirmek şu anda sadece [bu yöntemle](profiler-aspnetcore-linux.md)mümkündür.

## <a name="enable-profiler-for-your-app"></a><a id="installation"></a>Uygulamanız için Profiler'ı etkinleştirin
Bir uygulama için Profiler'ı etkinleştirmek için aşağıdaki yönergeleri izleyin. Farklı bir Azure hizmeti çalıştırıyorsanız, Profiler'ı desteklenen diğer platformlarda etkinleştirme yönergeleri aşağıda veda edilmiştir:
* [Bulut Hizmetleri](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Servis Kumaş Uygulamaları](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtual Machines](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Uygulama Öngörüleri Profiler, Uygulama Hizmetleri çalışma zamanının bir parçası olarak önceden yüklenir. Aşağıdaki adımlar, Uygulama Hizmetiniz için bunu nasıl etkinleştireceğinizgösterecektir. Uygulama Öngörüleri SDK'yı uygulamanız için oluşturma zamanında dahil etmiş olsanız bile aşağıdaki adımları izleyin.

1. Uygulama hizmetiniz için "Her Zaman Ayarı"nı etkinleştirin. Genel Ayarlar altında Uygulama Hizmetinizin Yapılandırma sayfasındaki ayarı güncelleyebilirsiniz.
1. Azure portalındaki **Uygulama Hizmetleri** bölmesine gidin.
1. Ayarlar **> Uygulama Öngörüleri** bölmesine gidin.

   ![Uygulama Hizmetleri portalında App Insights'ı etkinleştirin](./media/profiler/AppInsights-AppServices.png)

1. Yeni bir kaynak oluşturmak için bölmedeki yönergeleri izleyin veya uygulamanızı izlemek için mevcut bir App Insights kaynağını seçin. Ayrıca **Profiler'ın A'da**olduğundan emin olun. Application Insights kaynağınız Uygulama Hizmetinizden farklı bir abonelikteyse, Uygulama Öngörülerini yapılandırmak için bu sayfayı kullanamazsınız. Gerekli uygulama ayarlarını el ile oluşturarak bunu el ile de yapabilirsiniz. [Sonraki bölümde Profiler'ı el ile etkinleştirme yönergeleri yer almaktadır.](#enable-profiler-manually-or-with-azure-resource-manager) 

   ![App Insights site uzantısı ekle][Enablement UI]

1. Profiler artık bir Uygulama Hizmetleri Uygulama Ayarı kullanılarak etkinleştirildi.

    ![Profiler için Uygulama Ayarı][profiler-app-setting]

## <a name="enable-profiler-manually-or-with-azure-resource-manager"></a>Profiler'ı el ile veya Azure Kaynak Yöneticisi ile etkinleştirme
Uygulama Öngörüleri Profiler, Azure Uygulama Hizmetiniz için uygulama ayarları oluşturarak etkinleştirilebilir. Yukarıda gösterilen seçeneklerin yer alan sayfası bu uygulama ayarlarını sizin için oluşturur. Ancak, bir şablon veya başka yollardan bu ayarların oluşturulmasını otomatikleştirebilirsiniz. Application Insights kaynağınız Azure Uygulama Hizmetinizden farklı bir abonelikteyse, bu ayarlar da çalışır.
Profil oluşturucuyu etkinleştirmek için gereken ayarlar şunlardır:

|Uygulama Ayarı    | Değer    |
|---------------|----------|
|APPINSIGHTS_INSTRUMENTATIONKEY         | uygulama öngörüleri kaynağınız için iKey    |
|APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
|DiagnosticServices_EXTENSION_VERSION | ~3 |


Bu değerleri Azure [Kaynak Yöneticisi Şablonları](../../azure-monitor/app/azure-web-apps.md#app-service-application-settings-with-azure-resource-manager), [Azure Powershell](https://docs.microsoft.com/powershell/module/az.websites/set-azwebapp), [Azure CLI'yi](https://docs.microsoft.com/cli/azure/webapp/config/appsettings?view=azure-cli-latest)kullanarak ayarlayabilirsiniz.

### <a name="enabling-profiler-for-other-clouds-manually"></a>Diğer bulutlar için Profiler'ı el ile etkinleştirme

Diğer bulutlar için profil oluşturucuyu etkinleştirmek istiyorsanız, aşağıdaki uygulama ayarlarını kullanabilirsiniz.

|Uygulama Ayarı    | ABD Hükümet Değerleri| Çin Bulut |   
|---------------|---------------------|-------------|
|UygulamaInsightsProfilerEndpoint         | https://agent.serviceprofiler.azure.us    | https://profiler.applicationinsights.azure.cn |
|UygulamaInsightsEndpoint | https://dc.applicationinsights.us | https://dc.applicationinsights.azure.cn |

## <a name="disable-profiler"></a>Profil oluşturucuya devre dışı

Tek bir uygulamanın örneğini durdurmak veya yeniden başlatmak için, **Web İşleri**altında, uygulama kaynağına gidin. Profiler'ı silmek için **Uzantılar'a**gidin.

![Bir web işi için Profiler'ı devre dışı][disable-profiler-webjob]

Tüm uygulamalarınızda profil oluşturmayı mümkün olduğunca erken bir şekilde ortaya çıkarmak için etkinleştirmenizi öneririz.

Profiler'ın dosyaları, web uygulamanızda değişiklikleri dağıtmak için WebDeploy'ı kullanırken silinebilir. dağıtım sırasında App_Data klasörün silinmesini hariç tutarak silmeişlemini önleyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio’da Application Insights ile çalışma](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
