---
title: İzleyici Azure Dosya Eşitleme | Microsoft Docs
description: Azure Izleyici, depolama eşitleme hizmeti ve Windows Server kullanarak Azure Dosya Eşitleme dağıtımınızı nasıl izleyeceğinizi inceleyin.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 08/05/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 1d7b29bbd508223888c6f205e25008c0b29fecea
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87922943"
---
# <a name="monitor-azure-file-sync"></a>Azure Dosya Eşitleme’yi izleme

Şirket içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu koruyarak kuruluşunuzun dosya paylaşımlarını Azure dosyalarında merkezileştirmek için Azure Dosya Eşitleme kullanın. Azure Dosya Eşitleme, Windows Server’ı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilirsiniz. Dünyanın dört bir yanında ihtiyacınız olan sayıda önbellekler olabilir.

Bu makalede, Azure Izleyici, depolama eşitleme hizmeti ve Windows Server kullanılarak Azure Dosya Eşitleme dağıtımınızın nasıl izleneceği açıklanır.

Bu kılavuzda aşağıdaki senaryolar ele alınmıştır: 
- Azure Izleyici 'de Azure Dosya Eşitleme ölçümlerini görüntüleyin.
- Azure Izleyici 'de, kritik koşullar hakkında önceden bildirim almak için uyarılar oluşturun.
- Azure portal kullanarak Azure Dosya Eşitleme dağıtımınızın durumunu görüntüleyin.
- Azure Dosya Eşitleme dağıtımınızın durumunu izlemek için Windows sunucularınızda olay günlükleri ve performans sayaçlarını kullanma. 

## <a name="azure-monitor"></a>Azure İzleyici

Ölçümleri görüntülemek ve eşitleme, bulut katmanlama ve sunucu bağlantısı için uyarıları yapılandırmak üzere [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) 'yi kullanın.  

### <a name="metrics"></a>Ölçümler

Azure Dosya Eşitleme ölçümleri varsayılan olarak etkindir ve 15 dakikada bir Azure Izleyicisine gönderilir.

**Azure Izleyici 'de Azure Dosya Eşitleme ölçümlerini görüntüleme**
- **Azure Portal** **depolama eşitleme hizmetinize** gidin ve **ölçümler**' e tıklayın.
- **Ölçüm** açılan düğmesine tıklayın ve görüntülemek istediğiniz ölçümü seçin.

Azure Dosya Eşitleme için aşağıdaki ölçümler Azure Izleyici 'de bulunabilir:

| Ölçüm adı | Açıklama |
|-|-|
| Eşitlenen baytlar | Aktarılan verilerin boyutu (karşıya yükleme ve indirme).<br><br>Birim: bayt<br>Toplama türü: Toplam<br>Geçerli Boyutlar: sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |
| Bulut katmanlama geri çağırma | Geri çekilen verilerin boyutu.<br><br>**Note**: Bu ölçüm gelecekte kaldırılacak. Geri çekilen verilerin boyutunu izlemek için bulut katmanlama geri çekme boyutu ölçümünü kullanın.<br><br>Birim: bayt<br>Toplama türü: Toplam<br>Geçerli boyut: sunucu adı |
| Bulut katmanlama geri çağırma boyutu | Geri çekilen verilerin boyutu.<br><br>Birim: bayt<br>Toplama türü: Toplam<br>Geçerli boyut: sunucu adı, eşitleme grubu adı |
| Uygulamaya göre bulut katmanlama geri çağırma boyutu | Uygulama tarafından geri çekilen verilerin boyutu.<br><br>Birim: bayt<br>Toplama türü: Toplam<br>Geçerli boyut: uygulama adı, sunucu adı, eşitleme grubu adı |
| Bulut katmanlama geri çekme performansı | Veri geri çağırma üretilen iş boyutu.<br><br>Birim: bayt<br>Toplama türü: Toplam<br>Geçerli boyut: sunucu adı, eşitleme grubu adı |
| Dosyalar eşitlenmiyor | Eşitleme başarısız olan dosyaların sayısı.<br><br>Birim: sayım<br>Toplama türü: Toplam<br>Geçerli Boyutlar: sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |
| Eşitlenen dosyalar | Aktarılan dosya sayısı (karşıya yükleme ve indirme).<br><br>Birim: sayım<br>Toplama türü: Toplam<br>Geçerli Boyutlar: sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |
| Sunucu çevrimiçi durumu | Sunucudan alınan sinyal sayısı.<br><br>Birim: sayım<br>Toplama türü: en fazla<br>Geçerli boyut: sunucu adı |
| Eşitleme oturumu sonucu | Eşitleme oturumu sonucu (1 = başarılı eşitleme oturumu; 0 = başarısız eşitleme oturumu)<br><br>Birim: sayım<br>Toplama türleri: en fazla<br>Geçerli Boyutlar: sunucu uç noktası adı, eşitleme yönü, eşitleme grubu adı |

### <a name="alerts"></a>Uyarılar

Uyarılar, izleme verilerinizde önemli koşullar bulunduğunda size bir bildirim gönderir. Azure Izleyici 'de uyarıları yapılandırma hakkında daha fazla bilgi edinmek için bkz. [Microsoft Azure uyarılara genel bakış](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

**Azure Dosya Eşitleme için uyarı oluşturma**

- **Azure Portal** **depolama eşitleme hizmetinize** gidin. 
- Izleme bölümünde **Uyarılar** ' a ve ardından **+ Yeni uyarı kuralı**' na tıklayın.
- **Koşul Seç** ' e tıklayın ve uyarı için aşağıdaki bilgileri sağlayın: 
    - **Ölçüm**
    - **Boyut adı**
    - **Uyarı mantığı**
- **Eylem grubunu Seç** ' e tıklayın ve var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir eylem grubu (e-posta, SMS, vb.) ekleyin.
- Uyarı **kuralı adı**, **Açıklama** ve **önem derecesi**gibi **uyarı ayrıntılarını** girin.
- Uyarı oluşturmak için **Uyarı kuralı oluştur** ' a tıklayın.  

Aşağıdaki tabloda, izlemek için bazı örnek senaryolar ve uyarı için kullanılacak uygun ölçüm listelenmektedir:

| Senaryo | Uyarı için kullanılacak ölçüm |
|-|-|
| Sunucu uç noktası sistem durumu portalda bir hata gösteriyor | Eşitleme oturumu sonucu |
| Dosyalar bir sunucu veya bulut uç noktasına eşitlenemiyor | Dosyalar eşitlenmiyor |
| Kayıtlı sunucu, depolama eşitleme hizmeti ile iletişim kuramıyor | Sunucu çevrimiçi durumu |
| Bulut katmanlama geri çağırma boyutu günde 500 gib 'yi aştı  | Bulut katmanlama geri çağırma boyutu |

Bu senaryolar için uyarı oluşturma yönergeleri için [uyarı örnekleri](#alert-examples) bölümüne bakın.

## <a name="storage-sync-service"></a>Depolama Eşitleme Hizmeti

Kayıtlı sunucu sistem durumu, sunucu uç noktası sistem durumu ve ölçümleri görüntülemek için Azure portal depolama Eşitleme hizmetine gidin. Kayıtlı sunucu sistem durumunu **kayıtlı sunucular** dikey penceresinde ve sunucu uç noktası sistem durumu ' nu **eşitleme grupları** dikey penceresinde görüntüleyebilirsiniz.

### <a name="registered-server-health"></a>Kayıtlı sunucu durumu

- **Kayıtlı sunucu** durumu **çevrimiçi**ise, sunucu hizmetle başarılı bir şekilde iletişim kuruyor.
- **Kayıtlı sunucu** durumu **çevrimdışı görünüyorsa**, depolama eşitleme İzleyicisi işlemi (AzureStorageSyncMonitor.exe) çalışmıyor veya sunucu Azure dosya eşitleme hizmetine erişemiyor. Rehberlik için [sorun giderme belgelerine](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#server-endpoint-noactivity) bakın.

### <a name="server-endpoint-health"></a>Sunucu uç noktası sistem durumu

- Portaldaki sunucu uç noktası sistem durumu, sunucudaki telemetri olay günlüğüne kaydedilen eşitleme olaylarını temel alır (KIMLIK 9102 ve 9302). Hata iptal edildiğinde, eşitleme oturumu geçici bir hata nedeniyle başarısız olursa, eşitleme, geçerli eşitleme oturumu devam ettiği sürece portalda hala sağlıklı olarak görünebilir. Olay KIMLIĞI 9302, dosyaların uygulanmakta olup olmadığını belirlemekte kullanılır. Daha fazla bilgi için bkz. [eşitleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) ve [Eşitleme ilerleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Eşitleme ilerleme yaptığından Portal bir eşitleme hatası gösteriyorsa, rehberlik için [sorun giderme belgelerine](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) bakın.

### <a name="metric-charts"></a>Ölçüm grafikleri

- Aşağıdaki ölçüm grafikleri, depolama eşitleme hizmeti portalında görüntülenebilir:

  | Ölçüm adı | Açıklama | Dikey pencere adı |
  |-|-|-|
  | Eşitlenen baytlar | Aktarılan verilerin boyutu (karşıya yükleme ve indirme) | Eşitleme grubu, sunucu uç noktası |
  | Bulut katmanlama geri çağırma | Geri çekilen verilerin boyutu | Kayıtlı sunucular |
  | Dosyalar eşitlenmiyor | Eşitleme başarısız olan dosyaların sayısı | Sunucu uç noktası |
  | Eşitlenen dosyalar | Aktarılan dosya sayısı (karşıya yükleme ve indirme) | Eşitleme grubu, sunucu uç noktası |
  | Sunucu çevrimiçi durumu | Sunucudan alınan sinyal sayısı | Kayıtlı sunucular |

- Daha fazla bilgi için bkz. [Azure izleyici](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor).

  > [!Note]  
  > Depolama eşitleme hizmeti portalındaki grafiklerde 24 saatlik bir zaman aralığı vardır. Farklı zaman aralıklarını veya boyutları görüntülemek için Azure Izleyici 'yi kullanın.

## <a name="windows-server"></a>Windows Server

Azure Dosya Eşitleme aracısının yüklü olduğu Windows Server 'da, bulut katmanlaması, kayıtlı sunucu ve eşitleme sistem durumunu görüntüleyebilirsiniz.

### <a name="event-logs"></a>Olay günlükleri

Kayıtlı sunucu, eşitleme ve bulut katmanlama durumunu izlemek için sunucuda telemetri olay günlüğünü kullanın. Telemetri olay günlüğü, *uygulamalar ve Services\Microsoft\FileSync\Agent*altında Olay Görüntüleyicisi bulunur.

Eşitleme durumu:

- Olay KIMLIĞI 9102, eşitleme oturumu tamamlandıktan sonra günlüğe kaydedilir. Eşitleme oturumlarının başarılı olup olmadığını (**HRESULT = 0**) ve öğe başına Eşitleme hataları olup olmadığını öğrenmek için bu olayı kullanın. Daha fazla bilgi için bkz. [eşitleme durumu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) ve [öğe başına hatalar](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing) belgeleri.

  > [!Note]  
  > Bazen eşitleme oturumları genel başarısız olur ya da sıfır olmayan bir PerItemErrorCount sayısına sahiptir. Ancak, yine de ilerleme durumu ve bazı dosyalar başarıyla eşitlenir. Bunu, AppliedFileCount, AppliedDirCount, AppliedTombstoneCount ve AppliedSizeBytes gibi uygulanan alanlarda görebilirsiniz. Bu alanlar, oturumun ne kadarının başarılı olduğunu bildirir. Bir satırda birden çok eşitleme oturumu başarısız görürseniz ve artan uygulanmış sayımla karşılaşırsanız, bir destek bileti açmadan önce yeniden denemek için eşitleme süresi verin.

- Olay KIMLIĞI 9121, eşitleme oturumu tamamlandıktan sonra her öğe başına hata için günlüğe kaydedilir. Bu hatayla (**Persistentcount** ve **geçişli entcount**) eşitlemeyen dosya sayısını öğrenmek için bu olayı kullanın. Kalıcı öğe başına hataların araştırılması gerekir. Nasıl yaparım?, [eşitlenmeyen belirli dosya veya klasörler olup](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing)olmadığını görmek için bkz..

- Etkin bir eşitleme oturumu varsa olay KIMLIĞI 9302, her 5 dakikada bir günlüğe kaydedilir. Geçerli eşitleme oturumunun ilerleme yapıp yapmadığını (**AppliedItemCount > 0**) öğrenmek için bu olayı kullanın. Eşitleme ilerlemeye devam etmez, eşitleme oturumunun sonunda başarısız olması gerekir ve hata ile 9102 olay KIMLIĞI günlüğe kaydedilir. Daha fazla bilgi için bkz. [eşitleme ilerlemesi belgeleri](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).

Kayıtlı sunucu durumu:

- Olay KIMLIĞI 9301, bir sunucu işler için hizmeti sorguladığında her 30 saniyede bir günlüğe kaydedilir. GetNextJob **durum = 0**ile sonlandıysanız, sunucu hizmetle iletişim kurabilir. GetNextJob bir hata ile sonlandıysanız, kılavuzluk için [sorun giderme belgelerini](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#server-endpoint-noactivity) denetleyin.

Bulut katmanlama durumu:

- Bir sunucudaki katmanlama etkinliğini izlemek için, *uygulamalar ve Services\Microsoft\FileSync\Agent*altında Olay Görüntüleyicisi bulunan telemetri olay GÜNLÜĞÜNDE olay kimliği 9003, 9016 ve 9029 kullanın.

  - Olay KIMLIĞI 9003, bir sunucu uç noktası için hata dağıtımı sağlar. Örneğin: Toplam hata sayısı ve hata kodu. Hata kodu başına bir olay günlüğe kaydedilir.
  - Olay kimliği 9016, bir birim için hayalet kopya oluşturuluyor sonuçları sağlar. Örneğin: boş alan yüzdesi, oturumdaki hayalet dosya sayısı ve hayalet başarısız olan dosya sayısı.
  - Olay kimliği 9029, bir sunucu uç noktası için hayalet kopya oluşturuluyor oturum bilgileri sağlar. Örneğin: oturumda denenen dosya sayısı, oturumda katmanlı dosya sayısı ve zaten katmanlı dosya sayısı.
  
- Bir sunucudaki geri çekme etkinliğini izlemek için, *uygulamalar ve Services\Microsoft\FileSync\Agent*altında Olay Görüntüleyicisi bulunan telemetri olay GÜNLÜĞÜNDE olay kimliği 9005, 9006, 9009 ve 9059 kullanın.

  - Olay KIMLIĞI 9005, bir sunucu uç noktası için geri çağırma güvenilirliği sağlar. Örneğin: erişilen toplam benzersiz dosya ve başarısız erişimi olan toplam benzersiz dosya sayısı.
  - Olay KIMLIĞI 9006, bir sunucu uç noktası için geri çağırma hatası dağıtımı sağlar. Örneğin: Toplam başarısız Istek ve hata kodu. Hata kodu başına bir olay günlüğe kaydedilir.
  - Olay KIMLIĞI 9009, bir sunucu uç noktası için geri çağırma oturumu bilgilerini sağlar. Örneğin: DurationSeconds, CountFilesRecallSucceeded ve CountFilesRecallFailed.
  - Olay KIMLIĞI 9059, bir sunucu uç noktası için uygulama geri çağırma dağıtımı sağlar. Örneğin: Shareıd, Application Name ve TotalEgressNetworkBytes.

### <a name="performance-counters"></a>Performans sayaçları

Eşitleme etkinliğini izlemek için sunucuda Azure Dosya Eşitleme performans sayaçlarını kullanın.

Sunucuda Azure Dosya Eşitleme performans sayaçlarını görüntülemek için performans Izleyicisi 'ni (Perfmon.exe) açın. Sayaçları, **aktarılan AFS bayt** ve **AFS eşitleme işlemleri** nesneleri altında bulabilirsiniz.

Azure Dosya Eşitleme için aşağıdaki performans sayaçları performans Izleyicisinde kullanılabilir:

| Performans Object\sayaç adı | Açıklama |
|-|-|
| AFS bayt Transferred\indirilen bayt/sn | Saniye başına indirilen bayt sayısı. |
| AFS bayt Transferred\karşıya yüklenen bayt/sn | Saniye başına karşıya yüklenen bayt sayısı. |
| AFS bayt Transferred\toplam bayt/sn | Toplam bayt/saniye (karşıya yükleme ve indirme). |
| AFS Sync Operations\indirilen eşitleme dosyaları/sn | Saniye başına indirilen dosya sayısı. |
| AFS Sync Operations\karşıya yüklenen eşitleme dosyaları/sn | Saniye başına karşıya yüklenen dosya sayısı. |
| AFS Sync Operations\toplam eşitleme dosyası Işlemi/sn | Eşitlenen toplam dosya sayısı (karşıya yükleme ve indirme). |

## <a name="alert-examples"></a>Uyarı örnekleri
Bu bölüm Azure Dosya Eşitleme için bazı örnek uyarılar sağlar.

  > [!Note]  
  > Bir uyarı oluşturursanız ve çok gürültülü bir uyarı oluşturursanız eşik değerini ve uyarı mantığını ayarlayın.
  
### <a name="how-to-create-an-alert-if-the-server-endpoint-health-shows-an-error-in-the-portal"></a>Sunucu uç noktası durumu portalda bir hata gösteriyorsa uyarı oluşturma

1. **Azure Portal**Ilgili **depolama eşitleme hizmeti**' ne gidin. 
2. **İzleme** bölümüne gidin ve **Uyarılar**' a tıklayın. 
3. Yeni bir uyarı kuralı oluşturmak için **+ Yeni uyarı kuralına** tıklayın. 
4. **Koşul Seç**' i tıklatarak koşulu yapılandırın.
5. **Sinyal mantığını Yapılandır** dikey penceresinde, sinyal adı altında **eşitleme oturumu sonucu** ' na tıklayın.  
6. Aşağıdaki boyut yapılandırmasını seçin: 
    - Boyut adı: **sunucu uç noktası adı**  
    - İşlecinde**=** 
    - Boyut değerleri: **tüm geçerli ve gelecekteki değerler**  
7. **Uyarı mantığına** gidin ve aşağıdakileri doldurun: 
    - Eşik **statik** olarak ayarlandı 
    - İşleç: **küçüktür** 
    - Toplama türü: **en fazla**  
    - Eşik değeri: **1** 
    - Temel alınarak değerlendirilen: toplama ayrıntı düzeyi = **24 saat** | Değerlendirme sıklığı = **her saat** 
    - Bitti ' ye tıklayın **.** 
8. Var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir eylem grubu (e-posta, SMS, vb.) eklemek için **Eylem grubunu Seç** ' e tıklayın.
9. Uyarı **kuralı adı**, **Açıklama** ve **önem derecesi**gibi **uyarı ayrıntılarını** girin.
10. **Uyarı kuralı oluştur**’a tıklayın. 

### <a name="how-to-create-an-alert-if-files-are-failing-to-sync-to-a-server-or-cloud-endpoint"></a>Dosyalar bir sunucu veya bulut uç noktası ile eşitlenememesi durumunda uyarı oluşturma

1. **Azure Portal**Ilgili **depolama eşitleme hizmeti**' ne gidin. 
2. **İzleme** bölümüne gidin ve **Uyarılar**' a tıklayın. 
3. Yeni bir uyarı kuralı oluşturmak için **+ Yeni uyarı kuralına** tıklayın. 
4. **Koşul Seç**' i tıklatarak koşulu yapılandırın.
5. **Sinyal mantığını Yapılandır** dikey penceresinde, sinyal adı altında **eşitleme değil dosyalar** ' a tıklayın.  
6. Aşağıdaki boyut yapılandırmasını seçin: 
     - Boyut adı: **sunucu uç noktası adı**  
     - İşlecinde**=** 
     - Boyut değerleri: **tüm geçerli ve gelecekteki değerler**  
7. **Uyarı mantığına** gidin ve aşağıdakileri doldurun: 
     - Eşik **statik** olarak ayarlandı 
     - İşleç: **büyüktür** 
     - Toplama türü: **Toplam**  
     - Eşik değeri: **100** 
     - Temel alınarak değerlendirilen: toplama ayrıntı düzeyi = **5 dakika** | Değerlendirme sıklığı = **her 5 dakikada** bir 
     - Bitti ' ye tıklayın **.** 
8. Var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir eylem grubu (e-posta, SMS, vb.) eklemek için **Eylem grubunu Seç** ' e tıklayın.
9. Uyarı **kuralı adı**, **Açıklama** ve **önem derecesi**gibi **uyarı ayrıntılarını** girin.
10. **Uyarı kuralı oluştur**’a tıklayın. 

### <a name="how-to-create-an-alert-if-a-registered-server-is-failing-to-communicate-with-the-storage-sync-service"></a>Kayıtlı bir sunucu depolama eşitleme hizmeti ile iletişim kuramıyorsa uyarı oluşturma

1. **Azure Portal**Ilgili **depolama eşitleme hizmeti**' ne gidin. 
2. **İzleme** bölümüne gidin ve **Uyarılar**' a tıklayın. 
3. Yeni bir uyarı kuralı oluşturmak için **+ Yeni uyarı kuralına** tıklayın. 
4. **Koşul Seç**' i tıklatarak koşulu yapılandırın.
5. **Sinyal mantığını Yapılandır** dikey penceresinde, sinyal adı altında **sunucu çevrimiçi durumu** ' na tıklayın.  
6. Aşağıdaki boyut yapılandırmasını seçin: 
     - Boyut adı: **sunucu adı**  
     - İşlecinde**=** 
     - Boyut değerleri: **tüm geçerli ve gelecekteki değerler**  
7. **Uyarı mantığına** gidin ve aşağıdakileri doldurun: 
     - Eşik **statik** olarak ayarlandı 
     - İşleç: **küçüktür** 
     - Toplama türü: **en fazla**  
     - Eşik değeri (bayt): **1** 
     - Temel alınarak değerlendirilen: toplama ayrıntı düzeyi = **1 saat** | Değerlendirme sıklığı = **her 30 dakikada** bir 
     - Bitti ' ye tıklayın **.** 
8. Var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir eylem grubu (e-posta, SMS, vb.) eklemek için **Eylem grubunu Seç** ' e tıklayın.
9. Uyarı **kuralı adı**, **Açıklama** ve **önem derecesi**gibi **uyarı ayrıntılarını** girin.
10. **Uyarı kuralı oluştur**’a tıklayın. 

### <a name="how-to-create-an-alert-if-the-cloud-tiering-recall-size-has-exceeded-500gib-in-a-day"></a>Bulut katmanlama geri çağırma boyutu günde 500 gib 'yi aşarsa uyarı oluşturma

1. **Azure Portal**Ilgili **depolama eşitleme hizmeti**' ne gidin. 
2. **İzleme** bölümüne gidin ve **Uyarılar**' a tıklayın. 
3. Yeni bir uyarı kuralı oluşturmak için **+ Yeni uyarı kuralına** tıklayın. 
4. **Koşul Seç**' i tıklatarak koşulu yapılandırın.
5. **Sinyal mantığını Yapılandır** dikey penceresinde, sinyal adı altında **bulut katmanlama geri çağırma boyutu** ' na tıklayın.  
6. Aşağıdaki boyut yapılandırmasını seçin: 
     - Boyut adı: **sunucu adı**  
     - İşlecinde**=** 
     - Boyut değerleri: **tüm geçerli ve gelecekteki değerler**  
7. **Uyarı mantığına** gidin ve aşağıdakileri doldurun: 
     - Eşik **statik** olarak ayarlandı 
     - İşleç: **büyüktür** 
     - Toplama türü: **Toplam**  
     - Eşik değeri (bayt): **67108864000** 
     - Temel alınarak değerlendirilen: toplama ayrıntı düzeyi = **24 saat** | Değerlendirme sıklığı = **her saat** 
    - Bitti ' ye tıklayın **.** 
8. Var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir eylem grubu (e-posta, SMS, vb.) eklemek için **Eylem grubunu Seç** ' e tıklayın.
9. Uyarı **kuralı adı**, **Açıklama** ve **önem derecesi**gibi **uyarı ayrıntılarını** girin.
10. **Uyarı kuralı oluştur**’a tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosya Eşitleme dağıtımını planlama](storage-sync-files-planning.md)
- [Güvenlik duvarını ve proxy ayarlarını değerlendirin](storage-sync-files-firewall-and-proxy.md)
- [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)
- [Azure Dosya Eşitleme'de sorun giderme](storage-sync-files-troubleshoot.md)
- [Azure dosyaları hakkında sık sorulan sorular](storage-files-faq.md)
