---
title: VS'yi kullanarak Web İşlergeliştirme ve dağıtma
description: Visual Studio'da Azure Web İşlerini nasıl geliştirip, zamanlanmış bir görev oluşturmak da dahil olmak üzere Azure Uygulama Hizmetine nasıl dağıtılamayı öğrenin.
author: ggailey777
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.topic: conceptual
ms.custom: vs-azure
ms.date: 02/18/2019
ms.author: glenga
ms.reviewer: david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: d73c858bdbfee7c5b7c7e31b62b1f601b7b6838a
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81866449"
---
# <a name="develop-and-deploy-webjobs-using-visual-studio---azure-app-service"></a>Visual Studio'yu kullanarak Web İşleri geliştirme ve dağıtma - Azure App Service

Bu makalede, Bir Konsol Uygulaması projesini [Uygulama Hizmeti'ndeki](overview.md) bir web uygulamasına [Azure WebJob](https://go.microsoft.com/fwlink/?LinkId=390226)olarak dağıtmak için Visual Studio'nun nasıl kullanılacağı açıklanmaktadır. [Azure portalını](https://portal.azure.com)kullanarak Web İşlerin nasıl dağıtılanabildiğini öğrenmek için [Bkz. Web İşleri ile Arka Plan görevlerini Çalıştır.](webjobs-create.md)

Tek bir web uygulamasında birden çok Web İş Ii yayımlayabilirsiniz. Bir web uygulamasındaki her WebJob'un benzersiz bir adı olduğundan emin olun.

[Azure Web İşleri SDK](webjobs-sdk-how-to.md) sürümü 3.x, .NET Core uygulamaları veya .NET Framework uygulamaları olarak çalışan Webİşler geliştirmenize olanak sağlarken, sürüm 2.x yalnızca .NET Framework'ü destekler. Bir Webİşler projesini dağıtma şekliniz .NET Core projeleri ile .NET Framework projeleri için farklıdır.

## <a name="webjobs-as-net-core-console-apps"></a>.NET Core konsol uygulamaları olarak WebJobs

Webİşler'in sürüm 3.x'ini kullanırken, WebJobs'u .NET Core konsol uygulamaları olarak oluşturabilir ve yayımlayabilirsiniz. Bir .NET Core konsol uygulaması oluşturmak ve Bir WebJob olarak Azure'da yayımlamak için adım adım talimatlar için, [etkinlik odaklı arka plan işleme için Azure Web İşlerI SDK'sını başlatın'a](webjobs-sdk-get-started.md)bakın.

> [!NOTE]
> .NET Core WebJobs web projeleri ile bağlantılı olamaz. WebJob'unuzu bir web uygulamasıyla dağıtmanız gerekiyorsa, [WebJob'unuzu bir .NET Framework konsol uyrağı uygulaması olarak oluşturmanız](#webjobs-as-net-framework-console-apps)gerekir.  

### <a name="deploy-to-azure-app-service"></a>Azure App Service'e dağıtma

Visual Studio'dan .NET Core WebJob'u Uygulama Hizmetine yayımlamak, ASP.NET Core uygulamasını yayınlamakla aynı aracı kullanır.

[!INCLUDE [webjobs-publish-net-core](../../includes/webjobs-publish-net-core.md)] 

### <a name="webjob-types"></a>WebJob türleri

Varsayılan olarak, bir .NET Core konsol projesinden yayınlanan bir WebJob yalnızca tetiklendiğinde veya isteğe bağlı olarak çalışır. Ayrıca, projeyi [bir zamanlamada çalışacak](#scheduled-execution) veya sürekli çalışacak şekilde de güncelleştirebilirsiniz.

[!INCLUDE [webjobs-alwayson-note](../../includes/webjobs-always-on-note.md)]

#### <a name="scheduled-execution"></a>Zamanlanmış yürütme

Azure'da bir .NET Core konsol uygulaması yayımladığınızda, projeye yeni bir *settings.job* dosyası eklenir. Webİş'iniz için bir yürütme çizelgesi ayarlamak için bu dosyayı kullanın. Daha fazla bilgi için, [tetikleyici bir WebJob zamanlama'ya](#scheduling-a-triggered-webjob)bakın.

#### <a name="continuous-execution"></a>Sürekli yürütme

Azure'da Her Zaman Etkinleştirildiğinde WebJob'u sürekli olarak çalıştırmak için Visual Studio'yu kullanabilirsiniz.

1. Henüz yapmadıysanız, [projeyi Azure'da yayımlayın.](#deploy-to-azure-app-service)

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. **Yayımla** sekmesinde **Ayarlar'ı**seçin. 

1. Profil **Ayarları** iletişim kutusunda, **Webİş Türü**için **Sürekli'yi** seçin ve **Kaydet'i**seçin.

    ![Webİş için Ayarlar iletişim kutusunu yayımlama](./media/webjobs-dotnet-deploy-vs/publish-settings.png)

1. WebJob'u güncelleştirilmiş ayarlarla yeniden yayımlamak için **Yayımla'yı** seçin.

## <a name="webjobs-as-net-framework-console-apps"></a>.NET Framework konsol uyrağı uygulamaları olarak WebJobs  

Visual Studio WebJobs özellikli bir .NET Framework Console Application projesini dağıttığında, çalışma zamanı dosyalarını web uygulamasındaki uygun klasöre kopyalar (sürekli Web İşler için*App_Data/işler/sürekli* ve zamanlanmış veya isteğe bağlı Webİşler için *App_Data/işler/tetiklenir).*

Web İşler etkinleştirilmiş bir projede aşağıdaki öğeler eklenir:

* [Microsoft.Web.WebJobs.Publish](https://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paketi.
* Dağıtım ve zamanlayıcı ayarlarını içeren [bir webjob-publish-settings.json](#publishsettings) dosyası. 

![WebJob olarak dağıtımı etkinleştirmek için Konsol Uygulamasına nelerin eklenmediğini gösteren diyagram](./media/webjobs-dotnet-deploy-vs/convert.png)

Bu öğeleri varolan bir Konsol Uygulaması projesine ekleyebilir veya yeni bir Web İşler etkin leştirilmiş Konsol Uygulaması projesi oluşturmak için şablon kullanabilirsiniz. 

Bir projeyi kendi başına WebJob olarak dağıtabilir veya web projesini dağıttığınızda otomatik olarak dağıtacak şekilde bir web projesine bağlayabilirsiniz. Visual Studio projeleri bağlamak için web [projesindeki web işleri listesi.json](#webjobslist) dosyasında Webİşler etkinleştirilmiş projenin adını içerir.

![Web projesine bağlantı sağlayan WebJob projesini gösteren diyagram](./media/webjobs-dotnet-deploy-vs/link.png)

### <a name="prerequisites"></a>Önkoşullar

Visual Studio 2015 kullanıyorsanız, [.NET (Visual Studio 2015) için Azure SDK'yı](https://azure.microsoft.com/downloads/)yükleyin.

Visual Studio 2017 kullanıyorsanız, [Azure geliştirme iş yükünü](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-4---choose-workloads)yükleyin.

### <a name="enable-webjobs-deployment-for-an-existing-console-application-project"></a><a id="convert"></a>Varolan bir Konsol Uygulaması projesi için Webİşler dağıtımını etkinleştirme

İki seçeneğiniz vardır:

* [Bir web projesi yle otomatik dağıtımı etkinleştirin.](#convertlink)

  Bir web projesi dağıtırken otomatik olarak WebJob olarak dağıtılabilen varolan bir Konsol Uygulaması projesini yapılandırın. WebJob'unuzu ilgili web uygulamasını çalıştırdığınız aynı web uygulamasında çalıştırmak istediğinizde bu seçeneği kullanın.

* [Bir web projesi olmadan dağıtımı etkinleştirin.](#convertnolink)

  Varolan bir Konsol Uygulaması projesini, bir web projesine bağlantı olmadan tek başına bir WebJob olarak dağıtacak şekilde yapılandırın. Web uygulamasında hiçbir web uygulaması çalışmadan, bir web uygulamasında tek başına bir WebJob çalıştırmak istediğinizde bu seçeneği kullanın. Webİş kaynaklarınızı web uygulama kaynaklarınızdan bağımsız olarak ölçeklendirebilmek için bunu yapmak isteyebilirsiniz.

#### <a name="enable-automatic-webjobs-deployment-with-a-web-project"></a><a id="convertlink"></a>Bir web projesiyle otomatik Web İşler dağıtımını etkinleştirme

1. **Solution Explorer'da**web projesini sağ tıklatın ve ardından**Azure WebJob olarak Varolan Proje ekle'yi** **Add** > tıklatın.
   
    ![Azure WebJob olarak Mevcut Proje](./media/webjobs-dotnet-deploy-vs/eawj.png)
   
    [Azure Webİş Ekle](#configure) iletişim kutusu görüntülenir.
2. Proje **adı** açılır listesinde, WebJob olarak eklemek üzere Konsol Uygulaması projesini seçin.
   
    ![Azure Webİş Ekle iletişim kutusunda proje seçme](./media/webjobs-dotnet-deploy-vs/aaw1.png)
3. Azure [Webİş Ekle](#configure) iletişim kutusunu doldurun ve **ardından Tamam'ı**tıklatın. 

#### <a name="enable-webjobs-deployment-without-a-web-project"></a><a id="convertnolink"></a>Web Projesi olmadan Webİşler dağıtımını etkinleştirme
1. **Çözüm Gezgini'nde**Konsol Uygulaması projesini sağ tıklatın ve ardından **Azure Webİş olarak yayımla'yı tıklatın...**. 
   
    ![Azure WebJob olarak yayımlama](./media/webjobs-dotnet-deploy-vs/paw.png)
   
    Proje **ad** kutusunda seçili olan [Azure Webİş](#configure) ekle iletişim kutusu görüntülenir.
2. Azure [Webİş Ekle](#configure) iletişim kutusunu doldurun ve **ardından Tamam'ı**tıklatın.
   
   **Web Yayımla** sihirbazı görüntülenir.  Hemen yayımlamak istemiyorsanız sihirbazı kapatın. Girdiğiniz [ayarlar, projeyi dağıtmak](#deploy)istediğinizde kaydedilir.

### <a name="create-a-new-webjobs-enabled-project"></a><a id="create"></a>Web İşler etkinleştirilmiş yeni bir proje oluşturma
Yeni bir Web İşler etkinleştirilmiş proje oluşturmak için Konsol Uygulaması proje şablonunu kullanabilir ve [önceki bölümde](#convert)açıklandığı gibi Web İşler dağıtımını etkinleştirebilirsiniz. Alternatif olarak, Webİşler yeni proje şablonu kullanabilirsiniz:

* [Bağımsız bir WebJob için Webİşler yeni proje şablonundan yararlanma](#createnolink)
  
    Bir proje oluşturun ve bir web projesine bağlantısı olmadan tek başına bir WebJob olarak dağıtılmak üzere yapılandırın. Web uygulamasında hiçbir web uygulaması çalışmadan, bir web uygulamasında tek başına bir WebJob çalıştırmak istediğinizde bu seçeneği kullanın. Webİş kaynaklarınızı web uygulama kaynaklarınızdan bağımsız olarak ölçeklendirebilmek için bunu yapmak isteyebilirsiniz.
* [Web projesine bağlı bir WebJob için Webİşler yeni proje şablonunu kullanma](#createlink)
  
    Aynı çözümdeki bir web projesi dağıtıldığında, web işi olarak otomatik olarak dağıtılmak üzere yapılandırılan bir proje oluşturun. WebJob'unuzu ilgili web uygulamasını çalıştırdığınız aynı web uygulamasında çalıştırmak istediğinizde bu seçeneği kullanın.

> [!NOTE]
> WebJobs yeni proje şablonu NuGet paketlerini otomatik olarak yükler ve [Webİşler SDK](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs)için *Program.cs* kod içerir. WebJobs SDK'yı kullanmak istemiyorsanız, Program.cs ifadesini `host.RunAndBlock` kaldırın veya değiştirin. *Program.cs*
> 
> 

#### <a name="use-the-webjobs-new-project-template-for-an-independent-webjob"></a><a id="createnolink"></a>Bağımsız bir WebJob için Webİşler yeni proje şablonundan yararlanma
1. **Yeni Proje** **Dosyası'nı** > tıklatın ve ardından Yeni **Proje** iletişim kutusunda **Cloud** > **Azure WebJob'u (.NET Framework)** tıklatın.
   
    ![WebJob şablonu gösteren yeni Proje iletişim kutusu](./media/webjobs-dotnet-deploy-vs/np.png)
2. Konsol Uygulaması projesini [bağımsız bir Webİşler projesi yapmak](#convertnolink)için daha önce gösterilen yönergeleri izleyin.

#### <a name="use-the-webjobs-new-project-template-for-a-webjob-linked-to-a-web-project"></a><a id="createlink"></a>Web projesine bağlı bir WebJob için Webİşler yeni proje şablonunu kullanma
1. **Solution Explorer'daki**web projesine sağ tıklayın ve ardından**Yeni Azure Webİş Projesi** **Ekle'yi** > tıklatın.
   
    ![Yeni Azure WebJob Project menü girişi](./media/webjobs-dotnet-deploy-vs/nawj.png)
   
    [Azure Webİş Ekle](#configure) iletişim kutusu görüntülenir.
2. Azure [Webİş Ekle](#configure) iletişim kutusunu doldurun ve **ardından Tamam'ı**tıklatın.

### <a name="the-add-azure-webjob-dialog"></a><a id="configure"></a>Azure Webİş Ekle iletişim kutusu
**Azure Webİş Ekle** iletişim kutusu, WebJob adını girmenizi ve WebJob'unuzun modu ayarını çalıştırmanızı sağlar. 

![Azure Webİş iletişim kutusu ekleme](./media/webjobs-dotnet-deploy-vs/aaw2.png)

Bu iletişim kutusundaki alanlar, Azure portalının **Webİş Ekle** iletişim kutusundaki alanlara karşılık gelir. Daha fazla bilgi için Bkz. [Web İşleri ile Arka Plan görevlerini Çalıştır.](webjobs-create.md)

> [!NOTE]
> * Komut satırı dağıtımı hakkında bilgi için bkz: [Komut Satırı Etkinleştirme veya Azure Web İşlerinin Sürekli Teslimi.](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
> * Bir WebJob dağıtıyor ve ardından Webİş türünü değiştirmek ve yeniden dağıtmak istediğinize karar verirseniz, *web işleri-publish-settings.json* dosyasını silmeniz gerekir. Bu, Visual Studio'nun yayımlama seçeneklerini yeniden göstermesini sağlayacaktır, böylece WebJob türünü değiştirebilirsiniz.
> * Bir WebJob dağıtır ve daha sonra çalışma modunu sürekliden sürekli veya tam tersi olarak değiştirirseniz, Visual Studio yeniden dağıttığınızda Azure'da yeni bir WebJob oluşturur. Diğer zamanlama ayarlarını değiştirir, ancak çalışma modunu aynı bırakır veya Zamanlanmış ve İsteğe Bağlı arasında geçiş ayarlarsanız, Visual Studio yeni bir iş oluşturmak yerine varolan işi güncelleştirir.
> 
> 

### <a name="webjob-publish-settingsjson"></a><a id="publishsettings"></a>webjob-publish-settings.json
Web İşler dağıtımı için bir Konsol Uygulaması yapılandırdığınızda, Visual Studio [Microsoft.Web.WebJobs.Publish](https://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paketini yükler ve zamanlama bilgilerini Webİşler projesinin proje *Özellikleri* klasöründe bir *webiş yayımlama-settings.json* dosyasında saklar. İşte bu dosyanın bir örneği:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

Bu dosyayı doğrudan edinebilirsiniz ve Visual Studio IntelliSense sağlar. Dosya şeması depolanır [https://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) ve orada görüntülenebilir.  

### <a name="webjobs-listjson"></a><a id="webjobslist"></a>webjobs-list.json
Web İşler etkinleştirilmiş bir projeyi bir web projesine bağladiğinizde, Visual Studio WebJobs projesinin adını *web işi listesi.json* dosyasında web projesinin *Özellikleri* klasöründe depolar. Liste, aşağıdaki örnekte gösterildiği gibi birden çok Web İşler i

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

Bu dosyayı doğrudan edinebilirsiniz ve Visual Studio IntelliSense sağlar. Dosya şeması depolanır [https://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) ve orada görüntülenebilir.

### <a name="deploy-a-webjobs-project"></a><a id="deploy"></a>Web İşler projesini dağıtma
Bir web projesine bağlandığınız bir Webİşler projesi, web projesiyle otomatik olarak dağıtılır. Web projesi dağıtımı hakkında daha fazla bilgi için, sol navigasyonda**uygulama** dağıtma **kılavuzuna** > bakın.

Bir Webİşler projesini tek başına dağıtmak için **Solution Explorer'da** projeyi sağ tıklatın ve **Azure Webİş olarak yayımla'yı tıklatın...**. 

![Azure WebJob olarak yayımlama](./media/webjobs-dotnet-deploy-vs/paw.png)

Bağımsız bir WebJob için, web projeleri için kullanılan aynı **Web Yayımlama** sihirbazı görüntülenir, ancak daha az ayar değiştirilebilir.

## <a name="scheduling-a-triggered-webjob"></a>Tetiklenen Web İşi zamanlama

WebJobs, Bir WebJob'un ne zaman çalıştırılırken belirlemek için *bir settings.job* dosyasını kullanır. Webİş'iniz için bir yürütme çizelgesi ayarlamak için bu dosyayı kullanın. Aşağıdaki örnek, her saat 09:00-17:00 saatleri arasında çalışır:

```json
{
    "schedule": "0 0 9-17 * * *"
}
```

Bu dosya, Webİş klasörünün kökünde, Webİş'inizin komut dosyasıyla `wwwroot\app_data\jobs\triggered\{job name}` birlikte `wwwroot\app_data\jobs\continuous\{job name}`veya . Visual Studio'dan bir WebJob dağıtırken, dosya özelliklerinizi `settings.job` daha **yeniyse Kopya**olarak işaretleyin. 

Azure [portalından bir WebJob](webjobs-create.md)oluşturduğunuzda, settings.job dosyası sizin için oluşturulur.

[!INCLUDE [webjobs-alwayson-note](../../includes/webjobs-always-on-note.md)]

### <a name="cron-expressions"></a>CRON ifadeleri

WebJobs, Azure İşlevleri'nde zamanlayıcı tetikleyicisi olarak zamanlama için aynı CRON ifadelerini kullanır. CRON desteği hakkında daha fazla bilgi edinmek için [zamanlayıcı tetikleyici başvuru makalesine](../azure-functions/functions-bindings-timer.md#ncrontab-expressions)bakın.

[!INCLUDE [webjobs-cron-timezone-note](../../includes/webjobs-cron-timezone-note.md)]

### <a name="settingsjob-reference"></a>settings.job reference

Aşağıdaki ayarlar Webİşler tarafından desteklenir:

| **Ayar** | **Tür**  | **Açıklama** |
| ----------- | --------- | --------------- |
| `is_in_place` | Tümü | İlk olarak geçici bir klasöre kopyalanmadan işin yerinde çalışmasını sağlar. Daha fazla bilgi edinmek için [Webİşler çalışma dizinine](https://github.com/projectkudu/kudu/wiki/WebJobs#webjob-working-directory)bakın. |
| `is_singleton` | Sürekli | Webİşler'i yalnızca ölçeklendirildiğinde tek bir örnekte çalıştırın. Daha fazla bilgi için bkz: [Singleton olarak sürekli bir iş ayarla.](https://github.com/projectkudu/kudu/wiki/WebJobs-API#set-a-continuous-job-as-singleton) |
| `schedule` | Tetik -lenen | WebJob'u CRON tabanlı bir zamanlamayla çalıştırın. Daha fazla bilgi için [zamanlayıcı tetikleyici başvuru makalesine](../azure-functions/functions-bindings-timer.md#ncrontab-expressions)bakın. |
| `stopping_wait_time`| Tümü | Kapatma davranışının denetimini sağlar. Daha fazla bilgi [için, Bkz. Zarif kapatma.](https://github.com/projectkudu/kudu/wiki/WebJobs#graceful-shutdown) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [WebJobs SDK hakkında daha fazla bilgi edinin](webjobs-sdk-how-to.md)
