---
title: Azure dosyaları performans sorunlarını giderme kılavuzu
description: Azure dosya paylaşımlarında bilinen performans sorunlarını giderin. Bu sorunlarla karşılaşıldığında olası nedenleri ve ilgili geçici çözümleri bulun.
author: gunjanj
ms.service: storage
ms.topic: troubleshooting
ms.date: 09/15/2020
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: 7afaa057ecc94cf67d4fd5b041d95210fcf26717
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707603"
---
# <a name="troubleshoot-azure-files-performance-issues"></a>Azure dosyaları performans sorunlarını giderme

Bu makalede, Azure dosya paylaşımlarıyla ilgili bazı yaygın sorunlar listelenmektedir. Bu sorunlarla karşılaşıldığında olası nedenleri ve geçici çözümleri sağlar.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Yüksek gecikme süresi, düşük aktarım hızı ve genel performans sorunları

### <a name="cause-1-share-was-throttled"></a>Neden 1: paylaşma daraltıldı

Bir dosya paylaşımının ıOPS, giriş veya çıkış sınırlarına ulaşıldığında istekler azaltılır. Standart ve Premium dosya paylaşımlarının sınırlarını anlamak için bkz. [dosya paylaşımı ve dosya ölçeği hedefleri](https://docs.microsoft.com/azure/storage/files/storage-files-scale-targets#file-share-and-file-scale-targets).

Paylaşımınızın kısıtlandığından emin olmak için, portalda Azure ölçümlerinden yararlanabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Tüm hizmetler** ' i seçin ve ardından **ölçümler**' i arayın.

1. **Ölçümler**’i seçin.

1. Kaynak olarak depolama hesabınızı seçin.

1. Ölçüm ad alanı olarak **Dosya** ' yı seçin.

1. Ölçüm olarak **işlemler** ' i seçin.

1. **ResponseType** için bir filtre ekleyin ve herhangi bir isteğin bir yanıt **kodu (SMB** için) veya **CLIENTKıSıTıNGERROR** (REST için) yanıt koduna sahip olup olmadığını denetleyin.

![Premium dosya paylaşımları için ölçüm seçenekleri](media/storage-troubleshooting-premium-fileshares/metrics.png)

> [!NOTE]
> Bir dosya paylaşımının kısıtlandığı bir uyarı almak için, bkz. [bir dosya paylaşma kısıtlandığında uyarı oluşturma](#how-to-create-an-alert-if-a-file-share-is-throttled).

### <a name="solution"></a>Çözüm

- Standart bir dosya paylaşımı kullanıyorsanız, depolama hesabınızda [büyük dosya paylaşımlarını](https://docs.microsoft.com/azure/storage/files/storage-files-how-to-create-large-file-share?tabs=azure-portal) etkinleştirin. Büyük dosya paylaşımları, paylaşım başına 10.000 ıOPS 'ye kadar destekler.
- Premium dosya paylaşma kullanıyorsanız, ıOPS sınırını artırmak için sağlanan dosya paylaşımının boyutunu artırın. Daha fazla bilgi edinmek için bkz. Azure dosyaları Planlama Kılavuzu 'ndaki [Premium dosya paylaşımlarının sağlamasını anlama](https://docs.microsoft.com/azure/storage/files/storage-files-planning#understanding-provisioning-for-premium-file-shares) bölümü.

### <a name="cause-2-metadatanamespace-heavy-workload"></a>Neden 2: meta veri/ad alanı ağır iş yükü

İsteklerinizin çoğunluğu meta veri merkezli ise (CreateFile/OpenFile/CloseFile/QueryInfo/querydirectory gibi), okuma/yazma işlemlerine kıyasla gecikme daha kötüleşmeyecektir.

İsteklerinizin çoğunun meta veri merkezli olup olmadığını doğrulamak için yukarıdaki adımlardan aynı adımları kullanabilirsiniz. **ResponseType**için bir filtre eklemek yerine, **API adı**için bir filtre ekleyin.

![Ölçümlerinizin API adı için filtre](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>Geçici çözüm

- Meta veri işlemlerinin sayısını azaltmak için uygulamanın değiştirilip değiştirilemeyeceğini denetleyin.
- Dosya paylaşımında bir VHD ekleyin ve veri üzerinde dosya işlemleri gerçekleştirmek için VHD 'YI istemciden SMB 'ye bağlayın. Bu yaklaşım tek yazıcı ve birden çok okuyucu senaryosu için geçerlidir ve meta veri işlemlerinin yerel olmasına olanak sağlar ve yerel olarak doğrudan bağlanmış bir depolamaya benzer bir performans sunar.

### <a name="cause-3-single-threaded-application"></a>Neden 3: tek iş parçacıklı uygulama

Müşteri tarafından kullanılan uygulama tek iş parçacıklı ise, bu, sağlanan paylaşma boyutunuzu temel alarak mümkün olan en yüksek sayıdan/aktarım hızına göre önemli ölçüde daha düşük bir değer oluşmasına neden olabilir.

### <a name="solution"></a>Çözüm

- İş parçacığı sayısını artırarak uygulama paralelliğini artırın.
- Paralellik mümkün olduğunda uygulamalara geçiş yapın. Örneğin, kopyalama işlemleri için müşteriler Windows istemcilerinden AzCopy veya RoboCopy veya Linux istemcilerindeki **Parallel** komutunu kullanabilir.

## <a name="very-high-latency-for-requests"></a>İstekler için çok yüksek gecikme süresi

### <a name="cause"></a>Nedeni

İstemci VM dosyası, dosya paylaşımından farklı bir bölgede bulunabilir.

### <a name="solution"></a>Çözüm

- Uygulamayı dosya paylaşımıyla aynı bölgede bulunan bir VM 'den çalıştırın.

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>İstemci ağ tarafından desteklenen maksimum üretilen iş elde edemedi

Bunun olası nedenlerinden biri, SMB çoklu kanal desteğinin olmamasıdır. Şu anda Azure dosya paylaşımları yalnızca tek kanalı destekler, bu nedenle istemci sanal makineden sunucuya yalnızca bir bağlantı vardır. Bu tek bağlantı, istemci sanal makinesinde tek bir çekirdeğe gereğinden, bu nedenle bir VM 'den en fazla üretilen iş ulaşılabilir, tek bir çekirdekle bağlanır.

### <a name="workaround"></a>Geçici çözüm

- Daha büyük bir çekirdekli VM 'nin alınması, aktarım hızını artırmaya yardımcı olabilir.
- İstemci uygulamasını birden çok VM 'den çalıştırmak, aktarım hızını artırır.

- Mümkün olduğunda REST API 'Leri kullanın.

## <a name="throughput-on-linux-clients-is-significantly-lower-when-compared-to-windows-clients"></a>Linux istemcilerinde üretilen iş, Windows istemcileri ile karşılaştırıldığında önemli ölçüde düşüktür.

### <a name="cause"></a>Nedeni

Bu, Linux üzerinde SMB istemcisinin uygulanmasıyla ilgili bilinen bir sorundur.

### <a name="workaround"></a>Geçici çözüm

- Yükü birden çok VM arasında yayın.
- Aynı VM 'de, **nosharesock** seçeneğiyle birden çok bağlama noktası kullanın ve yükü bu bağlama noktalarına yayın.
- Linux 'ta, her **fsync** çağrısında SMB temizlemeyi zormaktan kaçınmak için **nostrictsync** seçeneğiyle bağlamayı deneyin. Azure dosyaları için bu seçenek veri tutarlılığını engellemez, ancak dizin listelemesi (**ls-l** komutu) üzerinde eski dosya meta verileri oluşmasına neden olabilir. Dosya meta verilerinin (**stat** komutu) doğrudan sorgulanması en güncel dosya meta verilerini döndürür.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Kapsamlı açık/kapalı işlemleri içeren meta veriler ağır iş yükleri için yüksek gecikme süreleri.

### <a name="cause"></a>Nedeni

Dizin kiraları için destek eksikliği yok.

### <a name="workaround"></a>Geçici çözüm

- Mümkünse, kısa bir süre içinde aynı dizinde aşırı açma/kapatma tanıtıcısından kaçının.
- Linux VM 'Ler için, bir bağlama seçeneği olarak **actimeo = \<sec> ** belirterek Dizin girişi önbellek zaman aşımını artırın. Bu, varsayılan olarak bir saniyedir ve üç ya da beş gibi daha büyük bir değer yardımcı olabilir.
- Linux sanal makineleri için, çekirdeği 4,20 veya daha yüksek bir sürüme yükseltin.

## <a name="low-iops-on-centosrhel"></a>CentOS/RHEL üzerinde düşük ıOPS

### <a name="cause"></a>Nedeni

Büyük GÇ derinliği, CentOS/RHEL üzerinde desteklenmez.

### <a name="workaround"></a>Geçici çözüm

- CentOS 8/RHEL 8 ' e yükseltin.
- Ubuntu olarak değiştirin.

## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Linux 'ta Azure dosyalarından yavaş dosya kopyalama

Azure dosyaları 'na ve Azure dosyalarına yavaş dosya kopyalama ile karşılaşıyorsanız, Linux sorun giderme kılavuzu 'ndaki [Linux 'ta bulunan ve Azure dosyalarına yavaş dosya](storage-troubleshoot-linux-file-connection-problems.md#slow-file-copying-to-and-from-azure-files-in-linux) kopyalama bölümüne göz atın.

## <a name="jitterysaw-tooth-pattern-for-iops"></a>IOPS için jtery/testere-tooth deseninin

### <a name="cause"></a>Nedeni

İstemci uygulaması, temel ıOPS 'yi sürekli olarak aşıyor. Şu anda, istek yükünün hizmet tarafı yumuşatması yoktur, bu nedenle istemci temel ıOPS 'yi aşarsa hizmet tarafından kısıtlanacaktır. Bu kısıtlama, istemcinin bir jtery/testere-tooth ıOPS düzeniyle karşılaşmasına neden olabilir. Bu durumda, istemci tarafından elde edilen ortalama ıOPS, taban ıOPS değerinden daha düşük olabilir.

### <a name="workaround"></a>Geçici çözüm

- Paylaşımın kısıtlanmaması için istemci uygulamasından gelen istek yükünü azaltın.
- Paylaşımın kısıtlanmaması için paylaşımın kotasını artırın.

## <a name="excessive-directoryopendirectoryclose-calls"></a>Aşırı sayıda DirectoryOpen/DirectoryClose çağrısı

### <a name="cause"></a>Nedeni

DirectoryOpen/DirectoryClose çağrılarının sayısı en üst API çağrılarından ise ve istemcinin bu çok sayıda çağrı yapmasına gerek duymuyorsanız, Azure istemci VM 'de yüklü virüsten koruma sorunu olabilir.

### <a name="workaround"></a>Geçici çözüm

- Bu sorunla ilgili bir düzelme, [Windows Için Nisan platformu güncelleştirmesinde](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform)sunulmaktadır.

## <a name="file-creation-is-slower-than-expected"></a>Dosya oluşturma beklenenden yavaş

### <a name="cause"></a>Nedeni

Çok sayıda dosya oluşturmaya bağlı olan iş yükleri, Premium dosya paylaşımlarının ve standart dosya paylaşımlarının performansı arasında önemli bir farklılık görmeyecektir.

### <a name="workaround"></a>Geçici çözüm

- Yok.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Windows 8.1 veya Server 2012 R2 'den yavaş performans

### <a name="cause"></a>Nedeni

GÇ yoğun iş yükleri için Azure dosyalarına erişirken beklenen gecikme süresinden daha yüksek.

### <a name="workaround"></a>Geçici çözüm

- Kullanılabilir [düzeltmeyi](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1)yükler.

## <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>Dosya paylaşma kısıtlandığında uyarı oluşturma

1. **Azure Portal** **depolama hesabınıza** gidin.
2. Izleme bölümünde **Uyarılar** ' a ve ardından **+ Yeni uyarı kuralı**' na tıklayın.
3. **Kaynağı Düzenle**' ye tıklayın, depolama hesabı için **dosya kaynağı türünü** seçin ve **bitti**' ye tıklayın. Örneğin, depolama hesabı adı contoso ise, contoso/dosya kaynağını seçin.
4. Koşul eklemek için **Koşul Seç** ' e tıklayın.
5. Depolama hesabı için desteklenen sinyallerin bir listesini göreceksiniz, **işlem** ölçümünü seçin.
6. **Sinyal mantığını Yapılandır** dikey penceresinde **Boyut adı** açılan düğmesine tıklayın ve **yanıt türü**' nü seçin.
7. **Boyut değerleri** açılan listesi ' ne tıklayın ve **başarılı olarak daraltma** (SMB Için) veya **CLIENTKıSıTLAR lingerror** (REST için) öğesini seçin.

  > [!NOTE]
  > Başarılı bir kısıtlama veya Clientazaltıngerror boyut değeri listelenmiyorsa bu, kaynağın kısıtlanmadığını gösterir. Boyut değerini eklemek için **boyut değerleri** açılan listesi ' nin yanına **özel değer Ekle** ' ye tıklayın, başarılı ' i **daraltma** veya **clientkısıtınror**yazın, **Tamam** ' a tıklayın ve ardından #7 adımını tekrarlayın.

8. **Boyut adı** açılan düğmesine tıklayın ve **dosya paylaşma**' yı seçin.
9. **Boyut değerleri** açılan düğmesine tıklayın ve uyarı vermek istediğiniz dosya paylaşımları seçin.

  > [!NOTE]
  > Dosya paylaşımında standart bir dosya paylaşımınız varsa, **tüm geçerli ve gelecekteki değerleri**seçin. Standart dosya paylaşımları için paylaşım başına ölçümler kullanılamadığından boyut değerleri açılır listesi dosya paylaşımlarını listelemiyor. Depolama hesabı içindeki herhangi bir dosya paylaşımı kısıtlanırsa ve uyarı hangi dosya paylaşımının kısıtlanmadığını tanımlamıyorsa standart dosya paylaşımları için azaltma uyarıları tetiklenecektir. Standart dosya paylaşımları için paylaşım başına ölçümler kullanılamadığından, her depolama hesabı için bir dosya paylaşımının olması önerilir.

10. **Uyarı parametrelerini** tanımlayın (eşik değeri, işleç, toplama ayrıntı düzeyi ve değerlendirme sıklığı) ve **bitti**' ye tıklayın.

  > [!TIP]
  > Statik bir eşik kullanıyorsanız, ölçüm grafiği dosya paylaşımının kısıtlandığı durumlarda makul bir eşik değeri belirlemesine yardımcı olabilir. Dinamik bir eşik kullanıyorsanız, ölçüm grafiği, hesaplanan eşikleri son verilere göre görüntüler.

11. Var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir **eylem grubu** (e-posta, SMS, vb.) eklemek için **Eylem grubunu Seç** ' e tıklayın.
12. Uyarı **kuralı adı**, **Açıklama** ve **önem derecesi**gibi **uyarı ayrıntılarını** girin.
13. Uyarı oluşturmak için **Uyarı kuralı oluştur** ' a tıklayın.

Azure Izleyici 'de uyarıları yapılandırma hakkında daha fazla bilgi edinmek için bkz. [Microsoft Azure uyarılara genel bakış]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="how-to-create-alerts-if-a-premium-file-share-is-trending-towards-being-throttled"></a>Premium dosya paylaşımının kısıtlanıyor olması durumunda uyarı oluşturma

1. **Azure Portal** **depolama hesabınıza** gidin.
2. Izleme bölümünde **Uyarılar** ' a ve ardından **+ Yeni uyarı kuralı**' na tıklayın.
3. **Kaynağı Düzenle**' ye tıklayın, depolama hesabı için **dosya kaynağı türünü** seçin ve **bitti**' ye tıklayın. Örneğin, depolama hesabı adı contoso ise, contoso/dosya kaynağını seçin.
4. Koşul eklemek için **Koşul Seç** ' e tıklayın.
5. Depolama hesabı için desteklenen sinyallerin bir listesini göreceksiniz, **Çıkış** ölçümünü seçin.

  > [!NOTE]
  > Giriş, çıkış veya Işlemler ayarladığınız eşik miktarını aştığında uyarılmak için 3 ayrı uyarı oluşturmanız gerekir. Bunun nedeni, uyarının yalnızca tüm koşulların karşılandığı zaman tetiklenir. Bu nedenle tüm koşulları tek bir uyarıya yerleştirirseniz, giriş, çıkış ve Işlem eşik miktarları aşıldıysa yalnızca uyarı alırsınız.

6. Aşağı kaydırın. **Boyut adı** açılan düğmesine tıklayın ve **dosya paylaşma**' yı seçin.
7. **Boyut değerleri** açılan düğmesine tıklayın ve uyarı vermek istediğiniz dosya paylaşımları seçin.
8. **Uyarı parametrelerini** tanımlayın (eşik değeri, işleç, toplama ayrıntı düzeyi ve değerlendirme sıklığı) ve **bitti**' ye tıklayın.

  > [!NOTE]
  > Çıkış, giriş ve Işlem ölçümleri dakika başına yapılır, ancak saniye başına çıkış, giriş ve ıOPS sağlanmış olmasına rağmen. (toplama ayrıntı düzeyi hakkında konuşur-dakikada > = daha gürültülü, fark olanı seçin) Bu nedenle, örneğin, sağlanan çıkış, 90 MIB/saniye ise ve eşiğin sağlanan çıkış için %80 olmasını istiyorsanız, aşağıdaki uyarı parametrelerini seçmelisiniz: **eşik değeri**için 75497472, **işleç**için büyük veya eşittir, **toplama türü**için Ortalama. Uyarının ne kadar gürültülü olmasını istediğinize bağlı olarak, toplama ayrıntı düzeyi ve değerlendirme sıklığı için seçilecek değerleri seçebilirsiniz. Örneğin, uyarımın bir saatin zaman diliminde ortalama giriş bölümüne bakması ve uyarı Kuralımın her saat içinde çalıştırılmasını istiyorum ise, **toplama ayrıntı düzeyi** için 1 saat ve **değerlendirme sıklığı**1 saat ' i seçiyorum.

9. Var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir **eylem grubu** (e-posta, SMS, vb.) eklemek için **Eylem grubunu Seç** ' e tıklayın.
10. Uyarı **kuralı adı**, **Açıklama** ve **önem derecesi**gibi **uyarı ayrıntılarını** girin.
11. Uyarı oluşturmak için **Uyarı kuralı oluştur** ' a tıklayın.

  > [!NOTE]
  > Premium dosya paylaşımınızın, sağlanan giriş nedeniyle kısıtlanmak üzere yakın olması durumunda bildirim almak için, lütfen 5. adım ' da olduğu gibi, **Giriş ölçümünü seçin** .

  > [!NOTE]
  > Premium dosya paylaşımınızın, sağlanan ıOPS nedeniyle kısıtlanmak üzere yakın olması durumunda, bazı değişiklikler yapmanız gerekir. 5. adımda, bunun yerine **işlem** ölçümünü seçin. Ayrıca, 10. adım için, **toplama türü** için tek seçenek Toplam ' dur. Bu nedenle, eşik değeri seçtiğiniz toplama ayrıntı düzeyine bağlı olacaktır. Örneğin, eşiğin sağlanan temel ıOPS 'nin %80 ' i olmasını istediyseniz ve **toplama ayrıntı düzeyi**için 1 saat seçtiniz, **eşik DEĞERI** taban çizgisi IOPS (bayt cinsinden) x 0,8 x 3600 olacaktır. Bu değişikliklerin yanı sıra, yukarıda listelenen adımların aynısını izleyin. 

Azure Izleyici 'de uyarıları yapılandırma hakkında daha fazla bilgi edinmek için bkz. [Microsoft Azure uyarılara genel bakış]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="see-also"></a>Ayrıca bkz.
* [Windows 'da Azure dosyaları sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
* [Linux 'ta Azure dosyaları sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)
* [Azure Dosyalar hakkında sık sorulan sorular (SSS)](storage-files-faq.md)
