---
title: Azure dosya paylaşımları performans sorunlarını giderme kılavuzu
description: Azure dosya paylaşımlarında bilinen performans sorunlarını giderin. Bu sorunlarla karşılaşıldığında olası nedenleri ve ilgili geçici çözümleri bulun.
author: gunjanj
ms.service: storage
ms.topic: troubleshooting
ms.date: 09/15/2020
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: 3e6490babb5a4e68c1ecd931251ea4eb99d6c3f5
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2020
ms.locfileid: "94630150"
---
# <a name="troubleshoot-azure-file-shares-performance-issues"></a>Azure dosya paylaşımları performans sorunlarını giderme

Bu makalede, Azure dosya paylaşımlarıyla ilgili bazı yaygın sorunlar listelenmektedir. Bu sorunlarla karşılaştığınızda olası nedenleri ve geçici çözümleri sağlar.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Yüksek gecikme süresi, düşük aktarım hızı ve genel performans sorunları

### <a name="cause-1-share-was-throttled"></a>Neden 1: paylaşma daraltıldı

Bir dosya paylaşımının g/ç işlem/saniye (ıOPS), giriş veya çıkış limitlerine ulaşıldığında istekler azaltılır. Standart ve Premium dosya paylaşımlarının sınırlarını anlamak için bkz. [dosya paylaşımı ve dosya ölçeği hedefleri](./storage-files-scale-targets.md#file-share-and-file-scale-targets).

Paylaşımınızın kısıtlanıp kısıtlanmadığını doğrulamak için portalda Azure ölçümlerine erişebilir ve bunları kullanabilirsiniz.

1. Azure portal depolama hesabınıza gidin.

1. Sol bölmede, **izleme** altında **ölçümler** ' i seçin.

1. Depolama hesabı kapsamınız için ölçüm ad alanı olarak **Dosya** ' yı seçin.

1. Ölçüm olarak **işlemler** ' i seçin.

1. **Yanıt türü** için bir filtre ekleyin ve ardından herhangi bir istekte aşağıdaki yanıt kodlarından birine sahip olup olmadığını kontrol edin:
   * **Başarılı bir kısıtlama** : sunucu ileti bloğu (SMB) için
   * **Clientkısıtlar Lingerror** : Rest için

   !["Yanıt türü" özellik filtresini gösteren Premium dosya paylaşımları için ölçüm seçeneklerinin ekran görüntüsü.](media/storage-troubleshooting-premium-fileshares/metrics.png)

   > [!NOTE]
   > Uyarı almak için, bu makalenin ilerleyen kısımlarında yer alan ["dosya paylaşma kısıtlandığında uyarı oluşturma"](#how-to-create-an-alert-if-a-file-share-is-throttled) bölümüne bakın.

### <a name="solution"></a>Çözüm

- Standart bir dosya paylaşımı kullanıyorsanız, depolama hesabınızda [büyük dosya paylaşımlarını](./storage-files-how-to-create-large-file-share.md?tabs=azure-portal) etkinleştirin. Büyük dosya paylaşımları, paylaşım başına 10.000 ıOPS 'ye kadar destekler.
- Premium dosya paylaşma kullanıyorsanız, ıOPS sınırını artırmak için sağlanan dosya paylaşımının boyutunu artırın. Daha fazla bilgi edinmek için, [Azure dosyaları Planlama Kılavuzu](./storage-files-planning.md#understanding-provisioning-for-premium-file-shares)'ndaki "Premium dosya paylaşımları Için sağlamayı anlama" bölümüne bakın.

### <a name="cause-2-metadata-or-namespace-heavy-workload"></a>Neden 2: meta veri veya ad alanı ağır iş yükü

İsteklerinizin büyük bölümü meta veri merkezli ise (CreateFile, OpenFile, CloseFile, QueryInfo veya querydirectory gibi), gecikme okuma/yazma işlemlerinden daha kötüleşmelidir.

İsteklerinizin çoğunun meta veri merkezli olup olmadığını öğrenmek için, 1. neden ' de özetlenen 1-4 adımlarını izleyerek başlayın. 5. adım için, **yanıt türü** için bir filtre eklemek yerine, **API adı** için bir özellik filtresi ekleyin.

!["API Name" özellik filtresini gösteren Premium dosya paylaşımları için ölçüm seçeneklerinin ekran görüntüsü.](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>Geçici çözüm

- Meta veri işlemlerinin sayısını azaltmak için uygulamanın değiştirilip değiştirilemeyeceğini denetleyin.
- Dosya paylaşımında bir sanal sabit disk (VHD) ekleyin ve VHD 'yi, verilere karşı dosya işlemleri gerçekleştirmek için istemciden SMB 'ye bağlayın. Bu yaklaşım tek yazıcı ve birden çok okuyucular senaryosu için geçerlidir ve meta veri işlemlerinin yerel olmasına olanak sağlar. Kurulum, yerel olarak doğrudan bağlı depolamayla benzer bir performans sağlar.

### <a name="cause-3-single-threaded-application"></a>Neden 3: tek iş parçacıklı uygulama

Kullandığınız uygulama tek iş parçacıklı ise, bu kurulum, sağlanan paylaşma boyutunuza bağlı olarak, olası en yüksek aktarım hızına göre önemli ölçüde düşük ıOPS hızına neden olabilir.

### <a name="solution"></a>Çözüm

- İş parçacığı sayısını artırarak uygulama paralelliğini artırın.
- Paralellik mümkün olduğunda uygulamalara geçiş yapın. Örneğin, kopyalama işlemleri için Windows istemcilerinden AzCopy veya RoboCopy veya Linux istemcilerinden **paralel** komut kullanabilirsiniz.

## <a name="very-high-latency-for-requests"></a>İstekler için çok yüksek gecikme süresi

### <a name="cause"></a>Nedeni

İstemci sanal makinesi (VM) dosya paylaşımından farklı bir bölgede bulunabilir.

### <a name="solution"></a>Çözüm

- Uygulamayı dosya paylaşımıyla aynı bölgede bulunan bir VM 'den çalıştırın.

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>İstemci ağ tarafından desteklenen maksimum üretilen iş elde edemedi

### <a name="cause"></a>Nedeni
Olası bir neden, çok kanallı SMB desteğinin olmamasından kaynaklanır. Şu anda Azure dosyaları yalnızca tek kanalı destekler, bu nedenle istemci sanal makineden sunucuya yalnızca bir bağlantı vardır. Bu tek bağlantı, istemci sanal makinesinde tek bir çekirdeğe gereğinden, bu nedenle bir VM 'den en fazla üretilen iş ulaşılabilir, tek bir çekirdekle bağlanır.

### <a name="workaround"></a>Geçici çözüm

- Daha büyük bir çekirdekli VM 'nin alınması, aktarım hızını artırmaya yardımcı olur.
- İstemci uygulamasını birden çok VM 'den çalıştırmak, aktarım hızını artırır.
- Mümkün olduğunda REST API 'Leri kullanın.

## <a name="throughput-on-linux-clients-is-significantly-lower-than-that-of-windows-clients"></a>Linux istemcilerinde üretilen iş, Windows istemcilerinden çok önemli ölçüde düşüktür

### <a name="cause"></a>Nedeni

Bu, Linux üzerinde SMB istemcisinin uygulanmasıyla ilgili bilinen bir sorundur.

### <a name="workaround"></a>Geçici çözüm

- Yükü birden çok VM arasında yayın.
- Aynı VM 'de, bir **nosharesock** seçeneğiyle birden çok bağlama noktası kullanın ve yükü bu bağlama noktalarında yayın.
- Linux 'ta, her **fsync** ÇAĞRıSıNDA bir SMB temizlemeyi zormaktan kaçınmak için bir **nostrictsync** seçeneği ile bağlama yapmayı deneyin. Azure dosyaları için bu seçenek veri tutarlılığını engellemez, ancak dizin listelerinde eski dosya meta verileri ( **ls-l** komutu) ile sonuçlanabilir. **Stat** komutunu kullanarak dosya meta verilerinin doğrudan sorgulanması en güncel dosya meta verilerini döndürür.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Kapsamlı açık/kapalı işlemleri içeren yoğun iş yükleri için yüksek gecikme süreleri

### <a name="cause"></a>Nedeni

Dizin kiraları için destek eksikliği yok.

### <a name="workaround"></a>Geçici çözüm

- Mümkünse, kısa bir süre içinde aynı dizinde aşırı bir açma/kapatma tutamacı kullanmaktan kaçının.
- Linux VM 'Ler için, bir bağlama seçeneği olarak **actimeo = \<sec>** belirterek Dizin girişi önbellek zaman aşımını artırın. Varsayılan olarak, zaman aşımı 1 saniyedir, bu nedenle 3 veya 5 saniye gibi daha büyük bir değer yardımcı olabilirler.
- CentOS Linux veya Red Hat Enterprise Linux (RHEL) VM 'Leri için, sistemi CentOS Linux 8,2 veya RHEL 8,2 ' ye yükseltin. Diğer Linux VM 'Ler için, çekirdeği 5,0 veya sonraki bir sürüme yükseltin.

## <a name="low-iops-on-centos-linux-or-rhel"></a>CentOS Linux veya RHEL üzerinde düşük ıOPS

### <a name="cause"></a>Nedeni

CentOS Linux veya RHEL üzerinde 1 ' den büyük bir g/ç derinliği desteklenmez.

### <a name="workaround"></a>Geçici çözüm

- CentOS Linux 8 veya RHEL 8 ' e yükseltin.
- Ubuntu olarak değiştirin.

## <a name="slow-file-copying-to-and-from-azure-file-shares-in-linux"></a>Linux 'ta Azure dosya paylaşımlarına yavaş dosya kopyalama

Yavaş dosya kopyalama ile karşılaşıyorsanız, [Linux sorun giderme kılavuzu](storage-troubleshoot-linux-file-connection-problems.md#slow-file-copying-to-and-from-azure-files-in-linux)'Ndaki "Linux 'ta bulunan ve Azure dosya paylaşımlarına yavaş dosya kopyalama" bölümüne bakın.

## <a name="jittery-or-sawtooth-pattern-for-iops"></a>IOPS için jtery veya testere dişi deseninin

### <a name="cause"></a>Nedeni

İstemci uygulaması, temel ıOPS 'yi sürekli olarak aşıyor. Şu anda, istek yükünün hizmet tarafı yumuşatması yoktur. İstemci, temel ıOPS 'yi aşarsa, hizmet tarafından kısıtlanacaktır. Azaltma, istemcinin bir jaray veya testere dişi IOPS düzeniyle karşılaşmasına yol açabilir. Bu durumda, istemci tarafından elde edilen ortalama ıOPS, taban ıOPS değerinden daha düşük olabilir.

### <a name="workaround"></a>Geçici çözüm
- Paylaşımın kısıtlanmaması için istemci uygulamasından gelen istek yükünü azaltın.
- Paylaşımın kısıtmaması için paylaşımın kotasını artırın.

## <a name="excessive-directoryopendirectoryclose-calls"></a>Aşırı sayıda DirectoryOpen/DirectoryClose çağrısı

### <a name="cause"></a>Nedeni

**Directoryopen/DirectoryClose** çağrılarının sayısı en üst API çağrılarından kaynaklanıyorsa ve istemcinin bu çok sayıda çağrı yapmasını istemiyorsanız, sorun Azure istemci sanal makinesinde yüklü olan virüsten koruma yazılımından kaynaklanıyor olabilir.

### <a name="workaround"></a>Geçici çözüm

- Bu sorunla ilgili bir düzelme, [Windows Için Nisan platformu güncelleştirmesinde](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform)sunulmaktadır.

## <a name="file-creation-is-slower-than-expected"></a>Dosya oluşturma beklenenden yavaş

### <a name="cause"></a>Nedeni

Çok sayıda dosya oluşturmayı kullanan iş yükleri, Premium dosya paylaşımları ve standart dosya paylaşımları arasındaki performans üzerinde önemli bir farklılık görmez.

### <a name="workaround"></a>Geçici çözüm

- Yok.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Windows 8.1 veya Server 2012 R2 'den yavaş performans

### <a name="cause"></a>Nedeni

G/ç yoğunluklu iş yükleri için Azure dosya paylaşımlarına erişirken beklenen gecikme süresinden daha yüksek.

### <a name="workaround"></a>Geçici çözüm

- Kullanılabilir [düzeltmeyi](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1)yükler.

## <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>Dosya paylaşma kısıtlandığında uyarı oluşturma

1. Azure portal depolama hesabınıza gidin.
1. **İzleme** bölümünde **Uyarılar** ' ı seçin ve ardından **Yeni uyarı kuralı** ' nı seçin.
1. **Kaynağı Düzenle** ' yi seçin, depolama hesabı için **dosya kaynağı türünü** seçin ve **bitti** ' yi seçin. Örneğin, depolama hesabı adı *contoso* ise, contoso/dosya kaynağını seçin.
1. Koşul eklemek için **Koşul Seç** ' i seçin.
1. Depolama hesabı için desteklenen sinyaller listesinde, **işlem** ölçümünü seçin.
1. **Sinyal mantığını Yapılandır** bölmesindeki **Boyut adı** açılır listesinde **yanıt türü** ' nü seçin.
1. **Boyut değerleri** aşağı açılan listesinde, başarılı olarak daraltma (SMB için) veya **Clientazaltlgerror** (REST için) öğesini seçin. **SuccessWithThrottling**

   > [!NOTE]
   > Ne başarılı bir şekilde **daraltma** ne de **Clientazaltıgerror** değeri listelenmiyorsa bu, kaynağın kısıtlanmadığını gösterir. Boyut değerini eklemek için, **boyut değerleri** aşağı açılan listesinin yanındaki **özel değer Ekle** ' yi seçin, **başarılı bir kısıtlama** veya **Clientkısıtıngerror** yazın, **Tamam** ' ı seçin ve 7. adımı yineleyin.

1. **Boyut adı** açılır listesinde **dosya paylaşma** ' yı seçin.
1. **Boyut değerleri** aşağı açılan listesinde, uyarmak istediğiniz dosya paylaşımını veya paylaşımlarını seçin.

   > [!NOTE]
   > Dosya paylaşımında standart bir dosya paylaşımınız varsa, **tüm geçerli ve gelecekteki değerleri** seçin. Boyut değerleri açılan listesi, standart dosya paylaşımları için paylaşım başına ölçümler kullanılamadığından dosya paylaşımlarını listelemiyor. Depolama hesabındaki herhangi bir dosya paylaşımı kısıtlanırsa ve uyarı hangi dosya paylaşımının kısıtlanmadığını tanımmıyorsa standart dosya paylaşımları için azaltma uyarıları tetiklenir. Standart dosya paylaşımları için paylaşım başına ölçümler kullanılamadığından, depolama hesabı başına bir dosya paylaşımı kullanmanızı öneririz.

1. **Eşik değeri** , **işleci** , **toplama ayrıntı düzeyi** ve **değerlendirme sıklığını** girerek uyarı parametrelerini tanımlayın ve **bitti** ' yi seçin.

    > [!TIP]
    > Statik eşik kullanıyorsanız, ölçüm grafiği dosya paylaşımının kısıtlandığı durumlarda makul bir eşik değeri belirlemenize yardımcı olabilir. Dinamik bir eşik kullanıyorsanız, ölçüm grafiği, hesaplanan eşikleri son verilere göre görüntüler.

1. **Eylem grubunu Seç** ' i seçin ve ardından var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir eylem grubu (örneğin, e-posta veya SMS) ekleyin.
1. **Uyarı kuralı adı** , **açıklaması** ve **önem derecesi** gibi uyarı ayrıntılarını girin.
1. Uyarı oluşturmak için **Uyarı kuralı oluştur** ' u seçin.

Azure Izleyici 'de uyarıları yapılandırma hakkında daha fazla bilgi edinmek için bkz. [Microsoft Azure uyarılara genel bakış]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="how-to-create-alerts-if-a-premium-file-share-is-trending-toward-being-throttled"></a>Premium dosya paylaşımının kısıtlanıyor olması durumunda uyarı oluşturma

1. Azure portal depolama hesabınıza gidin.
1. **İzleme** bölümünde **Uyarılar** ' ı seçin ve ardından **Yeni uyarı kuralı** ' nı seçin.
1. **Kaynağı Düzenle** ' yi seçin, depolama hesabı için **dosya kaynağı türünü** seçin ve **bitti** ' yi seçin. Örneğin, depolama hesabı adı *contoso* ise, contoso/dosya kaynağını seçin.
1. Koşul eklemek için **Koşul Seç** ' i seçin.
1. Depolama hesabı için desteklenen sinyaller listesinde **Çıkış** ölçümünü seçin.

   > [!NOTE]
   > Giriş, çıkış veya işlem değerleri ayarladığınız eşikleri aştığında uyarılmak için üç ayrı uyarı oluşturmanız gerekir. Bunun nedeni, bir uyarının yalnızca tüm koşulların karşılanması durumunda tetiklenmesi. Örneğin, tüm koşulları tek bir uyarıya yerleştirirseniz, yalnızca giriş, çıkış ve işlemler eşik tutarlarını aşarsa uyarılırsınız.

1. Aşağı kaydırın. **Boyut adı** açılır listesinde **dosya paylaşma** ' yı seçin.
1. **Boyut değerleri** aşağı açılan listesinde, uyarmak istediğiniz dosya paylaşımını veya paylaşımlarını seçin.
1. **Operatör** , **eşik değeri** , **toplama ayrıntı düzeyi** ve **değerlendirme sıklığı** aşağı açılan listelerinin değerlerini seçerek uyarı parametrelerini tanımlayın ve ardından **bitti** ' yi seçin.

   Çıkış, giriş ve işlem ölçümleri, saniye başına çıkış, giriş ve g/ç sağlanmış olmasına rağmen dakikada ifade edilir. Bu nedenle, örneğin, sağlanan çıkış, &nbsp; saniyede 90/iki bayt (MIB/sn) ise ve eşiğinin sağlanan çıkış için yüzde 80 olmasını istiyorsanız &nbsp; aşağıdaki uyarı parametrelerini seçin: 
   - **Eşik değeri** için: *75497472* 
   - **Operatör** için: *büyüktür veya eşittir*
   - **Toplama türü** için: *Ortalama*
   
   Uyarının ne kadar gürültülü olmasını istediğinize bağlı olarak, **toplama ayrıntı düzeyi** ve **değerlendirme sıklığı** için değerler de seçebilirsiniz. Örneğin, uyarının 1 saat boyunca ortalama giriş bölümüne bakması ve uyarı kuralınızın her saat içinde çalıştırılmasını istiyorsanız aşağıdakileri seçin:
   - **Toplama ayrıntı düzeyi** için: *1 saat*
   - **Değerlendirme sıklığı** : *1 saat*

1. **Eylem grubunu Seç** ' i seçin ve ardından var olan bir eylem grubunu seçerek veya yenisini oluşturarak uyarıya bir eylem grubu (örneğin, e-posta veya SMS) ekleyin.
1. **Uyarı kuralı adı** , **açıklaması** ve **önem derecesi** gibi uyarı ayrıntılarını girin.
1. Uyarı oluşturmak için **Uyarı kuralı oluştur** ' u seçin.

    > [!NOTE]
    > - Premium dosya paylaşımınızın, sağlanan giriş nedeniyle kısıtlanmak üzere *kapandığı* hakkında bildirim almak için, önceki yönergeleri izleyin, ancak aşağıdaki değişikliği yapın:
    >    - 5. adımda **Çıkış** yerine giriş **ölçümünü seçin** .
    >
    > - *Sağlanan IOPS nedeniyle* Premium dosya paylaşımınızın kısıtlanmak üzere kapandığı hakkında bildirim almak için, önceki yönergeleri izleyin, ancak aşağıdaki değişiklikleri yapın:
    >    - 5. adımda **Çıkış** yerine **işlem** ölçümünü seçin.
    >    - 10. adımda, **toplama türü** için tek seçenek *Toplam* ' dur. Bu nedenle, eşik değeri seçtiğiniz toplama ayrıntı düzeyine bağlıdır. Örneğin, eşiğin &nbsp; sağlanan temel IOPS 'nin yüzde 80 olmasını istiyorsanız ve **toplama ayrıntı düzeyi** için *1 saat* seçerseniz, **eşik değeri** taban çizgisi IOPS (bayt cinsinden) &times; &nbsp; 0,8 &times; &nbsp; 3600 olacaktır. 

Azure Izleyici 'de uyarıları yapılandırma hakkında daha fazla bilgi edinmek için bkz. [Microsoft Azure uyarılara genel bakış]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="see-also"></a>Ayrıca bkz.
- [Windows 'da Azure dosyaları sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)  
- [Linux 'ta Azure dosyaları sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)  
- [Azure Dosyalar SSS](storage-files-faq.md)
