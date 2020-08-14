---
title: Visual Studio kullanarak Web Işleri geliştirme ve dağıtma
description: Visual Studio 'da Azure Web Işleri geliştirmeyi ve zamanlanan bir görev oluşturma da dahil olmak üzere Azure App Service ' ye dağıtmayı öğrenin.
author: ggailey777
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.topic: conceptual
ms.custom: devx-track-csharp, vs-azure
ms.date: 07/30/2020
ms.author: glenga
ms.reviewer: david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: 14bb693ccaa1b1d16a1d07b7ee1cdeb4493960f5
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88212873"
---
# <a name="develop-and-deploy-webjobs-using-visual-studio"></a>Visual Studio kullanarak Web Işleri geliştirme ve dağıtma

Bu makalede, bir konsol uygulama projesini [Azure WebJob](https://go.microsoft.com/fwlink/?LinkId=390226)olarak [Azure App Service](overview.md) bir Web uygulamasına dağıtmak için Visual Studio 'nun nasıl kullanılacağı açıklanmaktadır. [Azure Portal](https://portal.azure.com)kullanarak WebJobs dağıtma hakkında daha fazla bilgi için, bkz. [Azure App Service Web işleri ile arka plan görevleri çalıştırma](webjobs-create.md).

[.NET Core uygulaması](#webjobs-as-net-core-console-apps) veya [.NET Framework uygulaması](#webjobs-as-net-framework-console-apps)olarak çalışan bir WebJob geliştirmeyi tercih edebilirsiniz. [Azure WebJobs SDK 'sının](webjobs-sdk-how-to.md) 3. x sürümü, .NET Core uygulamaları veya .NET Framework uygulamaları olarak çalışan Web işleri geliştirmenize olanak sağlarken, sürüm 2. x yalnızca .NET Framework destekler. Bir WebJobs projesi dağıtmanın yolu, .NET Core projeleri için .NET Framework projelerine göre farklılık gösteren bir yöntemdir.

Web uygulamasındaki her WebJob 'un benzersiz bir adı olması şartıyla, birden çok Web Işini tek bir Web uygulamasına yayımlayabilirsiniz.

## <a name="webjobs-as-net-core-console-apps"></a>.NET Core konsol uygulamaları olarak Web Işleri

Azure WebJobs SDK 'nın 3. x sürümünü içeren Web Işleri oluşturup .NET Core konsol uygulamaları olarak yayımlayabilirsiniz. .NET Core konsol uygulamasını bir Web Işi olarak Azure 'da oluşturup yayımlamaya yönelik adım adım yönergeler için bkz. [olay odaklı arka plan işleme Için Azure WebJobs SDK ile çalışmaya başlama](webjobs-sdk-get-started.md).

> [!NOTE]
> .NET Core Web Işleri Web projeleriyle bağlanamaz. WebJob 'unuzun bir Web uygulamasıyla dağıtılması gerekiyorsa, Web [işlerinizi .NET Framework konsol uygulaması olarak oluşturun](#webjobs-as-net-framework-console-apps).  

### <a name="deploy-to-azure-app-service"></a>Azure App Service'e dağıtma

.NET Core WebJob 'u Visual Studio 'dan Azure App Service yayımlamak, bir ASP.NET Core uygulaması yayımlamayla aynı araçları kullanır.

[!INCLUDE [webjobs-publish-net-core](../../includes/webjobs-publish-net-core.md)] 

## <a name="webjobs-as-net-framework-console-apps"></a>Web Işleri .NET Framework konsol uygulamaları olarak  

Web Işleri etkinleştirilmiş bir .NET Framework konsol uygulama projesini dağıtmak için Visual Studio kullanıyorsanız, çalışma zamanı dosyalarını Web uygulamasındaki uygun klasöre kopyalar App_Data (sürekli WebJobs için */Jobs/sürekli* ve zamanlanan ya da Isteğe bağlı Web işleri için *tetiklenen App_Data/Jobs/)* .

Visual Studio, bir Web Işleri etkin projesine aşağıdaki öğeleri ekler:

* [Microsoft. Web. WebJobs.](https://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paketini yayımlayın.
* Dağıtım ve zamanlayıcı ayarlarını içeren dosyadaki bir [webjob-publish-settings.js](#publishsettings) . 

![Bir Web Işi olarak dağıtımı etkinleştirmek üzere konsol uygulamasına nelerin eklendiğini gösteren diyagram](./media/webjobs-dotnet-deploy-vs/convert.png)

Bu öğeleri varolan bir konsol uygulaması projesine ekleyebilir veya yeni bir Web Işleri etkinleştirilmiş konsol uygulaması projesi oluşturmak için şablon kullanabilirsiniz. 

Bir projeyi kendi başına WebJob olarak dağıtın veya Web projesini her dağıttığınızda otomatik olarak dağıtılacak şekilde bir web projesine bağlayın. Visual Studio, projeleri bağlamak için Web projesindeki dosyadaki bir [webjobs-list.js](#webjobslist) Web işleri özellikli projenin adını içerir.

![Web projesine yönelik WebJob projesi bağlantısını gösteren diyagram](./media/webjobs-dotnet-deploy-vs/link.png)

### <a name="prerequisites"></a>Ön koşullar

[Azure geliştirme iş yüküyle](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-4---choose-workloads)visual Studio 2017 veya visual Studio 2019 ' ü yükler.

### <a name="enable-webjobs-deployment-for-an-existing-console-app-project"></a><a id="convert"></a> Mevcut bir konsol uygulaması projesi için Web Işleri dağıtımını etkinleştir

İki seçeneğiniz vardır:

* [Bir Web projesiyle otomatik dağıtımı etkinleştirin](#convertlink).

  Bir Web projesi dağıtırken, var olan bir konsol uygulaması projesini otomatik olarak WebJob olarak dağıtması için yapılandırın. WebJob uygulamanızı ilgili Web uygulamasını çalıştırdığınız Web uygulamasında çalıştırmak istediğinizde bu seçeneği kullanın.

* [Web projesi olmadan dağıtımı etkinleştirin](#convertnolink).

  Mevcut bir konsol uygulaması projesini, bir web projesine bağlantı olmadan, kendi başına WebJob olarak dağıtılacak şekilde yapılandırın. Web uygulamasında Web uygulaması üzerinde çalışan bir Web uygulaması olmadan, bir Web uygulamasında bir WebJob çalıştırmak istediğinizde bu seçeneği kullanın. Web uygulaması kaynaklarınızdan bağımsız olarak WebJob kaynaklarınızı ölçeklendirmek isteyebilirsiniz.

#### <a name="enable-automatic-webjobs-deployment-with-a-web-project"></a><a id="convertlink"></a> Web projesi ile otomatik Web Işleri dağıtımını etkinleştirme

1. **Çözüm Gezgini**web projesine sağ tıklayın ve ardından **Add**  >  **mevcut projeyi Azure WebJob olarak**Ekle ' yi seçin.
   
    ![Azure WebJob olarak mevcut proje](./media/webjobs-dotnet-deploy-vs/eawj.png)
   
    [Azure WebJob Ekle](#configure) iletişim kutusu görüntülenir.
2. **Proje adı** açılır listesinde, WebJob olarak eklenecek konsol uygulama projesini seçin.
   
    ![Azure WebJob Ekle iletişim kutusunda proje seçiliyor](./media/webjobs-dotnet-deploy-vs/aaw1.png)
3. [Azure WebJob Ekle](#configure) iletişim kutusunu tamamlayıp **Tamam**' ı seçin. 

#### <a name="enable-webjobs-deployment-without-a-web-project"></a><a id="convertnolink"></a> Web projesi olmadan WebJobs dağıtımını etkinleştirme
1. **Çözüm Gezgini**' de konsol uygulaması projesine sağ tıklayın ve ardından **Azure WebJob olarak Yayımla**' yı seçin. 
   
    ![Azure WebJob olarak Yayımla](./media/webjobs-dotnet-deploy-vs/paw.png)
   
    **Proje adı** kutusunda seçilen proje Ile [Azure WebJob Ekle](#configure) iletişim kutusu görünür.
2. [Azure WebJob Ekle](#configure) iletişim kutusunu tamamlayıp **Tamam**' ı seçin.
   
   **Web 'ı Yayımla** Sihirbazı görüntülenir. Hemen yayınlamak istemiyorsanız, Sihirbazı kapatın. Girdiğiniz ayarlar, [projeyi dağıtmak](#deploy)istediğinizde için kaydedilir.

### <a name="create-a-new-webjobs-enabled-project"></a><a id="create"></a>Yeni bir Web Işleri etkin proje oluşturun
Yeni bir WebJobs özellikli proje oluşturmak için konsol uygulaması proje şablonunu kullanın ve [önceki bölümde](#convert)açıklandığı gibi Web işleri dağıtımını etkinleştirin. Alternatif olarak, Web Işleri yeni-proje şablonunu kullanabilirsiniz:

* [Bağımsız bir WebJob için WebJobs yeni-proje şablonunu kullanın](#createnolink)
  
    Bir proje oluşturun ve bir Web projesi bağlantısı olmadan kendisini bir WebJob olarak dağıtmak üzere yapılandırın. Web uygulamasında Web uygulaması üzerinde çalışan bir Web uygulaması olmadan, bir Web uygulamasında bir WebJob çalıştırmak istediğinizde bu seçeneği kullanın. Web uygulaması kaynaklarınızdan bağımsız olarak WebJob kaynaklarınızı ölçeklendirmek isteyebilirsiniz.
* [Web projesine bağlı bir WebJob için WebJobs yeni-proje şablonunu kullanın](#createlink)
  
    Aynı çözümde bir Web projesi dağıtırken, WebJob olarak otomatik olarak dağıtılacak şekilde yapılandırılan bir proje oluşturun. WebJob uygulamanızı ilgili Web uygulamasını çalıştırdığınız Web uygulamasında çalıştırmak istediğinizde bu seçeneği kullanın.

> [!NOTE]
> Web Işleri yeni-proje şablonu, NuGet paketlerini otomatik olarak kurar ve [Web İşleri SDK 'sı](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs)için *program.cs* içinde kod içerir. WebJobs SDK 'sını kullanmak istemiyorsanız, `host.RunAndBlock` *program.cs*içindeki ifadeyi kaldırın veya değiştirin.
> 
> 

#### <a name="use-the-webjobs-new-project-template-for-an-independent-webjob"></a><a id="createnolink"></a> Bağımsız bir WebJob için WebJobs yeni-proje şablonunu kullanın
1. **Dosya**  >  **Yeni**  >  **Proje**' yi seçin. **Yeni bir proje Crete** Iletişim kutusunda C# için **Azure WebJob (.NET Framework)** araması yapın ve seçin.
   
2. [Konsol uygulama projesini bağımsız bir WebJobs projesi haline getirmek](#convertnolink)için önceki yönleri izleyin.

#### <a name="use-the-webjobs-new-project-template-for-a-webjob-linked-to-a-web-project"></a><a id="createlink"></a> Web projesine bağlı bir WebJob için WebJobs yeni-proje şablonunu kullanın
1. **Çözüm Gezgini**web projesine sağ tıklayın ve ardından **Add**  >  **yeni Azure WebJob projesi**Ekle ' yi seçin.
   
    ![Yeni Azure WebJob projesi menü girdisi](./media/webjobs-dotnet-deploy-vs/nawj.png)
   
    [Azure WebJob Ekle](#configure) iletişim kutusu görüntülenir.
2. [Azure WebJob Ekle](#configure) iletişim kutusunu tamamlayıp **Tamam**' ı seçin.


### <a name="webjob-publish-settingsjson-file"></a><a id="publishsettings"></a> Dosya üzerindewebjob-publish-settings.js
Web Işleri dağıtımı için bir konsol uygulaması yapılandırdığınızda, Visual Studio [Microsoft. Web. WebJobs. Publish paketini yayımlar](https://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) ve zamanlama bilgilerini Web işleri projesinin proje *özellikleri* klasöründeki bir *webjob-publish-settings.js* dosyasında depolar. Bu dosyaya bir örnek aşağıda verilmiştir:

```json
{
  "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
  "webJobName": "WebJob1",
  "startTime": "null",
  "endTime": "null",
  "jobRecurrenceFrequency": "null",
  "interval": null,
  "runMode": "Continuous"
}
```

Bu dosyayı doğrudan düzenleyebilirsiniz ve Visual Studio IntelliSense sağlar. Dosya şeması konumunda depolanır [https://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) ve burada görüntülenebilir.  

### <a name="webjobs-listjson-file"></a><a id="webjobslist"></a> Dosya üzerindewebjobs-list.js
WebJobs özellikli bir projeyi bir web projesine bağladığınızda, Visual Studio WebJobs projesinin adını Web projesinin *Özellikler* klasöründeki bir *webjobs-list.js* dosyasında depolar. Listede, aşağıdaki örnekte gösterildiği gibi birden çok WebJobs projesi bulunabilir:

```json
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
```

IntelliSense ile bu dosyayı doğrudan Visual Studio 'da düzenleyebilirsiniz. Dosya şeması konumunda depolanır [https://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) .

### <a name="deploy-a-webjobs-project"></a><a id="deploy"></a>WebJobs projesi dağıtma
Web projesine bağladığınız bir WebJobs projesi, Web projesi ile otomatik olarak dağıtılır. Web projesi dağıtımı hakkında daha fazla bilgi için bkz. **nasıl yapılır kılavuzlarında**  >  **uygulamayı** sol gezinmede dağıtma.

Bir WebJobs projesini kendi başına dağıtmak için **Çözüm Gezgini** ' de projeye sağ tıklayın ve **Azure WebJob olarak Yayımla**' yı seçin. 

![Azure WebJob olarak Yayımla](./media/webjobs-dotnet-deploy-vs/paw.png)

Bağımsız bir WebJob için Web projeleri için kullanılan aynı **Web yayımlama** Sihirbazı görüntülenir, ancak daha az ayarı değiştirilebilir.

### <a name="add-azure-webjob-dialog-box"></a><a id="configure"></a>Azure WebJob Ekle iletişim kutusu
**Azure WebJob Ekle** iletişim kutusu, WebJob için WebJob adını ve çalıştırma modu ayarını girmenize olanak sağlar. 

![Azure WebJob Ekle iletişim kutusu](./media/webjobs-dotnet-deploy-vs/aaw2.png)

Bu iletişim kutusundaki bazı alanlar Azure portal **WebJob Ekle** iletişim kutusunda bulunan alanlara karşılık gelir. Daha fazla bilgi için bkz. [Azure App Service Web işleri ile arka plan görevleri çalıştırma](webjobs-create.md).

WebJob dağıtım bilgileri:

* Komut satırı dağıtımı hakkında daha fazla bilgi için bkz. [Azure WebJobs 'un komut satırı veya sürekli teslimini etkinleştirme](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).

* Bir WebJob dağıtırsanız ve sonra WebJob türünü değiştirmek ve yeniden dağıtmak istediğinize karar verirseniz, dosyadaki *webjobs-publish-settings.js* silin. Bunun yapılması, Visual Studio 'nun yayımlama seçeneklerini yeniden görüntülemelerini sağlar, böylece WebJob türünü değiştirebilirsiniz.

* Bir WebJob dağıtırsanız ve daha sonra çalışma modunu sürekli olarak sürekli olmayan veya tam tersi olarak değiştirirseniz, Visual Studio yeniden dağıtırken Azure 'da yeni bir WebJob oluşturur. Diğer zamanlama ayarlarını değiştirirseniz, ancak çalıştırma modunu aynı veya zamanlanmış ve Isteğe bağlı olarak bırakırsanız, Visual Studio yeni bir tane oluşturmak yerine mevcut işi güncelleştirir.

## <a name="webjob-types"></a>WebJob türleri

WebJob türü *tetiklenebilir* ya da *sürekli*olabilir:

- Tetiklenen (varsayılan): tetiklenen bir WebJob, bağlama olayına, [zamanlamaya](#scheduling-a-triggered-webjob)göre veya el ile (isteğe bağlı) tetiklendiğinde başlar. Web uygulamasının üzerinde çalıştığı tüm örneklerde çalışır, ancak isteğe bağlı olarak WebJob 'u tek bir örneğe kısıtlayabilirsiniz.

- Sürekli: [sürekli](#continuous-execution) WebJob, WebJob oluşturulduğunda hemen başlar. Bu tür bir WebJob, sınırsız veya uzun süre çalışan işler için idealdir. İş sona erdirmek için yeniden başlatabilirsiniz.  

[!INCLUDE [webjobs-alwayson-note](../../includes/webjobs-always-on-note.md)]

### <a name="scheduling-a-triggered-webjob"></a>Tetiklenen Web İşi zamanlama

Bir konsol uygulamasını Azure 'Da yayımladığınızda, Visual Studio WebJob türünü varsayılan olarak **tetiklenen** olarak ayarlar ve projeye yeni bir *Settings. job* dosyası ekler. Tetiklenen WebJob türleri için bu dosyayı kullanarak WebJob için bir yürütme zamanlaması ayarlayabilirsiniz.

WebJob için bir yürütme zamanlaması ayarlamak üzere *Settings. job* dosyasını kullanın. Aşağıdaki örnek, 9 ' dan 5 ' e kadar her saat çalışır:

```json
{
    "schedule": "0 0 9-17 * * *"
}
```

Bu dosya, veya gibi WebJob betiğiyle WebJobs klasörünün kökünde bulunur `wwwroot\app_data\jobs\triggered\{job name}` `wwwroot\app_data\jobs\continuous\{job name}` . Visual Studio 'dan bir WebJob dağıttığınızda, **daha yeniyse**, Visual Studio 'daki *ayarlar. iş* dosyası özellikleri ' ni Kopyala olarak işaretleyin.

[Azure Portal bir WebJob oluşturursanız](webjobs-create.md), *Settings. job* dosyası sizin için oluşturulur.

#### <a name="cron-expressions"></a>CRON ifadeleri

WebJobs, Azure Işlevlerinde Zamanlayıcı tetikleyicisi olarak zamanlama için aynı CRON ifadelerini kullanır. CRON desteği hakkında daha fazla bilgi edinmek için bkz. [Azure işlevleri Için süreölçer tetikleyicisi](../azure-functions/functions-bindings-timer.md#ncrontab-expressions).

[!INCLUDE [webjobs-cron-timezone-note](../../includes/webjobs-cron-timezone-note.md)]

#### <a name="settingsjob-reference"></a>ayarlar. iş başvurusu

Web Işleri tarafından aşağıdaki ayarlar desteklenir:

| **Ayar** | **Tür**  | **Açıklama** |
| ----------- | --------- | --------------- |
| `is_in_place` | Tümü | WebJob 'un, önce geçici bir klasöre kopyalanmadan yerinde çalışmasına izin verir. Daha fazla bilgi için bkz. [WebJob çalışma dizini](https://github.com/projectkudu/kudu/wiki/WebJobs#webjob-working-directory). |
| `is_singleton` | Sürekli | Web işini yalnızca ölçeklendirildiğinde tek bir örnek üzerinde çalıştırın. Daha fazla bilgi için bkz. [tek bir iş olarak sürekli olarak ayarlama](https://github.com/projectkudu/kudu/wiki/WebJobs-API#set-a-continuous-job-as-singleton). |
| `schedule` | Diğini | WebJob 'u bir CRON tabanlı zamanlamaya göre çalıştırın. Daha fazla bilgi için bkz. [Ncrontab ifadeleri](../azure-functions/functions-bindings-timer.md#ncrontab-expressions). |
| `stopping_wait_time`| Tümü | , Kapatılma davranışının denetimine izin verir. Daha fazla bilgi için bkz. [düzgün kapanma](https://github.com/projectkudu/kudu/wiki/WebJobs#graceful-shutdown). |

### <a name="continuous-execution"></a>Sürekli yürütme

Azure 'da **her zaman açık** ' ı etkinleştirirseniz, WebJob 'u sürekli çalışacak şekilde değiştirmek Için Visual Studio 'yu kullanabilirsiniz:

1. Daha önce yapmadıysanız, [projeyi Azure 'da yayımlayın](#deploy-to-azure-app-service).

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. **Yayımla** sekmesinde **Düzenle**' yi seçin. 

1. **Profil ayarları** iletişim kutusunda, **WebJob türü**için **sürekli** ' i seçin ve ardından **Kaydet**' i seçin.

    ![WebJob için ayarları Yayımla iletişim kutusu](./media/webjobs-dotnet-deploy-vs/publish-settings.png)

1. WebJob 'u güncelleştirilmiş ayarlarla yeniden yayımlamak için **Yayımla** sekmesinde **Yayımla** ' yı seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web Işleri SDK 'Sı hakkında daha fazla bilgi edinin](webjobs-sdk-how-to.md)
