---
title: Uygulama Öngörüleri'nden telemetrinin sürekli ihracatı | Microsoft Dokümanlar
description: Tanılama ve kullanım verilerini Microsoft Azure'daki depolamaya dışa aktarın ve buradan indirin.
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: f6afe42e483ab7ad5810169fc301946c75308c29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298294"
---
# <a name="export-telemetry-from-application-insights"></a>Application Insights’tan telemetriyi dışarı aktarma
Telemetrinizi standart saklama süresinden daha uzun süre saklamak ister misiniz? Ya da özel bir şekilde işlemek? Sürekli İhracat bunun için idealdir. Application Insights portalında gördüğünüz olaylar JSON formatında Microsoft Azure'daki depolamaya aktarılabilir. Buradan verilerinizi indirebilir ve işlemek için ihtiyacınız olan kodu yazabilirsiniz.  

Sürekli dışa aktarma ayarlamadan önce, göz önünde bulundurmak isteyebileceğiniz bazı alternatifler vardır:

* Ölçümlerin veya arama sekmesinin üst kısmındaki Dışa Aktarma düğmesi, tabloları ve grafikleri bir Excel elektronik tablosuna aktarmanızı sağlar.

* [Analytics,](../../azure-monitor/app/analytics.md) telemetri için güçlü bir sorgu dili sağlar. Ayrıca sonuç dışa aktarabilir.
* [Verilerinizi Power BI'de keşfetmek](../../azure-monitor/app/export-power-bi.md )istiyorsanız, bunu Sürekli Dışa Aktarma'yı kullanmadan yapabilirsiniz.
* [Veri erişimi REST API,](https://dev.applicationinsights.io/) telemetrinize programlı olarak erişmenizi sağlar.
* Ayrıca Powershell üzerinden kurulum [sürekli dışa aktarım](https://docs.microsoft.com/powershell/module/az.applicationinsights/new-azapplicationinsightscontinuousexport)erişebilirsiniz.

Sürekli DışA Aktarım verilerinizi depolama alanına kopyaladıktan sonra (istediğiniz kadar kalabilecekleri), her zamanki [saklama süresi](../../azure-monitor/app/data-retention-privacy.md)için Uygulama Öngörüleri'nde kullanılabilir.

## <a name="continuous-export-advanced-storage-configuration"></a>Sürekli Dışa Aktarma gelişmiş depolama yapılandırması

Sürekli Dışa Aktarma aşağıdaki Azure depolama özelliklerini/yapılandırmalarını **desteklemez:**

* Azure Blob depolama alanıyla birlikte [VNET/Azure Depolama güvenlik duvarlarının](https://docs.microsoft.com/azure/storage/common/storage-network-security) kullanımı.

* Azure Blob depolama için [değişmez depolama.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage)

* [Azure Veri Gölü Depolama Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction).

## <a name="create-a-continuous-export"></a><a name="setup"></a>Sürekli Dışa Aktarma Oluşturma

1. Uygulamanız için Uygulama Öngörüleri kaynağında solda yapılandırıldığında, Sürekli Dışa Aktarma'yı açın ve **Ekle'yi**seçin:

2. Dışa aktarmak istediğiniz telemetri veri türlerini seçin.

3. Verileri depolamak istediğiniz bir [Azure depolama hesabı](../../storage/common/storage-introduction.md) oluşturun veya seçin. Depolama fiyatlandırma seçenekleri hakkında daha fazla bilgi için [resmi fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/storage/)ziyaret edin.

     Hedef Ekle, Dışa Aktarma, Depolama hesabı ve ardından yeni bir mağaza oluşturun veya varolan bir mağazayı seçin'i tıklatın.

    > [!Warning]
    > Varsayılan olarak, depolama konumu Application Insights kaynağınızla aynı coğrafi bölgeye ayarlanır. Farklı bir bölgede depolarsanız, transfer ücretlerine maruz kalabilirsiniz.

4. Depolama alanında bir kapsayıcı oluşturun veya seçin.

İhracatınızı oluşturduktan sonra, devam etmeye başlar. Yalnızca dışa aktarmayı oluşturduktan sonra gelen verileri alırsınız.

Veriler depolama alanında görünmeden önce yaklaşık bir saat gecikme olabilir.

İlk dışakverme tamamlandıktan sonra Azure Blob depolama kapsayıcınızda aşağıdakilere benzer bir yapı bulacaksınız: (Bu, topladığınız verilere bağlı olarak değişir.)

|Adı | Açıklama |
|:----|:------|
| [Kullanılabilir -lik](export-data-model.md#availability) | [Kullanılabilirlik web testlerini](../../azure-monitor/app/monitor-web-app-availability.md)raporlar.  |
| [Olay](export-data-model.md#events) | [TrackEvent()](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)tarafından oluşturulan özel olaylar. 
| [Özel durumlar](export-data-model.md#exceptions) |Sunucuda ve tarayıcıda [özel durumları](../../azure-monitor/app/asp-net-exceptions.md) bildirir.
| [Ileti](export-data-model.md#trace-messages) | [TrackTrace](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)tarafından gönderilen ve [günlük adaptörleri](../../azure-monitor/app/asp-net-trace-logs.md)tarafından .
| [Ölçümler](export-data-model.md#metrics) | Metrik API çağrıları tarafından oluşturulur.
| [Performans Sayaçları](export-data-model.md) | Uygulama Öngörüleri tarafından toplanan Performans Sayaçları.
| [Istek](export-data-model.md#requests)| [TrackRequest](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest)tarafından gönderildi. Standart modüller, sunucuda ölçülen sunucu yanıt süresini raporlar için bunu kullanır.| 

### <a name="to-edit-continuous-export"></a>Sürekli dışa aktarmayı sağlamak için

Sürekli dışa aktarma'ya tıklayın ve depolama hesabını seçin.

### <a name="to-stop-continuous-export"></a>Sürekli ihracatı durdurmak için

Dışa aktarmayı durdurmak için Devre Dışı Bırak'ı tıklatın. Yeniden Etkinleştir'i tıklattığınızda, dışa aktarma yeni verilerle yeniden başlatılacaktır. Dışa aktarma devre dışı bırakılırken portala gelen verileri alamazsınız.

Dışa aktarmayı kalıcı olarak durdurmak için silin. Bunu yapmak verilerinizi depolamadan silmez.

### <a name="cant-add-or-change-an-export"></a>Dışa aktarma ekleyip değiştiremez misiniz?
* Dışa aktarma ekinde veya değiştirmek için Sahibi, Katılımcıveya Uygulama Öngörüleri Katılımcısı erişim haklarına ihtiyacınız vardır. [Roller hakkında bilgi edinin.][roles]

## <a name="what-events-do-you-get"></a><a name="analyze"></a>Hangi olayları alabiliyorsun?
İhraç edilen veriler, istemci IP adresinden hesapladığımız konum verilerini eklememiz dışında, uygulamanızdan aldığımız ham telemetridir.

[Örnekleme](../../azure-monitor/app/sampling.md) tarafından atılan veriler dışa aktarılan verilere dahil edilmez.

Diğer hesaplanan ölçümler dahil edilmez. Örneğin, ortalama CPU kullanımını dışa aktarmıyoruz, ancak ortalamanın hesaplandığı ham telemetriyi dışa aktarıyoruz.

Veriler ayrıca, ayarladığınız [kullanılabilirlik web testlerinin](../../azure-monitor/app/monitor-web-app-availability.md) sonuçlarını da içerir.

> [!NOTE]
> **Örnekleme.** Uygulamanız çok fazla veri gönderiyorsa, örnekleme özelliği çalışabilir ve oluşturulan telemetrinin yalnızca bir kısmını gönderebilir. [Örnekleme hakkında daha fazla bilgi edinin.](../../azure-monitor/app/sampling.md)
>
>

## <a name="inspect-the-data"></a><a name="get"></a>Verileri inceleyin
Depolama alanını doğrudan portalda inceleyebilirsiniz. En sol daki menüde ana sayfayı tıklatın, üstte "Azure hizmetleri" depolama **hesaplarını**seçin, depolama hesabı adını seçin, genel bakış sayfasında hizmetler altındaki **Blobs'u** seçin ve son olarak kapsayıcı adını seçin.

Visual Studio'daki Azure depolama alanını incelemek için **Görünüm**ve **Bulut Gezgini'ni**açın. (Bu menü komutuna sahip değilseniz, Azure SDK'yı yüklemeniz gerekir: **Yeni Proje** iletişim kutusunu açın, Visual C#/Cloud'u genişletin ve **.NET için Microsoft Azure SDK'sını alın'ı**seçin .)

Blob mağazanızı açtığınızda, bir dizi blob dosyası içeren bir kapsayıcı görürsünüz. Uygulama Öngörüleri kaynak adınızdan türetilen her dosyanın URI'si, enstrümantasyon anahtarı, telemetri türü/tarih/saat. (Kaynak adı küçüktür ve enstrümantasyon anahtarı tireleri atlar.)

![Blob mağazasını uygun bir aletle inceleyin](./media/export-telemetry/04-data.png)

Tarih ve saat UTC'dir ve telemetrinin mağazaya yatırıldığı zamandır - oluşturulduğu saat değil. Yani verileri indirmek için kod yazarsanız, veriler arasında doğrusal hareket edebilir.

Yolun şekli aşağıda veda edin:

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

Konum

* `blobCreationTimeUtc`iç evreleme depolamasında blob oluşturulduğu zamandır
* `blobDeliveryTimeUtc`blob'un dışa aktarma hedef depolamasına kopyalanması zamanıdır

## <a name="data-format"></a><a name="format"></a>Veri biçimi
* Her blob birden çok '\n'-ayrılmış satırlar içeren bir metin dosyasıdır. Yaklaşık yarım dakikalık bir süre içinde işlenen telemetri içerir.
* Her satır, istek veya sayfa görünümü gibi bir telemetri veri noktasını temsil eder.
* Her satır biçimlendirilmemiş bir JSON belgesidir. Oturup bakmak istiyorsanız Visual Studio'da açın ve Dosyayı Edit, Advanced, Format Dosyasını seçin:

![Telemetriyi uygun bir araçla görüntüleme](./media/export-telemetry/06-json.png)

Süreler kenelerde, 10 000 ticks = 1 ms. Örneğin, bu değerler tarayıcıdan istek göndermek için 1 ms, almak için 3 ms ve tarayıcıda sayfayı işlemek için 1,8 s'lik bir süre gösterir:

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[Özellik türleri ve değerleri için ayrıntılı veri modeli başvurusu.](export-data-model.md)

## <a name="processing-the-data"></a>Verileri işleme
Küçük bir ölçekte, verilerinizi ayırmak, elektronik tabloya okumak ve benzeri için bazı kodlar yazabilirsiniz. Örnek:

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

Daha büyük bir kod örneği için [bkz.][exportasa]

## <a name="delete-your-old-data"></a><a name="delete"></a>Eski verilerinizi silme
Depolama kapasitenizi yönetmekten ve gerekirse eski verileri silip atabilirsiniz.

## <a name="if-you-regenerate-your-storage-key"></a>Depolama anahtarınızı yenilerseniz...
Depolamanızın anahtarını değiştirirseniz, sürekli dışa aktarma çalışmayı durdurur. Azure hesabınızda bir bildirim görürsünüz.

Sürekli Dışa Aktarma sekmesini açın ve dışa aktarmanızı edin. Dışa Aktarma Hedefini edin, ancak aynı depolama alanını seçili bırakın. Onaylamak için Tamam'ı tıklatın.

Sürekli dışa aktarım yeniden başlatılacaktır.

## <a name="export-samples"></a>Dışa aktarma örnekleri

* [Stream Analytics kullanarak SQL’ye aktarma][exportasa]
* [Akış Analizi örneği 2](export-stream-analytics.md)

Daha büyük ölçeklerde, buluttaki [HDInsight](https://azure.microsoft.com/services/hdinsight/) - Hadoop kümelerini göz önünde bulundurun. HDInsight, büyük verileri yönetmek ve analiz etmek için çeşitli teknolojiler sağlar ve bunu Application Insights'tan dışa aktarılan verileri işlemek için kullanabilirsiniz.

## <a name="q--a"></a>Soru-Cevap
* *Ama tek istediğim bir grafiğin tek seferlik indirimi.*  

    Evet, bunu yapabilirsin. Sekmenin üst kısmında **Veri Aktar'ı**tıklatın.
* *İhracat ayarladım ama mağazamda veri yok.*

    Dışa aktarmayı ayarladığınızdan beri Uygulama Öngörüleri uygulamanızdan herhangi bir telemetri aldı mı? Yalnızca yeni veriler alırsınız.
* *Bir dışa aktarma ayarlamaya çalıştım, ancak erişim engellendi*

    Hesabın kuruluşunuz tarafından sahip olunması durumunda, sahipler veya katkıda bulunanlar gruplarının bir üyesi olmanız gerekir.
* *Doğrudan kendi şirket içi mağazama ihracat yapabilir miyim?*

    Hayır, üzgünüm. Dışa aktarma motorumuz şu anda yalnızca Azure depolama ile çalışıyor.  
* *Mağazama koyduğunuz veri miktarının bir sınırı var mı?*

    Hayır. Siz dışa aktarmayı silene kadar verileri zorlamaya devam edeceğiz. Blob depolama için dış sınırlara basarsak dururuz, ama bu oldukça büyük bir şey. Ne kadar depolama alanı kullandığınızı kontrol etmek size kalmış.  
* *Depoda kaç tane damla görmem gerekiyor?*

  * Dışa aktarmak için seçtiğiniz her veri türü için her dakika yeni bir blob oluşturulur (veri varsa).
  * Buna ek olarak, yüksek trafikli uygulamalar için ek bölüm birimleri ayrılır. Bu durumda, her birim her dakika bir leke oluşturur.
* *Depolama alanımdaki anahtarı yeniden oluşturdum veya kapsayıcının adını değiştirdim ve şimdi dışa aktarma çalışmıyor.*

    Dışa aktarmayı ve dışa aktarma hedef sekmesini açın. Önce olduğu gibi seçilen aynı depolama alanını bırakın ve onaylamak için Tamam'ı tıklatın. Dışa aktarma yeniden başlatılacaktır. Değişiklik son birkaç gün içinde yse, veri kaybetmezsiniz.
* *İhracat'ı duraklatabilir miyim?*

    Evet. Devre dışı bırak'ı seçin.

## <a name="code-samples"></a>Kod örnekleri

* [Akış Analizi örneği](export-stream-analytics.md)
* [Stream Analytics kullanarak SQL’ye aktarma][exportasa]
* [Özellik türleri ve değerleri için ayrıntılı veri modeli başvurusu.](export-data-model.md)

<!--Link references-->

[exportasa]: ../../azure-monitor/app/code-sample-export-sql-stream-analytics.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
