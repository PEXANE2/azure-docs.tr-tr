---
title: Azure Dosya Eşitlemeyi İzle | Microsoft Dokümanlar
description: Azure Dosya Eşitlemeyi nasıl izlenir?
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: ac09f9b59bc6f47adc9311cc910352c1a0d73b5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68699284"
---
# <a name="monitor-azure-file-sync"></a>Azure Dosya Eşitleme’yi izleme

Kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda merkezileştirmek ve şirket içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu korumak için Azure Dosya Eşitlemeyi'ni kullanın. Azure Dosya Eşitleme, Windows Server’ı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, NFS ve FTPS dahil olmak üzere verilerinize yerel olarak erişmek için Windows Server'da kullanılabilen tüm protokolleri kullanabilirsiniz. Dünya çapında ihtiyacınız olduğu kadar önbelleke sahip olabilirsiniz.

Bu makalede, Azure Monitörü, Depolama Eşitleme Hizmeti ve Windows Server'ı kullanarak Azure Dosya Eşitleme dağıtımınızı nasıl izleyeceğiniz açıklanmaktadır.

Aşağıdaki izleme seçenekleri şu anda kullanılabilir.

## <a name="azure-monitor"></a>Azure İzleyici

Ölçümleri görüntülemek ve eşitleme, bulut katmanlama ve sunucu bağlantısı için uyarıları yapılandırmak için [Azure Monitor'u](https://docs.microsoft.com/azure/azure-monitor/overview) kullanın.  

### <a name="metrics"></a>Ölçümler

Azure Dosya Eşitleme ölçümleri varsayılan olarak etkinleştirilir ve her 15 dakikada bir Azure Monitor'a gönderilir.

Azure Dosya Eşitleme ölçümlerini Azure Monitor'da görüntülemek için **Depolama Eşitleme Hizmetleri** kaynak türünü seçin.

Azure Dosya Eşitlemi için aşağıdaki ölçümler Azure Monitor'da kullanılabilir:

| Ölçüm adı | Açıklama |
|-|-|
| Bayt senkronize | Aktarılan verilerin boyutu (yükleme ve indirme).<br><br>Birim: Bayt<br>Toplama Türü: Toplam<br>Uygulanabilir boyutlar: Sunucu Bitiş Noktası Adı, Eşitleme Yönü, Eşitgrup Adı |
| Bulut katmanlama geri çağırma | Geri çağrılan verilerin boyutu.<br><br>**Not**: Bu metrik gelecekte kaldırılacaktır. Geri çağrılan verilerin boyutunu izlemek için Bulut katmanlama boyutu ölçümünü kullanın.<br><br>Birim: Bayt<br>Toplama Türü: Toplam<br>Geçerli boyut: Sunucu Adı |
| Bulut katmanlama geri çağırma boyutu | Geri çağrılan verilerin boyutu.<br><br>Birim: Bayt<br>Toplama Türü: Toplam<br>Geçerli boyut: Sunucu Adı, Eşitleme Grubu Adı |
| Uygulamaya göre bulut katmanlama geri çağırma boyutu | Uygulama tarafından geri çağrılan verilerin boyutu.<br><br>Birim: Bayt<br>Toplama Türü: Toplam<br>Geçerli boyut: Uygulama Adı, Sunucu Adı, Eşitgrup Adı |
| Bulut katmanlama geri çağırma iş | Veri hatırlama verisinin boyutu.<br><br>Birim: Bayt<br>Toplama Türü: Toplam<br>Geçerli boyut: Sunucu Adı, Eşitleme Grubu Adı |
| Eşitleme olmayan dosyalar | Eşitleme yapmayan dosya sayısı.<br><br>Birim: Sayma<br>Toplama Türü: Toplam<br>Uygulanabilir boyutlar: Sunucu Bitiş Noktası Adı, Eşitleme Yönü, Eşitgrup Adı |
| Eşitlenen dosyalar | Aktarılan dosya sayısı (yükleme ve indirme).<br><br>Birim: Sayma<br>Toplama Türü: Toplam<br>Uygulanabilir boyutlar: Sunucu Bitiş Noktası Adı, Eşitleme Yönü, Eşitgrup Adı |
| Sunucu çevrimiçi durumu | Sunucudan alınan sinyal sayısı.<br><br>Birim: Sayma<br>Toplama Türü: Maksimum<br>Geçerli boyut: Sunucu Adı |
| Oturum sonucunu eşitleme | Eşitleme oturumu sonucu (1=başarılı eşitleme oturumu; 0=başarısız eşitleme oturumu)<br><br>Birim: Sayma<br>Toplama Türleri: Maksimum<br>Uygulanabilir boyutlar: Sunucu Bitiş Noktası Adı, Eşitleme Yönü, Eşitgrup Adı |

### <a name="alerts"></a>Uyarılar

Azure Monitor'da uyarıları yapılandırmak için Depolama Eşitleme Hizmeti'ni seçin ve ardından uyarı için kullanılacak [Azure Dosya Eşitleme metrikünü](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#metrics) seçin.  

Aşağıdaki tabloda izlenecek bazı örnek senaryolar ve uyarı için kullanılacak uygun metrik listelenir:

| Senaryo | Uyarı için kullanılacak metrik |
|-|-|
| Portalda sunucu uç noktası durumu = Hata | Oturum sonucunu eşitleme |
| Dosyalar bir sunucu veya bulut bitiş noktasıyla eşitleme de başarısız oluyor | Eşitleme olmayan dosyalar |
| Kayıtlı sunucu Depolama Eşitleme Hizmeti ile iletişim kuramaz | Sunucu çevrimiçi durumu |
| Bulut katmanlama geri çağırma boyutu bir günde 500GiB'i aştı  | Bulut katmanlama geri çağırma boyutu |

Azure Monitor'da uyarıları yapılandırma hakkında daha fazla bilgi edinmek [için Microsoft Azure'daki uyarılara Genel Bakış]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)bölümüne bakın.

## <a name="storage-sync-service"></a>Depolama Eşitleme Hizmeti

Kayıtlı sunucu durumunu, sunucu bitiş noktası durumunu ve ölçümleri görüntülemek için Azure portalındaki Depolama Eşitleme Hizmeti'ne gidin. **Kayıtlı sunucular** blade ve sunucu bitiş noktası durumu Sync **grupları** bıçak kayıtlı sunucu durumu görüntüleyebilirsiniz.

### <a name="registered-server-health"></a>Kayıtlı sunucu durumu

- Kayıtlı **sunucu** durumu **Çevrimiçiise,** sunucu hizmetle başarıyla iletişim kurar.
- Kayıtlı **sunucu** durumu **Çevrimdışı Görünüyorsa,** sunucudaki Depolama Eşitleme İzleyicisi (AzureStorageSyncMonitor.exe) işleminin çalıştığını doğrulayın. Sunucu bir güvenlik duvarı nın veya proxy'nin arkasındaysa, güvenlik duvarını ve proxy'yi yapılandırmak için [bu makaleye](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy) bakın.

### <a name="server-endpoint-health"></a>Sunucu bitiş noktası durumu

- Portaldaki sunucu bitiş noktası durumu, sunucudaki Telemetri olay günlüğünde günlüğe kaydedilen eşitleme olaylarını temel almaktadır (ID 9102 ve 9302). Hata iptal edilmesi gibi geçici bir hata nedeniyle eşitleme oturumu başarısız olursa, geçerli eşitleme oturumu ilerleme kaydettiği sürece eşitleme portalda sağlıklı görünmeye devam edebilir. Olay Kimliği 9302, dosyaların uygulanıp uygulanmamasını belirlemek için kullanılır. Daha fazla bilgi için, [eşitleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) ve [eşitleme ilerleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session)bakın.
- Eşitleme ilerleme kaydetmediği için portal eşitleme hatası gösteriyorsa, kılavuz için [sorun giderme belgelerine](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) bakın.

### <a name="metric-charts"></a>Metrik grafikler

- Aşağıdaki metrik grafikler Depolama Eşitleme Hizmeti portalında görüntülenebilir:

  | Ölçüm adı | Açıklama | Bıçak adı |
  |-|-|-|
  | Bayt senkronize | Aktarılan verilerin boyutu (yükleme ve indirme) | Eşitleme grubu, Sunucu bitiş noktası |
  | Bulut katmanlama geri çağırma | Geri çağrılan verilerin boyutu | Kayıtlı sunucular |
  | Eşitleme olmayan dosyalar | Eşitleme yapmayan dosya sayısı | Sunucu bitiş noktası |
  | Eşitlenen dosyalar | Aktarılan dosya sayısı (yükleme ve indirme) | Eşitleme grubu, Sunucu bitiş noktası |
  | Sunucu çevrimiçi durumu | Sunucudan alınan sinyal sayısı | Kayıtlı sunucular |

- Daha fazla bilgi için [Azure Monitor'a](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor)bakın.

  > [!Note]  
  > Depolama Eşitleme Hizmeti portalındaki grafiklerin zaman aralığı 24 saatdir. Farklı zaman aralıklarını veya boyutlarını görüntülemek için Azure Monitor'u kullanın.

## <a name="windows-server"></a>Windows Server

Windows Server'da bulut katmanlama, kayıtlı sunucu ve eşitleme durumunu görüntüleyebilirsiniz.

### <a name="event-logs"></a>Olay günlükleri

Kayıtlı sunucu, eşitleme ve bulut katmanlama durumunu izlemek için sunucudaki Telemetri olay günlüğünü kullanın. Telemetri olay *günlüğü, Uygulama ve Hizmetler\Microsoft\FileSync\Agent*altında Olay Görüntüleyici'de bulunur.

Eşitleme durumu:

- Olay Kimliği 9102 eşitleme oturumu bittikten sonra günlüğe kaydedilir. Eşitleme oturumlarının başarılı olup olmadığını **(HResult = 0**) ve madde başına eşitleme hataları olup olmadığını belirlemek için bu olayı kullanın. Daha fazla bilgi için [eşitleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) ve [madde başına hata](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing) belgelerine bakın.

  > [!Note]  
  > Bazen eşitleme oturumları genel olarak başarısız olur veya sıfır olmayan bir PerItemErrorCount'a sahiptir. Ancak, yine de ilerleme kaydeder ve bazı dosyalar başarıyla eşitler. Bunu AppliedFileCount, AppliedDirCount, AppliedTombstoneCount ve AppliedSizeBytes gibi Uygulamalı alanlarda görebilirsiniz. Bu alanlar, oturumun ne kadarının başarılı olduğunu söyler. Birden çok eşitleme oturumunun üst üste başarısız olduğunu görürseniz ve artan bir Uygulamalı sayım varsa, destek biletini açmadan önce yeniden denemek için eşitleme süresi verin.

- Etkin bir eşitleme oturumu varsa Event ID 9302 her 5 ila 10 dakikada bir günlüğe kaydedilir. Geçerli eşitleme oturumunun ilerleme kaydedip kaydedilmemasını belirlemek için bu olayı kullanın (**AppliedItemCount > 0).** Eşitleme ilerleme kaydetmiyorsa, eşitleme oturumu sonunda başarısız olur ve bir Event ID 9102 hatayla günlüğe kaydedilir. Daha fazla bilgi için [eşitleme ilerleme belgelerine](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session)bakın.

Kayıtlı sunucu durumu:

- Olay Kimliği 9301, bir sunucu işleri için hizmeti sorguladığında her 30 saniyede bir günlüğe kaydedilir. GetNextJob **durum = 0**ile biterse, sunucu hizmetle iletişim kurabilir. GetNextJob bir hatayla bitiyorsa, kılavuz için [sorun giderme belgelerini](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) denetleyin.

Bulut katmanlama durumu:

- Bir sunucudaki katmanlama etkinliğini izlemek için, *Uygulama ve Hizmetler\Microsoft\FileSync\Agent*altında Olay Görüntüleyici'de bulunan Telemetri olay günlüğünde Event ID 9003, 9016 ve 9029'u kullanın.

  - Olay Kimliği 9003, sunucu bitiş noktası için hata dağıtımı sağlar. Örneğin: Toplam Hata Sayısı ve Hata Kodu. Bir olay hata kodu başına günlüğe kaydedilir.
  - Olay Kimliği 9016, bir birim için gölgeleme sonuçları sağlar. Örneğin: Boş alan yüzdesi, oturumda gölgelenen dosya sayısı ve hayalet başarısız dosya sayısıdır.
  - Olay Kimliği 9029, sunucu bitiş noktası için gölgeleme oturumu bilgilerini sağlar. Örneğin: Oturumda denenen dosya sayısı, oturumda katmanlı dosya sayısı ve zaten katmanlı dosya sayısı.
  
- Sunucudaki geri çağırma etkinliğini izlemek için, *Uygulama ve Hizmetler\Microsoft\FileSync\Agent*altında Olay Görüntüleyici'de bulunan Telemetri olay günlüğünde Event ID 9005, 9006, 9009 ve 9059'u kullanın.

  - Olay KIMLIĞI 9005, sunucu bitiş noktası için geri çağırma güvenilirliği sağlar. Örneğin: Erişilen toplam benzersiz dosyaları ve başarısız erişime sahip toplam benzersiz dosyaları.
  - Olay Kimliği 9006, sunucu bitiş noktası için geri çağırma hatası dağıtımı sağlar. Örneğin: Toplam Başarısız İstekler ve Hata Kodu. Bir olay hata kodu başına günlüğe kaydedilir.
  - Olay Kimliği 9009, sunucu bitiş noktası için geri çağırma oturumu bilgilerini sağlar. Örneğin: DurationSeconds, CountFilesRecallSucceeded ve CountFilesRecallFailed.
  - Olay Kimliği 9059, sunucu bitiş noktası için uygulama geri çağırma dağıtımı sağlar. Örneğin: ShareId, Uygulama Adı ve TotalEgressNetworkBytes.

### <a name="performance-counters"></a>Performans sayaçları

Eşitleme etkinliğini izlemek için sunucudaki Azure Dosya Eşitleme performans sayaçlarını kullanın.

Sunucuda Azure Dosya Eşitleme performans sayaçlarını görüntülemek için Performans İzleyicisi'ni (Perfmon.exe) açın. Aktarılabilen **AFS Baytları** ve **AFS Eşitleme İşlemleri** nesnelerinin altında sayaçları bulabilirsiniz.

Azure Dosya Eşitlemi için aşağıdaki performans sayaçları Performans İzleyicisi'nde kullanılabilir:

| Performans Nesnesi\Sayaç Adı | Açıklama |
|-|-|
| AKTARıLAN AFS Bayt\İndirilen Bayt/sn | Saniyede indirilen bayt sayısı. |
| AFS Bayt Aktarılan\Yüklenen Bayt/sn | Saniyede yüklenen bayt sayısı. |
| AFS Bayt Aktarılalı\Toplam Bayt/sn | Saniyede toplam bayt (yükleme ve indirme). |
| AFS Sync İşlemleri\İndirilen Eşitleme Dosyaları/sn | Saniyede indirilen dosya sayısı. |
| AFS Sync İşlemleri\Yüklenen Eşitleme Dosyaları/sn | Saniyede yüklenen dosya sayısı. |
| AFS Eşitleme İşlemleri\Toplam Eşitleme Dosyası İşlemleri/sn | Senkronize edilen toplam dosya sayısı (yükleme ve indirme). |

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Güvenlik duvarı ve proxy ayarlarını göz önünde bulundurun](storage-sync-files-firewall-and-proxy.md)
- [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)
- [Azure Dosya Eşitleme ile ilgili sorunları giderme](storage-sync-files-troubleshoot.md)
- [Azure Dosyaları sık sorulan sorular](storage-files-faq.md)
