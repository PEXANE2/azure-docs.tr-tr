---
title: 'Öğretici: Log Analytics sorguları kullanmaya başlama'
description: Bu öğreticiden Azure portal Log Analytics kullanarak Azure Izleyici günlük sorgularını yazma ve yönetme hakkında bilgi edinin.
ms.subservice: logs
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 03/17/2020
ms.openlocfilehash: b337065f125d12e309dd1f7fcc56c2af72b1c28c
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90088366"
---
# <a name="tutorial-get-started-with-log-analytics-queries"></a>Öğretici: Log Analytics sorguları kullanmaya başlama

Bu öğretici, Azure portal Azure Izleyici günlük sorgularını yazmak, yürütmek ve yönetmek için Log Analytics nasıl kullanacağınızı gösterir. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve verilerinizden birçok diğer öngörü sağlamak için Log Analytics sorguları kullanabilirsiniz. 

Bu öğreticide, Log Analytics için kullanmayı öğreneceksiniz:

> [!div class="checklist"]
> * Günlük veri şemasını anlama
> * Basit sorgular yazma ve çalıştırma ve sorgular için zaman aralığını değiştirme
> * Sorgu sonuçlarını filtreleme, sıralama ve gruplama
> * Sorgu sonuçlarının görsellerini görüntüleme, değiştirme ve paylaşma
> * Sorguları ve sonuçları kaydetme, yükleme, dışarı aktarma ve kopyalama

Günlük sorguları hakkında daha fazla bilgi için bkz. [Azure izleyici 'de günlük sorgularına genel bakış](log-query-overview.md).<br/>
Günlük sorguları yazma hakkında ayrıntılı bir öğretici için bkz. [Azure izleyici 'de günlük sorgularını kullanmaya başlama](get-started-queries.md).

## <a name="open-log-analytics"></a>Log Analytics açın
Log Analytics kullanmak için bir Azure hesabında oturum açmış olmanız gerekir. Bir Azure hesabınız yoksa, [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Bu öğreticideki adımların çoğunu tamamlayabilmeniz için, [Bu tanıtım ortamını](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade)kullanarak çok sayıda örnek veri de kullanabilirsiniz. Tanıtım ortamıyla, sorguları kaydedemez veya sonuçları bir panoya sabitleyemez.

Azure Izleyici 'yi kullanarak en az bir Azure kaynağında günlük verilerini toplamak için kendi ortamınızı de kullanabilirsiniz. Bir Log Analytics çalışma alanını açmak için, Azure Izleyici sol gezinti bölmesinde **Günlükler**' i seçin. 

## <a name="understand-the-schema"></a>Şemayı anlama
 
*Şema* , mantıksal kategoriler altında gruplanmış bir tablo koleksiyonudur. Tanıtım şemasında, izleme çözümlerinin çeşitli kategorileri vardır. Örneğin, **Logmanagement** kategorisi Windows ve Syslog olaylarını, performans verilerini ve aracı sinyalleri içerir.

Şema tabloları, Log Analytics çalışma alanının **Tablolar** sekmesinde görüntülenir. Tablolar, her biri sütun adının yanında bulunan simgesiyle gösterilen bir veri türüne sahip sütunları içerir. Örneğin, **olay** tablosu, **bilgisayar** ve **EventCategory**gibi sayısal sütunlar gibi metin sütunları içerir.

![Ekran görüntüsünde, bilgisayar ve EventCategory vurgulanmış şekilde tablolar bölmesini vurgulayıp yeni bir sorguyla birlikte Azure portal günlükleri sayfası gösterilmektedir.](media/get-started-portal/schema.png)

## <a name="write-and-run-basic-queries"></a>Temel sorguları yazma ve çalıştırma

Log Analytics **sorgu düzenleyicisinde**yeni bir boş sorgu ile açılır.

![Log Analytics](media/get-started-portal/homepage.png)

### <a name="write-a-query"></a>Bir sorgu yazın

Azure Izleyici günlük sorguları kusto sorgu dilinin bir sürümünü kullanır. Sorgular, bir tablo adı ya da [arama](/azure/kusto/query/searchoperator) komutu ile başlayabilir. 

Aşağıdaki sorgu, **olay** tablosundan tüm kayıtları alır:

```Kusto
Event
```

Kanal (|) karakteri komutları ayırır, bu nedenle ilk komutun çıktısı bir sonraki komutun girişi olur. Tek bir sorguya dilediğiniz sayıda komut ekleyebilirsiniz. Aşağıdaki sorgu, **olay** tablosundan kayıtları alır ve ardından bunları herhangi bir özellikte oluşan terim **hatası** için arar:

```Kusto
Event 
| search "error"
```

Tek satır sonu sorguları okumayı kolaylaştırır. Birden çok satır sonu sorguyu ayrı sorgulara böler.

Aynı sorguyu yazmanın başka bir yolu da vardır:

```Kusto
search in (Event) "error"
```

İkinci örnekte, **Search** komutu yalnızca **Olaylar** tablosundaki kayıtları arama **hatası**için arar.

Varsayılan olarak, Log Analytics sorguları son 24 saatin bir zaman aralığıyla sınırlandırır. Farklı bir zaman aralığı ayarlamak için sorguya açık bir **TimeGenerated** filtresi ekleyebilir veya **zaman aralığı** denetimini kullanabilirsiniz.

### <a name="use-the-time-range-control"></a>Zaman aralığı denetimini kullanma
**Zaman aralığı** denetimini kullanmak için üstteki çubukta seçin ve ardından açılır listeden bir değer seçin veya özel bir zaman aralığı oluşturmak için **özel** ' i seçin.

![Saat seçici](media/get-started-portal/time-picker.png)

- Zaman aralığı değerleri UTC biçiminde olduğundan, yerel saat diliminize göre farklı olabilir.
- Sorgu **TimeGenerated**için açıkça bir filtre ayarladıysanız, zaman seçici denetimi **sorgu olarak ayarlanır**ve çakışmayı engellemek için devre dışıdır.

### <a name="run-a-query"></a>Sorgu çalıştırma
Bir sorgu çalıştırmak için imlecinizi sorgunun içine yerleştirin ve üstteki çubukta **Çalıştır** ' ı seçin veya **SHIFT** + **ENTER**tuşuna basın. Sorgu boş bir satır bulana kadar çalışır.

## <a name="filter-results"></a>Sonuçları filtreleme
Log Analytics sonuçları en fazla 10.000 kayıtla sınırlandırır. Gibi genel bir sorgu `Event` yararlı olacak çok fazla sonuç döndürüyor. Sorgu sonuçlarını sorgudaki tablo öğelerini kısıtlayarak veya sonuçlara açıkça bir filtre ekleyerek filtreleyebilirsiniz. Tablo öğeleri aracılığıyla filtreleme, yeni bir sonuç kümesi döndürür, ancak açık bir filtre var olan sonuç kümesi için geçerli olur.

### <a name="filter-by-restricting-table-elements"></a>Tablo öğelerini kısıtlayarak filtrele
Sorgudaki `Event` tablo öğelerini kısıtlayarak sorgu sonuçlarını **hata** olaylarına filtrelemek için:

1. Sorgu sonuçlarında, **Eventlevelname** sütununda **hata** olan herhangi bir kaydın yanındaki açılan oku seçin. 
   
1. Genişletilmiş Ayrıntılar bölümünde üzerine gelin ve **Eventlevelname**' in yanındaki **...** ' ı seçin ve ardından **"hata"** öğesini seçin. 
   
   ![Sorguya filtre Ekle](media/get-started-portal/add-filter.png)
   
1. **Sorgu Düzenleyicisi** 'ndeki sorgunun şu şekilde değiştirildiğini unutmayın:
   
   ```Kusto
   Event
   | where EventLevelName == "Error"
   ```
   
1. Yeni sorguyu çalıştırmak için **Çalıştır** ' ı seçin.

### <a name="filter-by-explicitly-filtering-results"></a>Sonuçları açıkça filtreleyerek filtrele
Sorgu `Event` sonuçlarını filtreleyerek sorgu sonuçlarını **hata** olaylarına göre filtrelemek için:

1. Sorgu sonuçlarında, **Eventlevelname**sütun başlığının yanındaki **filtre** simgesini seçin. 
   
1. Açılır pencerenin ilk alanında, **eşittir**' i seçin ve sonraki alana *hata*girin. 
   
1. **Filtre**' yi seçin.
   
   ![Ekran görüntüsü, olayları EventLevelName 'e göre filtrelemeye yönelik bağlamsal menüye sahip bir sonuç tablosu gösterir.](media/get-started-portal/filter.png)

## <a name="sort-group-and-select-columns"></a>Sütunları Sırala, Gruplandır ve Seç
Sorgu sonuçlarını **TimeGenerated [UTC]** gibi belirli bir sütuna göre sıralamak için sütun başlığını seçin. Artan ve azalan sıra arasında geçiş yapmak için başlığı tekrar seçin.

![Sütunları Sırala](media/get-started-portal/sort-column.png)

Sonuçları düzenlemenin başka bir yolu da gruplara göre yapılır. Sonuçları belirli bir sütuna göre gruplandırmak için, sütun başlığını, sütun **üst bilgisini sürükleyin ve bu sütuna göre gruplandırmak üzere buraya bırakın**. Alt gruplar oluşturmak için diğer sütunları üst çubuğa sürükleyin. Çubuktaki grupları ve alt grupları hiyerarşiyi ve sıralamayı yeniden düzenleyebilirsiniz.

![Ekran görüntüsü EventLevelName ve Computer alt gruplarının sorgu sonuçlarını gösterir.](media/get-started-portal/groups.png)

Sonuçlarda sütunları gizlemek veya göstermek için tablonun üzerindeki **sütunlar** ' ı seçin ve ardından aşağı açılan listeden istediğiniz sütunları seçin veya seçimi kaldırın.

![Sütun seçme](media/get-started-portal/select-columns.png)

## <a name="view-and-modify-charts"></a>Grafikleri görüntüleme ve değiştirme
Sorgu sonuçlarını görsel biçimlerde de görebilirsiniz. Örnek olarak aşağıdaki sorguyu girin:

```Kusto
Event 
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1d) 
| summarize count() by Source 
```

Varsayılan olarak, sonuçlar bir tabloda görüntülenir. Sonuçları bir grafik görünümünde görmek için tablonun üzerindeki **grafik** ' i seçin.

![Çubuk Grafik](media/get-started-portal/bar-chart.png)

Sonuçlar yığılmış çubuk grafiğinde görüntülenir. Sonuçların diğer görünümlerini göstermek için **yığılmış sütun** veya **pasta** gibi diğer seçenekleri seçin.

![Pasta grafiği](media/get-started-portal/pie-chart.png)

Görünümün özelliklerini, örneğin x ve y eksenleri veya gruplandırma ve bölme tercihlerini denetim çubuğundan el ile değiştirebilirsiniz.

Ayrıca, [işleme](/azure/kusto/query/renderoperator) işlecini kullanarak sorguda tercih edilen görünümü ayarlayabilirsiniz.

## <a name="pin-results-to-a-dashboard"></a>Sonuçları panoya sabitleme

Log Analytics bir sonuç tablosu veya grafiğini paylaşılan bir Azure panosuna sabitlemek için üstteki çubukta **panoya sabitle** ' yi seçin. 

![Panoya sabitle](media/get-started-portal/pin-dashboard.png)

**Başka bir panoya sabitle** bölmesine sabitlemek üzere bir paylaşılan Pano seçin veya oluşturun ve **Uygula**' yı seçin. Tablo veya grafik seçili Azure panosunda görünür.

![Panoya sabitlenmiş grafik](media/get-started-portal/pin-dashboard2.png)

Paylaşılan bir panoya sabitettiğiniz tablo veya grafik aşağıdaki basitleştirmeleri içerir: 

- Veriler son 14 güne sınırlıdır.
- Tabloda yalnızca en fazla dört sütun ve en üstteki yedi satır gösterilir.
- Çok sayıda ayrık kategori içeren grafikler, daha az doldurulmuş kategorileri otomatik olarak tek bir **diğer** bin içine gruplayın.

## <a name="save-load-or-export-queries"></a>Sorguları kaydetme, yükleme veya dışarı aktarma

Bir sorgu oluşturduktan sonra sorguyu veya sonuçları başkalarıyla kaydedebilir veya paylaşabilirsiniz. 

### <a name="save-queries"></a>Sorguları kaydet

Bir sorguyu kaydetmek için:

1. Üstteki çubukta **Kaydet** ' i seçin.
   
1. **Kaydet** iletişim kutusunda, a – z, a – z, 0-9, boşluk, tire, alt çizgi, nokta, parantez veya boru karakterlerini kullanarak sorguya bir **ad**verin. 
   
1. Sorgunun **sorgu** veya **işlev**olarak kaydedilip edilmeyeceğini seçin. İşlevler, diğer sorguların başvurulacağını olan sorgulardır. 
   
   Bir sorguyu işlev olarak kaydetmek için, diğer sorguların bu sorguyu çağırmak üzere kullanacağı kısa bir ad olan bir **Işlev diğer**adı sağlayın.
   
1. Bir Log Analytics çalışma alanım varsa sorgu **Gezgini** 'nin sorgu için kullanacağı bir **Kategori** belirtin. (Kategoriler, uygulamalar için Application Insights sorguları için kullanılamaz)
   
1. **Kaydet**’i seçin.
   
   ![İşlevi Kaydet](media/get-started-portal/save-function.png)

### <a name="load-queries"></a>Sorguları yükle
Kaydedilmiş bir sorguyu yüklemek için sağ üst köşedeki **sorgu Gezgini** ' ni seçin. Tüm sorguları kategoriye göre listeleyerek **sorgu Gezgini** bölmesi açılır. Kategorileri genişletin veya arama çubuğuna bir sorgu adı girin, sonra **sorgu düzenleyicisine**yüklemek için bir sorgu seçin. Sorgu adının yanındaki yıldızı seçerek bir sorguyu **Sık kullanılanlara** göre işaretleyebilirsiniz.

![Sorgu Gezgini](media/get-started-portal/query-explorer.png)

### <a name="export-and-share-queries"></a>Sorguları dışarı ve paylaşma
Bir sorguyu dışarı aktarmak için, üstteki çubukta **dışarı aktar** ' ı seçin ve ardından **CSV 'ye aktar-tüm sütunlar**, **CSV tarafından görüntülenmiş sütunlara**Aktar veya **Power BI (mquery) öğesine dışarı aktar** seçeneğini belirleyin.

Aşağıdaki videoda Log Analytics Excel ile tümleştirme gösterilmektedir.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4Asme]

Bir sorguya bir bağlantı paylaşmak için üstteki çubukta **Bağlantıyı Kopyala** ' yı seçin ve ardından **sorguya bağlantıyı kopyala**, **sorgu metnini kopyala**veya **sorgu sonuçlarını** panoya kopyala ' yı seçin. Sorgu bağlantısını aynı çalışma alanına erişimi olan diğerlerine gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Izleyici günlük sorgularını yazma hakkında daha fazla bilgi edinmek için sonraki öğreticiye ilerleyin.
> [!div class="nextstepaction"]
> [Azure Izleyici günlük sorgularını yazma](get-started-queries.md)
