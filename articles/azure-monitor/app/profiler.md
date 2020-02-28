---
title: Application Insights ile canlı Azure App Service uygulamalar profili Microsoft Docs
description: Azure App Service Application Insights Profiler ile canlı uygulamalar profilini yapın.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: ba9a2aca73dbdb8de298b68670fd6ab16f810a4d
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77671551"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Application Insights ile canlı Azure App Service uygulamalar profili

ASP.NET üzerinde profil oluşturucuyu ve temel hizmet katmanını veya üstünü kullanarak Azure App Service çalıştıran uygulamaları ASP.NET Core çalıştırabilirsiniz. Linux 'ta profil oluşturucuyu etkinleştirmek Şu anda yalnızca [Bu yöntem](profiler-aspnetcore-linux.md)aracılığıyla mümkündür.

## <a id="installation"></a>Uygulamanız için profil oluşturucuyu etkinleştirin
Bir uygulama için profil oluşturucuyu etkinleştirmek üzere aşağıdaki yönergeleri izleyin. Farklı bir Azure hizmeti türü çalıştırıyorsanız, desteklenen diğer platformlarda profil oluşturucuyu etkinleştirme talimatları aşağıda verilmiştir:
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric uygulamalar](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Sanal Makineler](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler, App Services çalışma zamanının bir parçası olarak önceden yüklenir. Aşağıdaki adımlarda, App Service için nasıl etkinleştirileceği gösterilmektedir. Application Insights SDK 'sını uygulamanıza derleme zamanında dahil etseniz bile bu adımları izleyin.

1. App Service için "Always On" ayarını etkinleştirin. Ayarları, genel ayarlar altında App Service yapılandırma sayfasında güncelleştirebilirsiniz.
1. Azure portal **App Services** bölmesine gidin.
1. **Ayarlar > Application Insights** bölmesine gidin.

   ![Uygulama Hizmetleri portalında App ınsights'ı etkinleştirme](./media/profiler/AppInsights-AppServices.png)

1. Uygulamanızı izlemek üzere yeni bir kaynak oluşturmak veya var olan bir Application Insights kaynağını seçmek için bölmedeki yönergeleri izleyin. Ayrıca, profil oluşturucunun **Açık**olduğundan da emin olun. Application Insights kaynağınız App Service farklı bir abonelikte yer alıyorsa Application Insights yapılandırmak için bu sayfayı kullanamazsınız. Gerekli uygulama ayarlarını el ile oluşturarak yine de bunu el ile yapabilirsiniz. [Sonraki bölümde, profil oluşturucuyu el ile etkinleştirmeye yönelik yönergeler yer alır.](#enable-profiler-manually-or-with-azure-resource-manager) 

   ![App Insights site uzantısı Ekle][Enablement UI]

1. Profiler, artık bir uygulama hizmetleri uygulama ayarı kullanılarak etkinleştirilir.

    ![Profiler uygulama ayarı][profiler-app-setting]

## <a name="enable-profiler-manually-or-with-azure-resource-manager"></a>Profil oluşturucuyu el ile veya Azure Resource Manager etkinleştirme
Application Insights Profiler, Azure App Service için uygulama ayarları oluşturularak etkinleştirilebilir. Yukarıda gösterilen seçeneklere sahip sayfa sizin için bu uygulama ayarlarını oluşturur. Ancak bu ayarların oluşturulmasını, şablon veya başka yollarla otomatik hale getirebilirsiniz. Application Insights kaynağınız Azure App Service farklı bir abonelikte ise bu ayarlar da çalışacaktır.
Profil oluşturucuyu etkinleştirmek için gereken ayarlar şunlardır:

|Uygulama Ayarı    | Değer    |
|---------------|----------|
|APPINSIGHTS_INSTRUMENTATIONKEY         | Application Insights kaynağınız için Ikey    |
|APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
|DiagnosticServices_EXTENSION_VERSION | ~ 3 |


Bu değerleri [Azure Resource Manager şablonları](../../azure-monitor/app/azure-web-apps.md#app-service-application-settings-with-azure-resource-manager), [Azure POWERSHELL](https://docs.microsoft.com/powershell/module/az.websites/set-azwebapp), [Azure CLI](https://docs.microsoft.com/cli/azure/webapp/config/appsettings?view=azure-cli-latest)kullanarak ayarlayabilirsiniz.

### <a name="enabling-profiler-for-other-clouds-manually"></a>Diğer bulutlar için profil oluşturucuyu el ile etkinleştirme

Diğer bulutlara yönelik profil oluşturucuyu etkinleştirmek istiyorsanız, aşağıdaki uygulama ayarlarını kullanabilirsiniz.

|Uygulama Ayarı    | ABD devlet değerleri| Çin bulutu |   
|---------------|---------------------|-------------|
|Applicationınsightsprofilerendpoint         | https://agent.serviceprofiler.azure.us    | https://profiler.applicationinsights.azure.cn |
|Applicationınsi, Sendpoint | https://dc.applicationinsights.us | https://dc.applicationinsights.azure.cn |

## <a name="disable-profiler"></a>Profiler devre dışı bırak

Tek bir uygulamanın örneği için profil oluşturucuyu durdurmak veya yeniden başlatmak için, **Web işleri**altında uygulama kaynağına gidin. Profil oluşturucuyu silmek için **uzantılara**gidin.

![Bir web işi için Profiler devre dışı bırak][disable-profiler-webjob]

Herhangi bir performans sorununu mümkün olduğunca erken bulacak şekilde sağlamak için tüm uygulamalarınızda profil oluşturucunun etkin olmasını öneririz.

Web uygulamanıza değişiklikler dağıtmak için WebDeploy kullanılırken profil oluşturucunun dosyaları silinebilir. App_Data klasörünün dağıtım sırasında silinmesini dışlayarak silme işlemini engelleyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio 'da Application Insights çalışma](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
