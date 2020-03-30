---
title: Sorun Giderme Azure Tanılama uzantısı
description: Azure Sanal Makineler, Hizmet Kumaşı veya Bulut Hizmetleri'nde Azure tanılamalarını kullanırken sorunları giderin.
ms.subservice: diagnostic-extension
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/08/2019
ms.openlocfilehash: 043369bd6112c4cac36539bbd764393d889439c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274586"
---
# <a name="azure-diagnostics-troubleshooting"></a>Azure Tanılama sorunlarını giderme
Bu makalede, Azure Tanılama'yı kullanmakla ilgili sorun giderme bilgileri açıklanmaktadır. Azure tanılama hakkında daha fazla bilgi için Azure [Tanılama genel bakış](diagnostics-extension-overview.md)bakın.

## <a name="logical-components"></a>Mantıksal bileşenler
**Tanılama Eklentisi Başlatıcısı (DiagnosticsPluginLauncher.exe)**: Azure Tanılama uzantısını başlatır. Giriş noktası süreci olarak hizmet vermektedir.

**Diagnostics Plugin (DiagnosticsPlugin.exe)**: İzleme aracısının ömrünü yapılandırır, başlatır ve yönetir. Bu başlatıcısı tarafından başlatılan ana süreçtir.

**İzleme Aracısı\*(MonAgent .exe süreçleri)**: Tanılama verilerini izler, toplar ve aktarın.  

## <a name="logartifact-paths"></a>Günlük/yapı yolları
Aşağıda bazı önemli günlükleri ve eserler için yollar vardır. Bu bilgilere belgenin geri kalanında atıfta bulunuruz.

### <a name="azure-cloud-services"></a>Azure Cloud Services
| Yapıt | Yol |
| --- | --- |
| **Azure Tanılama yapılandırma dosyası** | %SystemDrive%\Packages\Eklentiler\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<sürümü>\Config.txt |
| **Günlük dosyaları** | C:\Logs\Eklentiler\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<sürümü>\ |
| **Tanılama verileri için yerel depo** | C:\Kaynaklar\Dizin\<CloudServiceDeploymentID>. \<RoleName>. DiagnosticStore\WAD0107\Tablolar |
| **Aracı yapılandırma dosyalarını izleme** | C:\Kaynaklar\Dizin\<CloudServiceDeploymentID>. \<RoleName>. DiagnosticStore\WAD0107\Yapılandırma\MaConfig.xml |
| **Azure Tanılama uzantı paketi** | %SystemDrive%\Packages\Eklentiler\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<sürümü> |
| **Günlük toplama yardımcı yolu** | %SystemDrive%\Paketler\GuestAgent\ |
| **MonAgentHost günlük dosyası** | C:\Kaynaklar\Dizin\<CloudServiceDeploymentID>. \<RoleName>. DiagnosticStore\WAD0107\Configuration\MonAgentHost.<seq_num>.log |

### <a name="virtual-machines"></a>Sanal makineler
| Yapıt | Yol |
| --- | --- |
| **Azure Tanılama yapılandırma dosyası** | C:\Packages\Eklentiler\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<sürüm>\RuntimeSettings |
| **Günlük dosyaları** | C:\WindowsAzure\Günlükler\Eklentiler\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\ |
| **Tanılama verileri için yerel depo** | C:\WindowsAzure\Günlükler\Eklentiler\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Tablolar |
| **Aracı yapılandırma dosyalarını izleme** | C:\WindowsAzure\Günlükler\Eklentiler\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Configuration\MaConfig.xml |
| **Durum dosyası** | C:\Packages\Eklentiler\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<sürümü>\Durum |
| **Azure Tanılama uzantı paketi** | C:\Packages\Eklentiler\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>|
| **Günlük toplama yardımcı yolu** | C:\WindowsAzure\Günlükler\WaAppAgent.log |
| **MonAgentHost günlük dosyası** | C:\WindowsAzure\Günlükler\Eklentiler\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Configuration\MonAgentHost.<seq_num>.log |

## <a name="metric-data-doesnt-appear-in-the-azure-portal"></a>Azure portalında metrik veriler görünmüyor
Azure Tanılama, Azure portalında görüntülenebilen metrik veriler sağlar. Portaldaki verileri görmekte sorun yaşıyorsanız, ilgili\* metrik kayıtların orada olup olmadığını görmek ve [kaynak sağlayıcısı](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services) Microsoft.Insights'ın kayıtlı olduğundan emin olmak için Azure Tanılama depolama hesabındaki WADMetrics tablosunu kontrol edin.

Burada, **tablonun PartitionKey** kaynak kimliği, sanal makine veya sanal makine ölçek kümesidir. **RowKey** metrik adıdır (performans sayacı adı olarak da bilinir).

Kaynak kimliği yanlışsa, kaynak kimliğinin doğru ayarlıp ayarlolmadığını görmek için **Tanılama** **Yapılandırma** > **Ölçümleri** > **Kaynak Kimliği'ni** denetleyin.

Belirli bir ölçüm için veri yoksa, metrik (performans sayacı) dahil olup olmadığını görmek için **Diagnostics Configuration** > **PerformanceCounter'ı** kontrol edin. Varsayılan olarak aşağıdaki sayaçları etkinleştiriz:
- \Processor(_Total)\% Processor Time
- \Memory\Available Bytes
- \ASP.NET Uygulamaları(__Toplam__)\İstek/Sn
- \ASP.NET Uygulamaları(__Toplam__)\Hatalar Toplam/Sn
- \ASP.NET\İstekler Sıraya
- \ASP.NET\İstekler Reddedildi
- \İşlemci(w3wp)\% İşlemci Süresi
- \İşlem(w3wp)\Özel Baytlar
- \Process(WaiISHost)\% İşlemci Zamanı
- \Process(WaIISHost)\Özel Baytlar
- \Process(WaworkerHost)\% İşlemci Zamanı
- \Process(WaWorkerHost)\Özel Baytlar
- \Bellek\Sayfa Hataları/sn
- \.NET CLR Bellek\% _(Global)_ GC'de Zaman
- \LogicalDisk(C:)\Disk Bayt/sn Yazma
- \LogicalDisk(C:)\Disk Okuma Bayt/sn
- \LogicalDisk(D:)\Disk Bayt/sn Yazma
- \LogicalDisk(D:)\Disk Okuma Bayt/sn

Yapılandırma doğru ayarlanmışsa ancak metrik verileri hala göremiyorsanız, sorun gidermenize yardımcı olmak için aşağıdaki yönergeleri kullanın.


## <a name="azure-diagnostics-is-not-starting"></a>Azure Tanılama başlatmıyor
Azure Diagnostik'in neden başlatılamadığı hakkında bilgi için, daha önce sağlanan günlük dosyaları konumundaki **DiagnosticsPluginLauncher.log** ve **DiagnosticsPlugin.log** dosyalarına bakın.

Bu günlükleri `Monitoring Agent not reporting success after launch`gösteriyorsa, monAgentHost.exe başlatılması bir hata olduğu anlamına gelir. Önceki bölümde belirtilen `MonAgentHost log file` konumdaki günlüklere bakın.

Günlük dosyalarının son satırı çıkış kodunu içerir.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
**Negatif** çıkış kodu bulursanız, [Başvurular bölümündeki](#references) [çıkış kodu tablosuna](#azure-diagnostics-plugin-exit-codes) bakın.

## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Tanılama verileri Azure Depolama'ya günlüğe kaydedilmedi
Verilerin hiçbirinin görüntüleninip görünmeyip görünmeyin veya bazı verilerin görüntüleninip görünmeyini belirleyin.

### <a name="diagnostics-infrastructure-logs"></a>Tanılama altyapı günlükleri
Tanılama, Tanılama altyapı günlüklerinde tüm hataları kaydeder. [Yapılandırmanızda Tanılama altyapı günlüklerinin yakalanmasını etkinleştirdiğinizden](#how-to-check-diagnostics-extension-configuration)emin olun. Ardından, yapılandırılan depolama hesabınızdaki `DiagnosticInfrastructureLogsTable` tabloda görünen alakalı hataları hızlı bir şekilde alabilirsiniz.

### <a name="no-data-is-appearing"></a>Veri görünmüyor
Olay verilerinin hiç görünmemesinin en yaygın nedeni, depolama hesabı bilgilerinin yanlış tanımlanmış olmasıdır.

Çözüm: Tanılama yapılandırmanızı düzeltin ve Tanılama'yı yeniden yükleyin.

Depolama hesabı doğru şekilde yapılandırılırsa, makineye uzaktan erişim ve *DiagnosticsPlugin.exe* ve *MonAgentCore.exe'nin* çalıştığını doğrulayın. Çalışmıyorlarsa, Azure Tanılama'daki adımları izleyin [başlatılamıyor.](#azure-diagnostics-is-not-starting)

İşlemler çalışıyorsa, [veriler yerel olarak mı yakalanır?](#is-data-getting-captured-locally)

Bu, sorunu çözmezse, aşağıdakileri deneyin:

1. Aracıyı kaldırın
2. C:\WindowsAzure\Günlükleri\Eklentileri\Microsoft.Azure.Diagnostics.IaaSDiagnostics dizini kaldırma
3. Aracıyı yeniden yükle


### <a name="part-of-the-data-is-missing"></a>Verilerin bir kısmı eksik
Bazı veriler alıyorsanız ancak tümü değil, veri toplama/aktarım ardışık alanı doğru ayarlanmış demektir. Sorunu daraltmak için buradaki alt bölümlere uyun.

#### <a name="is-the-collection-configured"></a>Koleksiyon yapılandırıldı mı?
Tanılama yapılandırması, toplanacak belirli bir veri türü için yönergeler içerir. Yalnızca koleksiyon için yapılandırdığınız verileri aradığınızı doğrulamak için [yapılandırmanızı gözden geçirin.](#how-to-check-diagnostics-extension-configuration)

#### <a name="is-the-host-generating-data"></a>Ana bilgisayar veri üretiyor mu?
- **Performans sayaçları**: Perfmon'u açın ve sayacı kontrol edin.

- **İzleme günlükleri**: VM'ye uzaktan erişim ve uygulamanın config dosyasına bir TextWriterTraceListener ekleyin.  Metin https://msdn.microsoft.com/library/sk36c28t.aspx dinleyicisini ayarlamaya bakın.  Öğenin `<trace>` `<trace autoflush="true">`.<br />
Oluşturulan izleme günlüklerini görmüyorsanız, eksik olan izleme günlükleri hakkında daha fazla bkz.

- **ETW izleri**: VM'ye uzaktan erişim ve PerfView'i yükleyin.  PerfView,Dosya **File** > **Kullanıcı Komutu** > **listen etwprovder1** > **etwprovider2**çalıştırın ve benzeri. **Listen** komutu büyük/küçük harf duyarlıdır ve virgülle ayrılmış ETW sağlayıcıları listesi arasında boşluk olamaz. Komut çalışmazsa, perfview aracının sağ alt kısmındaki **Günlük** düğmesini seçerek hangi çalıştırma girişiminde bulunduğunu ve sonucun ne olduğunu görebilirsiniz.  Girişin doğru olduğunu varsayarsak, yeni bir pencere açılır. Birkaç saniye içinde, ETW izlerini görmeye başlarsınız.

- **Olay günlükleri**: VM'ye uzaktan erişim. Açın `Event Viewer`ve olayların var olduğundan emin olun.

#### <a name="is-data-getting-captured-locally"></a>Veriler yerel olarak mı yakalanıyor?
Ardından, verilerin yerel olarak yakalandığından emin olun.
Veriler, tanılama verileri `*.tsf` için yerel depodaki dosyalarda yerel olarak depolanır. Farklı günlük türleri farklı `.tsf` dosyalarda toplanır. Adlar Azure Depolama'daki tablo adlarına benzer.

Örneğin, `Performance Counters` 'de `PerformanceCountersTable.tsf`toplanır. Olay günlükleri `WindowsEventLogsTable.tsf`toplanır. Yerel koleksiyon dosyalarını açmak ve diskte toplandığını gördüğünüzü doğrulamak için [Yerel günlük çıkarma](#local-log-extraction) bölümündeki yönergeleri kullanın.

Günlüklerin yerel olarak toplandığını görmüyorsanız ve ana bilgisayar veri sağlandığının veri üründe olduğundan önümüzdeki Yapılandırmanızı dikkatle gözden geçirin.

Ayrıca MonitoringAgent MaConfig.xml için oluşturulan yapılandırmayı gözden geçirin. İlgili günlük kaynağını açıklayan bir bölüm olduğunu doğrulayın. Ardından, Tanılama yapılandırması ile izleme aracısı yapılandırması arasındaki çeviride kaybolmadığını doğrulayın.

#### <a name="is-data-getting-transferred"></a>Veriler aktarılıyor mu?
Verilerin yerel olarak yakalandığını doğruladıysanız ancak depolama hesabınızda hala göremiyorsanız, aşağıdaki adımları izleyin:

- Doğru bir depolama hesabı sağladığınızı ve verilen depolama hesabının anahtarlarını devirmediğinizi doğrulayın. Azure Bulut Hizmetleri için bazen insanların güncellemediğini `useDevelopmentStorage=true`görürsünüz.

- Sağlanan depolama hesabının doğru olduğundan doğrulayın. Bileşenlerin genel depolama bitiş noktalarına ulaşmasını engelleyen ağ kısıtlamaları olmadığından emin olun. Bunu yapmanın bir yolu makineye uzaktan erişim ve sonra aynı depolama hesabına kendiniz bir şeyler yazmaya çalışın.

- Son olarak, izleme aracısı tarafından hangi hataların raporlandığına bakabilirsiniz. İzleme aracısı, tanılama `maeventtable.tsf`verileri için yerel mağazada bulunan oturumlarını yazar. Bu dosyayı açmak için [Yerel günlük çıkarma](#local-log-extraction) bölümündeki yönergeleri izleyin. Ardından, depolamaya yazan `errors` yerel dosyalara okunan hataları gösteren hatalar olup olmadığını belirlemeye çalışın.

### <a name="capturing-and-archiving-logs"></a>Günlükleri yakalama ve arşivleme
Desteğe başvurmayı düşünüyorsanız, sizden sorabilecekleri ilk şey makinenizden günlükleri toplamaktır. Bunu kendin yaparak zamandan tasarruf edebilirsin. Log `CollectGuestLogs.exe` toplama yardımcı yolda yardımcı programı çalıştırın. Aynı klasördeki tüm ilgili Azure günlükleri içeren bir .zip dosyası oluşturur.

## <a name="diagnostics-data-tables-not-found"></a>Tanılama veri tabloları bulunamadı
Azure depolama alanında ETW olaylarını barındıran tablolar aşağıdaki kod kullanılarak adlandırılır:

```csharp
        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;
```

Örnek aşağıda verilmiştir:

```XML
        <EtwEventSourceProviderConfiguration provider="prov1">
          <Event id="1" />
          <Event id="2" eventDestination="dest1" />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider="prov2">
          <DefaultEvents eventDestination="dest2" />
        </EtwEventSourceProviderConfiguration>
```
```JSON
"EtwEventSourceProviderConfiguration": [
    {
        "provider": "prov1",
        "Event": [
            {
                "id": 1
            },
            {
                "id": 2,
                "eventDestination": "dest1"
            }
        ],
        "DefaultEvents": {
            "eventDestination": "DefaultEventDestination",
            "sinks": ""
        }
    },
    {
        "provider": "prov2",
        "DefaultEvents": {
            "eventDestination": "dest2"
        }
    }
]
```
Bu kod dört tablo oluşturur:

| Olay | Tablo adı |
| --- | --- |
| sağlayıcı="prov1" &lt;Olay id="1" /&gt; |WADEvent+MD5("prov1")+"1" |
| provider="prov1" &lt;Olay id="2" eventDestination="dest1" /&gt; |WADdest1 |
| sağlayıcı="prov1" &lt;DefaultEvents /&gt; |WADDefault+MD5("prov1") |
| sağlayıcı="prov2" &lt;DefaultEvents olayHedef="dest2" /&gt; |WADdest2 |

## <a name="references"></a>Başvurular

### <a name="how-to-check-diagnostics-extension-configuration"></a>Tanılama uzantısı yapılandırması nasıl denetler?
Uzantı yapılandırmanızı denetlemenin en kolay yolu [Azure Kaynak Gezgini'ne](https://resources.azure.com)gitmek ve ardından Azure Diagnostics uzantısının (IaaSDiagnostics / PaaDiagnostics) bulunduğu sanal makineye veya bulut hizmetine gitmektir.

Alternatif olarak, makinenin içine uzak masaüstü ve Günlük yapıları yol bölümünde açıklanan Azure Tanılama Yapılandırma dosyasına bakın.

Her iki durumda da **Microsoft.Azure.Diagnostics**ve **ardından xmlCfg** veya **WadCfg** alanını arayın.

Sanal bir makinede arama yapıyorsanız ve **WadCfg** alanı mevcutsa, bu config'in JSON formatında olduğu anlamına gelir. **XmlCfg** alanı varsa, bu config XML ve base64-kodlanmış olduğu anlamına gelir. Diagnostics tarafından yüklenen XML'yi görmek için [şifresini çözmeniz](https://www.bing.com/search?q=base64+decoder) gerekir.

Bulut hizmeti rolü için, yapılandırmayı diskten seçerseniz, veriler base64 kodludur, bu nedenle Tanılama tarafından yüklenen XML'yi görmek için bu kodu [çözmeniz](https://www.bing.com/search?q=base64+decoder) gerekir.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Azure Diagnostics eklenti çıkış kodları
Eklenti aşağıdaki çıkış kodlarını döndürür:

| Çıkış kodu | Açıklama |
| --- | --- |
| 0 |Başarılı. |
| -1 |Genel hata. |
| -2 |rcf dosyasını yükleyemiyor.<p>Bu iç hata yalnızca konuk ajan eklenti başlatıcısı VM'de el ile yanlış çağrılması durumunda gerçekleşmelidir. |
| -3 |Tanılama yapılandırma dosyasını yükleyemiyor.<p><p>Çözüm: Şema doğrulamayı geçmeyen bir yapılandırma dosyasından kaynaklanır. Çözüm şemaya uygun bir yapılandırma dosyası sağlamaktır. |
| -4 |İzleme aracısı Diagnostics'in başka bir örneği de zaten yerel kaynak dizinini kullanıyor.<p><p>Çözüm: **LocalResourceDirectory**için farklı bir değer belirtin. |
| -6 |Konuk ajan eklenti başlatıcısı geçersiz bir komut satırı ile Tanılama başlatmaya çalıştı.<p><p>Bu iç hata yalnızca konuk ajan eklenti başlatıcısı VM'de el ile yanlış çağrılması durumunda gerçekleşmelidir. |
| -10 |Tanılama eklentisi işlenmemiş bir özel durumla çıktı. |
| -11 |Konuk ajan, izleme aracısını başlatmak ve izlemekten sorumlu işlemi oluşturamadı.<p><p>Çözüm: Yeni işlemler başlatmak için yeterli sistem kaynağının kullanılabildiğini doğrulayın.<p> |
| -101 |Tanılama eklentisini ararken geçersiz bağımsız değişkenler.<p><p>Bu iç hata yalnızca konuk ajan eklenti başlatıcısı VM'de el ile yanlış çağrılması durumunda gerçekleşmelidir. |
| -102 |Eklenti işlemi kendini başlatılması mümkün değildir.<p><p>Çözüm: Yeni işlemler başlatmak için yeterli sistem kaynağının kullanılabildiğini doğrulayın. |
| -103 |Eklenti işlemi kendini başlatılması mümkün değildir. Özellikle, logger nesnesi oluşturmak mümkün değildir.<p><p>Çözüm: Yeni işlemler başlatmak için yeterli sistem kaynağının kullanılabildiğini doğrulayın. |
| -104 |Konuk temsilci tarafından sağlanan rcf dosyasını yükleyemiyor.<p><p>Bu iç hata yalnızca konuk ajan eklenti başlatıcısı VM'de el ile yanlış çağrılması durumunda gerçekleşmelidir. |
| -105 |Tanılama eklentisi Tanılama yapılandırma dosyasını açamaz.<p><p>Bu iç hata yalnızca Tanılama eklentisi VM'de el ile yanlış çağrıldığında gerçekleşmelidir. |
| -106 |Tanılama yapılandırma dosyasını okuyamıyorum.<p><p>Şema doğrulaması geçmeyen bir yapılandırma dosyası neden olur. <br><br>Çözüm: Şema ile uyumlu bir yapılandırma dosyası sağlayın. Daha fazla bilgi için [Tanılama Uzantısı Yapılandırmasını nasıl denetleyeceklerine](#how-to-check-diagnostics-extension-configuration)bakın. |
| -107 |İzleme aracısına kaynak dizin geçişi geçersizdir.<p><p>Bu iç hata yalnızca izleme aracısı VM'de el ile yanlış çağrıldıysa meydana gelmelidir.</p> |
| -108 |Diagnostics yapılandırma dosyasını izleme aracısı yapılandırma dosyasına dönüştüremiyor.<p><p>Bu iç hata yalnızca Tanılama eklentisi geçersiz bir yapılandırma dosyasıyla el ile çağrıldığında gerçekleşmelidir. |
| -110 |Genel Tanılama yapılandırma hatası.<p><p>Bu iç hata yalnızca Tanılama eklentisi geçersiz bir yapılandırma dosyasıyla el ile çağrıldığında gerçekleşmelidir. |
| -111 |İzleme aracısını çalıştıramıyor.<p><p>Çözüm: Yeterli sistem kaynağının mevcut olduğunu doğrulayın. |
| -112 |Genel hata |

### <a name="local-log-extraction"></a>Yerel günlük çıkarma
İzleme aracısı günlükleri ve yapıları `.tsf` dosya olarak toplar. Dosya `.tsf` okunamıyor, ancak aşağıdaki `.csv` gibi bir dönüştürebilirsiniz:

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
Çağrılan `<relevantLogFile>.csv` yeni bir dosya, ilgili `.tsf` dosyayla aynı yolda oluşturulur.

>[!NOTE]
> Bu yardımcı programı yalnızca ana .tsf dosyasına (örneğin PerformanceCountersTable.tsf) karşı çalıştırmanız gerekir. Eşlik eden dosyalar (örneğin,\*\*001.tsf\*\*PerformanceCountersTables_, PerformanceCountersTables_ 002.tsf vb.) otomatik olarak işlenir.

### <a name="more-about-missing-trace-logs"></a>Eksik izleme günlükleri hakkında daha fazla şey

>[!NOTE]
> IaaS VM'nizde çalışan bir uygulamada DiagnosticsMonitorTraceListener'ı yapılandırmadığınız sürece aşağıdaki bilgiler çoğunlukla Azure Bulut Hizmetleri için geçerlidir.

- **DiagnosticMonitorTraceListener** web.config veya app.config yapılandırıldı emin olun.  Bu, bulut hizmeti projelerinde varsayılan olarak yapılandırılır. Ancak, bazı müşteriler bu yorumu, izleme ifadelerinin tanılama tarafından toplanmamasını neden olur.

- Günlükler **OnStart** veya **Çalıştır** yönteminden yazMıyorsa, **DiagnosticMonitorTraceListener'ın** app.config'de olduğundan emin olun.  Varsayılan olarak web.config, ancak bu sadece w3wp.exe içinde çalışan kod için geçerlidir. Yani WaIISHost.exe çalışan izleri yakalamak için app.config gerekir.

- **Diagnostics.Debug.WriteXXX** yerine **Diagnostics.Trace.TraceXXX** kullandığınızdan emin olun. Hata Ayıklama deyimleri bir sürüm yapısından kaldırılır.

- Derlenen kodun **aslında Diagnostics.Trace satırlarına** sahip olduğundan emin olun (doğrulamak için Reflektör, ildasm veya ILSpy kullanın). Trace koşullu derleme simgesini kullanmadığınız sürece **Diagnostics.Trace** komutları derlenmiş ikiliden kaldırılır. Bu, bir proje oluşturmak için msbuild kullanırken oluşan yaygın bir sorundur.   

## <a name="known-issues-and-mitigations"></a>Bilinen sorunlar ve azaltıcı etkenler
Bilinen azaltıcı etkenlerle ilgili bilinen sorunların bir listesi aşağıda veda edilebilmiş tir:

**1. .NET 4.5 bağımlılık**

Windows Azure TanıLama Uzantısı,.NET 4.5 çerçevesiveya daha sonra bir çalışma zamanı bağımlılığı vardır. Yazım sırasında, Azure Bulut Hizmetleri için sağlanan tüm makinelerin yanı sıra Azure sanal makinelerine dayanan tüm resmi resimlerde .NET 4.5 veya daha sonra yüklenir.

Windows Azure Tanılama Uzantısı'nı .NET 4.5 veya daha sonra sınayan bir makinede çalıştırmayı denediğiniz bir durumla karşılaşmak yine de mümkündür. Bu, makinenizi eski bir görüntüveya anlık görüntüden oluşturduğunuzda veya kendi özel diskinizi getirdiğinizde gerçekleşir.

Bu genellikle **DiagnosticsPluginLauncher.exe** çalıştırırken bir çıkış kodu **255** olarak tezahür eder. Hata, işlenmemiş özel durum nedeniyle olur:
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Azaltma:** .NET 4.5 veya daha sonra makinenize yükleyin.

**2. Performans sayaçları verileri depolama da mevcuttur, ancak portalda gösterilmez**

Sanal makinelerdeki portal deneyimi varsayılan olarak belirli performans sayaçlarını gösterir. Performans sayaçlarını görmüyorsanız ve depolama alanında kullanılabilir olduğu için verilerin oluşturulduğunu biliyorsanız, aşağıdakileri denetleyin:

- Depolamadaki verilerin İngilizce'de sayaç adları olup olmadığı. Sayaç adları İngilizce değilse, portal metrik grafiği onu tanıyamaz. **Azaltma**: Sistem hesapları için makinenin dilini İngilizce olarak değiştirin. Bunu yapmak için **Denetim Masası** > Bölge**Yönetim Kopya** > **Ayarları'nı****Region** > seçin. Ardından, özel dilin sistem hesabına uygulanmaması için **HoşGeldiniz ekranını ve sistem hesaplarını** seçin.

- Performans sayacı adlarınızda\*joker karakterler kullanıyorsanız, performans sayaçları Azure Depolama lavabosuna gönderildiğinde portal yapılandırılan ve toplanan sayacı ilişkilendiremez. **Azaltma**: Joker karakterleri kullanabileceğinizden ve portalın\*performans sayaçlarınızı Azure Monitör ütesine yönlendirebileceğinden emin olmak için.

