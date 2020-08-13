---
title: Tek bir ölçüm uyarı kuralında birden çok zaman serisini izleme
description: Birden çok zaman serisi için tek bir uyarı kuralı kullanarak ölçeğe uyar
author: harelbr
ms.author: harelbr
ms.topic: conceptual
ms.date: 08/11/2020
ms.subservice: alerts
ms.openlocfilehash: f7ca91ca49d9357285e1307c5051ef5685ad24c9
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88187120"
---
# <a name="monitor-multiple-time-series-in-a-single-metric-alert-rule"></a>Tek bir ölçüm uyarı kuralında birden çok zaman serisini izleme

Tek bir ölçüm uyarısı kuralı, bir veya daha fazla ölçüm zaman serisini izlemek için kullanılabilir ve bu da kaynakları ölçekteki izlemeye daha kolay hale getirir.

## <a name="metric-time-series"></a>Ölçüm süresi-serisi

Ölçüm zaman serisi, bir süre içinde yakalanan bir dizi ölçümdür (veya "ölçüm değerleri"). 

Örnek:

- Bir sanal makinenin CPU kullanımı
- Bir depolama hesabına gelen baytlar (giriş)
- Bir Web uygulamasının başarısız isteklerinin sayısı



## <a name="alert-rule-on-a-single-time-series"></a>Tek bir zaman serisinde uyarı kuralı
Bir uyarı kuralı, aşağıdaki tüm koşullara uyan tek bir zaman serisini izler:
-   Kural tek bir hedef kaynağı izler 
-   Tek bir koşul içerir
-   Boyutları seçmeden bir ölçümü değerlendirir (ölçümün boyutları desteklediği varsayılarak)

Böyle bir uyarı kuralına örnek (yalnızca gösterilen ilgili özelliklerle birlikte):
-   Hedef kaynak: *myVM1*
-   Ölçüm: *CPU yüzdesi*
-   İşleç: *büyüktür*
-   Eşik: *70*


Bu uyarı kuralı için tek bir ölçüm zaman serisi izlenir:
-   *Kaynak*= ' myVM1 ' (%70) > CPU yüzdesi

![Tek bir zaman serisi üzerinde bir uyarı kuralı](media/alerts-metric-multiple-time-series-single-rule/simple-alert-rule.png)

## <a name="alert-rule-on-multiple-time-series"></a>Birden çok zaman serisinde uyarı kuralı
Bir uyarı kuralı, aşağıdaki özelliklerden en az birini kullanıyorsa birden çok zaman serisini izler: 
-   Birden çok kaynak
-   Birden çok koşul 
-   Birden çok boyut


## <a name="multiple-resources-multi-resource"></a>Birden çok kaynak (çoklu kaynak)

Tek bir ölçüm uyarısı kuralı, kaynakların aynı türde olması ve aynı Azure bölgesinde mevcut olması şartıyla birden çok kaynağı izleyebilir. Bu kural türünün kullanılması karmaşıklığı azaltır ve bakımını yapmak için kullandığınız uyarı kurallarının toplam sayısını azaltır. 

Böyle bir uyarı kuralına örnek:
-   Hedef kaynak: *myVM1, myVM2*
-   Ölçüm: *CPU yüzdesi*
-   İşleç: *büyüktür*
-   Eşik: *70*

Bu uyarı kuralı için iki ölçüm zaman serisi ayrı olarak izlenir:
-   *Kaynak*= ' myVM1 ' (%70) > CPU yüzdesi
-   *Kaynak*= ' myVM2 ' (%70) > CPU yüzdesi

![Çoklu kaynak uyarı kuralı](media/alerts-metric-multiple-time-series-single-rule/multi-resource-alert-rule.png)
 
Birden çok kaynak uyarı kuralında, koşul her bir kaynak için **ayrı olarak** değerlendirilir (veya her kaynağa karşılık gelen ölçüm zaman serisinin her biri için daha doğru). Bu, uyarıların her kaynak için ayrı olarak da tetiklenme anlamına gelir.

Örneğin, yukarıdaki %70 ' ün üzerinde CPU için izleme için yukarıdaki uyarı kuralını belirlediğimiz varsayılmıştır. Değerlendirilen zaman diliminde (yani, son 5 dakika)
-   *MyVM1* 'In *CPU yüzdesi* %70 ' den büyük 
-   *MyVM2* 'In *CPU yüzdesi* %50 ' dir

Uyarı kuralı *myVM1*üzerinde tetiklenir, ancak *myVM2*değil. Tetiklenen bu uyarılar bağımsızdır. Ayrıca, her bir sanal makinenin ayrı davranışına bağlı olarak farklı zamanlarda da çözüm alabilir.

Bu özellik için desteklenen çoklu kaynak uyarı kuralları ve kaynak türleri hakkında daha fazla bilgi için bkz. [Azure izleyici 'de ölçüm uyarılarını kullanarak ölçekte izleme](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor).

> [!NOTE] 
> Birden çok kaynağı izleyen bir ölçüm uyarı kuralında yalnızca tek bir koşula izin verilir.

## <a name="multiple-conditions-multi-condition"></a>Birden çok koşul (çoklu koşul)

Tek bir ölçüm uyarı kuralı, uyarı kuralı başına en fazla beş koşul da izleyebilir. 

Örnek:

- Hedef kaynak: *myVM1*
- Condition1
  - Ölçüm: *CPU yüzdesi*
  - İşleç: *büyüktür*
  - Eşik: *70*
- Condition2
  - Ölçüm: *Toplam ağ*
  - İşleç: *büyüktür*
  - Eşik: *20 MB*

Bu uyarı kuralı için iki ölçüm zaman serisi izleniyor:

- *Kaynak*= ' myVM1 ' (%70) > CPU yüzdesi
- Toplam *kaynak*= ' myVM1 ' > 20 MB

![Çok durumlu bir uyarı kuralı](media/alerts-metric-multiple-time-series-single-rule/multi-condition-alert-rule.png)
 
Koşullar arasında bir ' AND ' işleci kullanılır. **Tüm** koşullar karşılandığında uyarı kuralı bir uyarı tetikler. Tetiklenen uyarı, koşullardan en az birinin artık karşılanmamaları durumunda çözümlenir. 

> [!NOTE]
> Birden çok koşula sahip bir uyarı kuralında boyutlar kullanılırken kısıtlamalar vardır. Daha fazla bilgi için bkz. [birden çok koşula sahip bir ölçüm uyarı kuralında boyutlar kullanırken kısıtlamalar](alerts-troubleshoot-metric.md#restrictions-when-using-dimensions-in-a-metric-alert-rule-with-multiple-conditions).


## <a name="multiple-dimensions-multi-dimension"></a>Birden çok boyut (çok boyutlu)

Tek bir ölçüm uyarı kuralı, bir ölçümün birden çok boyut değerini de izleyebilir. Ölçüm boyutları, ölçüm değerini tanımlayan ek verileri taşıyan ad-değer çiftleridir. Örneğin, bir depolama hesabının **işlem** ölçümü, **API adı**adlı bir boyuta sahiptir ve her BIR işlem tarafından çağrılan API 'nin adını tanımlar (örneğin, GetBlob, DeleteBlob, putpage). Boyutların kullanımı isteğe bağlıdır, ancak ölçüyü filtrelemeye ve yalnızca belirli zaman serisini izlemeye izin verir, böylece ölçüm tüm boyut değerlerinin bir toplamı olarak birlikte konur. 

Örneğin, işlem sayısı tüm API adlarında (toplanan veriler) yüksek olduğunda bir uyarının tetiklenmesini seçebilirsiniz veya belirli API adları için işlem sayısı yüksek olduğunda yalnızca uyarıya daha fazla bölün. 

Birden çok boyutu izleyen bir uyarı kuralı örneği şunlardır:

- Hedef kaynak: *myStorage1*
- Ölçüm: *işlemler*
- Boyutlar
  * API adı = *GetBlob, DeleteBlob, PutPage*
- İşleç: *büyüktür*
- Eşik: *70*

Bu uyarı kuralı için üç ölçüm zaman serisi izleniyor:

- *Resource*= ' myStorage1 ' ve *API Name*= ' GetBlob ' adlı işlemler > 70
- *Resource*= ' myStorage1 ' ve *API Name*= ' DeleteBlob ' olan işlemler > 70
- *Resource*= ' myStorage1 ' ve *API Name*= ' putpage ' adlı işlemler > 70

![Bir boyuttan değerleri olan çok boyutlu bir uyarı kuralı](media/alerts-metric-multiple-time-series-single-rule/multi-dimension-1.png)

Çok boyutlu ölçüm uyarısı kuralı, bir ölçümün **farklı** boyutlarından birden çok boyut değerini de izleyebilir. Bu durumda, uyarı kuralı, seçilen boyut değerlerinin tüm boyut değer birleşimlerini **ayrı ayrı** izler.

Bu tür bir uyarı kuralına örnek:

- Hedef kaynak: *myStorage1*
- Ölçüm: *işlemler*
- Boyutlar
  * API adı = *GetBlob, DeleteBlob, PutPage*
  * Kimlik doğrulaması = *SAS, AccountKey*
- İşleç: *büyüktür*
- Eşik: *70*

Bu uyarı kuralı için, altı ölçüm zaman serisi ayrı olarak izlenir:

- *Resource*= ' myStorage1 ' ve *API Name*= ' GetBlob ' ve *Authentication*= ' SAS ' olan işlemler > 70
- *Resource*= ' myStorage1 ' ve *API Name*= ' GetBlob ' ve *Authentication*= ' accountkey ' olan işlemler > 70
- *Resource*= ' myStorage1 ' ve *API Name*= ' DeleteBlob ' ve *Authentication*= ' SAS ' olan işlemler 70 >
- *Resource*= ' myStorage1 ' ve *API Name*= ' DeleteBlob ' ve *Authentication*= ' accountkey ' olan işlemler > 70
- *Resource*= ' myStorage1 ' ve *API Name*= ' Putpage ' ve *Authentication*= ' SAS ' olan işlemler 70 >
- *Resource*= ' myStorage1 ' ve *API Name*= ' Putpage ' ve *Authentication*= ' accountkey ' olan işlemler 70 >

![Birden çok boyuttan değerleri olan çok boyutlu bir uyarı kuralı](media/alerts-metric-multiple-time-series-single-rule/multi-dimension-2.png)
 
### <a name="advanced-multi-dimension-features"></a>Gelişmiş çok boyutlu Özellikler

1.  **Tüm geçerli ve gelecekteki boyutları seçme** – gelecekteki değerler de dahil olmak üzere bir boyutun tüm olası değerlerini izlemeyi tercih edebilirsiniz. Bu tür bir uyarı kuralı, her bir boyut değeri eklendiğinde veya kaldırıldığında uyarı kuralını değiştirmenize gerek kalmadan, boyutun tüm değerlerini izlemek için otomatik olarak ölçeklendirilir.
2.  **Boyutları hariç tutma** : bir boyut değeri için ' ≠ ' (hariç tutma) işlecini seçmek, gelecekteki değerler dahil olmak üzere bu boyutun diğer tüm değerlerini seçmeye eşdeğerdir.
3.  **Yeni ve özel boyutlar** : Azure Portal görüntülenen boyut değerleri, son üç gün içinde toplanan ölçüm verilerine dayalıdır. Aradığınız boyut değeri henüz yayılmazsa, özel bir boyut değeri ekleyebilirsiniz.

![Gelişmiş çok boyutlu Özellikler](media/alerts-metric-multiple-time-series-single-rule/advanced-features.png)


## <a name="metric-alerts-pricing"></a>Ölçüm uyarıları fiyatlandırması

Ölçüm uyarı kurallarının fiyatlandırması, [Azure izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)bulunabilir.

Ölçüm uyarı kuralı oluştururken, belirtilen fiyat tahmini seçili özelliklere ve kural yapılandırmasından ve geçerli ölçüm değerlerinden belirlenen izlenen zaman serisi sayısına göre belirlenir. Ancak aylık ücretler, zaman serisinin gerçek değerlendirmelerine dayanır ve bu nedenle, bazı zaman serilerinin değerlendirmek için veri yoksa ve uyarı kuralı dinamik olarak ölçeklendirilmesine olanak sağlayan özellikler kullanıyorsa, orijinal tahmine göre farklılık gösterebilir.

Örneğin, bir uyarı kuralı çok boyutlu özellikten yararlandığında yüksek fiyatlı bir tahmin gösterebilir ve çok sayıda boyut değeri kombinasyonu seçilirse, bu da birçok zaman serisini izlemeye neden olur. Ancak, boyut değerleri birleşimlerinden kaynaklanan tüm zaman serilerinin, gerçekten değerlendirmek üzere verileri yoksa, bu uyarı kuralının gerçek ücreti daha düşük olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Ölçüm uyarılarını ve [dinamik eşikleri](alerts-dynamic-thresholds.md)kullanarak ölçeğe göre izleme hakkında daha fazla bilgi edinin.
