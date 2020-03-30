---
title: Azure Uygulama Öngörüleri kullanım kohortları | Microsoft Dokümanlar
description: Ortak bir şeyi olan farklı kümeleri veya kullanıcıları, oturumları, olayları veya işlemleri analiz edin
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 04/10/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 0c552e8f3e732c49da02b2f5704fb9cf312fb3e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671095"
---
# <a name="application-insights-cohorts"></a>Uygulama Öngörüleri kohortları

Kohort, ortak bir şey olan bir kullanıcı, oturum, olay veya işlem kümesidir. Azure Uygulama Öngörüleri'nde kohortlar bir analiz sorgusu yla tanımlanır. Belirli bir kullanıcı kümesini veya olayı tekrar tekrar analiz etmeniz gereken durumlarda, kohortlar size ilgilendiğiniz seti tam olarak ifade etme esnekliği sağlayabilir.

![Kohortlar bölmesi](./media/usage-cohorts/001.png)

## <a name="cohorts-versus-basic-filters"></a>Kohortlar temel filtrelere karşı

Kohortlar filtrelere benzer şekillerde kullanılır. Ancak kohortların tanımları özel analitik sorgularından oluşturulur, bu nedenle çok daha uyumlu ve karmaşıktırlar. Filtrelerden farklı olarak, ekibinizin diğer üyelerinin bunları yeniden kullanabilmesi için kohortları kaydedebilirsiniz.

Uygulamanızda yeni bir özellik deneyen bir kullanıcı kohortunu tanımlayabilirsiniz. Bu kohortu Application Insights kaynağınıza kaydedebilirsiniz. Gelecekte bu kayıtlı belirli kullanıcı grubunu analiz etmek kolaydır.

> [!NOTE]
> Oluşturulduktan sonra, kohortlar Kullanıcılar, Oturumlar, Etkinlikler ve Kullanıcı Akışları araçlarından edinilebilir.

## <a name="example-engaged-users"></a>Örnek: Meşgul kullanıcılar

Ekibiniz, ilgili bir kullanıcıyı belirli bir ay içinde uygulamanızı beş veya daha fazla kez kullanan herkes olarak tanımlar. Bu bölümde, bu meşgul kullanıcıların bir kohort tanımlar.

1. Kohortlar aracını açın.

2. Şablon **Galerisi** sekmesini seçin. Çeşitli kohortlar için şablonlar koleksiyonu görürsünüz.

3. **Kullanılan Günlere Göre Meşgul Kullanıcıları**Seçiniz.

    Bu kohort için üç parametre vardır:
    * Hangi olayların ve sayfa görünümlerin "kullanım" olarak sayAcağını seçtiğiniz **etkinlikler.**
    * **Dönem**, bir ayın tanımı.
    * **UsedAtLeastCustom**, kullanıcıların meşgul olarak saymak için bir süre içinde bir şey kullanmak gerekir kaç kez.

4. **UsedAtLeastCustom'ı** **5+ güne**değiştirin ve **Süreyi** 28 gün varsayılan olarak bırakın.

    ![Etkileşimde bulunan kullanıcılar](./media/usage-cohorts/003.png)

    Şimdi bu kohort, son 28 gün içinde 5 ayrı günde herhangi bir özel olay veya sayfa görünümüyle gönderilen tüm kullanıcı kodlarını temsil eder.

5. **Kaydet'i**seçin.

   > [!TIP]
   > Kohortunuza "Meşgul Kullanıcılar (5+ Gün)" gibi bir ad verin. Bu Uygulama Öngörüleri kaynağına erişimi olan diğer kişilerin bu kohortu görmesini isteyip istemediğinize bağlı olarak bu raporu "Raporlarım" veya "Paylaşılan raporlarım" olarak kaydedin.

6. **Galeriye Geri Dön'ün**

### <a name="what-can-you-do-by-using-this-cohort"></a>Bu kohort kullanarak ne yapabilirsiniz?

Kullanıcılar aracını açın. **Açılan** kutuda, Kullanıcılar altında oluşturduğunuz kohort'u seçin. **Users who belong to**

Şimdi Kullanıcılar aracı bu kullanıcı kohortuna filtrelenir:

![Kullanıcılar bölmesi belirli bir kohort için filtre](./media/usage-cohorts/004.png)

Dikkat etmesi gereken birkaç önemli şey:

* Bu kümeyi normal filtreler aracılığıyla oluşturamazsınız. Tarih mantığı daha gelişmiştir.
* Kullanıcılar aracındaki normal filtreleri kullanarak bu kohorta daha fazla filtre uygulayabilirsiniz. Bu nedenle, kohort 28 günlük pencerelerde tanımlansa da, Kullanıcılar aracındaki zaman aralığını 30, 60 veya 90 gün olarak ayarlayabilirsiniz.

Bu filtreler sorgu oluşturucu aracılığıyla ifade etmek imkansız daha karmaşık soruları destekler. Buna bir örnek, _son 28 gün içinde meşgul olan kişilerdir. Son 60 günde aynı insanlar nasıl biri oldu?_

## <a name="example-events-cohort"></a>Örnek: Olaylar kohort

Ayrıca olaylardan oluşan kohortlar da yapabilirsiniz. Bu bölümde, olaylar ve sayfa görünümleri bir kohort tanımlar. Sonra diğer araçlardan bunları nasıl kullanacağınızı görürsünüz. Bu kohort, ekibinizin _etkin kullanımı_ veya belirli bir yeni özellik ile ilgili bir küme olarak gördüğü bir dizi olay tanımlayabilir.

1. Kohortlar aracını açın.

2. Şablon **Galerisi** sekmesini seçin. Çeşitli kohortlar için şablonlar topluluğu görürsünüz.

3. **Etkinlikler Seçici'yi**seçin.

    ![Etkinlik seçicinin ekran görüntüsü](./media/usage-cohorts/006.png)

4. **Etkinlikler** açılır kutusunda, kohortta olmak istediğiniz olayları seçin.

5. Kohort kaydedin ve ona bir isim verin.

## <a name="example-active-users-where-you-modify-a-query"></a>Örnek: Sorguyı değiştirdiğiniz etkin kullanıcılar

Önceki iki kohort açılır kutular kullanılarak tanımlanmıştır. Ancak, toplam esneklik için analitik sorguları kullanarak kohortları da tanımlayabilirsiniz. Nasıl olduğunu görmek için, Birleşik Krallık'tan bir kullanıcı kohortu oluşturun.

![Cohorts aracının kullanımı ile yürürken animasyonlu görüntü](./media/usage-cohorts/cohorts0001.gif)

1. Kohortlar aracını açın, **Şablon Galerisi** sekmesini seçin ve Boş Kullanıcılar **kohort'unu**seçin.

    ![Boş kullanıcılar kohort](./media/usage-cohorts/001.png)

    Üç bölüm vardır:
   * Ekibinizdeki diğer kişiler için kohort'u daha ayrıntılı olarak tanımladığınız Bir Markdown metin bölümü.

   * Önceki iki örnekteki **Etkinlikler** ve diğer açılır kutular gibi kendi parametrelerinizi yaptığınız bir parametreler bölümü.

   * Bir analitik sorgu kullanarak kohort tanımladığınız bir sorgu bölümü.

     Sorgu bölümünde, [bir analitik sorgu yazarsınız.](/azure/kusto/query) Sorgu, tanımlamak istediğiniz kohortaçıklayan belirli satır kümesini seçer. Kohortlar aracı sonra dolaylı bir "| sorguya user_Id" yan tümcesi ile özetleyin. Bu veriler, sorgunuzun sonuçları döndürdiğinden emin olmak için tablodaki sorgunun altında önizlemeedilir.

     > [!NOTE]
     > Sorguyu görmüyorsanız, daha uzun yapmak ve sorguyu göstermek için bölümü yeniden boyutlandırmayı deneyin. Bu bölümün başındaki animasyonlu .gif yeniden boyutlandırma davranışını gösterir.

2. Aşağıdaki metni sorgu düzenleyicisine kopyalayıp yapıştırın:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. **Sorgu Çalıştır**'ı seçin. Tabloda kullanıcı işllerinin görülmediğini görüyorsanız, uygulamanızın kullanıcılarının olduğu bir ülke/bölgeye geçirin.

4. Kohortun adını kaydet ve adlandır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

_Belirli bir ülkeden/bölgeden bir kullanıcı kohortu tanımladım. Kullanıcılar aracındaki bu kohortu, sadece o ülkeye/bölgeye bir filtre ayarlamakla karşılaştırdığımda, farklı sonuçlar görüyorum. Neden?_

Kohortlar ve filtreler farklıdır. Birleşik Krallık'tan (önceki örnekgibi tanımlanmış) bir kullanıcı grubunuz olduğunu ve sonuçlarını "Ülke veya Bölge = Birleşik Krallık" filtresini ayarlamakla karşılaştırdığınızı varsayalım.

* Kohort sürümü, geçerli zaman aralığında Birleşik Krallık'tan bir veya daha fazla etkinlik gönderen kullanıcıların tüm olaylarını gösterir. Ülkeye veya bölgeye göre bölünürseniz, büyük olasılıkla birçok ülke ve bölge görürsünüz.
* Filtreler sürümü yalnızca Birleşik Krallık'tan gelen olayları gösterir. Ancak ülkeye veya bölgeye göre bölünürseniz, yalnızca Birleşik Krallık'ı görürsünüz.

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Analytics sorgu dili](https://go.microsoft.com/fwlink/?linkid=856587)
* [Kullanıcılar, oturumlar, etkinlikler](usage-segmentation.md)
* [Kullanıcı akışları](usage-flows.md)
* [Kullanıma genel bakış](usage-overview.md)
