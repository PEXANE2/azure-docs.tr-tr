---
title: Application Insights ile canlı Azure App Service uygulamalar profili Microsoft Docs
description: Azure App Service Application Insights Profiler ile canlı uygulamalar profilini yapın.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: f0702c09d2803507f07f74d97767c781825bf34f
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83778553"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Application Insights ile canlı Azure App Service uygulamalar profili

ASP.NET üzerinde profil oluşturucuyu ve temel hizmet katmanını veya üstünü kullanarak Azure App Service çalıştıran uygulamaları ASP.NET Core çalıştırabilirsiniz. Linux 'ta profil oluşturucuyu etkinleştirmek Şu anda yalnızca [Bu yöntem](profiler-aspnetcore-linux.md)aracılığıyla mümkündür.

## <a name="enable-profiler-for-your-app"></a><a id="installation"></a>Uygulamanız için profil oluşturucuyu etkinleştirin
Bir uygulama için profil oluşturucuyu etkinleştirmek üzere aşağıdaki yönergeleri izleyin. Farklı bir Azure hizmeti türü çalıştırıyorsanız, desteklenen diğer platformlarda profil oluşturucuyu etkinleştirme talimatları aşağıda verilmiştir:
* [Bulut Hizmetleri](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric uygulamalar](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Sanal makineler](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler, App Services çalışma zamanının bir parçası olarak önceden yüklenir. Aşağıdaki adımlarda, App Service için nasıl etkinleştirileceği gösterilmektedir. Application Insights SDK 'sını uygulamanıza derleme zamanında dahil etseniz bile bu adımları izleyin.

1. App Service için "Always On" ayarını etkinleştirin. Ayarları, genel ayarlar altında App Service yapılandırma sayfasında güncelleştirebilirsiniz.
1. Azure portal **App Services** bölmesine gidin.
1. **Ayarlar > Application Insights** bölmesine gidin.

   ![App Services portalında App Insights 'ı etkinleştirme](./media/profiler/AppInsights-AppServices.png)

1. Uygulamanızı izlemek üzere yeni bir kaynak oluşturmak veya var olan bir Application Insights kaynağını seçmek için bölmedeki yönergeleri izleyin. Ayrıca, profil oluşturucunun **Açık**olduğundan da emin olun. Application Insights kaynağınız App Service farklı bir abonelikte yer alıyorsa Application Insights yapılandırmak için bu sayfayı kullanamazsınız. Gerekli uygulama ayarlarını el ile oluşturarak yine de bunu el ile yapabilirsiniz. [Sonraki bölümde, profil oluşturucuyu el ile etkinleştirmeye yönelik yönergeler yer alır.](#enable-profiler-manually-or-with-azure-resource-manager) 

   ![App Insights site uzantısı Ekle][Enablement UI]

1. Profil Oluşturucu artık bir App Services uygulama ayarı kullanılarak etkinleştirildi.

    ![Profil Oluşturucu için uygulama ayarı][profiler-app-setting]

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
|Applicationınsightsprofilerendpoint         | `https://agent.serviceprofiler.azure.us`    | `https://profiler.applicationinsights.azure.cn` |
|Applicationınsi, Sendpoint | `https://dc.applicationinsights.us` | `https://dc.applicationinsights.azure.cn` |

## <a name="disable-profiler"></a>Profil oluşturucuyu devre dışı bırak

Tek bir uygulamanın örneği için profil oluşturucuyu durdurmak veya yeniden başlatmak için **WebJobs** altında, ApplicationInsightsProfiler3 adlı WebJob 'u durdurun. Profil Oluşturucu yukarıda açıklanan Application Insights sayfasındaki anahtar kullanılarak devre dışı bırakılmış olsa bile, profil oluşturucu işlemi çalışmaya devam edecektir. Profil Oluşturucu, etkin olup olmadığını kontrol eder. Devre dışıysa, tekrar denetlemeden önce bir süre uyku moduna geçer. Devre dışı bırakılmışsa profil oluşturma yapmaz. Bu WebJob 'u devre dışı bırakırsanız, etkin olup olmadığını denetlemek için profil oluşturucu işlemi hiç çalışmaz.

  ![Web işi için profil oluşturucuyu devre dışı bırakma][disable-profiler-webjob]

Herhangi bir performans sorununu mümkün olduğunca erken bulacak şekilde sağlamak için tüm uygulamalarınızda profil oluşturucunun etkin olmasını öneririz.

Web uygulamanıza değişiklikler dağıtmak için WebDeploy kullanılırken profil oluşturucunun dosyaları silinebilir. App_Data klasörünün dağıtım sırasında silinmesini dışlayarak silme işlemini engelleyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio’da Application Insights ile çalışma](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
