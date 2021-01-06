---
title: Azure Veri Gezgini Azure Izleyici ile Azure Veri Gezgini kümesini izleme (Önizleme) | Microsoft Docs
description: Bu makalede Azure Veri Gezgini kümeleri için Azure Monitor Insights açıklanır.
services: azure-monitor
ms.topic: conceptual
ms.date: 01/05/2021
author: lgayhardt
ms.author: lagayhar
ms.openlocfilehash: e5884cbbc13fb2353898c617dffdf18b114f1b86
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97901794"
---
# <a name="monitoring-your-azure-data-explorer-cluster-with-azure-monitor-for-azure-data-explorer-preview"></a>Azure Veri Gezgini Azure Izleyici ile Azure Veri Gezgini kümenizi izleme (Önizleme)
Azure Veri Gezgini için Azure Izleyici (Önizleme), küme Performanslarınızın, işlemlerinin, kullanımınızın ve hatalarının birleştirilmiş bir görünümünü sunarak kümelerinizin kapsamlı bir şekilde izlenmesini sağlar.
Bu makale, Azure Veri Gezgini (Önizleme) için Azure Izleyici 'yi ekleme ve kullanmayı anlamanıza yardımcı olur.

## <a name="introduction-to-azure-monitor-for-azure-data-explorer-preview"></a>Azure için Azure Izleyici 'ye giriş Veri Gezgini (Önizleme)

Deneyimle karşılaşmadan önce, bilgileri nasıl sunduklarını ve görselleştirir.
-    Sorgu, giriş ve dışarı aktarma işlemlerinin performansını kolayca izlemek için kümelerinizin birincil ölçümlerinin anlık görüntü görünümünü gösteren **Ölçek perspektifinde** .
-   Ayrıntılı analiz gerçekleştirmeye yardımcı olması için belirli bir Azure Veri Gezgini kümesinin **detaya gitme Analizi** .
-    Hangi ölçümleri görmek istediğinizi değiştirmek, Sınırlarınızla hizalı olan eşikleri değiştirmek veya ayarlamak ve kendi özel çalışma kitaplarınızı kaydetmek için **özelleştirilebilir** . Çalışma kitabındaki grafikler Azure panolarına sabitlenebilir.

## <a name="view-from-azure-monitor-at-scale-perspective"></a>Azure Izleyici 'den (ölçekli perspektifte) görüntüle

Azure Izleyici 'den, sorgular için ölçümler, alım işlemleri ve abonelik içindeki birden çok kümeden dışarı aktarma işlemleri dahil olmak üzere, küme için ana performans ölçümlerini görüntüleyebilir ve performans sorunlarını belirlemenize yardımcı olabilirsiniz.

Tüm aboneliklerinizde kümelerinizin performansını görüntülemek için aşağıdaki adımları gerçekleştirin:

1. [Azure Portal](https://portal.azure.com/) oturum açın

2. Azure portal sol bölmeden **izleyici** ' yi seçin ve Öngörüler hub 'ı bölümünde **Azure Veri Gezgini kümeleri (Önizleme)** seçeneğini belirleyin.

![Birden çok grafik içeren genel bakış deneyiminin ekran görüntüsü](./media/data-explorer-insights/insights-hub.png)

### <a name="overview-tab"></a>Genel Bakış sekmesi

Seçili aboneliğin **genel bakış** sekmesinde Tablo, abonelik Içinde gruplanmış Azure Veri Gezgini kümeleri için etkileşimli ölçümleri görüntüler. Aşağıdaki açılan listelerden seçtiğiniz seçeneklere göre sonuçlara filtre uygulayabilirsiniz:

* Abonelikler – yalnızca Azure Veri Gezgini kümelerine sahip abonelikler listelenir.

* Azure Veri Gezgini kümeleri – varsayılan olarak, yalnızca en fazla beş küme önceden seçilmiştir. Kapsam seçicisinde tüm veya birden çok küme seçerseniz, en fazla 200 kümesi döndürülür.

* Zaman aralığı: varsayılan olarak, yapılan ilgili seçimlere göre son 24 saati bilgileri görüntüler.

Sayaç kutucuğu, açılan liste altında, seçilen aboneliklerdeki Azure Veri Gezgini kümelerinin toplam sayısını kaydeder ve kaç tane seçili olduğunu yansıtır. Sütunlar için koşullu renk kaynakları vardır: canlı tut, CPU, Alım kullanımı ve önbellek kullanımı. Turuncu kodlu hücrelerde, küme için sürdürülebilirlik olmayan değerler vardır. 

Her bir ölçüm 'in neyi temsil ettiğini daha iyi anlamak için [Azure Veri Gezgini ölçümleriyle](https://docs.microsoft.com/azure/data-explorer/using-metrics#cluster-metrics)ilgili belgelerde okumanız önerilir.

### <a name="query-performance-tab"></a>Sorgu performansı sekmesi

Bu sekme, sorgu süresini, eşzamanlı sorguların toplam sayısını ve kısıtlanan sorguların toplam sayısını gösterir.

![Sorgu performansı sekmesinin ekran görüntüsü](./media/data-explorer-insights/query-performance.png)

### <a name="ingestion-performance-tab"></a>Alım performansı sekmesi

Bu sekmede alma gecikmesi, başarılı Alım sonuçları, başarısız Alım sonuçları, alma hacmi ve olay/IoT Hub 'Ları için işlenen olaylar gösterilmektedir.

[![Alım performansı sekmesinin ekran görüntüsü](./media/data-explorer-insights/ingestion-performance.png)](./media/data-explorer-insights/ingestion-performance.png#lightbox)

### <a name="streaming-ingest-performance-tab"></a>Akış alma performansı sekmesi

Bu sekme, ortalama veri hızı, ortalama süre ve istek oranı hakkında bilgi sağlar.

### <a name="export-performance-tab"></a>Dışarı aktarma performansı sekmesi

Bu sekme, sürekli dışa aktarma işlemlerine yönelik dışarı aktarılmış kayıtlar, eksik işlem, bekleyen sayısı ve kullanım yüzdesi hakkında bilgi sağlar.

## <a name="view-from-an-azure-data-explorer-cluster-resource-drill-down-analysis"></a>Azure Veri Gezgini kümesi kaynağından görüntüleme (detaya gitme Analizi)

Azure Veri Gezgini kümelerini Azure Izleyici 'ye doğrudan bir Azure Veri Gezgini kümesinden erişmek için:

1. Azure portal **Azure Veri Gezgini kümeleri**' ni seçin.

2. Listeden bir Azure Veri Gezgini kümesi seçin. İzleme bölümünde Öngörüler ' i **(Önizleme)** seçin.

Bu görünümlere Ayrıca Azure Monitor Insights görünümündeki Azure Veri Gezgini kümesinin kaynak adı seçilerek de erişilebilir.

Azure Veri Gezgini için Azure Izleyici, genel bir izleme çözümü sağlamak için hem günlükleri hem de ölçümleri birleştirir. Günlük tabanlı görselleştirmelerin dahil edilmesi, kullanıcıların [Azure Veri Gezgini kümesinin tanılama günlüğünü etkinleştirmesini ve bir Log Analytics çalışma alanına göndermesini gerektirir.](https://docs.microsoft.com/azure/data-explorer/using-diagnostic-logs?tabs=commands-and-queries#enable-diagnostic-logs) Etkinleştirilmesi gereken tanılama günlükleri şunlardır: **komut**, **sorgu**, **Tabloayrıntıları** ve **tableusagestatistik**.

!["Izleme için günlükleri etkinleştir" metnini görüntüleyen mavi düğme ekran görüntüsü](./media/data-explorer-insights/enable-logs.png)


 **Genel bakış** sekmesi şunları gösterir:

- Ölçüm kutucukları, sistem durumunu hızlı bir şekilde değerlendirmek için kümenin kullanılabilirliğini ve genel durumunu vurgular.

- Etkin [danışman önerilerin](https://docs.microsoft.com/azure/data-explorer/azure-advisor) ve [kaynak sistem](https://docs.microsoft.com/azure/data-explorer/monitor-with-resource-health) durumunun özeti.

- En yüksek CPU ve bellek tüketicilerini ve zaman içindeki benzersiz kullanıcı sayısını gösteren grafikler.


[![Azure Veri Gezgini kümesi kaynağından Görünüm ekran görüntüsü](./media/data-explorer-insights/overview.png)](./media/data-explorer-insights/overview.png#lightbox)

**Anahtar ölçümleri** sekmesi, kümenin bazı ölçümlerinin birleştirilmiş bir görünümünü gösterir: genel ölçümler, sorgu ile ilgili, alım ile ilgili ve akış alımı ile ilgili ölçümler.

[![Başarısızlık görünümü ekran görüntüsü](./media/data-explorer-insights/key-metrics.png)](./media/data-explorer-insights/key-metrics.png#lightbox)

**Kullanım** sekmesi, kullanıcıların kümenin komutları ve sorgularının performansını derinlemesine olarak belirlemesine olanak tanır. Bu sayfada şunları yapabilirsiniz:
 
 - Hangi kullanıcıların ve uygulamaların en çok sorguyu gönderdiğini veya en fazla CPU ve belleği tükettiğini görün (Bu sayede, kümenin işlenmesi için en yoğun sorguları hangi kullanıcıların gönderdiğini anlayabilmeniz gerekir).
 - Başarısız sorgulara göre popüler kullanıcıları ve uygulamaları belirler.
 - Kullanıcı ve uygulamaya göre geçmiş günlük ortalama (son 16 gün boyunca) ile karşılaştırıldığında, son değişiklikleri sorgu sayısında belirler.
 - Kullanıcı, uygulama ve komut türüne göre sorgu, bellek ve CPU tüketimi sayısında eğilimleri ve en üst noktaları belirler.

[![Komut ve sorgu sayısı, komut ve sorgu sayısına göre en iyi sorumlular ve komut türlerine göre en üst komutları içeren işlem görünümünün ekran görüntüsü](./media/data-explorer-insights/usage.png)](./media/data-explorer-insights/usage.png#lightbox)

[![Uygulamaya göre sorgu sayısı çizgi grafiklerini içeren işlemler görünümünün ekran görüntüsü, uygulamaya göre toplam bellek ve uygulamaya göre toplam CPU](./media/data-explorer-insights/usage-2.png)](./media/data-explorer-insights/usage-2.png#lightbox)

**Tablolar** sekmesi, kümedeki tabloların en son ve geçmiş özelliklerini gösterir. En fazla alanı hangi tabloların tükettiğini görebilirsiniz, büyüme geçmişini tablo boyutuna, sık verilere ve zaman içindeki satır sayısına göre takip edebilirsiniz.

**Önbellek** sekmesi kullanıcıların gerçek sorgularını gözden geçirin ve bunları yapılandırılmış önbellek ilkesiyle karşılaştırın (her tablo için). En çok sorgu ve tablo tarafından kullanılan tabloları tanımlayabilir ve önbellek ilkesini buna uygun şekilde uyarlayabilir. Azure Advisor 'daki belirli tablolar üzerinde belirli bir önbellek ilkesi önerisi alabilirsiniz (Şu anda önbellek önerileri yalnızca [ana Azure Danışmanı panosundan](https://docs.microsoft.com/azure/data-explorer/azure-advisor#use-the-azure-advisor-recommendations)kullanılabilir) ve sorguların en az %95 ' i için son 30 güne ve en iyi duruma getirilmiş bir önbellek ilkesine göre görünür. Azure Advisor 'daki önbellek azaltma önerileri, "verilere göre sınırlanmış" kümeler için kullanılabilir (kümenin düşük CPU ve düşük Alım kullanımı, ancak yüksek veri kapasitesi nedeniyle, küme ölçeklendiremez veya azaltılamamıştır). 

[![Önbellek ayrıntılarının ekran görüntüsü](./media/data-explorer-insights/cache-tab.png)](./media/data-explorer-insights/cache-tab.png#lightbox)

## <a name="pin-to-azure-dashboard"></a>Azure panosuna sabitle

Bölümün sağ üst köşesindeki raptiye simgesini seçerek ölçüm bölümlerinin herhangi birini ("ölçekli" perspektifinden) bir Azure panosuna sabitleyebilirsiniz.

![PIN simgesinin seçili ekran görüntüsü](./media/data-explorer-insights/pin.png)

## <a name="customize-azure-monitor-for-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi için Azure Izleyicisini özelleştirme

Bu bölümde, veri analizi ihtiyaçlarınızı desteklemeye yönelik olarak özelleştirmek üzere çalışma kitabını düzenlemeyle ilgili yaygın senaryolar vurgulanmıştır:
* Çalışma kitabını her zaman belirli bir abonelik veya Azure Veri Gezgini kümesi seçmek üzere kapsama
* Kılavuzdaki ölçümleri değiştirme
* Eşikleri veya renk işlemeyi/kodlamayı Değiştir

En üstteki araç çubuğundan **Özelleştir** düğmesini seçerek düzenleme modunu etkinleştirerek özelleştirmeleri başlatabilirsiniz.

![Özelleştirme düğmesinin ekran görüntüsü](./media/data-explorer-insights/customize.png)

Özelleştirmeler, yayımlanan çalışma kitabımızda varsayılan yapılandırmanın üzerine yazılmasını engellemek için özel bir çalışma kitabına kaydedilir. Çalışma kitapları, sizin için özel Raporlarım bölümünde veya kaynak grubuna erişimi olan herkesin erişebileceği paylaşılan Raporlar bölümünde bir kaynak grubuna kaydedilir. Özel çalışma kitabını kaydettikten sonra başlatmak için çalışma kitabı galerisine gitmeniz gerekir.

![Çalışma kitabı galerinin ekran görüntüsü](./media/data-explorer-insights/gallery.png)

## <a name="troubleshooting"></a>Sorun giderme

Genel sorun giderme kılavuzu için, adanmış çalışma kitabı tabanlı Öngörüler [sorun giderme makalesine](troubleshoot-workbooks.md)bakın.

Bu bölüm, Azure Veri Gezgini kümesi (Önizleme) için Azure Izleyicisi 'ni kullanırken karşılaşabileceğiniz bazı yaygın sorunların tanılanması ve sorun gidermede size yardımcı olur. Belirli sorununuzla ilgili bilgileri bulmak için aşağıdaki listeyi kullanın.

### <a name="why-dont-i-see-all-my-subscriptions-in-the-subscription-picker"></a>Abonelik seçicide neden tüm aboneliklerimi görmüyorum?

Yalnızca Azure portal üstbilgisindeki "Dizin + abonelik" bölümünde seçilen abonelik filtresinden seçilen Azure Veri Gezgini kümelerini içeren abonelikler gösteriliyor.

![Abonelik filtresi ekran görüntüsü](./media/key-vaults-insights-overview/Subscriptions.png)

### <a name="why-do-i-not-see-any-data-for-my-azure-data-explorer-cluster-under-the-usage-tables-or-cache-sections"></a>Azure Veri Gezgini kümeniz için kullanım, tablolar veya önbellek bölümlerinde hiç bir veri görmüyorum

Günlük tabanlı verilerinizi görüntülemek için, izlemek istediğiniz her Azure Veri Gezgini kümesi için [tanılama günlüklerini etkinleştirmeniz](https://docs.microsoft.com/azure/data-explorer/using-diagnostic-logs?tabs=commands-and-queries#enable-diagnostic-logs) gerekir. Bu, her küme için Tanılama ayarları altında yapılabilir. Verilerinizi bir Log Analytics çalışma alanına göndermeniz gerekecektir. Etkinleştirilmesi gereken tanılama günlükleri şunlardır: komut, sorgu, Tabloayrıntıları ve Tableusagestatistik.

### <a name="i-have-already-enabled-logs-for-my-azure-data-explorer-cluster-why-am-i-still-unable-to-see-my-data-under-commands-and-queries"></a>Azure Veri Gezgini Kümem için günlükleri zaten etkinleştirdim, neden hala komut ve sorgu altında verilerimi göremiyorum

Şu anda tanılama günlükleri geriye dönük olarak çalışmaz, bu nedenle veriler yalnızca Azure Veri Gezgini gerçekleştirilen eylemler yapıldıktan sonra görünmeye başlar. Bu nedenle, Azure Veri Gezgini kümenizin ne kadar etkin olduğuna bağlı olarak, saatler bir güne kadar zaman alabilir.


## <a name="next-steps"></a>Sonraki adımlar

Çalışma kitaplarının desteklemek için tasarlandıkları senaryoları, mevcut raporların yeni nasıl yazılacağını ve özelleştirildiğini ve [Azure izleyici çalışma kitaplarını kullanarak etkileşimli raporlar oluşturma](../platform/workbooks-overview.md)konusunu gözden geçirin.
