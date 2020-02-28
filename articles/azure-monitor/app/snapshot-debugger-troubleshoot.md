---
title: Azure Application Insights Snapshot Debugger sorunlarını giderme
description: Bu makalede, Application Insights Snapshot Debugger etkinleştirme veya kullanmada sorun yaşayan geliştiricilere yardımcı olacak sorun giderme adımları ve bilgiler sunulmaktadır.
ms.topic: conceptual
author: brahmnes
ms.date: 03/07/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 485f35ed249ab7f6bbb987d8c79afe20287cd25a
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77671418"
---
# <a id="troubleshooting"></a>Application Insights Snapshot Debugger etkinleştirme veya anlık görüntüleri görüntüleme sorunlarını giderme
Uygulamanız için Application Insights Snapshot Debugger etkinleştirdiyseniz, ancak özel durumlar için anlık görüntüler görmüyorsanız, bu yönergeleri kullanarak sorun giderme yapabilirsiniz. Anlık görüntülerin üretilmesinin pek çok farklı nedeni olabilir. Olası yaygın nedenlerin bazılarını belirlemek için anlık görüntü sistem durumu denetimini çalıştırabilirsiniz.

## <a name="use-the-snapshot-health-check"></a>Anlık görüntü sistem durumu denetimini kullanma
Bazı yaygın sorunlar Aç hata ayıklama gösterilmiyor anlık sonuçlanır. Bir tarihi geçmiş anlık görüntü toplayıcının, örneğin kullanma; Günlük karşıya yükleme sınırına ulaşması; veya belki de anlık görüntü yalnızca karşıya yüklemek için bir uzun sürüyor. Sık karşılaşılan sorunları gidermek için anlık görüntü sistem durumu denetimi kullanın.

Uçtan uca izleme görünümünün anlık görüntü sistem durumu denetimi alan özel durum bölmesinde bir bağlantı yoktur.

![Anlık görüntü durum denetimi girin](./media/snapshot-debugger/enter-snapshot-health-check.png)

Etkileşimli, sohbet benzeri arabirimi, sık karşılaşılan sorun için arar ve bunları düzeltmek için size yol gösterir.

![Sistem durumu denetimi](./media/snapshot-debugger/healthcheck.png)

Ardından, sorunu çözmezse, sorun giderme adımları aşağıdaki kılavuzuna başvurun.

## <a name="verify-the-instrumentation-key"></a>İzleme anahtarını doğrulayın

Yayımlanan uygulamanızı doğru izleme anahtarını kullandığınızdan emin olun. Genellikle, izleme anahtarı Applicationınsights.config dosyasından okunur. Değer portalında gördüğünüz Application Insights kaynağı için izleme anahtarı ile aynı olduğunu doğrulayın.

## <a name="preview-versions-of-net-core"></a>.NET Core 'un önizleme sürümleri
Uygulama .NET Core 'un önizleme sürümünü kullanıyorsa ve Snapshot Debugger portalda [Application Insights bölmesi](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json) aracılığıyla etkinleştirildiyse Snapshot Debugger başlatılamayabilir. [Diğer ortamlar için Snapshot Debugger etkinleştir](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) bölümündeki yönergeleri izleyerek, [Application Insights bölmesi](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)aracılığıyla etkinleştirmenin ***yanı sıra*** [Microsoft. ApplicationInsights. snapshotcollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini uygulamaya ekleyin.


## <a name="upgrade-to-the-latest-version-of-the-nuget-package"></a>NuGet paketinin en son sürüme yükseltme

Snapshot Debugger [portalda Application Insights bölmesi](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)aracılığıyla etkinleştirildiyse, uygulamanız zaten en son NuGet paketini çalıştırıyor olmalıdır. Snapshot Debugger [Microsoft. ApplicationInsights. snapshotcollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet paketini ekleyerek etkinleştirilmişse, Microsoft. ApplicationInsights. snapshotcollector 'ın en son sürümünü kullandığınızdan emin olmak Için Visual Studio 'Nun NuGet paket yöneticisini kullanın. Sürüm notları https://github.com/Microsoft/ApplicationInsights-Home/issues/167 şurada bulunabilir

## <a name="check-the-uploader-logs"></a>Yükleyici günlükleri denetleyin

Bir mini döküm dosyası (.dmp), bir anlık görüntü oluşturulduktan sonra disk üzerinde oluşturulur. Ayrı yükleyici işlemi bu mini döküm dosyası oluşturur ve bunu, Application Insights Snapshot Debugger depolama ilişkili tüm pdb birlikte yükler. Mini döküm dosyası başarıyla karşıya yüklendikten sonra bu diskinden silinir. Yükleyici işlem için günlük dosyaları diskte tutulur. App Service ortamında bu günlükleri `D:\Home\LogFiles`bulabilirsiniz. Bu günlük dosyaları bulmak için App Service Kudu yönetim sitesi kullanın.

1. App Service uygulamanızı Azure portalında açın.
2. **Gelişmiş Araçlar**' a tıklayın veya **kudu**araması yapın.
3. **Git**' e tıklayın.
4. **Hata ayıklama konsolu** aşağı açılan liste kutusunda **cmd**' yi seçin.
5. **Günlük dosyaları**' na tıklayın.

`Uploader_` veya `SnapshotUploader_` ve `.log` uzantılı bir ada sahip en az bir dosya görmeniz gerekir. Tüm günlük dosyalarını indirin veya bunları bir tarayıcıda açmak için uygun simgeye tıklayın.
App Service örneğine tanımlayan benzersiz bir son eke dosya adını içerir. App Service örneğinizin birden fazla makine üzerinde barındırılıyorsa, her makine için ayrı günlük dosyası vardır. Karşıya yükleyen yeni bir mini döküm dosyası algıladığında, bu günlük dosyasına kaydedilir. Başarılı bir anlık görüntü ve karşıya yükleme örneği aşağıda verilmiştir:

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
> Yukarıdaki örnekte Microsoft.applicationınsights.snapshotcollectya da NuGet Paketi 1.2.0 sürümüne bulunur. Önceki sürümlerde, Uploader işlemi `MinidumpUploader.exe` olarak adlandırılır ve günlük daha az ayrıntılıdır.

Önceki örnekte, izleme anahtarı `c12a605e73c44346a984e00000000000`. Bu değer, uygulamanızın izleme anahtarını eşleşmesi gerekir.
Mini döküm, KIMLIK `139e411a23934dc0b9ea08a626db16c5`bir anlık görüntü ile ilişkilendirilir. Application Insights Analytics ilişkili özel durum telemetrisi bulmak için bu kimliği daha sonra kullanabilirsiniz.

Karşıya yükleyen her 15 dakikada hakkında yeni pdb tarar. Bir örneği aşağıda verilmiştir:

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

App Service _barındırılmayan_ uygulamalar için, Uploader günlükleri mini dökümler ile aynı klasörlerdir: `%TEMP%\Dumps\<ikey>` (`<ikey>`, izleme anahtarınız).

## <a name="troubleshooting-cloud-services"></a>Bulut Hizmetleri sorunlarını giderme
Bulut hizmetlerindeki roller için varsayılan geçici klasör kayıp anlık görüntüleri için önde gelen mini döküm dosyaları tutmak için çok küçük olabilir.
İhtiyaç duyulan alanı uygulamanızı eşzamanlı anlık görüntü sayısı ve toplam çalışma kümesine bağlıdır.
Çalışma kümesi 32-bit ASP.NET web rolü, genellikle 200 MB ile en fazla 500 MB arasındadır.
En az iki eş zamanlı anlık görüntüler için izin verin.
Örneğin, uygulamanız çalışma kümesi toplam 1 GB kullanıyorsa, en az 2 anlık görüntülerini depolamak için GB disk alanı olduğundan emin olmalısınız.
Anlık görüntüler için ayrılmış bir yerel kaynak ile bulut hizmeti rolünü yapılandırmak için aşağıdaki adımları izleyin.

1. Yeni bir yerel kaynak, bulut hizmet tanımı (.csdef) dosyasını düzenleyerek bulut hizmetinize ekleyin. Aşağıdaki örnek, 5 GB boyutundaki `SnapshotStore` adlı bir kaynağı tanımlar.
   ```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
   ```

2. `SnapshotStore` yerel kaynağına işaret eden bir ortam değişkeni eklemek için rolünüzün başlangıç kodunu değiştirin. Çalışan rolleri için, kod rolün `OnStart` metoduna eklenmelidir:
   ```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
   ```
   Web rolleri için (ASP.NET), kod Web uygulamanızın `Application_Start` metoduna eklenmelidir:
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

3. `SnapshotCollector` tarafından kullanılan geçici klasör konumunu geçersiz kılmak için rolünüzün ApplicationInsights. config dosyasını güncelleştirin
   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Use the SnapshotStore local resource for snapshots -->
      <TempFolder>%SNAPSHOTSTORE%</TempFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

## <a name="overriding-the-shadow-copy-folder"></a>Gölge kopya klasörü geçersiz kılma

Anlık görüntü toplayıcının başlatıldığında anlık görüntü yükleyici işlemi çalıştırmak için uygun diskte bir klasörü bulmayı dener. Seçtiğiniz klasör gölge kopya klasör olarak bilinir.

Anlık görüntü toplayıcının, bazı iyi bilinen konumları, anlık görüntü Uploader ikili dosyalarını kopyalamak için izinlere sahip sağlamaktan denetler. Aşağıdaki ortam değişkenlerini kullanılır:
- Fabric_Folder_App_Temp
- LOCALAPPDATA
- APPDATA
- TEMP

Uygun bir klasör bulunamazsa, _"uygun bir gölge kopya klasörü bulunamadı_ " hatası bildiren bir hata raporlar Snapshot Collector.

Kopyalama başarısız olursa, `ShadowCopyFailed` bir hata raporlar Snapshot Collector.

Uploader başlatılamaz Snapshot Collector, bir `UploaderCannotStartFromShadowCopy` hatası raporlar. İletinin gövdesi genellikle `System.UnauthorizedAccessException`içerir. Bu hata genellikle uygulama sınırlı izinlere sahip bir hesap altında çalıştığı için oluşur. Hesabın gölge kopya klasörüne yazmak için yeterli izne sahip, ancak kod yürütmek için izinlere sahip değil.

Bu hatalar genellikle başlangıç sırasında gerçekleşdiğinden, genellikle _"Uploader başlatılamadı"_ hatası ortaya çıkar `ExceptionDuringConnect` bir hata oluştu.

Bu hataları geçici olarak çözmek için, `ShadowCopyFolder` yapılandırma seçeneği aracılığıyla gölge kopya klasörünü el ile belirtebilirsiniz. Örneğin, Applicationınsights.config kullanma:

   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Override the default shadow copy folder. -->
      <ShadowCopyFolder>D:\SnapshotUploader</ShadowCopyFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

Veya, bir .NET Core uygulaması ile appsettings.json kullanıyorsanız:

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

## <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Application ınsights'ı özel durumların anlık görüntülerle bulmak için arama yapın

Anlık görüntü oluşturulduğunda, özel durum oluşturmaya bir anlık görüntü kimliği ile etiketlenir. Özel durum telemetrisi Application Insights'a bildirildiğinde bu anlık görüntü kimliği bir özel özellik olarak dahil edilir. Application Insights **arama** kullanarak, tüm telemetrisini `ai.snapshot.id` özel özelliği ile bulabilirsiniz.

1. Azure portalında Application Insights kaynağınıza göz atın.
2. **Ara**' ya tıklayın.
3. Arama metin kutusuna `ai.snapshot.id` yazın ve ENTER tuşuna basın.

![Portalda bir anlık görüntü kimliği ile telemetri arayın](./media/snapshot-debugger/search-snapshot-portal.png)

Bu arama sonuç vermedi, anlık görüntü yok seçili zaman aralığındaki uygulamanız için Application ınsights'ı bildirildi.

Yükleyici günlükleri belirli bir anlık görüntüye Kimliğinden aramak için bu kimliği arama kutusuna yazın. Karşıya yüklenen bildiğiniz bir anlık görüntü için telemetri bulamazsanız, aşağıdaki adımları izleyin:

1. İzleme anahtarını doğrulayarak doğru Application Insights kaynağı arıyorsanız denetleyin.

2. Yükleyici günlük itibaren zaman damgası kullanarak, o zaman aralığınızı kapsayacak şekilde arama zaman aralığı filtresini ayarlayın.

Bu anlık görüntü Kimliğe sahip bir özel durum yine de görmüyorsanız, özel durum telemetrisi Application ınsights'ı bildirilen değildi. Bu durum, anlık görüntü sürdüğünü sonra uygulamanızı kilitlendi, ancak özel durum telemetrisi bildirilen önce gerçekleşebilir. Bu durumda, beklenmeyen yeniden başlatmalar veya işlenmemiş özel durumlar olup olmadığını görmek için `Diagnose and solve problems` altındaki App Service günlüklerini denetleyin.

## <a name="edit-network-proxy-or-firewall-rules"></a>Ağ proxy veya güvenlik duvarı kurallarını Düzenle

Uygulamanız İnternet'e bir ara sunucu veya bir güvenlik duvarı üzerinden bağlanıyorsa, Snapshot Debugger hizmetiyle iletişim kurmak için uygulamanıza izin vermek için kuralları düzenlemek gerekebilir. Snapshot Debugger tarafından kullanılan IP 'Ler, Azure Izleyici hizmeti etiketine dahildir.
