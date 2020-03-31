---
title: Azure Dosyaları performans sorun giderme kılavuzu
description: Azure dosya paylaşımları ve ilişkili geçici geçici çözümlerle bilinen performans sorunları.
author: gunjanj
ms.service: storage
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: 09e55abcd97317b87f8a272afa51c6b4ace572e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77598094"
---
# <a name="troubleshoot-azure-files-performance-issues"></a>Azure Dosyaları performans sorunlarını giderme

Bu makalede, Azure dosya paylaşımları ile ilgili bazı yaygın sorunlar listelenist. Bu sorunlarla karşılaşıldığında olası nedenler ve geçici çözüm sağlar.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Yüksek gecikme, düşük iş tükenme ve genel performans sorunları

### <a name="cause-1-share-experiencing-throttling"></a>Neden 1: Azaltma yı paylaşan hisse

Bir prim payı varsayılan kota 100 GiB, 100 temel IOPS (bir saat için 300 kadar patlama potansiyeli ile) sağlar. Sağlama ve IOPS ile ilişkisi hakkında daha fazla bilgi için, planlama kılavuzunun [Sağlanan paylaşımlar](storage-files-planning.md#understanding-provisioning-for-premium-file-shares) bölümüne bakın.

Payınızın azaltılmış olup olmadığını doğrulamak için portalda Azure Ölçümleri'nden yararlanabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. **Tüm hizmetleri** seçin ve ardından **Ölçümleri**arayın.

1. **Ölçümler**’i seçin.

1. Kaynak olarak depolama hesabınızı seçin.

1. Metrik ad alanı olarak **Dosya'yı** seçin.

1. Metrik olarak **Hareketler'i** seçin.

1. **YanıtTürü** için bir filtre ekleyin ve herhangi bir istek **Başarı IleThrottling** (SMB için) veya **ClientThrottlingError** (REST için) yanıt kodu olup olmadığını görmek için denetleyin.

![Premium fileto lar için ölçüm seçenekleri](media/storage-troubleshooting-premium-fileshares/metrics.png)

> [!NOTE]
> Dosya paylaşımı azaltılırsa uyarı almak için, [dosya paylaşımı azaltılırsa nasıl uyarı oluşturulabildiğini](#how-to-create-an-alert-if-a-file-share-is-throttled)öğrenin.

### <a name="solution"></a>Çözüm

- Payınızda daha yüksek bir kota belirterek hisse sağlanan kapasiteyi artırın.

### <a name="cause-2-metadatanamespace-heavy-workload"></a>Neden 2: Metadata/namespace ağır iş yükü

İsteklerinizin çoğu meta veri merkezliyse (createfile/openfile/closefile/queryinfo/querydirectory gibi) okuma/yazma işlemlerine kıyasla gecikme daha kötü olacaktır.

İsteklerinizin çoğunun meta veri merkezli olup olmadığını doğrulamak için yukarıdaki adımları kullanabilirsiniz. **Yanıt Türü**için bir filtre eklemek yerine, **API Adı**için bir filtre ekleyin.

![Ölçümlerinizdeki API Adı filtresi](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>Geçici çözüm

- Uygulamanın meta veri işlemleri sayısını azaltmak için değiştirilip değiştirilemeyebileceğini denetleyin.
- Verilere karşı dosya işlemleri gerçekleştirmek için dosya paylaşımına bir VHD ekleyin ve istemciden SMB üzerinden VHD dağılayın. Bu yaklaşım, tek yazar ve birden çok okuyucu senaryosu için çalışır ve meta veri işlemlerinin yerel olmasını sağlar ve yerel doğrudan bağlı depolamaya benzer performans sunar.

### <a name="cause-3-single-threaded-application"></a>Neden 3: Tek dişli uygulama

Müşteri tarafından kullanılan uygulama tek iş parçacığı ysa, bu, sağlanan hisse boyutunuza bağlı olarak mümkün olan maksimum iOPS/iş parçacığından önemli ölçüde daha düşük sonuçlanabilir.

### <a name="solution"></a>Çözüm

- İş parçacığı sayısını artırarak uygulama paralelliğini artırın.
- Paralelliğin mümkün olduğu uygulamalara geçin. Örneğin, kopyalama işlemleri için müşteriler Windows istemcilerinden AzCopy veya RoboCopy veya Linux istemcileri üzerindeki **paralel** komutu kullanabilir.

## <a name="very-high-latency-for-requests"></a>İstekler için çok yüksek gecikme

### <a name="cause"></a>Nedeni

VM istemcisi dosya paylaşımından farklı bir bölgede bulunabilir.

### <a name="solution"></a>Çözüm

- Uygulamayı dosya paylaşımıyla aynı bölgede bulunan bir VM'den çalıştırın.

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>İstemci ağ tarafından desteklenen maksimum verim elde edemiyor

Bunun bir potansiyel nedeni bir eksikliği fo SMB çok kanallı destek. Şu anda Azure dosya paylaşımları yalnızca tek kanalı desteklediğinden, istemci VM'den sunucuya yalnızca bir bağlantı vardır. Bu tek bağlantı istemci VM'de tek bir çeki ye doğru saptanmıştır, böylece VM'den elde edilebilen maksimum verim tek bir çekirdekle bağlanır.

### <a name="workaround"></a>Geçici çözüm

- Daha büyük bir çekirdekle VM elde etmek, iş bilginin artmasına yardımcı olabilir.
- İstemci uygulamasını birden çok VM'den çalıştırmak iş bilgisini artırır.

- Mümkünse REST API'lerini kullanın.

## <a name="throughput-on-linux-clients-is-significantly-lower-when-compared-to-windows-clients"></a>Linux istemcileri üzerindeki iş başına iş, Windows istemcilerine kıyasla önemli ölçüde daha düşüktür.

### <a name="cause"></a>Nedeni

Bu Linux üzerinde Kobİ istemcisi uygulanması ile bilinen bir konudur.

### <a name="workaround"></a>Geçici çözüm

- Yükü birden çok VM'ye yayın.
- Aynı VM'de, **nosharesock** seçeneği olan birden fazla montaj noktası kullanın ve yükü bu montaj noktalarına yaydı.
- Linux'ta, her **fsync** aramada SMB floş zorlamaktan kaçınmak için **nostrictsync** seçeneği ile montaj deneyin. Azure Dosyaları için bu seçenek veri tutarlılığını engellemez, ancak dizin kaydında **(ls -l** komutu) eski dosya meta verilerine neden olabilir. Doğrudan dosyanın meta verilerini sorgulayan **(stat** komutu) en güncel dosya meta verilerini döndürecektir.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Kapsamlı açık/yakın işlemleri içeren meta veri ağır iş yükleri için yüksek gecikmeler.

### <a name="cause"></a>Nedeni

Dizin kiralamaları için destek eksikliği.

### <a name="workaround"></a>Geçici çözüm

- Mümkünse, kısa bir süre içinde aynı dizinde aşırı açma/kapatma kolundan kaçının.
- Linux VM'leri için, montaj seçeneği olarak **actimeo=\<sec>** belirterek dizin giriş önbelleği zaman aşındırma süresini artırın. Varsayılan olarak, bir saniyedir, bu nedenle üç veya beş gibi daha büyük bir değer yardımcı olabilir.
- Linux VM'leri için çekirdeği 4.20 veya daha yüksek bir yükseltme.

## <a name="low-iops-on-centosrhel"></a>CentOS/RHEL'de Düşük IOPS

### <a name="cause"></a>Nedeni

CentOS/RHEL'de birden büyük IO derinliği desteklenmez.

### <a name="workaround"></a>Geçici çözüm

- CentOS 8 / RHEL 8'e yükseltin.
- Ubuntu'ya geçiş.

## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Linux'taki Azure Dosyalarına ve Azure Dosyalarından yavaş dosya kopyalama

Azure Dosyalarına ve Azure Dosyalarından yavaş dosya kopyalama yaşıyorsanız, Linux sorun giderme kılavuzunda [Linux bölümündeki Azure Dosyaları'na ve bu dosyalardan kopyalama](storage-troubleshoot-linux-file-connection-problems.md#slow-file-copying-to-and-from-azure-files-in-linux) yavaş dosyaya bir göz atın.

## <a name="jitterysaw-tooth-pattern-for-iops"></a>IOPS için jittery/testere-diş deseni

### <a name="cause"></a>Nedeni

İstemci uygulaması sürekli temel IOPS aşıyor. Şu anda, istek yükünün servis tarafı düzgünleştirilmesi yoktur, bu nedenle istemci temel IOPS'yi aşarsa, hizmet tarafından daraltılacaktır. Bu azaltma, istemcinin gergin/testere dişi IOPS deseni yaşamasını sağlayabilir. Bu durumda, istemci tarafından elde edilen ortalama IOPS temel IOPS daha düşük olabilir.

### <a name="workaround"></a>Geçici çözüm

- Payın daraltılmayan istemci uygulamasından gelen istek yükünü azaltın.
- Payın daraltılmaması için pay kotasını artırın.

## <a name="excessive-directoryopendirectoryclose-calls"></a>Aşırı DizinOpen/DirectoryClose aramaları

### <a name="cause"></a>Nedeni

DirectoryOpen/DirectoryClose çağrılarının sayısı en iyi API aramaları arasındaysa ve istemcinin bu kadar çok arama yapmasını beklemiyorsanız, Azure istemcisi VM'de yüklü olan virüsten koruma yla ilgili bir sorun olabilir.

### <a name="workaround"></a>Geçici çözüm

- Bu sorun için bir düzeltme [Windows için Nisan Platform Güncelleştirmesi](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform)mevcuttur.

## <a name="file-creation-is-slower-than-expected"></a>Dosya oluşturma beklenenden daha yavaş

### <a name="cause"></a>Nedeni

Çok sayıda dosya oluşturmaya dayanan iş yükleri, premium dosya paylaşımlarının performansı yla standart dosya paylaşımları arasında önemli bir fark görmez.

### <a name="workaround"></a>Geçici çözüm

- Yok.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Windows 8.1 veya Server 2012 R2'den yavaş performans

### <a name="cause"></a>Nedeni

IO yoğun iş yükleri için Azure Dosyalarına erişim beklenenden daha yüksek gecikme.

### <a name="workaround"></a>Geçici çözüm

- Kullanılabilir [düzeltmeyi](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1)yükleyin.

## <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>Dosya paylaşımı azaltılırsa uyarı oluşturma

1. Azure [portalında,](https://portal.azure.com) **Monitör'e**tıklayın. 

2. **Uyarılar'ı** tıklatın ve ardından **+ Yeni uyarı kuralını**tıklatın.

3. Uyarmak istediğiniz dosya paylaşımını içeren **depolama hesabı/dosya** kaynağını seçmek için **Seç'i** tıklatın ve ardından **Bitti'yi**tıklatın. Örneğin, depolama hesabı adı contoso ise, contoso/dosya kaynağını seçin.

4. Koşul eklemek için **Ekle'yi** tıklatın.

5. Depolama hesabı için desteklenen sinyallerin listesini görürsünüz, **Hareketler** metrik'ini seçin.

6. **Yapılandırma sinyali mantık** bıçağında **Yanıt türü** boyutuna gidin, Boyut **değerleri** açılır masını tıklatın ve **SuccessWithThrottling** (SMB için) veya **ClientThrottlingError** (REST için) seçeneğini belirleyin. 

  > [!NOTE]
  > SuccessWithThrottling veya ClientThrottlingError boyut değeri listelenmemişse, bu kaynağın daraltılmış olmadığı anlamına gelir.  Boyut değerini eklemek için, **+** **Boyut değerleri** açılır bırakma nın yanındakine tıklayın, **SuccessWithThrottling** veya **ClientThrottlingError**yazın, **Tamam'ı** tıklatın ve ardından adım #6 yineleyin.

7. **Dosya Paylaş** boyutuna gidin, **Boyut değerleri** açılır'ı tıklatın ve uyarmak istediğiniz dosya paylaşımını(lar) seçin. 

  > [!NOTE]
  > Dosya paylaşımı standart bir dosya paylaşımıysa, hisse başına ölçümler standart dosya paylaşımları için kullanılamadığından, boyut değerleri açılır boş olur. Depolama hesabı içindeki herhangi bir dosya paylaşımı daraltılırsa ve uyarı hangi dosya paylaşımının azaltıldığını belirlemezse, standart dosya paylaşımları için azaltma uyarıları tetiklenir. Standart dosya paylaşımları için hisse başına ölçümler kullanılamadığından, öneri depolama hesabı başına bir dosya paylaşımına sahip olmaktır. 

8. Metrik **uyarı** kuralını değerlendirmek için kullanılan uyarı parametrelerini (eşik, işleç, toplama tanecikleri ve frekans) tanımlayın ve **Bitti'yi**tıklatın.

  > [!TIP]
  > Statik bir eşik kullanıyorsanız, dosya paylaşımı şu anda azaltılıyorsa, metrik grafik makul bir eşiğin belirlenmesine yardımcı olabilir. Dinamik bir eşik kullanıyorsanız, metrik grafik en son verilere dayalı olarak hesaplanan eşikleri görüntüler.

9. Varolan bir eylem grubu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir **eylem grubu** (e-posta, SMS, vb.) ekleyin.

10. **Uyarı kural adı,** **Açıklama** ve **Önem**derecesi gibi **Uyarı ayrıntılarını** doldurun.

11. Uyarıyı oluşturmak için **uyarı kuralı** oluştur'u tıklatın.

Azure Monitor'da uyarıları yapılandırma hakkında daha fazla bilgi edinmek [için Microsoft Azure'daki uyarılara Genel Bakış]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)bölümüne bakın.
