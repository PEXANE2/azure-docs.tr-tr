---
title: Azure Uygulama Öngörüleri'nde Docker uygulamalarını izleyin | Microsoft Dokümanlar
description: Docker perf sayaçları, olaylar ve özel durumlar, konteyner uygulamalarının telemetrisi ile birlikte Uygulama Öngörüleri'nde görüntülenebilir.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: 5ff3d6a2b31425a3fb6857c8c6aed45f7ef18d64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669616"
---
# <a name="monitor-docker-applications-in-application-insights-deprecated"></a>Uygulama Öngörülerinde Docker uygulamalarını izleyin (Amortismana Uğradı)

> [!NOTE]
> Bu çözüm amortismana uğradı. Konteyner izlemedeki mevcut yatırımlarımız hakkında daha fazla bilgi edinmek [için kapsayıcılar için Azure Monitor'a](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview)bir kez daha çıkmanızı öneririz.

[Docker](https://www.docker.com/) kapsayıcılarından gelen yaşam döngüsü olayları ve performans sayaçları Application Insights'ta grafiklenebilir. Uygulama [Öngörüleri](https://hub.docker.com/r/microsoft/applicationinsights/) görüntüsünü ana bilgisayarınızdaki bir kapsayıcıya yükleyin ve ana bilgisayar için performans sayaçlarının yanı sıra diğer görüntüler için de görüntülenir.

Docker ile uygulamalarınızı tüm bağımlılıklarla birlikte tamamlanmış hafif kaplarda dağıtırsınız. Docker Engine çalıştıran herhangi bir ana makinede çalışacaklar.

Docker ana bilgisayarınızda [Application Insights görüntüsünü](https://hub.docker.com/r/microsoft/applicationinsights/) çalıştırdığınızda aşağıdaki avantajlardan yararlanırsınız:

* Ana bilgisayarda çalışan tüm kaplar hakkında yaşam döngüsü telemetrisi - başlatın, durdurun ve benzeri.
* Tüm kapsayıcılar için performans sayaçları. CPU, bellek, ağ kullanımı ve daha fazlası.
* Kapsayıcılarda çalışan uygulamalarda [Java için Application Insights SDK yüklüyseniz,](../../azure-monitor/app/java-get-started.md) bu uygulamaların tüm telemetrileri kapsayıcı ve ana bilgisayar makinesini tanımlayan ek özelliklere sahip olur. Örneğin, birden fazla ana bilgisayarda çalışan bir uygulama örnekleriniz varsa, uygulama telemetrinizi ana bilgisayara göre kolayca filtreleyebilirsiniz.

## <a name="set-up-your-application-insights-resource"></a>Uygulama Öngörüleri kaynağınızı ayarlama

1. Microsoft [Azure portalında](https://azure.com) oturum açın ve uygulamanız için Application Insights kaynağını açın; veya [yeni bir tane oluşturun.](../../azure-monitor/app/create-new-resource.md ) 
   
    *Hangi kaynağı kullanmalıyım?* Ana bilgisayarınızda çalıştırdığınız uygulamalar başka biri tarafından geliştirildiyse, [yeni bir Uygulama Öngörüleri kaynağı oluşturmanız](../../azure-monitor/app/create-new-resource.md )gerekir. Bu, telemetriyi görüntülediğiniz ve analiz ettiğiniz yerdir. (Uygulama türü için 'Genel' seçeneğini belirleyin.)
   
    Ancak uygulamaların geliştiricisiyseniz, her birine [Application Insights SDK eklediğinizi](../../azure-monitor/app/java-get-started.md) umuyoruz. Bunların hepsi tek bir iş uygulamasının gerçekten bileşenleriyse, hepsini tek bir kaynağa telemetri gönderecek şekilde yapılandırabilirsiniz ve docker yaşam döngüsünü ve performans verilerini görüntülemek için aynı kaynağı kullanırsınız. 
   
    Üçüncü bir senaryo, uygulamaların çoğunu geliştirmiş siniz, ancak bunların telemetrilerini görüntülemek için ayrı kaynaklar kullanıyorsunuz. Bu durumda, büyük olasılıkla Docker verileri için ayrı bir kaynak oluşturmak da isteyebilirsiniz.

2. **Essentials** açılır düğmesini tıklatın ve Enstrümantasyon Anahtarını kopyalayın. Bunu SDK'ya telemetrisini nereye göndereceğini söylemek için kullanıyorsun.

Tarayıcı pencerenizi kullanışlı tutun, çünkü yakında telemetrinize bakmak için geri geleceksiniz.

## <a name="run-the-application-insights-monitor-on-your-host"></a>Uygulama Öngörüleri monitörünü ana bilgisayarda çalıştırın

Artık telemetriyi görüntüleyecek bir yerin olduğuna göre, toplayıp gönderecek konteynerleştirilmiş uygulamayı ayarlayabilirsiniz.

1. Docker ev sahibinize bağlanın.
2. Enstrümantasyon anahtarınızı bu komutta edin ve çalıştırın:
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

Docker ana bilgisayar başına yalnızca bir Application Insights görüntüsü gereklidir. Uygulamanız birden çok Docker ana bilgisayarına dağıtılırsa, her ana bilgisayardaki komutu yineleyin.

## <a name="update-your-app"></a>Uygulamanızı güncelleştirme
Uygulamanız [Java için Application Insights SDK](../../azure-monitor/app/java-get-started.md)ile çalgılanmışsa, projenizdeki ApplicationInsights.xml dosyasına `<TelemetryInitializers>` aşağıdaki satırı ekleyin:

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

Bu, uygulamanızdan gönderilen her telemetri öğesine konteyner ve ana bilgisayar kimliği gibi Docker bilgilerini ekler.

## <a name="view-your-telemetry"></a>Telemetrinizi görüntüleme
Azure portalındaki Application Insights kaynağınıza geri dön.

Docker karosu tıklayın.

Docker uygulamasından gelen verileri kısa bir süre içinde görürsünüz, özellikle de Docker motorunuzda çalışan başka konteynerler varsa.

### <a name="docker-container-events"></a>Docker konteyner etkinlikleri
![örnek](./media/docker/13.png)

Tek tek olayları araştırmak için [Ara'yı](../../azure-monitor/app/diagnostic-search.md)tıklatın. İstediğiniz olayları bulmak için arama yapın ve filtreleyin. Daha fazla ayrıntı almak için herhangi bir etkinliği tıklatın.

### <a name="exceptions-by-container-name"></a>Kapsayıcı adına göre özel durumlar
![örnek](./media/docker/14.png)

### <a name="docker-context-added-to-app-telemetry"></a>Docker bağlamı uygulama telemetrisine eklendi
AI SDK ile yapılan başvurudan gönderilen istek telemetrisi, Docker bağlam bilgileri ile zenginleştirilmiştir.

## <a name="q--a"></a>Soru-Cevap
*Uygulama Öngörüleri Bana Docker'dan alamemem için ne veriyor?*

* Performans sayaçlarının konteyner ve görüntüye göre ayrıntılı dökümü.
* Kapsayıcı ve uygulama verilerini tek bir panoya entegre edin.
* Daha fazla analiz için [telemetriyi](export-telemetry.md) veritabanına, Power BI'ye veya başka bir panoya dışa aktarın.

*Uygulamanın kendisinden telemetriyi nasıl alabilirim?*

* Uygulama Öngörüleri SDK'yı uygulamaya yükleyin. Öğrenin nasıl: [Java web uygulamaları,](../../azure-monitor/app/java-get-started.md) [Windows web uygulamaları](../../azure-monitor/app/asp-net.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar

* [Java için Uygulama Öngörüleri](../../azure-monitor/app/java-get-started.md)
* [Node.js için Application Insights](../../azure-monitor/app/nodejs.md)
* [ASP.NET için Uygulama Öngörüleri](../../azure-monitor/app/asp-net.md)
