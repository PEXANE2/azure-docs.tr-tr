---
title: Tanılama ve İleti Çözümleyicisi ile veri işlemlerini sorun giderme
titleSuffix: Azure Storage
description: Azure Depolama Analizi, AzCopy ve Microsoft İleti Çözümleyicisi ile uçtan uca sorun giderme yi gösteren bir öğretici
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 12/20/2019
ms.author: normesta
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 69983502fb7d099f474fb1c4c084f5d381a173e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76314768"
---
# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Azure Depolama ölçümlerini ve günlüğe kaydetmeyi, AzCopy’yi ve İleti Çözümleyicisi’ni kullanarak uçtan uca sorun giderme

[!INCLUDE [storage-selector-portal-e2e-troubleshooting](../../../includes/storage-selector-portal-e2e-troubleshooting.md)]

Tanılama ve sorun giderme, Microsoft Azure Depolama ile istemci uygulamaları oluşturmak ve desteklemek için önemli bir beceridir. Azure uygulamasının dağıtılmış yapısı nedeniyle, tanılama ve sorun giderme hataları ve performans sorunları geleneksel ortamlardan daha karmaşık olabilir.

Bu öğreticide, performansı etkileyebilecek belirli hataları nasıl tanımlayabileceğimizi ve istemci uygulamasını optimize etmek için Microsoft ve Azure Depolama tarafından sağlanan araçları kullanarak bu hataları nasıl giderebileceğimizi gösteriyoruz.

Bu öğretici, uçtan uca sorun giderme senaryosunun uygulamalı olarak araştırılmasını sağlar. Azure depolama uygulamalarıyla ilgili sorun giderme için ayrıntılı bir kavramsal kılavuz için [Bkz.](storage-monitoring-diagnosing-troubleshooting.md)

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Azure Depolama uygulamalarını sorun giderme araçları

Microsoft Azure Depolama'yı kullanarak istemci uygulamalarının giderilmesi için, bir sorunun ne zaman oluştuğunu ve sorunun nedeninin ne olabileceğini belirlemek için bir araç birleşimini kullanabilirsiniz. Bu araçlar şunları içerir:

* **Azure Depolama Analitiği**. [Azure Depolama Analitiği,](/rest/api/storageservices/Storage-Analytics) Azure Depolama için ölçümler ve günlüğe kaydetme sağlar.

  * **Depolama ölçümleri,** depolama hesabınız için hareket ölçümlerini ve kapasite ölçümlerini izler. Ölçümleri kullanarak, uygulamanızın çeşitli ölçümlere göre nasıl performans gösterdiğini belirleyebilirsiniz. Depolama Analizi tarafından izlenen ölçüm türleri hakkında daha fazla bilgi için [Depolama Analizi Ölçümleri Tablosu Şeması'na](/rest/api/storageservices/Storage-Analytics-Metrics-Table-Schema) bakın.
  * **Depolama günlüğü,** Azure Depolama hizmetlerine gelen her isteği sunucu tarafındaki bir günlükte kaydeder. Günlük, gerçekleştirilen işlem, işlemin durumu ve gecikme bilgileri de dahil olmak üzere her istek için ayrıntılı verileri izler. Depolama [Analitiği](/rest/api/storageservices/Storage-Analytics-Log-Format) tarafından günlüklere yazılan istek ve yanıt verileri hakkında daha fazla bilgi için Depolama Analizi Günlük Biçimi'ne bakın.

* **Azure portalı**. [Azure portalında](https://portal.azure.com)depolama hesabınız için ölçümleri ve günlüğe kaydetmeyi yapılandırabilirsiniz. Ayrıca, uygulamanızın zaman içinde nasıl performans gösterdiğini gösteren grafikleri ve grafikleri görüntüleyebilir ve uygulamanız belirli bir metrik için beklenenden farklı performans gösteriyorsa sizi bilgilendirmek için uyarıları yapılandırabilirsiniz.

    Azure portalında izlemeyi yapılandırma hakkında bilgi almak için [Azure portalındaki bir depolama hesabını izleyin'e](storage-monitor-storage-account.md) bakın.
* **Azcopy**. Azure Depolama için sunucu günlükleri blob olarak depolanır, böylece günlük lekelerini Microsoft İleti Çözümleyicisi kullanarak analiz için yerel bir dizine kopyalamak için AzCopy'yi kullanabilirsiniz. AzCopy hakkında daha fazla bilgi için [AzCopy Command-Line Utility ile veri aktarımı](storage-use-azcopy.md) bakın.
* **Microsoft İleti Çözümleyicisi**. İleti Çözümleyici, günlük dosyalarını tüketen ve günlük verilerini görsel bir biçimde görüntüleyen ve günlük verilerini hataları ve performans sorunlarını çözümlemek için kullanabileceğiniz yararlı kümelere filtrelemeyi, aramayı ve gruplandırmayı kolaylaştıran bir araçtır. İleti Çözümleyicisi hakkında daha fazla bilgi için [Microsoft İleti Çözümleyicisi Çalışma Kılavuzu'na](https://technet.microsoft.com/library/jj649776.aspx) bakın.

## <a name="about-the-sample-scenario"></a>Örnek senaryo hakkında

Bu öğretici için, Azure Depolama ölçümlerinin Azure depolama yı çağıran bir uygulama için düşük yüzde başarı oranını gösterdiği bir senaryoyu inceleriz. Düşük yüzde başarı oranı ölçümü [(Azure portalında](https://portal.azure.com) ve ölçümler tablolarında **Yüzde Başarı** olarak gösterilir) başarılı olan, ancak 299'dan büyük bir HTTP durum kodu döndüren işlemleri izler. Sunucu tarafındaki depolama günlüğü dosyalarında, bu işlemler **ClientOtherErrors'ın**işlem durumuyla kaydedilir. Düşük yüzde başarı ölçümü hakkında daha fazla bilgi için bkz: [Ölçümler düşük YüzdeBaşarı gösterir veya analitik günlük girişleri ClientOtherErrors işlem durumu ile işlemleri var.](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success)

Azure Depolama işlemleri, normal işlevlerinin bir parçası olarak 299'dan büyük HTTP durum kodlarını döndürebilir. Ancak bazı durumlarda bu hatalar, istemci uygulamanızı gelişmiş performans için en iyi duruma getirebileceğini gösterir.

Bu senaryoda, düşük yüzde lik başarı oranını %100'ün altında olarak değerlendireceğiz. Ancak, gereksinimlerinize göre farklı bir metrik düzey seçebilirsiniz. Uygulamanızın sınanışı sırasında, temel performans ölçümleriniz için bir temel tolerans oluşturmanızı öneririz. Örneğin, teste dayalı olarak, uygulamanızın %90 veya %85 tutarlı bir başarı oranına sahip olması gerektiğini belirleyebilirsiniz. Ölçüm verileriniz uygulamanın bu numaradan sapdığını gösteriyorsa, artışa neyin neden olabileceğini araştırabilirsiniz.

Örnek senaryomuz için, yüzde başarı oranı ölçümünün %100'ün altında olduğunu belirledikten sonra, ölçümlerle ilişkili hataları bulmak için günlükleri inceleyeceğiz ve bunları yüzde daha düşük başarı oranına neyin neden olduğunu bulmak için kullanacağız. Özellikle 400 aralığındaki hatalara bakacağız. Daha yakından 404 (Bulunamadı) hataları araştıracağız.

### <a name="some-causes-of-400-range-errors"></a>400 aralıklı hataların bazı nedenleri

Aşağıdaki örnekler, Azure Blob Depolama'ya yönelik istekler için yaklaşık 400 aralıklı hataların bir örneğini ve bunların olası nedenlerini göstermektedir. Bu hatalardan herhangi biri ve 300 aralığındaki hatalar ve 500 aralığındaki hatalar, yüzde düşük bir başarı oranına katkıda bulunabilir.

Aşağıdaki listelerin tamamlanmaktan çok uzak olduğunu unutmayın. Genel Azure Depolama hataları ve depolama hizmetlerinin her birine özgü hatalar hakkında ayrıntılar için MSDN'deki Durum ve [Hata Kodları'na](https://msdn.microsoft.com/library/azure/dd179382.aspx) bakın.

#### <a name="status-code-404-not-found-examples"></a>Durum Kodu 404 (Bulunamadı) Örnekleri

Bir kapsayıcı veya blob karşı okuma işlemi blob veya konteyner bulunamadı çünkü başarısız olduğunda oluşur.

* Bu istekten önce bir kapsayıcı veya blob başka bir istemci tarafından silinmişse oluşur.
* Var olup olmadığını kontrol ettikten sonra kapsayıcı veya blob oluşturan bir API çağrısı kullanıyorsanız oluşur. CreateIfNotExists API'leri, kapsayıcının veya blob'un varlığını kontrol etmek için önce bir HEAD araması yapar; yoksa, 404 hatası döndürülür ve sonra kapsayıcı veya blob yazmak için ikinci bir PUT çağrısı yapılır.

#### <a name="status-code-409-conflict-examples"></a>Durum Kodu 409 (Çakışma) Örnekleri

* Yeni bir kapsayıcı veya blob oluşturmak için bir Create API kullanırsanız, önce varlığı denetlemeden oluşur ve bu ada sahip bir kapsayıcı veya blob zaten var olur.
* Bir kapsayıcı siliniyorsa oluşur ve silme işlemi tamamlanmadan önce aynı ada sahip yeni bir kapsayıcı oluşturmaya çalışırsınız.
* Bir kapsayıcı veya blob üzerinde bir kira belirtirseniz oluşur ve zaten bir kira mevcut.

#### <a name="status-code-412-precondition-failed-examples"></a>Durum Kodu 412 (Ön Koşul Başarısız) Örnekleri

* Koşullu üstbilgi tarafından belirtilen durum karşılanmadığında oluşur.
* Belirtilen kira kimliği kapsayıcı veya blob üzerinde kira kimliği eşleşmez oluşur.

## <a name="generate-log-files-for-analysis"></a>Analiz için günlük dosyaları oluşturma

Bu öğreticide, aşağıdakilerden herhangi biriyle çalışmayı seçebilmekle birlikte, üç farklı günlük dosyası türüyle çalışmak için İleti Çözümleyicisi'ni kullanırız:

* Azure Depolama günlüğünü etkinleştirdiğinizde oluşturulan **sunucu günlüğü.** Sunucu günlüğü, Azure Depolama hizmetlerinden birine karşı çağrılan her işlemle ilgili verileri içerir - blob, kuyruk, tablo ve dosya. Sunucu günlüğü, hangi işlemin çağrıldığını ve hangi durum kodunun döndürüldüğü ve istek ve yanıtla ilgili diğer ayrıntıları gösterir.
* .NET uygulamanızın içinden istemci tarafı oturum açmayı etkinleştirdiğinizde oluşturulan **.NET istemci günlüğü.** İstemci günlüğü, istemcinin isteği nasıl hazırladığı ve yanıtı nasıl aldığı ve işlediği hakkında ayrıntılı bilgiler içerir.
* Azure Depolama'ya yönelik işlemler de dahil olmak üzere HTTP/HTTPS istek ve yanıt verilerinde veri toplayan **HTTP ağ izleme günlüğü.** Bu öğreticide, Ileti Çözümleyicisi aracılığıyla ağ izlemeyi oluşturacağız.

### <a name="configure-server-side-logging-and-metrics"></a>Sunucu tarafı günlüğü ve ölçümlerini yapılandırma

İlk olarak, çözümlemek için hizmet tarafından veri ye sahip olmak için Azure Depolama günlüğe kaydetme ve ölçümleri yapılandırmamız gerekir. Günlüğe kaydetme ve ölçümleri [Azure portalı](https://portal.azure.com)üzerinden PowerShell'i kullanarak veya programlı olarak çeşitli şekillerde yapılandırabilirsiniz. Bkz. [Ölçümleri Etkinleştir](storage-analytics-metrics.md#enable-metrics-using-the-azure-portal) ve günlük işlemlerini ve ölçümleri yapılandırmayla ilgili ayrıntılar için [günlüğe kaydetmeyi etkinleştirin.](storage-analytics-logging.md#enable-storage-logging)

### <a name="configure-net-client-side-logging"></a>Yapıl.NET istemci tarafı günlüğü

Bir .NET uygulaması için istemci tarafı günlüğe kaydetmeyi yapılandırmak için, uygulamanın yapılandırma dosyasında (web.config veya app.config) .NET tanılamasını etkinleştirin. Ayrıntılar için MSDN'de Java için Microsoft Azure Depolama SDK ile [.NET Depolama İstemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn782839.aspx) ve [İstemci tarafı Oturum Açma](https://msdn.microsoft.com/library/azure/dn782844.aspx) ile Istemci tarafı Oturum Açma'ya bakın.

İstemci tarafı günlüğü, istemcinin isteği nasıl hazırladığı ve yanıtı nasıl aldığı ve işlediği hakkında ayrıntılı bilgiler içerir.

Depolama İstemci Kitaplığı, istemci tarafındaki günlük verilerini uygulamanın yapılandırma dosyasında (web.config veya app.config) belirtilen konumda depolar.

### <a name="collect-a-network-trace"></a>Ağ izleme toplama

İstemci uygulamanız çalışırken bir HTTP/HTTPS ağ izleme toplamak için İleti Çözümleyicisi'ni kullanabilirsiniz. İleti Çözümleyicisi arka uçta [Fiddler](https://www.telerik.com/fiddler) kullanır. Ağ izlemeyi toplamadan önce, Fiddler'ı şifrelenmemiş HTTPS trafiğini kaydacak şekilde yapılandırmanızı öneririz:

1. [Fiddler'ı](https://www.telerik.com/download/fiddler)yükleyin.
2. Fiddler'ı başlatın.
3. Araçları Seçin **| Kemancı Seçenekleri**.
4. Seçenekler iletişim kutusunda, https **CONNECTs'leri yakalayın** ve **https trafiğini deşifre** edin, aşağıda gösterildiği gibi seçilir.

![Fiddler Seçeneklerini Yapılandırma](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Öğretici için, önce İleti Çözümleyicisi'nde bir ağ izlemesini toplayın ve kaydedin, ardından izlemeyi ve günlükleri çözümlemek için bir analiz oturumu oluşturun. İleti Çözümleyicisi'nde ağ izleme toplamak için:

1. İleti Çözümleyicisinde **Dosya | Hızlı İzleme | Şifresiz HTTPS**.
2. İz hemen başlayacak. Yalnızca depolama trafiğini izbırakacak şekilde yapılandırabilmemiz için izlemeyi durdurmak için **Durdur'u** seçin.
3. İzleme oturumunu düzenledirmek için **Edit'i** seçin.
4. **Microsoft-Pef-WebProxy** ETW sağlayıcısının sağındaki **Yapıla** bağlantısını seçin.
5. Gelişmiş **Ayarlar** iletişim kutusunda **Sağlayıcı** sekmesini tıklatın.
6. Ana **Bilgisayar Adı Filtresi** alanında, boşluklara ayrılmış depolama uç noktalarınızı belirtin. Örneğin, uç noktalarınızı aşağıdaki gibi belirtebilirsiniz; depolama `storagesample` hesabınızın adına değiştirin:

    `storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net`

7. İletişim kutusundan çıkın ve izlemede yalnızca Azure Depolama ağ trafiğinin izlemede yer alabilmesi için izlemeyi ana bilgisayar adı filtresi yle toplamaya başlamak için **Yeniden Başlat'ı** tıklatın.

> [!NOTE]
> Ağ izlemenizi toplamayı bitirdikten sonra, HTTPS trafiğinin şifresini çözmek için Fiddler'da değiştirmiş olabileceğiniz ayarları geri almanızı öneririz. Fiddler Options iletişim kutusunda, **HTTPS CON'ları Ele Geçir'i** seçin ve HTTPS Trafik onay kutularını **deşifre edin.**

Daha fazla bilgi için Technet'teki [Ağ İzleme Özelliklerini Kullanma'ya](https://technet.microsoft.com/library/jj674819.aspx) bakın.

## <a name="review-metrics-data-in-the-azure-portal"></a>Azure portalında ölçüm verilerini gözden geçirme

Uygulamanız bir süre için çalışmaya başladığında, hizmetinizin nasıl performans gösterdiğini gözlemlemek için [Azure portalında](https://portal.azure.com) görünen metrik grafiklerini inceleyebilirsiniz.

İlk olarak, Azure portalındaki depolama hesabınıza gidin. Varsayılan olarak, hesap bıçağında **Başarı yüzdesi** ölçümüne sahip bir izleme grafiği görüntülenir. Grafiği daha önce farklı ölçümleri görüntülemek üzere değiştirdiyseniz, **Başarı yüzdesi** ölçüsüekleyin.

Artık eklediğiniz diğer ölçümlerle birlikte izleme grafiğinde **Başarı yüzdesini** görürsünüz. Message Analyzer'daki günlükleri analiz ederek bir sonraki araştırmayı yapacağımız senaryoda, yüzde başarı oranı %100'ün biraz altındadır.

Metrik grafikler ekleme ve özelleştirme hakkında daha fazla bilgi için [bkz.](storage-monitor-storage-account.md#customize-metrics-charts)

> [!NOTE]
> Depolama ölçümlerini etkinleştirdikten sonra metrik verilerinizin Azure portalında görünmesi biraz zaman alabilir. Bunun nedeni, geçerli saat geçene kadar önceki saate ait saatlik ölçümlerin Azure portalında görüntülenmemesidir. Ayrıca, dakika ölçümleri şu anda Azure portalında görüntülenmez. Bu nedenle, ölçümleri etkinleştirdiğinizde, ölçümlerverilerini görmek iki saat kadar sürebilir.
>
>

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Sunucu günlüklerini yerel bir dizine kopyalamak için AzCopy'yi kullanın

Azure Depolama sunucu günlüğü verilerini blobs'a yazarken, ölçümler tablolara yazılır. Günlük lekeleri depolama hesabınız için `$logs` tanınmış kapsayıcıda mevcuttur. Günlük lekeleri, araştırmak istediğiniz zaman aralığını kolayca bulabilmeniz için yıl, ay, gün ve saate göre hiyerarşik olarak adlandırılır. Örneğin, `storagesample` hesapta, 01/02/2015 için günlük lekeler için konteyner, 08-9 `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`am, . Bu kapsayıcıdaki tek tek lekeler, `000000.log`'den başlayarak sırayla adlandırılır.

Bu sunucu tarafındaki günlük dosyalarını yerel makinenizde seçtiğiniz bir konuma indirmek için AzCopy komut satırı aracını kullanabilirsiniz. Örneğin, 2 Ocak 2015 tarihinde gerçekleşen blob işlemleri için günlük dosyalarını klasöre `C:\Temp\Logs\Server`indirmek için aşağıdaki komutu kullanabilirsiniz; depolama `<storageaccountname>` hesabınızın adı ile değiştirin:

```azcopy
azcopy copy 'http://<storageaccountname>.blob.core.windows.net/$logs/blob/2015/01/02' 'C:\Temp\Logs\Server'  --recursive
```

AzCopy, [Azure İndirmeler](https://azure.microsoft.com/downloads/) sayfasından indirilebilir. AzCopy'yi kullanma hakkında daha fazla bilgi için, [AzCopy Command-Line Utility ile veri aktarımı'na](storage-use-azcopy.md)bakın.

Sunucu tarafındaki günlükleri indirme hakkında ek bilgi için [bkz.](https://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata)

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Günlük verilerini çözümlemek için Microsoft İleti Çözümleyicisini kullanma

Microsoft İleti Çözümleyicisi, sorun giderme ve tanılama senaryolarında protokol ileti trafiğini, olayları ve diğer sistem veya uygulama iletilerini yakalamak, görüntülemek ve çözümlemek için bir araçtır. İleti Çözümleyicisi ayrıca günlük ve kaydedilmiş izleme dosyalarından veri yüklemenize, toplamanıza ve çözümlemenize de olanak tanır. İleti Çözümleyicisi hakkında daha fazla bilgi için [Microsoft İleti ÇözümleyiciSi Çalışma Kılavuzu'na](https://technet.microsoft.com/library/jj649776.aspx)bakın.

İleti Çözümleyicisi, sunucu, istemci ve ağ günlüklerini çözümlemenize yardımcı olan Azure Depolama varlıkları içerir. Bu bölümde, depolama günlüklerinde yüzde düşük başarı sorununu gidermek için bu araçların nasıl kullanılacağını tartışacağız.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>İleti Çözümleyicisini ve Azure Depolama Varlıklarını İndirin ve yükleyin

1. [İleti Çözümleyicisini](https://www.microsoft.com/download/details.aspx?id=44226) Microsoft İndirme Merkezi'nden indirin ve yükleyiciyi çalıştırın.
2. İleti Çözümleyicisi başlatın.
3. **Araçlar** menüsünden **Varlık Yöneticisi'ni**seçin. Varlık **Yöneticisi** iletişim kutusunda, **İndirilenler'i**seçin ve ardından **Azure Depolama'ya**filtre uygulayın. Aşağıdaki resimde gösterildiği gibi Azure Depolama Varlıkları'nı görürsünüz.
4. Azure Depolama Varlıkları'nı yüklemek için **Görüntülenen Tüm Öğeleri Eşitle'yi** tıklatın. Kullanılabilir varlıklar şunlardır:
   * **Azure Depolama Renk Kuralları:** Azure Depolama renk kuralları, bir izlemede belirli bilgiler içeren iletileri vurgulamak için renk, metin ve yazı tipi stilleri kullanan özel filtreler tanımlamanıza olanak tanır.
   * **Azure Depolama Grafikleri:** Azure Depolama grafikleri, sunucu günlük verilerini grafikleyen önceden tanımlanmış grafiklerdir. Şu anda Azure Depolama grafiklerini kullanmak için sunucu günlüğünü yalnızca Çözümleme İzole'sine yükleyebilirsiniz.
   * **Azure Depolama Ayrıştıcıları:** Azure Depolama ayrışdırıcıları, Çözümleme İzole'sinde görüntülemek için Azure Depolama istemcisini, sunucusunu ve HTTP günlüklerini ayrıştır.
   * **Azure Depolama Filtreleri:** Azure Depolama filtreleri, Verilerinizi Çözümleme İzole'sinde sorgulamak için kullanabileceğiniz önceden tanımlanmış ölçütlerdir.
   * **Azure Depolama Görünümü Düzenleri:** Azure Depolama görünüm düzenleri, Çözümleme İzole'sindeki önceden tanımlanmış sütun düzenleri ve gruplandırmalarıdır.
5. Varlıkları yükledikten sonra İleti Çözümleyicisini yeniden başlatın.

![İleti Çözümleyici Varlık Yöneticisi](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [!NOTE]
> Bu öğreticinin amaçları doğrultusunda gösterilen Tüm Azure Depolama varlıklarını yükleyin.
>
>

### <a name="import-your-log-files-into-message-analyzer"></a>Günlük dosyalarınızı İleti Çözümleyicisine aktarın

Kaydedilen tüm günlük dosyalarınızı (sunucu tarafı, istemci tarafı ve ağ) çözümleme için Microsoft İleti Çözümleyicisi'nde tek bir oturuma aktarabilirsiniz.

1. Microsoft İleti Çözümleyicisi'ndeki **Dosya** menüsünde **Yeni Oturum'u**tıklatın ve ardından **Boş Oturum'u**tıklatın. Yeni **Oturum** iletişim kutusunda, çözümleme oturumunuz için bir ad girin. Oturum **Ayrıntıları** panelinde **Dosyalar** düğmesine tıklayın.
2. İleti Çözümleyicisi tarafından oluşturulan ağ izleme verilerini yüklemek için **Dosya Ekle'ye**tıklayın, .matp dosyanızı web izleme oturumunuzdan kaydettiğiniz konuma göz atın, .matp dosyasını seçin ve **Aç'ı**tıklatın.
3. Sunucu tarafındaki günlük verilerini yüklemek için **Dosya Ekle'ye**tıklayın, sunucu tarafındaki günlüklerinizi indirdiğiniz konuma göz atın, analiz etmek istediğiniz zaman aralığı için günlük dosyalarını seçin ve **Aç'ı**tıklatın. Ardından, **Oturum Ayrıntıları** panelinde, Her sunucu tarafındaki günlük dosyası için Metin **Günlüğü Yapılandırması** açılır bırakmasını **AzureStorageLog** olarak ayarlayın ve Microsoft İleti Çözümleyicisinin günlük dosyasını doğru şekilde ayrışdırabilmesini sağlayın.
4. İstemci tarafı günlük verilerini yüklemek için **Dosya Ekle'ye**tıklayın, istemci tarafı günlüklerinizi kaydettiğiniz konuma göz atın, çözümlemek istediğiniz günlük dosyalarını seçin ve **Aç'ı**tıklatın. Ardından, **Oturum Ayrıntıları** panelinde, Microsoft İleti Çözümleyicisinin günlük dosyasını doğru şekilde ayrışdıradığından emin olmak için, istemci tarafındaki her günlük dosyası için Metin **Günlüğü Yapılandırması** açılır layını **AzureStorageClientDotNetV4** olarak ayarlayın.
5. Günlük verilerini yüklemek ve ayrıştmak için **Yeni Oturum** iletişim kutusunda **Başlat'ı** tıklatın. Günlük verileri İleti Çözümleyici ÇözümleyiciSi Grid'inde görüntülenir.

Aşağıdaki resimde sunucu, istemci ve ağ izleme günlüğü dosyalarıyla yapılandırılan bir örnek oturum gösterilmektedir.

![İleti Çözümleyici Oturumunu Yapılandırma](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

İleti Çözümleyici'nin günlük dosyalarını belleğe yüklediğini unutmayın. Büyük bir günlük veri setiniz varsa, Ileti Çözümleyici'den en iyi performansı elde etmek için verifiltre etmek istersiniz.

İlk olarak, gözden geçirmekle ilgilendiğiniz zaman dilimini belirleyin ve bu zaman dilimini mümkün olduğunca küçük tutun. Çoğu durumda, en fazla dakika veya saat bir süre gözden geçirmek isteyeceksiniz. İhtiyaçlarınızı karşılayabilen en küçük günlük kümesini içeri aktarın.

Hala büyük miktarda günlük veriniz varsa, yüklemeden önce günlük verilerinizi filtrelemek için bir oturum filtresi belirtmek isteyebilirsiniz. Oturum **Filtresi** kutusunda, önceden tanımlanmış bir filtre seçmek için **Kitaplık** düğmesini seçin; örneğin, zaman aralığında filtrelemek için Azure Depolama filtrelerinden **Genel Zaman Filtresi I'i** seçin. Daha sonra, görmek istediğiniz aralığın başlangıç ve bitiş zaman damgasını belirtmek için filtre ölçütlerini edinebilirsiniz. Ayrıca belirli bir durum koduna da filtre uygulayabilirsiniz; örneğin, yalnızca durum kodunun 404 olduğu günlük girişlerini yüklemeyi seçebilirsiniz.

Günlük verilerini Microsoft İleti Çözümleyicisine aktarma hakkında daha fazla bilgi için TechNet'ten [İleti Verilerini Alma'ya](https://technet.microsoft.com/library/dn772437.aspx) bakın.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Günlük dosyası verilerini ilişkilendirmek için istemci istek kimliğini kullanma

Azure Depolama İstemci Kitaplığı, her istek için otomatik olarak benzersiz bir istemci isteği kimliği oluşturur. Bu değer istemci günlüğüne, sunucu günlüğüne ve ağ izlemesine yazılır, böylece Ileti Çözümleyicisi içindeki üç günlükteki verileri ilişkilendirmek için kullanabilirsiniz. İstemci istek kimliği hakkında ek bilgi için [İstemci isteği kimliğine](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) bakın.

Aşağıdaki bölümlerde, istemci istek kimliğine göre verileri ilişkilendirmek ve gruplandırmak için önceden yapılandırılmış ve özel düzen görünümlerinin nasıl kullanılacağı açıklanmıştır.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Çözümleme Izgarasında görüntülemek için bir görünüm düzeni seçin

İleti Çözümleyicisi için Depolama Varlıkları, verilerinizi farklı senaryolar için yararlı gruplandırmalar ve sütunlarla görüntülemek için kullanabileceğiniz önceden yapılandırılmış görünümler olan Azure Depolama Görünümü Düzenleri içerir. Ayrıca özel görünüm düzenleri oluşturabilir ve bunları yeniden kullanmak için kaydedebilirsiniz.

Aşağıdaki resimde, araç çubuğu şeridinden **Görünümü Düzen'i** seçerek kullanılabilen Görünüm **Düzeni** menüsü gösterilmektedir. Azure Depolama'nın görünüm düzenleri menüdeki **Azure Depolama** düğümü altında gruplandırılır. Arama kutusunda `Azure Storage` yalnızca Azure Depolama görünüm düzenlerine filtre uygulayacak şekilde arama yapabilirsiniz. Ayrıca, sık kullanılan yapmak ve menünün üst kısmında görüntülemek için bir görünüm düzeninin yanındaki yıldızı da seçebilirsiniz.

![Düzen menüsünü görüntüle](./media/storage-e2e-troubleshooting/view-layout-menu.png)

İlk olarak, **ClientRequestID ve Modüle göre gruplandır'ı**seçin. Bu görünüm düzeni, önce istemci istek kimliğine, ardından kaynak günlük dosyasına (veya İleti Çözümleyicisindeki **Modül)** üç günlükten verileri gruplatır. Bu görünümle, belirli bir istemci istek kimliğine ayrıntılı bilgi verebilir ve bu istemci istek kimliği için üç günlük dosyasının verilerini görebilirsiniz.

Aşağıdaki resimde, örnek günlük verilerine uygulanan ve sütunalt kümesi görüntülenen bu düzen görünümü gösterilmektedir. Belirli bir istemci istek kimliği için Çözümleme İzi'nin istemci günlüğünden, sunucu günlüğünden ve ağ izlemesinden verileri görüntülediğini görebilirsiniz.

![Azure Depolama Görünümü Düzeni](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

> [!NOTE]
> Farklı günlük dosyaları farklı sütunlara sahiptir, bu nedenle çözümleme ızgarasında birden çok günlük dosyasından gelen veriler görüntülendiğinde, bazı sütunlar belirli bir satır için veri içermeyebilir. Örneğin, yukarıdaki resimde, istemci günlük satırları **Zaman damgası,** **Zaman Aşımı**, **Kaynak**ve **Hedef** sütunları için herhangi bir veri göstermez, çünkü bu sütunlar istemci günlüğünde bulunmaz, ancak ağ izlemesinde bulunur. Benzer şekilde, **Zaman Damgası** sütunu sunucu günlüğünden zaman damgası verilerini görüntüler, ancak sunucu günlüğünün bir parçası olmayan **ZamanAşımı**, **Kaynak**ve **Hedef** sütunları için hiçbir veri görüntülenmez.
>
>

Azure Depolama görünüm düzenlerini kullanmanın yanı sıra, kendi görünüm düzenlerinizi de tanımlayabilir ve kaydedebilirsiniz. Verileri gruplandırmak için diğer istenilen alanları seçebilir ve gruplandırmayı özel düzeninizin bir parçası olarak kaydedebilirsiniz.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Analiz Kılavuz'una renk kuralları uygulama

Depolama Varlıkları, Çözümleme İzole'sindeki farklı hata türlerini tanımlamak için görsel bir araç sunan renk kuralları da içerir. Önceden tanımlanmış renk kuralları HTTP hataları için geçerlidir, bu nedenle yalnızca sunucu günlüğü ve ağ izleme için görünür.

Renk kurallarını uygulamak için araç çubuğu şeridinden **Renk Kuralları'nı** seçin. Menüde Azure Depolama renk kurallarını görürsünüz. Öğretici için, aşağıdaki resimde gösterildiği gibi **İstemci Hataları (400 ile 499 arasında StatusCode)** seçeneğini belirleyin.

![Azure Depolama Görünümü Düzeni](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Azure Depolama renk kurallarını kullanmanın yanı sıra, kendi renk kurallarınızı da tanımlayabilir ve kaydedebilirsiniz.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>400 aralıklı hataları bulmak için günlük verilerini gruplandırma ve filtreleme

Ardından, 400 aralığındaki tüm hataları bulmak için günlük verilerini gruplayışve filtreleyeceğiz.

1. Analiz Izgara'sında **Ki Durum Kodu** sütununa yer bulun, sütun başlığına sağ tıklayın ve **Grup'u**seçin.
2. Ardından, **ClientRequestId** sütununda grup. Çözümleme İzgarası'ndaki verilerin artık durum koduna ve istemci istek kimliğine göre düzenlendiğini görürsünüz.
3. Zaten görüntülenmemişse Görünüm Filtresi aracı penceresini görüntüleyin. Araç çubuğu şeridinde **Araç Windows'u**seçin ve ardından **Filtreyi Görüntüle'yi**seçin.
4. Yalnızca 400 aralıklık hataları görüntülemek için günlük verilerini filtrelemek için **Görünüm Filtresi** penceresine aşağıdaki filtre ölçütlerini ekleyin ve **Uygula'yı**tıklatın:

    `(AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)`

Aşağıdaki resimde bu gruplandırma ve filtre sonuçlarını gösterir. Örneğin, durum kodu 409 için gruplandırmanın altındaki **ClientRequestID** alanını genişletmek, bu durum koduyla sonuçlanan bir işlem gösterir.

![Azure Depolama Görünümü Düzeni](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Bu filtreyi uyguladıktan sonra, istemci günlüğünde **StatusCode** sütunu içermediği için istemci günlüğündeki satırların dışlanmış olduğunu görürsünüz. İlk olarak, 404 hatasını bulmak için sunucu ve ağ izleme günlüklerini gözden geçireceğiz ve ardından bunlara yol açan istemci işlemlerini incelemek üzere istemci günlüğüne döneceğiz.

> [!NOTE]
> Durum kodu null olduğu günlük girişleri içeren filtreye bir ifade eklerseniz, **Durum Kodu** sütununa filtre uygulayabilir ve istemci günlüğü de dahil olmak üzere üç günlükteki verileri görüntülemeye devam edebilirsiniz. Bu filtre ifadesini oluşturmak için şunları kullanın:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
>
> Bu filtre istemci günlüğünden tüm satırları döndürür ve yalnızca sunucu günlüğünden ve durum kodunun 400'den büyük olduğu HTTP günlüğünden satırlar. İstemci istek kimliği ve modülüne göre gruplanan görünüm düzenine uygularsanız, üç günlüklerin de temsil edildiği girişleri bulmak için günlük girişlerinde arama yapabilir veya gezinebilirsiniz.

### <a name="filter-log-data-to-find-404-errors"></a>404 hata bulmak için günlük verilerini filtreleme

Depolama Varlıkları, aradığınız hataları veya eğilimleri bulmak için günlük verilerini daraltmak için kullanabileceğiniz önceden tanımlanmış filtreler içerir. Ardından, önceden tanımlanmış iki filtre uygularız: biri sunucu ve ağ izleme günlüklerini 404 hata için filtreler, diğeri de belirli bir zaman aralığındaki verileri filtreler.

1. Zaten görüntülenmemişse Görünüm Filtresi aracı penceresini görüntüleyin. Araç çubuğu şeridinde **Araç Windows'u**seçin ve ardından **Filtreyi Görüntüle'yi**seçin.
2. Görünüm Filtresi penceresinde **Kitaplık'ı**seçin `Azure Storage` ve Azure Depolama filtrelerini bulmak için arama yapın. **Tüm günlüklerde 404 (Bulunamadı) iletileri**için filtreyi seçin.
3. **Kitaplık** menüsünü yeniden görüntüleyin ve **Genel Zaman Filtresi'ni**bulun ve seçin.
4. Filtrede gösterilen zaman damgalarını görüntülemek istediğiniz aralıkta edin. Bu, çözümlenecek veri aralığını daraltmaya yardımcı olur.
5. Filtreniz aşağıdaki örneğe benzer görünmelidir. Filtreyi Çözümle ızgarasına uygulamak için **Uygula'yı** tıklatın.

    `((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
    (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)`

    ![Azure Depolama Görünümü Düzeni](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Günlük verilerinizi analiz edin

Verilerinizi gruplandırıp filtrelediğinize göre, 404 hata oluşturan tek tek isteklerin ayrıntılarını inceleyebilirsiniz. Geçerli görünüm düzeninde, veriler istemci istek kimliğine, ardından günlük kaynağına göre gruplandırılır. StatusCode alanının 404 içerdiği isteklere filtre uygulama yaptığımızdan, istemci günlük verilerini değil, yalnızca sunucu ve ağ izleme verilerini görürsünüz.

Aşağıdaki resimde, blob olmadığı için Get Blob işleminin 404 verdiği özel bir istek gösterilmektedir. İlgili verileri görüntülemek için bazı sütunların standart görünümden kaldırıldığını unutmayın.

![Filtrelenmiş Sunucu ve Ağ İzleme Günlükleri](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Ardından, istemcinin hata olduğunda hangi eylemleri gerçekleştirdiğini görmek için bu istemci istek kimliğini istemci günlük verileriyle ilişkilendiririz. İkinci bir sekmede açılan istemci günlük verilerini görüntülemek için bu oturum için yeni bir Çözümleme Izgara görünümü görüntüleyebilirsiniz:

1. İlk olarak, **ClientRequestId** alanının değerini panoya kopyalayın. Bunu, her iki satırı seçerek, **ClientRequestId** alanını bularak, veri değerine sağ tıklayarak ve **'ClientRequestId' kopyasını**seçerek yapabilirsiniz.
2. Araç çubuğu şeridinde **Yeni Görüntüleyici'yi**seçin ve ardından yeni bir sekme açmak için **Çözümlemesi Izgara'yı** seçin. Yeni sekme, günlük dosyalarınızdaki tüm verileri gruplandırma, filtreleme veya renk kuralları olmadan gösterir.
3. Araç çubuğu **şeridinde, Düzen'i Görüntüle'yi**seçin ve ardından **Azure Depolama** bölümü altındaki **Tüm .NET İstemci Sütunları'nı** seçin. Bu görünüm düzeni istemci günlüğünden gelen verileri, sunucu ve ağ izleme günlüklerini gösterir. Varsayılan olarak **MessageNumber** sütununda sıralanır.
4. Ardından, istemci istek kimliğini için istemci günlüğünde arama yapın. Araç çubuğu şeridinde **İletibul'u**seçin ve **ardından Bul** alanında istemci istek kimliğine özel bir filtre belirtin. Kendi istemci istek kimliğinizi belirterek filtre için bu sözdizimini kullanın:

    `*ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"`

İleti Çözümleyicisi, arama ölçütlerinin istemci istek kimliğiyle eşleştiği ilk günlük girişini bulur ve seçer. İstemci günlüğünde, her istemci istek kimliği için birkaç giriş vardır, bu nedenle hepsini bir arada görmeyi kolaylaştırmak için bunları **ClientRequestId** alanında gruplandırmak isteyebilirsiniz. Aşağıdaki resimde, belirtilen istemci istek kimliği için istemci günlüğündeki tüm iletiler gösterilmektedir.

![404 hata gösteren istemci günlüğü](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Bu iki sekmedeki görünüm düzenlerinde gösterilen verileri kullanarak, hataya neyin neden olduğunu belirlemek için istek verilerini çözümleyebilirsiniz. Daha önceki bir olayın 404 hatasına yol açmış olup olmadığını görmek için bundan önceki isteklere de bakabilirsiniz. Örneğin, blob silinmiş olabilir mi, yoksa hata bir kapsayıcı veya blob üzerinde CreateIfNotExists API çağıran istemci uygulaması nedeniyle olup olmadığını belirlemek için bu istemci istek kimliği öncesinde istemci günlük girişleri gözden geçirebilirsiniz. İstemci günlüğünde, **açıklama** alanında blob adresini bulabilirsiniz; sunucu ve ağ izleme günlüklerinde, bu bilgiler **Özet** alanında görünür.

404 hatasını ortaya çıkan lekenin adresini bildiğinizde, daha fazla araştırma yapabilirsiniz. Aynı blob'daki işlemlerle ilişkili diğer iletiler için günlük girişlerinde arama yaparsa, istemcinin varlığı daha önce silip silmediğini denetleyebilirsiniz.

## <a name="analyze-other-types-of-storage-errors"></a>Diğer depolama hatalarını çözümleme

Artık günlük verilerinizi çözümlemek için İleti Çözümleyicisi'ni kullanmaya aşina olduğunuza göre, görünüm düzenlerini, renk kurallarını ve arama/filtreleme yi kullanarak diğer hata türlerini çözümleyebilirsiniz. Aşağıdaki tablolarda karşılaşabileceğiniz bazı sorunlar ve bunları bulmak için kullanabileceğiniz filtre ölçütleri listelenebönemlidir. Filtreler oluşturma ve İleti Çözümleyicifiltreleme dili hakkında daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/jj819365.aspx)

| Araştırmak için... | Filtre İfadesi'ni kullanın... | İfade Günlük (İstemci, Sunucu, Ağ, Tümü) için geçerlidir |
| --- | --- | --- |
| Kuyruktaki ileti tesliminde beklenmeyen gecikmeler |AzureStorageClientDotNetV4.Description "Başarısız işlemi yeniden deneme" içerir. |İstemci |
| YÜZDETHROttlingError HTTP Artış |HTTP' ye göre. Response.StatusCode == 500 &#124;&#124; HTTP. Response.StatusCode == 503 |Ağ |
| YüzdeTimeoutError artış |HTTP' ye göre. response.statusCode == 500 |Ağ |
| YüzdeTimeoutError artış (tüm) |*Durum Kodu == 500 |Tümü |
| YüzdeAğ Hatasında Artış |AzureStorageClientDotNetV4.EventLogEntry.Level < 2 |İstemci |
| HTTP 403 (Yasak) iletileri |HTTP' ye göre. Response.StatusCode == 403 |Ağ |
| HTTP 404 (Bulunamadı) iletileri |HTTP' ye göre. Response.StatusCode == 404 |Ağ |
| 404 (tümü) |*Durum Kodu == 404 |Tümü |
| Paylaşılan Erişim İmzası (SAS) yetkilendirme sorunu |AzureStorageLog.RequestStatus == "SASAuthorizationError" |Ağ |
| HTTP 409 (Çakışma) iletileri |HTTP' ye göre. Response.StatusCode == 409 |Ağ |
| 409 (tümü) |*Durum Kodu == 409 |Tümü |
| Düşük YüzdeBaşarı veya analitik günlük girişleri ClientOtherErrors işlem durumu ile işlemleri var |AzureStorageLog.RequestStatus == "ClientOtherError" |Sunucu |
| Nagle Uyarısı |((AzureStorageStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) ve (AzureStorageLog.RequestPacketSize <1460) ve (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS >= 200) |Sunucu |
| Sunucu ve Ağ günlüklerinde zaman aralığı |#Timestamp >= 2014-10-20T16:36:38 ve #Timestamp <= 2014-10-20T16:36:39 |Sunucu, Ağ |
| Sunucu günlüklerinde zaman aralığı |AzureStorageLog.Timestamp >= 2014-10-20T16:36:38 ve AzureStorageLog.Timestamp <= 2014-10-20T16:36:39 |Sunucu |

## <a name="next-steps"></a>Sonraki adımlar

Azure Depolama'da uçtan uca senaryolar hakkında daha fazla bilgi için şu kaynaklara bakın:

* [Microsoft Azure Depolama izleme, tanılama ve sorun giderme](storage-monitoring-diagnosing-troubleshooting.md)
* [Depolama Analizi](https://msdn.microsoft.com/library/azure/hh343270.aspx)
* [Azure portalında depolama hesabını izleme](storage-monitor-storage-account.md)
* [AzCopy Command-Line Utility ile veri aktarımı](storage-use-azcopy.md)
* [Microsoft İleti Çözümleyicisi’ni Kullanma Kılavuzu](https://technet.microsoft.com/library/jj649776.aspx)
