---
title: Application Insights Profiler ile Core Azure Linux web uygulamaları ASP.NET profil | Microsoft Dokümanlar
description: Application Insights Profiler'ın nasıl kullanılacağı nasıI kavramsal bir genel bakış ve adım adım öğretici.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 02/23/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 9c98cd5d3d4d76f9455e4c036aa32a4ead20cfff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671724"
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Application Insights Profiler ile Core Azure Linux web uygulamalarını profil ASP.NET

Bu özellik şu anda önizleme sürümündedir.

[Application Insights'ı](../../azure-monitor/app/app-insights-overview.md)kullanırken canlı web uygulamanızın her yönteminde ne kadar zaman harcandığını öğrenin. Application Insights Profiler artık Azure Uygulama Hizmeti'nde Linux'ta barındırılan ASP.NET Core web uygulamaları için kullanılabilir. Bu kılavuz, Profiler izlerinin Core Linux web uygulamaları ASP.NET için nasıl toplanabileceği yle ilgili adım adım talimatlar sağlar.

Bu izlemi tamamladıktan sonra, uygulamanız resimde gösterilen izlemeler gibi Profiler izlerini toplayabilir. Bu örnekte, Profiler izleme belirli bir web isteği bekleme harcanan zaman nedeniyle yavaş olduğunu gösterir. Uygulamayı yavaşlatan koddaki *sıcak yol* bir alev simgesiyle işaretlenir. **HomeController** bölümündeki **About** yöntemi, **thread.sleep** işlevini aradığı için web uygulamasını yavaşlatıyor.

![Profil oluşturucu izleri](./media/profiler-aspnetcore-linux/profiler-traces.png)

## <a name="prerequisites"></a>Ön koşullar
Aşağıdaki yönergeler tüm Windows, Linux ve Mac geliştirme ortamları için geçerlidir:

* [.NET Core SDK 2.1.2 veya sonrası'nı](https://dotnet.microsoft.com/download/archives)yükleyin.
* Başlarken Yönergeleri izleyerek Git'i yükleyin [- Git'i Yükleyin.](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## <a name="set-up-the-project-locally"></a>Projeyi yerel olarak ayarlama

1. Makinenizde komut istemi penceresi açın. Aşağıdaki yönergeler tüm Windows, Linux ve Mac geliştirme ortamları için çalışır.

1. Core MVC web uygulaması ASP.NET oluşturun:

    ```
    dotnet new mvc -n LinuxProfilerTest
    ```

1. Çalışma dizini proje için kök klasörü değiştirin.

1. Profiler izlerini toplamak için NuGet paketini ekleyin:

    ```shell
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

1. Program.cs'da Uygulama Öngörülerini Etkinleştirin:

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseApplicationInsights() // Add this line of code to Enable Application Insights
            .UseStartup<Startup>();
    ```
    
1. profiler'ı Startup.cs etkinleştirin:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddServiceProfiler(); // Add this line of code to Enable Profiler
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    }
    ```

1. Rasgele birkaç saniye geciktirmek için **HomeController.cs** bölümüne bir kod satırı ekleyin:

    ```csharp
        using System.Threading;
        ...

        public IActionResult About()
            {
                Random r = new Random();
                int delay = r.Next(5000, 10000);
                Thread.Sleep(delay);
                return View();
            }
    ```

1. Değişikliklerinizi yerel depoya kaydedin ve işlemeyin:

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-the-linux-web-app-to-host-your-project"></a>Projenizi barındırmak için Linux web uygulamasını oluşturun

1. Linux'ta Uygulama Hizmeti'ni kullanarak web uygulaması ortamını oluşturun:

    ![Linux web uygulamasını oluşturma](./media/profiler-aspnetcore-linux/create-linux-appservice.png)

2. Dağıtım kimlik bilgilerini oluşturun:

    > [!NOTE]
    > Web uygulamanızı dağıtırken daha sonra kullanmak üzere parolanızı kaydedin.

    ![Dağıtım kimlik bilgilerini oluşturma](./media/profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Dağıtım seçeneklerini seçin. Azure portalındaki yönergeleri izleyerek web uygulamasında yerel bir Git deposu ayarlayın. Git deposu otomatik olarak oluşturulur.

    ![Git deposunu ayarlama](./media/profiler-aspnetcore-linux/setup-git-repo.png)

Daha fazla dağıtım seçeneği için [bu makaleye](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type)bakın.

## <a name="deploy-your-project"></a>Projenizi dağıtma

1. Komut İstem penceresinizde, projenizin kök klasörüne göz atın. Uygulama Hizmeti'ndeki depoyu işaret etmek için git uzaktan depo ekleyin:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```

    * Dağıtım kimlik bilgilerini oluşturmak için kullandığınız **kullanıcı adını** kullanın.
    * Linux'taki Uygulama Hizmeti'ni kullanarak web uygulamasını oluşturmak için kullandığınız **uygulama adını** kullanın.

2. Değişiklikleri Azure'a iterek projeyi dağıtın:

    ```
    git push azure master
    ```

Aşağıdaki örneğe benzer çıktı görmeniz gerekir:

    ```
    Counting objects: 9, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (8/8), done.
    Writing objects: 100% (9/9), 1.78 KiB | 911.00 KiB/s, done.
    Total 9 (delta 3), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id 'd7369a99d7'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling ASP.NET Core Web Application deployment.
    remote: ......
    remote:   Restoring packages for /home/site/repository/EventPipeExampleLinux.csproj...
    remote: .
    remote:   Installing Newtonsoft.Json 10.0.3.
    remote:   Installing Microsoft.ApplicationInsights.Profiler.Core 1.1.0-LKG
    …

    ```

## <a name="add-application-insights-to-monitor-your-web-apps"></a>Web uygulamalarınızı izlemek için Uygulama Öngörüleri ekleyin

1. [Uygulama Öngörüleri kaynağı oluşturun.](./../../azure-monitor/app/create-new-resource.md )

2. Application Insights kaynağının **iKey** değerini kopyalayın ve web uygulamalarınızda aşağıdaki ayarları ayarlayın:

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ```

    Uygulama ayarları değiştirildiğinde, site otomatik olarak yeniden başlatılır. Yeni ayarlar uygulandıktan sonra Profiler hemen iki dakika çalışır. Profiler daha sonra her saat başı iki dakika çalışır.

3. Web sitenize biraz trafik oluşturun. **Hakkında** sayfayı birkaç kez yenileyerek trafik oluşturabilirsiniz.

4. Olayların Uygulama Öngörüleri'ne topolması için iki ila beş dakika bekleyin.

5. Azure portalındaki Uygulama Öngörüleri **Performans** bölmesine göz atın. Bölmenin sağ alt kısmındaki Profiler izlerini görüntüleyebilirsiniz.

    ![Profiler izlerini görüntüle](./media/profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="profile-now-button-doesnt-work-for-linux-profiler"></a>Profil Şimdi düğmesi Linux Profiler için çalışmıyor
App Insights profil oluşturucusu Linux sürümü, profil şimdi düğmesini kullanarak isteğe bağlı profil oluşturmayı henüz desteklemez.


## <a name="next-steps"></a>Sonraki adımlar
Azure Uygulama Hizmeti tarafından barındırılan özel kapsayıcılar kullanıyorsanız, Uygulama Öngörüleri Profil oluşturucusu etkinleştirmek [için kapsayıcılaştırılmış ASP.NET Core uygulaması için Servis Profil Oluşturucusu etkinleştir'deki](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) yönergeleri izleyin.

Uygulama Öngörüleri GitHub deposuna herhangi bir sorun veya öneri bildirin: [ApplicationInsights-Profiler-AspNetCore: Sorunlar](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues).
