---
title: Azure Monitör'de günlük sorgularına başlayın | Microsoft Dokümanlar
description: Bu makalede, Azure Monitor'da günlük sorguları yazmaya başlamak için bir öğretici verilmektedir.
ms.subservice: logs
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 10/24/2019
ms.openlocfilehash: f56abe2bf6ccea1f55f9b3fe94b75016d449b46b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77670189"
---
# <a name="get-started-with-log-queries-in-azure-monitor"></a>Azure Monitor'da günlük sorgularına başlayın

> [!NOTE]
> En az bir sanal makineden veri topluyorsanız, bu alıştırmayı kendi ortamınızda çalışabilirsiniz. Eğer o zaman bol miktarda örnek veri içeren [Demo ortamımızı](https://portal.loganalytics.io/demo)kullanmazsanız.

Bu eğitimde Azure Monitor'da günlük sorguları yazmayı öğreneceksiniz. Size nasıl öğreteceğiniz:

- Sorgu yapısını anlama
- Sorgu sonuçlarını sıralama
- Filtre sorgu sonuçları
- Bir zaman aralığı belirtin
- Sonuçlara hangi alanların dahil ediletini seçin
- Özel alanları tanımlayın ve kullanın
- Toplam ve grup sonuçları

Azure portalında Log Analytics'i kullanma hakkında bir eğitim için Azure [MonitörÜ Log Analytics'e başlayın'](get-started-portal.md)ı görün.<br>
Azure Monitor'daki günlük sorguları hakkında daha fazla bilgi için Azure [Monitor'daki günlük sorgularına genel bakış](log-query-overview.md)bölümüne bakın.

Aşağıdaki bu öğretici bir video sürümü ile birlikte izleyin:

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE42pGX]

## <a name="writing-a-new-query"></a>Yeni bir sorgu yazma
Sorgular bir tablo adı veya *arama* komutu yla başlayabilir. Sorgu için net bir kapsam tanımladığı ndan ve hem sorgu performansını hem de sonuçların alaka düzeyini iyileştirdiğinden, tablo adı ile başlamalısınız.

> [!NOTE]
> Azure İzleyici tarafından kullanılan Kusto sorgu dili, büyük/küçük harfe duyarlıdır. Dil anahtar sözcükleri genellikle küçük harfle yazılır. Sorguda tablo veya sütun adlarını kullanırken, şema bölmesinde gösterildiği gibi doğru servis talebinin kullandığınızdan emin olun.

### <a name="table-based-queries"></a>Tablo tabanlı sorgular
Azure Monitor, her biri birden çok sütundan oluşan günlük verilerini tablolarda düzenler. Tüm tablolar ve sütunlar Analytics portalındaki Log Analytics'teki şema bölmesinde gösterilir. İlgilendiğiniz bir tabloyu belirleyin ve ardından biraz veriye göz atın:

```Kusto
SecurityEvent
| take 10
```

Yukarıda gösterilen sorgu, belirli bir sırada *SecurityEvent* tablosundan 10 sonuç döndürür. Bu bir tabloya bir göz atın ve yapısını ve içeriğini anlamak için çok yaygın bir yoldur. Nasıl inşa edildigini inceleyelim:

* Sorgu tablo adı *SecurityEvent* ile başlar - bu bölüm sorgunun kapsamını tanımlar.
* Boru (|) karakteri komutları ayırır, böylece aşağıdaki komutun girişinde ilkinin çıktısı. İstediğiniz sayıda borulu eleman ekleyebilirsiniz.
* Boruyu takiben, tablodan belirli sayıda rasgele kayıt döndüren **take** komutudur.

Biz aslında eklemeden `| take 10` bile sorgu çalıştırabilir - bu hala geçerli olurdu, ama 10.000 sonuçlara kadar dönebilir.

### <a name="search-queries"></a>Arama sorguları
Arama sorguları daha az yapılandırılmıştır ve genellikle sütunlarından herhangi birinde belirli bir değer içeren kayıtları bulmak için daha uygundur:

```Kusto
search in (SecurityEvent) "Cryptographic"
| take 10
```

Bu sorgu, "Şifreleme" ifadesini içeren kayıtlar için *SecurityEvent* tablosunda arama lar tutar. Bu kayıtlardan 10 kaydı döndürülür ve görüntülenir. Biz `in (SecurityEvent)` parçası atlayıp sadece çalıştırmak, `search "Cryptographic"`arama daha uzun sürer ve daha az verimli olacak *tüm* tablolar, üzerinden gidecek.

> [!WARNING]
> Daha fazla veri işlemeleri gerekdığından, arama sorguları genellikle tablo tabanlı sorgulardan daha yavaştır. 

## <a name="sort-and-top"></a>Sırala ve üst
Birkaç kayıt almak için **take** yararlı olsa da, sonuçlar seçilir ve belirli bir sırada görüntülenir. Sıralı bir görünüm elde **sort** etmek için, tercih edilen sütuna göre sıralayabilirsiniz:

```Kusto
SecurityEvent   
| sort by TimeGenerated desc
```

Bu rağmen çok fazla sonuç döndürebilir ve aynı zamanda biraz zaman alabilir. Yukarıdaki *sorgu, tüm* SecurityEvent tablosunu TimeGenerated sütununa göre sıralar. Analytics portalı daha sonra ekranı yalnızca 10.000 kayıt göstermek için sınırlar. Bu yaklaşım tabii ki optimal değildir.

Yalnızca en son 10 kaydı elde etmenin en iyi yolu, sunucu tarafındaki tüm tabloyu sıralayan ve daha sonra en iyi kayıtları döndüren **üst**düzey eki kullanmaktır:

```Kusto
SecurityEvent
| top 10 by TimeGenerated
```

Azalan varsayılan sıralama sırasıdır, bu nedenle genellikle **desc** bağımsız değişkenini atlarız. Çıktı şu şekilde görünecektir:

![En İyi 10](media/get-started-queries/top10.png)


## <a name="where-filtering-on-a-condition"></a>Nerede: bir koşulda filtreleme
Filtreler, adlarıyla gösterildiği gibi, verileri belirli bir koşula göre filtreler. Bu, sorgu sonuçlarını ilgili bilgilerle sınırlamanın en yaygın yoludur.

Bir sorguya filtre eklemek için, bir veya daha fazla koşulun izlediği **yeri** operatör kullanın. Örneğin, aşağıdaki sorgu yalnızca _Düzey_ _8'e_eşit olan *SecurityEvent* kayıtlarını döndürür:

```Kusto
SecurityEvent
| where Level == 8
```

Filtre koşullarını yazarken aşağıdaki ifadeleri kullanabilirsiniz:

| İfadeler | Açıklama | Örnek |
|:---|:---|:---|
| == | Eşitliği kontrol edin<br>(büyük/küçük harf duyarlı) | `Level == 8` |
| =~ | Eşitliği kontrol edin<br>(büyük/küçük harf duyarsız) | `EventSourceName =~ "microsoft-windows-security-auditing"` |
| !=, <> | Eşitsizliği kontrol edin<br>(her iki ifade de aynıdır) | `Level != 4` |
| *ve*, *veya* | Koşullar arasında gerekli| `Level == 16 or CommandLine != ""` |

Birden çok koşula göre filtre lemek için, aşağıdakileri **kullanabilirsiniz:**

```Kusto
SecurityEvent
| where Level == 8 and EventID == 4672
```

veya boru birden fazla **elemanları** birbiri ardına:

```Kusto
SecurityEvent
| where Level == 8 
| where EventID == 4672
```
    
> [!NOTE]
> Değerler farklı türlere sahip olabilir, bu nedenle doğru türde karşılaştırma yapmak için bunları döküm gerekebilir. Örneğin, SecurityEvent *Düzey* sütunu String türündendir, bu nedenle üzerinde sayısal işleçler kullanabilmeniz için önce onu *int* veya *long*gibi sayısal bir türe dökmeniz gerekir:`SecurityEvent | where toint(Level) >= 10`

## <a name="specify-a-time-range"></a>Bir zaman aralığı belirtin

### <a name="time-picker"></a>Zaman seçici
Zaman seçici Çalıştır düğmesinin yanındadır ve yalnızca son 24 saate ait kayıtları sorguladiğimizı gösterir. Bu, tüm sorgulara uygulanan varsayılan zaman aralığıdır. Yalnızca son saate ait kayıtları almak için _Son saat'i_ seçin ve sorguyu yeniden çalıştırın.

![Saat Seçici](media/get-started-queries/timepicker.png)


### <a name="time-filter-in-query"></a>Sorguda zaman filtresi
Sorguya bir zaman filtresi ekleyerek kendi zaman aralığınızı da tanımlayabilirsiniz. Zaman filtresini tablo adından hemen sonra yerleştirmek en iyisidir: 

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m) 
| where toint(Level) >= 10
```

Yukarıdaki zaman filtresi `ago(30m)` "30 dakika önce" anlamına gelir, bu nedenle bu sorgu yalnızca son 30 dakikadaki kayıtları döndürür. Diğer zaman birimleri gün (2b), dakika (25m) ve saniye (10s) içerir.


## <a name="project-and-extend-select-and-compute-columns"></a>Proje ve Genişlet: sütunları seçin ve hesapla
Sonuçlara eklemek için belirli sütunları seçmek için **projeyi** kullanın:

```Kusto
SecurityEvent 
| top 10 by TimeGenerated 
| project TimeGenerated, Computer, Activity
```

Önceki örnek bu çıktıyı oluşturur:

![Proje sonuçlarını sorgula](media/get-started-queries/project.png)

**Sütunları** yeniden adlandırmak ve yenilerini tanımlamak için project'i de kullanabilirsiniz. Aşağıdaki örnek, aşağıdakileri yapmak için projeyi kullanır:

* Yalnızca *Bilgisayar* ve *TimeGenerated* orijinal sütunlarını seçin.
* *Etkinlik* sütununu *EventDetails*olarak yeniden adlandırın.
* *EventCode*adında yeni bir sütun oluşturun. **Substring()** işlevi, Etkinlik alanından yalnızca ilk dört karakteri almak için kullanılır.


```Kusto
SecurityEvent
| top 10 by TimeGenerated 
| project Computer, TimeGenerated, EventDetails=Activity, EventCode=substring(Activity, 0, 4)
```

**genişletme,** tüm özgün sütunları sonuç kümesinde tutar ve ek sütunları tanımlar. Aşağıdaki *sorgu, EventCode* sütununa eklemek için **genişlet'i** kullanır. Bu sütunun tablo sonuçlarının sonunda görüntülenemeyebilir ve bu durumda onu görüntülemek için kaydın ayrıntılarını genişletmeniz gerekeceğini unutmayın.

```Kusto
SecurityEvent
| top 10 by TimeGenerated
| extend EventCode=substring(Activity, 0, 4)
```

## <a name="summarize-aggregate-groups-of-rows"></a>Özetlemek: toplam satır grupları
Bir veya daha fazla sütuna göre kayıt gruplarını tanımlamak ve bunlara toplulaştırmauygulamak için **özetle'yi** kullanın. **Özetin** en yaygın kullanımı, her gruptaki sonuç sayısını döndüren *sayımdır.*

Aşağıdaki sorgu, son saatteki tüm *Perf* kayıtlarını inceler, *ObjectName'ye*göre gruplanır ve her gruptaki kayıtları sayar: 
```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize count() by ObjectName
```

Bazen grupları birden çok boyuta göre tanımlamak mantıklı dır. Bu değerlerin her benzersiz birleşimi ayrı bir grup tanımlar:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize count() by ObjectName, CounterName
```

Başka bir yaygın kullanım her grup üzerinde matematiksel veya istatistiksel hesaplamalar gerçekleştirmektir. Örneğin, aşağıdakiher bilgisayar için ortalama *Karşı Değer* hesaplar:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize avg(CounterValue) by Computer
```

Ne yazık ki, farklı performans sayaçlarını bir araya getirdiğimiz için bu sorgunun sonuçları anlamsızdır. Bunu daha anlamlı hale getirmek için, *CounterName* ve *Computer'ın*her bir kombinasyonu için ortalamayı ayrı ayrı hesaplamalıyız:

```Kusto
Perf
| where TimeGenerated > ago(1h)
| summarize avg(CounterValue) by Computer, CounterName
```

### <a name="summarize-by-a-time-column"></a>Zaman sütununa göre özetle
Gruplandırma sonuçları, bir zaman sütununa veya başka bir sürekli değere de dayalı olabilir. Ancak özetleme, `by TimeGenerated` zaman aralığındaki her bir milisaniye için gruplar oluşturur, çünkü bunlar benzersiz değerlerdir. 

Sürekli değerlere dayalı gruplar oluşturmak için, aralığı **depo gözü**kullanarak yönetilebilir birimlere ayırmak en iyisidir. Aşağıdaki sorgu, belirli bir bilgisayarda boş belleği *(Kullanılabilir MBytes)* ölçen *Perf* kayıtlarını çözümler. Son 7 gün içinde her 1 saatlik sürenin ortalama değerini hesaplar:

```Kusto
Perf 
| where TimeGenerated > ago(7d)
| where Computer == "ContosoAzADDS2" 
| where CounterName == "Available MBytes" 
| summarize avg(CounterValue) by bin(TimeGenerated, 1h)
```

Çıktıyı daha net hale getirmek için, zaman içinde kullanılabilir belleği gösteren bir zaman çizelgesi olarak görüntülemeyi seçersiniz:

![Zaman içinde bellek sorgusu](media/get-started-queries/chart.png)



## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitor günlük sorgularında dizeleri ile Work](string-operations.md)ile günlük sorgusunda dize verilerini kullanma hakkında daha fazla bilgi edinin.
- [Azure Monitor günlük sorgularında Gelişmiş toplamalarla](advanced-aggregations.md)bir günlük sorgusunda veri toplama hakkında daha fazla bilgi edinin.
- [Azure Monitor günlük sorgularında Joins](joins.md)ile birden çok tablodan verilere nasıl katılacağınızı öğrenin.
- [KQL dil referanstüm](/azure/kusto/query/)Kusto sorgu dili ile ilgili belgeleri alın.
