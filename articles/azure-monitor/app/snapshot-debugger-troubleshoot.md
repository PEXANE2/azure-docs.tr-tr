---
title: Sorun Giderme Azure Uygulama Öngörüleri Anlık Görüntü Hata Ayıklama
description: Bu makalede, Uygulama Öngörüleri Anlık Görüntü Hata Ayıklama'yı etkinleştirme veya kullanmada sorun yaşayan geliştiricilere yardımcı olmak için sorun giderme adımları ve bilgiler yer alıyor.
ms.topic: conceptual
author: brahmnes
ms.date: 03/07/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 485f35ed249ab7f6bbb987d8c79afe20287cd25a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671418"
---
# <a name="troubleshoot-problems-enabling-application-insights-snapshot-debugger-or-viewing-snapshots"></a><a id="troubleshooting"></a>Uygulama Öngörüleri Anlık Görüntü Hata Ayıklayıcısını veya anlık görüntüleri görüntülemeyi sağlayan sorunları giderme
Uygulamanız için Application Insights Snapshot Debugger'ı etkinleştirdiyseniz, ancak özel durumlar için anlık görüntü görmüyorsanız, sorun gidermek için bu yönergeleri kullanabilirsiniz. Anlık görüntülerin oluşturulmamalarının birçok farklı nedeni olabilir. Olası yaygın nedenlerden bazılarını belirlemek için anlık görüntü durumu denetimini çalıştırabilirsiniz.

## <a name="use-the-snapshot-health-check"></a>Anlık görüntü sistem durumu denetimini kullanma
Çeşitli sık karşılaşılan sorunlar, Hata Ayıklama Anlık Görüntüsünün görünmemesi ile sonuçlanır. Eski bir Anlık Görüntü Toplayıcısı kullanma, örneğin; günlük yükleme sınırına ulaşmak; ya da belki de anlık sadece yüklemek için uzun bir zaman alıyor. Sık karşılaşılan sorunları gidermek için Anlık Görüntü Durumu Denetimi'ni kullanın.

Sizi Anlık Görüntü Durumu Denetimi'ne götüren uçtan uca izleme görünümünün özel durum bölmesinde bir bağlantı vardır.

![Anlık görüntü sistem durumu denetimi girin](./media/snapshot-debugger/enter-snapshot-health-check.png)

Etkileşimli, sohbet benzeri arayüz sık karşılaşılan sorunları arar ve bunları düzeltmeniz için size yol açar.

![Sağlık Kontrolü](./media/snapshot-debugger/healthcheck.png)

Bu sorunu çözmüyorsa, aşağıdaki el ile sorun giderme adımlarına bakın.

## <a name="verify-the-instrumentation-key"></a>Enstrümantasyon anahtarını doğrulayın

Yayınlanmış uygulamanızda doğru enstrümantasyon anahtarını kullandığınızdan emin olun. Genellikle enstrümantasyon anahtarı ApplicationInsights.config dosyasından okunur. Değerin portalda gördüğünüz Application Insights kaynağının enstrümantasyon anahtarıyla aynı olduğunu doğrulayın.

## <a name="preview-versions-of-net-core"></a>.NET Core Önizleme Sürümleri
Uygulama .NET Core'un önizleme sürümünü kullanıyorsa ve Anlık Görüntü Hata Ayıklayıcı sıyrık portaldaki [Application Insights bölmesi](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json) aracılığıyla etkinleştirildiyse, Anlık Görüntü Hataayıklayıcı başlatılamayabilir. [Uygulama Öngörüleri bölmesini](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)etkinleştirmeye ***ek olarak*** uygulama ile [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini eklemek [için önce diğer ortamlar için Anlık Görüntü Debugger'ı etkinleştir'i](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) etkinleştirme yönergelerini izleyin.


## <a name="upgrade-to-the-latest-version-of-the-nuget-package"></a>NuGet paketinin en son sürümüne yükseltin

Anlık Görüntü Hata Ayıklama [portalındaki Uygulama Öngörüleri bölmesi](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)aracılığıyla etkinleştirilmişse, uygulamanız zaten en son NuGet paketini çalıştırıyor olmalıdır. [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini ekleyerek Snapshot Debugger etkinleştirildiyse, Microsoft.ApplicationInsights.SnapshotCollector'ın en son sürümünü kullandığınızdan emin olmak için Visual Studio'nun NuGet Paket Yöneticisi'ni kullanın. Yayın notları şu anda bulunabilir:https://github.com/Microsoft/ApplicationInsights-Home/issues/167

## <a name="check-the-uploader-logs"></a>Yükleyici günlüklerini kontrol edin

Anlık görüntü oluşturulduktan sonra diskte bir minidump dosyası (.dmp) oluşturulur. Ayrı bir yükleyici işlemi, bu minidump dosyasını oluşturur ve ilişkili PDF'lerle birlikte, Uygulama Öngörüleri Anlık Hata Ayıklayıcı depolama alanına yükler. Minidökümü başarıyla yüklendikten sonra diskten silinir. Yükleyici işleminin günlük dosyaları diskte tutulur. Bir Uygulama Hizmeti ortamında, bu oturum `D:\Home\LogFiles`açma'yı bulabilirsiniz. Bu günlük dosyalarını bulmak için Uygulama Hizmeti için Kudu yönetim sitesini kullanın.

1. Uygulama Hizmeti uygulamanızı Azure portalında açın.
2. **Gelişmiş Araçlar'ı**tıklatın veya **Kudu'da**arama yapın.
3. **Git'e**tıklayın.
4. Hata **Ayıklama konsolu** açılır liste kutusunda **CMD'yi**seçin.
5. **LogFiles'ı**tıklatın.

Bir adla `Uploader_` `SnapshotUploader_` veya `.log` uzantıyla başlayan en az bir dosya görmeniz gerekir. Günlük dosyalarını indirmek veya tarayıcıda açmak için uygun simgeyi tıklatın.
Dosya adı, Uygulama Hizmeti örneğini tanımlayan benzersiz bir sonek içerir. Uygulama Hizmeti örneğiniz birden fazla makinede barındırılıyorsa, her makine için ayrı günlük dosyaları vardır. Yükleyici yeni bir minidump dosyası algıladığında, günlük dosyasına kaydedilir. Başarılı bir anlık görüntü ve yükleme örneği aşağıda verilmiştir:

```
SnapshotUploader.exe Information: 0 : Received Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Creating minidump from Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Dump placeholder file created: 139e411a23934dc0b9ea08a626db16c5.dm_
    DateTime=2018-03-09T01:42:41.8728496Z
SnapshotUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7525022Z
SnapshotUploader.exe Information: 0 : Successfully wrote minidump to D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 214.42 MB (uncompressed)
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Upload successful. Compressed size 86.56 MB
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2018-03-09T01:42:59.6809606Z
SnapshotUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2018-03-09T01:42:59.8059929Z
SnapshotUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:59.8530649Z
```

> [!NOTE]
> Yukarıdaki örnek, Microsoft.ApplicationInsights.SnapshotCollector NuGet paketinin 1.2.0 sürümünden dir. Önceki sürümlerde yükleyici işlemi `MinidumpUploader.exe` çağrılır ve günlük daha az ayrıntılıdır.

Önceki örnekte, enstrümantasyon `c12a605e73c44346a984e00000000000`anahtarı . Bu değer, uygulamanız için enstrümantasyon anahtarıyla eşleşmelidir.
Minidökümü kimlik `139e411a23934dc0b9ea08a626db16c5`ile bir anlık görüntü ile ilişkilidir. Bu kimliği daha sonra Application Insights Analytics'teki ilişkili özel durum telemetrisini bulmak için kullanabilirsiniz.

Yükleyici her 15 dakikada bir yeni PDB'leri tarar. Bir örneği aşağıda verilmiştir:

```
SnapshotUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Scanning D:\home\site\wwwroot for local PDBs.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2018-03-09T01:47:19.4614027Z
SnapshotUploader.exe Information: 0 : Deleted PDB scan marker : D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\6368.pdbscan
    DateTime=2018-03-09T01:47:19.4614027Z
```

App _Service'de barındırılan olmayan_ uygulamalarda, yükleyici günlükleri minidump'larla aynı `%TEMP%\Dumps\<ikey>` klasördedir: (enstrümantasyon anahtarınız nerede). `<ikey>`

## <a name="troubleshooting-cloud-services"></a>Sorun Giderme Bulut Hizmetleri
Bulut Hizmetleri'ndeki roller için varsayılan geçici klasör, minidump dosyalarını tutmak için çok küçük olabilir ve bu da anlık görüntü kaybına yol açabilir.
Gereken alan, uygulamanızın toplam çalışma kümesine ve eşzamanlı anlık görüntü sayısına bağlıdır.
32 bit ASP.NET web rolünün çalışma kümesi genellikle 200 MB ile 500 MB arasındadır.
En az iki eşzamanlı anlık görüntü için izin verin.
Örneğin, uygulamanız toplam çalışma kümesinin 1 GB'ını kullanıyorsa, anlık görüntüleri depolamak için en az 2 GB disk alanı olduğundan emin olmalısınız.
Anlık görüntüler için özel bir yerel kaynakla Bulut Hizmeti rolünüzü yapılandırmak için aşağıdaki adımları izleyin.

1. Bulut Hizmeti tanımı (.csdef) dosyasını düzenleyerek Bulut Hizmetinize yeni bir yerel kaynak ekleyin. Aşağıdaki örnekte, 5 `SnapshotStore` GB boyutuyla adlandırılan bir kaynak tanımlanır.
   ```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
   ```

2. Yerel kaynağa işaret eden bir ortam değişkeni `SnapshotStore` eklemek için rolünüzün başlangıç kodunu değiştirin. İşçi Rolleri için kod, rolünüzün yöntemine `OnStart` eklenmelidir:
   ```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
   ```
   Web Rolleri (ASP.NET) için kod, web uygulamanızın `Application_Start` yöntemine eklenmelidir:
   ```csharp
   using Microsoft.WindowsAzure.ServiceRuntime;
   using System;

   namespace MyWebRoleApp
   {
       public class MyMvcApplication : System.Web.HttpApplication
       {
          protected void Application_Start()
          {
             Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
             // TODO: The rest of your application startup code
          }
       }
   }
   ```

3. Rolünüzün ApplicationInsights.config dosyasını güncelleştirerek kullanılan geçici klasör konumunu geçersiz kılın`SnapshotCollector`
   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Use the SnapshotStore local resource for snapshots -->
      <TempFolder>%SNAPSHOTSTORE%</TempFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

## <a name="overriding-the-shadow-copy-folder"></a>Gölge Kopya klasörünü geçersiz kılma

Anlık Görüntü Toplayıcısı başlatıldığında, diskte Anlık Görüntü Yükleyici işlemini çalıştırmak için uygun bir klasör bulmaya çalışır. Seçilen klasör Gölge Kopya klasörü olarak bilinir.

Anlık Görüntü Toplayıcı, enstantane yükleyici ikililerini kopyalama izinlerine sahip olduğundan emin olmak için birkaç iyi bilinen konumu denetler. Aşağıdaki ortam değişkenleri kullanılır:
- Fabric_Folder_App_Temp
- Localappdata
- Appdata
- Temp

Uygun bir klasör bulunamazsa, Anlık Görüntü _Toplayıcısı "Uygun bir gölge kopya klasörü bulamadım"_ diyerek bir hata bildirir.

Kopya başarısız olursa, Anlık `ShadowCopyFailed` Görüntü Toplayıcı bir hata bildirir.

Yükleyici başlatılamazsa, Snapshot Collector bir `UploaderCannotStartFromShadowCopy` hata bildirir. İletinin gövdesi genellikle `System.UnauthorizedAccessException`içerir. Uygulama azaltılmış izinleri olan bir hesap altında çalışıyor, çünkü bu hata genellikle oluşur. Hesabın gölge kopya klasörüne yazma izni vardır, ancak kodu yürütme izni yoktur.

Bu hatalar genellikle başlangıç sırasında meydana geldiğinden, `ExceptionDuringConnect` genellikle _"Yükleyici başlatılamadı"_ diyerek bir hata takip edilir.

Bu hataların üzerinde çalışmak için, yapılandırma seçeneği aracılığıyla `ShadowCopyFolder` gölge kopyalama klasörünü el ile belirtebilirsiniz. Örneğin, ApplicationInsights.config kullanarak:

   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Override the default shadow copy folder. -->
      <ShadowCopyFolder>D:\SnapshotUploader</ShadowCopyFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

Veya .NET Core uygulaması yla appsettings.json kullanıyorsanız:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "ShadowCopyFolder": "D:\\SnapshotUploader"
     }
   }
   ```

## <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Anlık görüntülerle özel durumları bulmak için Uygulama Öngörüleri aramasını kullanma

Anlık görüntü oluşturulduğunda, atma özel durumu anlık görüntü kimliğiyle etiketlenir. Özel durum telemetrisi Application Insights'a bildirildiğinde, bu anlık görüntü kimliği özel bir özellik olarak dahil edilir. Uygulama Öngörülerinde **Arama'yı** kullanarak, `ai.snapshot.id` özel özelliği olan tüm telemetrileri bulabilirsiniz.

1. Azure portalındaki Application Insights kaynağınıza göz atın.
2. **Ara**’ya tıklayın.
3. Arama `ai.snapshot.id` metin kutusuna yazın ve Enter tuşuna basın.

![Portalda anlık görüntü kimliğiyle telemetri arama](./media/snapshot-debugger/search-snapshot-portal.png)

Bu arama sonuç döndürmezse, seçili zaman aralığında uygulamanız için Uygulama Öngörüleri'ne anlık görüntü bildirilmemiştir.

Yükleyici günlüklerinden belirli bir anlık görüntü kimliği aramak için bu kimliği Arama kutusuna yazın. Yüklendiğini bildiğiniz bir anlık görüntü için telemetri bulamıyorsanız aşağıdaki adımları izleyin:

1. Enstrümantasyon anahtarını doğrulayarak doğru Application Insights kaynağına baktığınızı iki kez kontrol edin.

2. Yükleyici günlüğündeki zaman damgasını kullanarak, aramanın Zaman Aralığı filtresini bu zaman aralığını kapsayacak şekilde ayarlayın.

Bu anlık görüntü kimliğiyle ilgili bir özel durum hala göremiyorsanız, özel durum telemetrisi Application Insights'a bildirilmemişdemektir. Bu durum, uygulamanız anlık görüntü aldıktan sonra çöktü, ancak özel durum telemetrirapor önce bu durum olabilir. Bu durumda, beklenmeyen yeniden başlatmalar `Diagnose and solve problems` veya işlenmemiş özel durumlar olup olmadığını görmek için Altındaki Uygulama Hizmeti günlüklerini denetleyin.

## <a name="edit-network-proxy-or-firewall-rules"></a>Ağ proxy'si veya güvenlik duvarı kurallarını edin

Uygulamanız bir proxy veya güvenlik duvarı aracılığıyla Internet'e bağlanırsa, uygulamanızın Anlık Görüntü Hata Ayıklayıcı hizmetiyle iletişim kurabilmesi için kuralları düzeltmeniz gerekebilir. Anlık Görüntü Hataayıkçısı tarafından kullanılan IP'ler Azure Monitor hizmet etiketine dahildir.
