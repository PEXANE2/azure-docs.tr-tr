---
title: Eşleme veri akışlarını izleme
description: Azure Data Factory eşleme verileri akışlarını görsel olarak izleme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/19/2020
ms.openlocfilehash: 77dda42b27aa6f5fb505fe65667876523cb3f5d2
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88650902"
---
# <a name="monitor-data-flows"></a>Veri akışlarını izleme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri akışınızı oluşturma ve hata ayıklamayı tamamladıktan sonra, veri akışınızı bir işlem hattı bağlamı içindeki bir zamanlamaya göre yürütülecek şekilde zamanlamak istersiniz. Azure Data Factory işlem hattını Tetikleyicileri kullanarak zamanlayabilirsiniz. Ya da işlem hattı bağlamında veri akışınızı test etmek için tek çalıştırma yürütme yürütmek üzere Azure Data Factory işlem hattı Oluşturucusisinden şimdi Tetikle seçeneğini kullanabilirsiniz.

İşlem hattınızı yürüttüğünüzde, işlem hattını ve veri akışı etkinliği dahil olmak üzere işlem hattında bulunan tüm etkinlikleri izleyebilirsiniz. Sol taraftaki Azure Data Factory UI panelinde bulunan izleyici simgesine tıklayın. Aşağıdakine benzer bir ekran görebilirsiniz. Vurgulanan simgeler, veri akışı etkinliği dahil olmak üzere işlem hattındaki etkinliklerin ayrıntılarına gitmenizi sağlar.

![Veri akışı Izleme](media/data-flow/mon001.png "Veri Akışını İzleme")

Bu düzeyde istatistikler ve çalışma süreleri ve durumu da dahil olmak üzere istatistikleri görürsünüz. Etkinlik düzeyindeki çalıştırma KIMLIĞI, işlem hattı düzeyindeki çalıştırma KIMLIĞINDEN farklı. Önceki düzeydeki çalıştırma KIMLIĞI işlem hattı içindir. Duyun seçilmesi, veri akışı yürütmenize ilişkin ayrıntılı ayrıntılar sağlar.

![Veri akışı Izleme](media/data-flow/monitoring-details.png "Veri Akışını İzleme")

Grafik düğüm izleme görünümünde olduğunuzda, veri akışı grafiklerinizin yalnızca Basitleştirilmiş bir görünüm sürümünü görebilirsiniz.

![Veri akışı Izleme](media/data-flow/mon003.png "Veri Akışını İzleme")

ADF izleme ekranından veri akışlarınızın performansını izlemeye ilişkin bir genel bakış aşağıda verilmiştir:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4u4mH]

## <a name="view-data-flow-execution-plans"></a>Veri akışı yürütme planlarını görüntüleme

Veri akışınız Spark 'ta yürütüldüğünde, Azure Data Factory veri akışınızı tamamen temel alarak en iyi kod yollarını belirler. Ek olarak, yürütme yolları farklı genişleme düğümlerinde ve veri bölümlerinde de gerçekleşebilir. Bu nedenle, izleme grafiği, dönüştürmelerinizin yürütme yolunu dikkate alarak akışınızın tasarımını temsil eder. Tek tek düğümleri seçtiğinizde, kümede birlikte yürütülen kodu temsil eden "gruplandırmaları" görebilirsiniz. Gördüğünüz zamanlamalar ve sayımlar, tasarımınızda bireysel adımlara karşılık bu grupları temsil eder.

![Veri akışı Izleme](media/data-flow/mon004.png "Veri Akışını İzleme")

* İzleme penceresinde açık alanı seçtiğinizde, alt bölmedeki istatistikler her bir havuz için zamanlama ve satır sayılarını ve dönüştürme kökenini için havuz verilerine işaret eden dönüşümleri görüntüler.

* Tek tek dönüştürmeleri seçtiğinizde, sağ bölmede bölüm istatistiklerini, sütun sayılarını, çarpıklığı (bölümler arasında dağıtılan veriler ne kadar eşit olduğu) ve basıklık (çıkmanız gerekirse ' in verileri) gösteren ek geri bildirim alırsınız.

* Düğüm görünümünde havuzu seçtiğinizde, kökenini sütununu görebilirsiniz. Her sütun, veri akışınız genelinde havuza akacak şekilde birikmiş üç farklı yöntem vardır. Bunlar:

  * Hesaplandı: sütunu, Koşullu işleme için veya veri akışındaki bir ifade dahilinde kullanın, ancak bunu havuzda gösterme
  * Türetilmiş: sütun, akışta oluşturduğunuz yeni bir sütundur, diğer bir deyişle, kaynakta mevcut değildir
  * Eşlendi: kaynak, kaynaktan geldiğini ve bir havuz alanıyla eşlemenizi sağlar
  * Veri akışı durumu: yürütmenin geçerli durumu
  * Küme başlangıç zamanı: veri akışı yürütmenizle ilgili JıT Spark işlem ortamının elde edilecek süre miktarı
  * Dönüşüm sayısı: akışınızda kaç dönüştürme adımı yürütüldüğü
  
![Veri akışı Izleme](media/data-flow/monitornew.png "Veri akışı Izleme yeni")

## <a name="total-sink-processing-time-vs-transformation-processing-time"></a>Toplam Havuz Işleme süresi ile dönüşüm Işleme süresi karşılaştırması

Her bir dönüştürme aşaması, her bir bölüm yürütme süresi toplandığında bu aşamanın tamamlanacağı toplam süreyi içerir. Havuza tıkladığınızda, "havuz Işleme süresi" görüntülenir. Bu süre, *dönüştürme süresinin toplamını ve verilerinizi* hedef deponuza yazmak için geçen g/ç süresini içerir. Havuz Işleme süresi ve dönüşümün Toplam arasındaki fark, verileri yazmak için g/ç zamanı olur.

Ayrıca, ADF ardışık düzen izleme görünümündeki veri akışı etkinliğinizden JSON çıkışını açarsanız her bölüm dönüştürme adımı için ayrıntılı zamanlamayı görebilirsiniz. JSON, her bölüm için milisaniyelik zamanlama içerir, ancak UX izleme görünümü birlikte eklenen bölümlerin toplam zamanlamadır:

```
 {
     "stage": 4,
     "partitionTimes": [
          14353,
          14914,
          14246,
          14912,
          ...
         ]
}
```

### <a name="post-processing-time"></a>İşlem sonrası süre

Haritaınızda bir havuz dönüştürme simgesi seçtiğinizde, sağdaki slayt paneli alt kısımdaki "işlem sonrası zamanı" adlı ek bir veri noktası gösterir. Bu, verileriniz yüklendikten, dönüştürüldükten ve yazıldıktan *sonra* Spark kümesinde işinizi yürütmek için harcanan süre miktarıdır. Bu süre, bağlantı havuzlarını kapatmayı, sürücü kapatmayı, dosyaları silmeyi, birleştirme dosyalarını, vb. içerebilir. Akışınızda "dosyaları taşı" ve "çıktıyı tek dosyaya kaydet" gibi eylemler gerçekleştirdiğinizde, işlem sonrası süre değerinde bir artış görürsünüz.
  
## <a name="monitor-icons"></a>Simgeleri izle

Bu simge, dönüştürme verilerinin kümede zaten önbelleğe alındığı anlamına gelir; bu nedenle zamanlamalar ve yürütme yolu şu hesaba alınır:

![Veri akışı Izleme](media/data-flow/mon005.png "Veri Akışını İzleme")

Ayrıca, dönüşümde yeşil daire simgeleri görürsünüz. Bunlar, verilerin akan havuz sayısını temsil eder.
