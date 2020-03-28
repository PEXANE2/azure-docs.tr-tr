---
title: 'Öğretici: Log Analytics sorgularına başlayın'
description: Azure portalında Log Analytics'i kullanarak Azure Monitor günlük sorgularını nasıl yazacağımı ve yönettiğinizi bu öğreticiden öğrenin.
ms.subservice: logs
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 03/17/2020
ms.openlocfilehash: 29e24166218a6757cded9d1b002321800ab0c073
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80055614"
---
# <a name="tutorial-get-started-with-log-analytics-queries"></a>Öğretici: Log Analytics sorgularına başlayın

Bu öğretici, Azure portalında Azure Monitor günlük sorgularını yazmak, yürütmek ve yönetmek için Log Analytics'i nasıl kullanacağınızı gösterir. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve verilerinizden daha birçok öngörü sağlamak için Günlük Analizi sorgularını kullanabilirsiniz. 

Bu eğitimde, Log Analytics'i nasıl kullanacağınızı şunları için öğreneceksiniz:

> [!div class="checklist"]
> * Günlük veri şemasını anlama
> * Basit sorgular yazın ve çalıştırın ve sorgular için zaman aralığını değiştirin
> * Filtreleme, sıralama ve grup sorgusu sonuçları
> * Sorgu sonuçlarının görsellerini görüntüleme, değiştirme ve paylaşma
> * Sorguları ve sonuçları kaydetme, yükleme, dışa aktarma ve kopyalama

Günlük sorguları hakkında daha fazla bilgi için [Azure Monitor'daki günlük sorgularına genel bakış](log-query-overview.md)bölümüne bakın.<br/>
Günlük sorguları yazma yla ilgili ayrıntılı bir öğretici için Azure [Monitor'da günlük sorgularına başlayın'a](get-started-queries.md)bakın.

## <a name="open-log-analytics"></a>Açık Günlük Analizi
Log Analytics'i kullanmak için bir Azure hesabında oturum açmanız gerekir. Azure hesabınız yoksa, [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

Bu öğreticideki adımların çoğunu tamamlamak için, bol miktarda örnek veri içeren [bu demo ortamını](https://portal.loganalytics.io/demo)kullanabilirsiniz. Demo ortamı yla, sorguları kaydedemez veya sonuçları panoya sabitleyemezsin.

En az bir Azure kaynağında günlük verileri toplamak için Azure Monitor kullanıyorsanız, kendi ortamınızı da kullanabilirsiniz. Bir Log Analytics çalışma alanı açmak için Azure Monitor'unuzda niçin **Günlükler'i**seçin. 

## <a name="understand-the-schema"></a>Şemayı anlama
*Şema,* mantıksal kategoriler altında gruplanmış tablolar topluluğudur. Demo şeması, izleme çözümlerinden çeşitli kategorilere sahiptir. Örneğin, **LogManagement** kategorisi Windows ve Syslog olayları, performans verileri ve aracı kalp atışlarını içerir.

Şema tabloları, Log Analytics çalışma alanının **Tablolar** sekmesinde görünür. Tablolar, her biri sütun adının yanındaki simgeyle gösterilen bir veri türüne sahip sütunlar içerir. Örneğin, **Olay** tablosu **Bilgisayar** gibi metin sütunları ve **EventCategory**gibi sayısal sütunlar içerir.

![Şema](media/get-started-portal/schema.png)

## <a name="write-and-run-basic-queries"></a>Temel sorguları yazma ve çalıştırma

Log Analytics **Sorgu düzenleyicisinde**yeni bir boş sorguyla açılır.

![Log Analytics](media/get-started-portal/homepage.png)

### <a name="write-a-query"></a>Sorgu yazma
Azure Monitor günlük sorguları, Kusto sorgu dilinin bir sürümünü kullanır. Sorgular bir tablo adı veya [arama](/azure/kusto/query/searchoperator) komutu yla başlayabilir. 

Aşağıdaki sorgu **Olay** tablosundan tüm kayıtları alır:

```Kusto
Event
```

Boru (|) karakteri komutları ayırır, bu nedenle ilk komutun çıktısı bir sonraki komutun girişidir. Tek bir sorguya istediğiniz sayıda komut ekleyebilirsiniz. Aşağıdaki **sorgu, Olay** tablosundan kayıtları alır ve sonra herhangi bir özellikte terim **hatası** için onları arar:

```Kusto
Event 
| search "error"
```

Tek satır sonu, sorguların okunmasını kolaylaştırır. Birden fazla satır sonu sorguyu ayrı sorgulara böler.

Aynı sorguyazmak için başka bir yolu:

```Kusto
search in (Event) "error"
```

İkinci örnekte, **arama** komutu yalnızca **Olaylar** tablosundaki kayıtları terim **hatası**için arar.

Varsayılan olarak, Log Analytics sorguları son 24 saatlik bir zaman aralığıyla sınırlar. Farklı bir zaman aralığı ayarlamak için, sorguya açık bir **TimeGenerated** filtresi ekleyebilir veya **Zaman aralığı** denetimini kullanabilirsiniz.

### <a name="use-the-time-range-control"></a>Zaman aralığı denetimini kullanma
**Zaman aralığı** denetimini kullanmak için, üst çubukta seçin ve ardından açılır listeden bir değer seçin veya özel bir zaman aralığı oluşturmak için **Özel'i** seçin.

![Zaman seçici](media/get-started-portal/time-picker.png)

- Zaman aralığı değerleri UTC'dedir ve bu da yerel saat diliminizden farklı olabilir.
- Sorgu açıkça **TimeGenerated**için bir filtre ayarlarsa , zaman seçici denetimi **sorguda Ayarlanan'ı**gösterir ve çakışmayı önlemek için devre dışı bırakılır.

### <a name="run-a-query"></a>Sorgu çalıştırma
Sorgu çalıştırmak için imlecinizi sorgunun içinde bir yere yerleştirin ve üst çubukta **Çalıştır'ı** seçin veya **Shift**+**Enter**tuşuna basın. Sorgu boş bir satır bulana kadar çalışır.

## <a name="filter-results"></a>Sonuçları filtreleme
Log Analytics sonuçları en fazla 10.000 kayıtla sınırlandırıyor. Gibi `Event` genel bir sorgu yararlı olmak için çok fazla sonuç döndürür. Sorgudaki tablo öğelerini kısıtlayarak veya sonuçlara açıkça filtre ekleyerek sorgu sonuçlarını filtreleyebilirsiniz. Tablo öğeleri üzerinden filtreleme yeni bir sonuç kümesi döndürür, açık bir filtre varolan sonuç kümesi için geçerlidir.

### <a name="filter-by-restricting-table-elements"></a>Tablo öğelerini kısıtlayarak filtreleme
Sorgudaki `Event` tablo öğelerini kısıtlayarak sorgu sonuçlarını **Hata** olaylarına filtrelemek için:

1. Sorgu sonuçlarında, **EventLevelName** sütununda **Hata** olan herhangi bir kaydın yanındaki açılır ok'u seçin. 
   
1. Genişletilmiş ayrıntılarda, üzerinde gezinmek ve **EventLevelName**yanında **...** seçin ve sonra **"Hata" ekle'yi**seçin. 
   
   ![Sorguya filtre ekleme](media/get-started-portal/add-filter.png)
   
1. **Sorgu** düzenleyicisindeki sorgunun şimdi aşağıdaki şekilde değiştirildiğe dikkat edin:
   
   ```Kusto
   Event
   | where EventLevelName == "Error"
   ```
   
1. Yeni sorguyu çalıştırmak için **Çalıştır'ı** seçin.

### <a name="filter-by-explicitly-filtering-results"></a>Sonuçları açıkça filtreleyerek filtreleme
`Event` Sorgu sonuçlarını filtreleyerek sorgu sonuçlarını **Hata** olaylarına filtrelemek için:

1. Sorgu sonuçlarında, **EventLevelName**başlığı altında sütunun yanındaki **Filtre** simgesini seçin. 
   
1. Açılır pencerenin ilk alanında, **"Is"** seçeneğini belirleyin ve bir sonraki alana *hata*girin. 
   
1. **Filtre'yi**seçin.
   
   ![Filtre](media/get-started-portal/filter.png)

## <a name="sort-group-and-select-columns"></a>Sütunları sırala, gruplandırma ve seçme
Sorgu sonuçlarını **TimeGenerated [UTC]** gibi belirli bir sütuna göre sıralamak için sütun başlığını seçin. Artan ve azalan sıra arasında geçiş yapmak için başlığı yeniden seçin.

![Sütunu sıralama](media/get-started-portal/sort-column.png)

Sonuçları düzenlemenin başka bir yolu da gruplara göredir. Sonuçları belirli bir sütuna göre gruplandırmak için, sütun üstbilgisini, sütun üstbilgisini sürükleme etiketli sonuç tablosunun üzerindeki çubuğuna sürükleyin **ve bu sütuna göre gruplandırmak için buraya bırakın.** Alt gruplar oluşturmak için diğer sütunları üst çubuğuna sürükleyin. Hiyerarşiyi ve çubuktaki grupların ve alt grupların sırasını yeniden düzenleyebilirsiniz.

![Gruplar](media/get-started-portal/groups.png)

Sonuçlardaki sütunları gizlemek veya göstermek için tablonun üzerindeki **Sütunlar'ı** seçin ve ardından açılan listeden istediğiniz sütunları seçin veya seçin.

![Sütun seçme](media/get-started-portal/select-columns.png)

## <a name="view-and-modify-charts"></a>Grafikleri görüntüleme ve değiştirme
Sorgu sonuçlarını görsel biçimlerde de görebilirsiniz. Örnek olarak aşağıdaki sorguyu girin:

```Kusto
Event 
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1d) 
| summarize count() by Source 
```

Varsayılan olarak, sonuçlar bir tabloda görünür. Sonuçları grafik görünümünde görmek için tablonun üstündeki **Grafik'i** seçin.

![Çubuk Grafik](media/get-started-portal/bar-chart.png)

Sonuçlar yığılmış çubuk grafikte görünür. Sonuçların diğer görünümlerini göstermek için **Yığılmış Sütun** veya **Pasta** gibi diğer seçenekleri seçin.

![Pasta grafiği](media/get-started-portal/pie-chart.png)

Görünümün x ve y eksenleri gibi özelliklerini veya denetim çubuğundan el ile ayırma ve bölme tercihlerini değiştirebilirsiniz.

İşle işlecini kullanarak sorgunun kendisinde [render](/azure/kusto/query/renderoperator) tercih edilen görünümü de ayarlayabilirsiniz.

## <a name="pin-results-to-a-dashboard"></a>Sonuçları panoya sabitleme
Bir sonuç tablosunu veya grafiği Log Analytics'ten paylaşılan bir Azure panosuna sabitlemek için üst çubuktaki **panoya** Sabitle'yi seçin. 

![Panoya sabitle](media/get-started-portal/pin-dashboard.png)

Başka **bir pano bölmesine** Pin'de, sabitlemek için paylaşılan bir pano seçin veya oluşturun ve **Uygula'yı**seçin. Tablo veya grafik, seçili Azure panosunda görünür.

![Panoya sabitlenmiş grafik](media/get-started-portal/pin-dashboard2.png)

Paylaşılan bir panoya sabitlediğiniz bir tablo veya grafik aşağıdaki basitleştirmelere sahiptir: 

- Veriler son 14 günle sınırlıdır.
- Bir tablo da yalnızca en fazla dört sütun ve en üstteki yedi satırı gösterir.
- Birçok ayrı kategoriye sahip grafikler, daha az nüfuslu kategorileri otomatik olarak tek bir **başkası** kutusu olarak gruplandırır.

## <a name="save-load-or-export-queries"></a>Sorguları kaydetme, yükleme veya dışa aktarma
Bir sorgu oluşturduktan sonra, sorguyu veya sonuçları başkalarıyla kaydedebilir veya paylaşabilirsiniz. 

### <a name="save-queries"></a>Sorguları kaydet
Bir sorgukaydetmek için:

1. Üst çubukta **Kaydet'i** seçin.
   
1. **Kaydet** iletişim kutusunda, a-z, A-Z, 0-9, boşluk, tire, alt puan, dönem, parantez veya boru karakterlerini kullanarak sorguya bir **Ad**verin. 
   
1. **Sorguyu Sorgu** olarak mı yoksa **Işlev**olarak mı kaydedin. İşlevler, diğer sorguların başvurulabileceği sorgulardır. 
   
   Bir sorguyı bir iþlev olarak kaydetmek için, bu sorguyı aramak için kullanılabilen diğer sorgular için kısa bir ad olan Bir **ışık Takma**Ad sağlayın.
   
1. Sorgu **için** kullanılacak sorgu **gezgini** kategorisi sağlayın.
   
1. **Kaydet'i**seçin.
   
   ![Fonksiyonu kaydet](media/get-started-portal/save-function.png)

### <a name="load-queries"></a>Yükleme sorguları
Kaydedilen bir sorguyüklemek için sağ üstteki **Sorgu gezgini'ni** seçin. **Sorgu gezgini** bölmesi, tüm sorguları kategoriye göre listeler. Kategorileri genişletin veya arama çubuğuna bir sorgu adı girin, ardından **sorgu düzenleyicisine**yüklemek için bir sorgu seçin. Sorgu adının yanındaki yıldızı seçerek sorguyu **Sık Kullanılan** olarak işaretleyebilirsiniz.

![Sorgu gezgini](media/get-started-portal/query-explorer.png)

### <a name="export-and-share-queries"></a>Sorguları dışa aktarma ve paylaşma
Sorgu dışa aktarmak için üst çubukta **Dışa** Aktar'ı seçin ve ardından **CSV'ye Dışa Aktar'ı**seçin - tüm sütunlar , **CSV'ye dışa aktarma - sütunları görüntüle (Sütunları**Güç **BI 'ye (M sorgusu)** açılır listeden dışa aktarın.

Sorgu bağlantısını paylaşmak için üst çubuktaki **Kopyala bağlantısını** seçin ve ardından **sorgu için Kopyala bağlantısını**seçin, sorgu **metnini kopyalayın**veya sorgu sonuçlarını panoya kopyalamak için **kopyalayın.** Sorgu bağlantısını aynı çalışma alanına erişimi olan diğer herkese gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Monitor günlük sorguları yazma hakkında daha fazla bilgi edinmek için bir sonraki öğreticiye ilerleyin.
> [!div class="nextstepaction"]
> [Azure Monitör günlük sorguları yazma](get-started-queries.md)
