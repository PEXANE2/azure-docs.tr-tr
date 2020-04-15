---
title: Hızlı Başlangıç - Maliyet analiziyle Azure maliyetlerini keşfetme
description: Bu hızlı başlangıç, Azure kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanmanıza yardımcı olur.
author: bandersmsft
ms.author: banders
ms.date: 04/07/2020
ms.topic: quickstart
ms.service: cost-management-billing
ms.reviewer: micflan
ms.custom: seodec18
ms.openlocfilehash: e63e3ef999db7053609fb098cd2b7583143a2937
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874509"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Hızlı Başlangıç: Maliyet analiziyle maliyetleri araştırma ve analiz etme

Azure maliyetlerinizi düzgün bir şekilde denetlemeden ve iyileştirmeden önce maliyetlerin kuruluşunuzun neresinden kaynaklandığını anlamanız gerekir. Hizmetlerinizin tutarının ne kadar olacağını bilmek, ortamlarınızı ve sistemlerinizi desteklemek için de yararlıdır. Maliyetlerin tüm kapsamıyla görünür olması kuruluşun harcama desenlerini doğru anlamak için önemlidir. Harcama desenlerini, bütçeler gibi maliyet denetim mekanizmalarını güçlendirmek için kullanabilirsiniz.

Bu hızlı başlangıçta, kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanırsınız. Maliyetlerin zaman içinde nerede oluştuğunu anlamak ve harcama eğilimlerini tanımlamak için kuruluşa göre toplanmış maliyetleri görüntüleyebilirsiniz. Bütçeye göre aylık, üç aylık, hatta yıllık maliyet eğilimlerini tahmin etmek için zaman içinde tahakkuk eden maliyetleri görüntüleyebilirsiniz. Bütçe, mali kısıtlamalara uymaya yardımcı olur. Bütçe, harcama düzensizliklerini yalıtmak amacıyla günlük veya aylık maliyetleri görüntülemek için de kullanılır. Dahası, geçerli raporun verilerini daha fazla analiz etmek için veya dış sistemde kullanmak üzere indirebilirsiniz.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

- Maliyet analizinde maliyetleri gözden geçirme
- Maliyet görünümlerini özelleştirme
- Maliyet analizi verilerini indirme

## <a name="prerequisites"></a>Ön koşullar

Maliyet analizi, farklı türdeki Azure hesaplarını destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir.

Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](../../cost-management/assign-access-acm-data.md).

Yeni bir aboneliğiniz varsa Maliyet Yönetimi özelliklerini hemen kullanamazsınız. Maliyet Yönetimi özelliklerini kullanabilmek için 48 saat kadar beklemeniz gerekebilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="review-costs-in-cost-analysis"></a>Maliyet analizinde maliyetleri gözden geçirme

Maliyetlerinizi maliyet analizinde incelemek için Azure portalında kapsamı açın ve menüden **Maliyet analizi**'ni seçin. Örneğin **Abonelikler**'e gidin, listeden bir abonelik belirleyin ve menüden **Maliyet analizi**'ni seçin. Maliyet analizinde farklı bir kapsama geçiş yapmak için **Kapsam** hapını kullanın. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

Veri birleştirmesi sağlamak ve maliyet bilgilerine erişimi denetlemek için seçtiğiniz kapsam Maliyet Yönetimi’nin tamamında kullanılır. Kapsamları kullandığınızda, birden çok kapsam seçemezsiniz. Bunun yerine, diğerlerinin toplandığı büyük bir kapsam seçer ve iç içe yerleştirilmiş kapsamları ihtiyacınıza göre daraltırsınız. Bazı kullanıcılar birden fazla iç içe yerleştirilmiş kapsamı içine alan tek bir üst kapsama erişim iznine sahip olmayabileceğinden bu yaklaşımı anlamak önemlidir.

Maliyet Analizi’nin nasıl kullanılacağı hakkında daha fazla bilgi edinmek için [Azure portalında Maliyet Yönetimi’ni kullanma](https://www.youtube.com/watch?v=mfxysF-kTFA) videosunu izleyin. Diğer videoları izlemek için [Maliyet Yönetimi YouTube kanalını](https://www.youtube.com/c/AzureCostManagement) ziyaret edin.

>[!VIDEO https://www.youtube.com/embed/mfxysF-kTFA]

İlk maliyet analizi görünümünde aşağıdaki alanlar bulunur.

**Birikmiş maliyet görünümü**: Önceden tanımlanmış maliyet analizi görünümü yapılandırmasını temsil eder. Her bir görünüm tarih aralığı, ayrıntı düzeyi, gruplama ölçütü ve filtreleme ayarlarını içerir. Varsayılan görünüm, geçerli faturalama dönemine ait birikmiş maliyetleri gösterir ancak diğer yerleşik görünümlere geçiş yapabilirsiniz. Daha fazla bilgi için bkz. [Maliyet görünümlerini özelleştirme](#customize-cost-views).

**Gerçek maliyet**: Geçerli aya ait olan toplam kullanım ve satın alma maliyetlerini, tahakkuk ettikleri ve faturanızda görüntülenecekleri şekilde gösterir.

**Tahmin**: Seçtiğiniz zaman dönemine ait tahmini maliyet toplamını gösterir.

**Bütçe**: Varsa, seçilen kapsam için planlanan harcama limitini gösterir.

**Birikmiş ayrıntı düzeyi**: Fatura döneminin başından itibaren toplam günlük maliyetleri gösterir. Fatura hesabınız veya aboneliğiniz için [bütçe oluşturduktan](tutorial-acm-create-budgets.md) sonra, bütçeye göre harcama eğiliminizi hemen görebilirsiniz. Tarihin üzerine gelerek o gün için birikmiş maliyeti görüntüleyebilirsiniz.

**Özet (halka) grafikler**: Toplam maliyeti ortak bir standart özellikler kümesine ayırarak dinamik özetler sağlar. Geçerli ay için en büyükten en küçüğe kadar tüm maliyetleri gösterir. İstediğiniz zaman farklı bir özet seçerek özet grafikleri değiştirebilirsiniz. Maliyetler varsayılan olarak şu kategorilere ayrılır: hizmet (ölçüm kategorisi), konum (bölge) ve alt kapsam. Örneğin, fatura hesapları altında kayıt hesapları, abonelikler altında kaynak grupları ve kaynak grupları altında kaynaklar bulunur.

![Azure portalındaki maliyet analizinin ilk görünümü](./media/quick-acm-cost-analysis/cost-analysis-01.png)

### <a name="understand-forecast"></a>Tahmini anlama

Maliyet tahmini, seçili döneme ait maliyetlerinizle ilgili bir tahmin gösterir. Model, zaman serisi regresyon modelini temel alır. Maliyetleri doğru bir şekilde tahmin etmek için en az 10 günlük güncel maliyetler ve kullanım verileri gerekir. Belirli bir süre için tahmin modeli, tahmin dönemine ait eşit eğitim verilerine ihtiyaç duyar. Örneğin üç aylık tahmin için en az üç aylık güncel maliyet ve kullanım verileri gerekir.

Model, en fazla altı aylık eğitim verilerini kullanarak bir yıllık maliyet tahmininde bulunabilir. Tahminini değiştirmek için en az yedi günlük eğitim verisine ihtiyaç duyar. Bu tahmin, maliyet ve kullanım düzenlerindeki ani artışlar ve düşüşler gibi önemli değişiklikleri temel alır. Tahmin, **Gruplama ölçütü** özelliklerindeki her bir öğe için ayrı tahminler oluşturmaz. Yalnızca toplam birikmiş maliyetlerle ilgili tahmin sunar. Birden çok para birimi kullanıyorsanız model yalnızca ABD doları cinsindeki maliyetler için tahmin sağlar.

## <a name="customize-cost-views"></a>Maliyet görünümlerini özelleştirme

Maliyet analizi, en yaygın hedeflere göre iyileştirilmiş dört yerleşik görünüme sahiptir:

Görünüm | Yanıt verdiği sorular
--- | ---
Birikmiş maliyet | Bu ay şimdiye kadar ne kadar harcama yaptım? Bütçemin dışına çıkar mıyım?
Günlük maliyet | Son 30 gün içinde günlük maliyetlerde artış yaşandı mı?
Hizmete göre maliyet | Son üç faturada aylık kullanım nasıl değişti?
Kaynağa göre maliyet | Bu ay en yüksek maliyete sahip olan kaynaklar hangileri?

![Bu ay için örnek seçimi gösteren görünüm seçici](./media/quick-acm-cost-analysis/view-selector.png)

Öte yandan, birçok durumda daha derin analizler gerekir. Özelleştirme, seçilen tarihle sayfanın en üstünde başlatılır.

Maliyet analizi, varsayılan olarak geçerli ayın verilerini gösterir. Tarih seçiciyi kullanarak yaygın tarih aralıkları arasında hızlıca geçiş yapabilirsiniz. Örneğin son yedi gün, geçen ay, geçerli yıl veya özel tarih aralığı seçebilirsiniz. Kullandıkça öde abonelikleri ayrıca faturalama döneminize göre belirlenen ve takvim ayına bağlı olmayan tarih aralıklarına da sahiptir (geçerli faturalama dönemi veya son fatura gibi). Menünün üst tarafındaki **<ÖNCEKİ** ve **SONRAKİ>** bağlantılarını kullanarak önceki veya sonraki döneme geçiş yapabilirsiniz. Örneğin **<ÖNCEKİ** bağlantısı, **Son 7 gün** görünümünü **8-14 gün önce** veya **15-21 gün önce** olarak değiştirir.

![Bu ay için örnek seçimi gösteren tarih seçici](./media/quick-acm-cost-analysis/date-selector.png)

Maliyet analizi varsayılan olarak **birikmiş** maliyetleri gösterir. Birikmiş maliyetler, günlük maliyetleri önceki günlerin maliyetleriyle toplayarak ilerler ve günlük birikmiş maliyetlerinizin sürekli artan bir görünümünü sunar. Bu görünüm, seçilen zaman aralığı için bütçeye göre nasıl bir eğilim gösterdiğinizi ortaya koymak için iyileştirilmiştir.

Olası bütçe ihlallerini belirlemek için tahmin grafiği görünümünü kullanabilirsiniz. Olası bir bütçe ihlali söz konusu olduğunda tahmini fazla harcama kırmızı renkle gösterilir. Ayrıca grafikte bir gösterge simgesi de gösterilir. Simgenin üzerine geldiğinizde bütçe ihlali için tahmini tarih gösterilir.

![Olası bütçe ihlalini gösteren örnek](./media/quick-acm-cost-analysis/budget-breach.png)

Ayrıca, her günün maliyetlerini gösteren bir **günlük** görünüm vardır. Günlük görünüm büyüme eğilimini göstermez. Görünüm, günden güne maliyet sıçrama yaptığında veya iyice düştüğünde ortaya çıkan düzensizlikleri gösterecek şekilde tasarlanmıştır. Bütçe seçtiyseniz, günlük görünüm günlük bütçenize ilişkin bir tahmin de gösterir.

Günlük maliyetleriniz tutarlı olarak tahmini günlük bütçenin üzerinde olduğunda aylık bütçenizin aşılacağını öngörebilirsiniz. Tahmini günlük bütçe, bütçenizi alt düzeyde görselleştirmeye yardımcı olan bir araçtır. Günlük maliyetlerinizde dalgalanmalar olduğunda tahmini günlük bütçenin aylık bütçeyle karşılaştırılması daha az kesinlik sağlar.

Harcama tahmini etkinleştirildiğinde son harcamaların günlük görünümü aşağıdaki gibi olur.
![Geçerli aya ait örnek günlük maliyetleri gösteren günlük görünüm](./media/quick-acm-cost-analysis/daily-view.png)

Harcama tahmini kapattığınızda ileri tarihler için öngörülen harcamayı göremezsiniz. Ayrıca geçmişe ait maliyetlere baktığınızda maliyet tahmininde herhangi bir maliyet gösterilmez.

Genellikle 8-12 saat içinde tüketilen kaynaklara ait veriler veya bildirimler gösterilir.


Maliyetlerin dökümünü almak ve en çok katkıda bulunanları belirlemek için ortak özelliklere göre **gruplayın**. Örneğin kaynak etiketlerine göre gruplamak için istediğiniz etiket anahtarını seçin. Her bir etiket değerinin maliyetlerine ait döküm gösterilir ve etiketin uygulanmadığı kaynaklar ek bir segmente eklenir.

Çoğu [Azure kaynağı etiketlemeyi destekler](../../azure-resource-manager/management/tag-support.md). Ancak bazı etiketler Maliyet Yönetimi ve faturalandırma için kullanılamaz. Ayrıca kaynak gruplarının etiketlenmesi de desteklenmez. Etiket desteği, etiket kaynağa uygulandıktan *sonra* bildirilen kullanımlar için geçerlidir. Etiketler, maliyet toplamları için geriye dönük olarak uygulanmaz.

Maliyet verilerinin görünürlüğünü artırmak üzere Azure etiket ilkesini kullanma hakkında bilgi edinmek için [Azure Maliyet Yönetimi ile etiket ilkelerini gözden geçirme](https://www.youtube.com/watch?v=nHQYcYGKuyw) videosunu izleyin.

Burada, geçerli aya ait Azure hizmet maliyetlerinin bir görünümü yer almaktadır.

![Geçen aya ait örnek Azure hizmet maliyetlerini gösteren gruplandırılmış günlük birikmiş görünüm](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Maliyet analizi varsayılan olarak tüm kullanım ve satın alma maliyetlerini tahakkuk ettikleri ve faturanızda görünecekleri şekilde gösterir. Buna **Gerçek maliyet** de denir. Gerçek maliyeti görüntülemek, faturanızı mutabık kılmak için idealdir. Ancak harcama anomalileri ve diğer maliyet değişikliklerini takip ederken maliyette yaşanan satın alma kaynaklı ani artışlar uyarı verilmesine neden olabilir. Rezervasyon satın alma maliyetlerinin neden olduğu ani artışları ortadan kaldırmak için **Amorti edilen maliyet** görünümüne geçin.

![Rezervasyon satın alma işlemlerinin döneme yayıldığını ve rezervasyonu kullanan kaynaklara ayrıldığını görmek için gerçek ve amorti edilen maliyetler arasında geçiş yapın](./media/quick-acm-cost-analysis/metric-picker.png)

Amorti edilen maliyet, rezervasyon satın alma işlemlerini günlük parçalara böler ve rezervasyon süresine yayar. Örneğin 1 Ocak tarihinde 365 ABD doları tutarında bir satın alma işlemi görmek yerine 1 Ocak ile 31 Aralık arasındaki her gün 1,00 ABD doları değerinde satın alma işlemi görürsünüz. Bu maliyetler, temel amorti işlemlerinin yanı sıra rezervasyonu kullanan kaynaklara yeniden atanır ve onlarla ilişkilendirilir. Örneğin 1,00 ABD doları değerindeki günlük ücretin iki sanal makineye ayrılması durumunda gün içinde iki farklı 0,50 ABD doları ücret görürsünüz. Rezervasyonun bir bölümünün aynı gün kullanılmaması halinde ilgili sanal makine ile ilişkilendirilmiş 0,50 ABD doları değerinde ücret ve `UnusedReservation` ücret türüne sahip ayrı bir 0,50 ABD doları harcama görürsünüz. Kullanılmayan rezervasyon maliyetleri yalnızca amorti edilen maliyet görünümünde görüntülenebilir.

Maliyetlerin gösterilmesindeki değişiklik nedeniyle gerçek maliyet ile amorti edilen maliyet görünümlerinde farklı toplam değerlerin gösterildiğini unutmayın. Genel olarak amorti edilen maliyetler görüntülendiğinde rezervasyon satın alma işlemi gerçekleştirilen ayların toplam maliyeti azalacak, rezervasyon satın alma işlemini takip eden ayların maliyeti ise artacaktır. Amorti etme yalnızca rezervasyon satın alma işlemleri için geçerlidir ve şu an için Azure Market satın alma işlemlerine uygulanmaz.

Aşağıdaki görüntüde kaynak grubu adları gösterilmiştir. Etiketlere göre gruplandırarak etiket başına toplam maliyeti görüntüleyebilir veya **Kaynağa göre maliyet** görünümünü kullanarak belirli bir kaynağa ait olan tüm etiketleri görebilirsiniz.

![Kaynak grubu adlarını gösteren tam kapsamlı geçerli görünüm](./media/quick-acm-cost-analysis/full-data-set.png)

Maliyetler belirli bir özniteliğe göre gruplanırken maliyet açısından ilk 10 katkıda bulunan en yüksekten en düşüğe doğru gösterilir. Toplamda 10'dan fazla grup varsa maliyet açısından ilk dokuz katkıda bulunana ek olarak kalan tüm grupları temsil eden bir **Diğer** grubu gösterilir. Etiketlere göre gruplandırma yaptığınızda etiket anahtarının uygulanmadığı maliyetler için bir **Etiketlenmemiş** grubu görünür. Etiketlenmemiş maliyetler diğer etiketlenmiş maliyetlerden yüksek olsa dahi **Etiketlenmemiş** grubu her zaman son sırada yer alır. 10 veya daha fazla etiket değeri varsa etiketlenmemiş maliyetler, **Diğer** grubuna dahil edilir. Tüm değerleri en yüksek maliyetten en düşük maliyete doğru sıralanmış şekilde görmek için tablo görünümüne geçin ve ayrıntı düzeyini **Yok** olarak ayarlayın.

Klasik sanal makineler, ağ ve depolama kaynakları ayrıntılı fatura bilgisi paylaşmaz. Bunlar maliyet gruplarında **Klasik hizmetler** olarak gösterilir.

Ana grafiğin altındaki özet grafikler farklı gruplandırmalar gösterir ve bunlar seçilen zaman aralığı ve filtreler için genel maliyetlerinizin daha geniş bir görünümünü sunar. Toplu maliyetleri herhangi bir boyuta göre görüntülemek için bir özellik veya etiket seçin.

![Özet grafikleri gösteren örnek](./media/quick-acm-cost-analysis/pivot-charts.png)

Herhangi bir görünümde tam veri kümesini görüntüleyebilirsiniz. Sunulan veriler, uyguladığınız seçimlere veya filtrelere göre belirlenir. Tam veri kümesini görmek için **grafik türü** listesini ve ardından **Tablo** görünümünü seçin.

![Geçerli görünüm için tablo görünümünde sunulan veriler](./media/quick-acm-cost-analysis/chart-type-table-view.png)


## <a name="understanding-grouping-and-filtering-options"></a>Gruplandırma ve filtreleme seçeneklerini anlama

Maliyet analizinde birçok gruplandırma ve filtreleme seçeneği vardır. Gruplandırma ve filtreleme seçenekleriyle ilgili bir video izlemek istiyorsanız [Maliyet Yönetimi boyutlara ve etiketlere göre raporlama](https://www.youtube.com/watch?v=2Vx7V17zbmk) videosunu izleyin. Diğer videoları izlemek için [Maliyet Yönetimi YouTube kanalını](https://www.youtube.com/c/AzureCostManagement) ziyaret edin.

>[!VIDEO https://www.youtube.com/embed/2Vx7V17zbmk]

Aşağıdaki tabloda en yaygın kullanılan gruplandırma ve filtreleme seçenekleri ile bunları ne zaman kullanmanız gerektiği listelenmiştir.

| Özellik | Kullanılması gereken durumlar | Notlar |
| --- | --- | --- |
| **Kullanılabilirlik alanları** | AWS maliyetlerini kullanılabilirlik alanına göre ayırmanızı sağlar. | Yalnızca AWS kapsamları ve yönetim grupları için geçerlidir. Azure verileri, kullanılabilirlik alanı içermez ve **Geçerli değil** şeklinde görünür. |
| **Faturalama dönemi** | PAYG maliyetlerini faturalandırıldıkları (veya faturalandırılacakları) aya göre ayırmanızı sağlar. | Faturalandırılan PAYG ücretlerini tam olarak görüntülemek için **Faturalama dönemi** seçeneğini kullanabilirsiniz. Özel bir tarih aralığına göre filtreleme gerçekleştiriyorsanız faturalama döneminin önüne ve sonuna 2 gün daha ekleyin. Tam faturalama dönemi tarihleriyle sınırlamak, faturayla eşleşme yapılmasına engel olabilir. Fatura dönemindeki tüm faturaların maliyetlerini gösterir. Belirli bir faturayı filtrelemek için **Fatura Kimliği** değerini kullanın. EA ve MCA takvim ayı üzerinden faturalandırıldığından yalnızca PAYG abonelikleri için geçerlidir. EA/MCA hesapları, aynı hedefe ulaşmak için tarih seçicide takvim aylarını veya aylık ayrıntı düzeyini kullanabilir. |
| **Ücret türü** | Kullanım, satın alma, para iadesi ve kullanılmayan rezervasyon maliyetlerini ayırmanızı sağlar. | Rezervasyon satın alma işlemleri ve para iadeleri yalnızca gerçek maliyetler kullanıldığında mevcuttur, amorti edilen maliyetlerle kullanılamaz. Kullanılmayan rezervasyon maliyetleri yalnızca amorti edilen maliyetler görüntülendiğinde kullanılabilir. |
| **Departman** | Maliyetleri EA departmanına göre ayırmanızı sağlar. | Yalnızca EA ve yönetim grupları için kullanılabilir. PAYG aboneliklerinde departman yoktur ve **Geçerli değil** veya **atanmamış** olarak görünür. |
| **Kayıt hesabı** | Maliyetleri EA hesabı sahibine göre ayırmanızı sağlar. | Yalnızca EA ödeme hesapları, departmanları ve yönetim grupları için kullanılabilir. PAYG aboneliklerinde EA kaydı hesabı yoktur ve **Geçerli değil** veya **atanmamış** olarak görünür. |
| **Sıklık** | Kullanım tabanlı, tek seferlik ve yinelenen maliyetleri ayırmanızı sağlar. | |
| **Fatura Kimliği** | Maliyetleri düzenlenen faturaya göre ayırmanızı sağlar. | Faturalandırılmamış ücretler henüz bir fatura kimliğine sahip değildir ve EA maliyetleri fatura ayrıntılarını içermediğinden **Geçerli değil** şeklinde gösterilir.  |
| **Ölçüm** | Maliyetleri kullanım ölçümüne göre ayırmanızı sağlar. | Satın alma işlemleri ve Market kullanımı **Geçerli değil** şeklinde gösterilir. Satın alma işlemlerini tanımlamak için **Ücret türü**, Market ücretlerini tanımlamak için ise **Yayımcı türü** bilgilerine bakın. |
| **İşlem** | AWS maliyetlerini işleme göre ayırmanızı sağlar. | Yalnızca AWS kapsamları ve yönetim grupları için geçerlidir. Azure verileri işlem içermez ve **Geçerli değil** şeklinde görünür. Bunun yerine **Ölçüm** seçeneğini kullanabilirsiniz. |
| **Fiyatlandırma modeli** | Maliyetleri isteğe bağlı, rezervasyon veya spot kullanımına göre ayırmanızı sağlar. | Satın alma işlemleri **OnDemand** olarak gösterilir. **Geçerli değil** ifadesini görürseniz kullanımın rezervasyon mu yoksa isteğe bağlı kullanım mı olduğunu belirlemek için **Rezervasyon** ölçütüne göre, satın alma işlemlerini tanımlamak için ise **Ücret türü** ölçütüne göre gruplayın.
| **Sağlayıcı** | AWS ve Azure maliyetlerini ayırmanızı sağlar. | Yalnızca yönetim grupları için kullanılabilir. |
| **Yayımcı türü** | AWS, Azure ve Market maliyetlerini ayırmanızı sağlar. |  |
| **Rezervasyon** | Maliyetleri rezervasyona göre ayırmanızı sağlar. | Rezervasyon ile ilişkilendirilmemiş olan kullanımlar veya satın alma işlemleri **Geçerli değil** şeklinde gösterilir. Diğer Azure, AWS veya Market satın alma işlemlerini tanımlamak için **Yayımcı türü** ölçütüne göre gruplayın. |
| **Kaynak** | Maliyetleri kaynağa göre ayırmanızı sağlar. | Satın alma işlemleri EA/PAYG ödeme hesabı veya MCA faturalama profili düzeyinde uygulandığından ve belirli bir kaynakla ilişkilendirilmediğinden **Geçerli değil** şeklinde gösterilir. Diğer Azure, AWS veya Market satın alma işlemlerini tanımlamak için **Yayımcı türü** ölçütüne göre gruplayın. |
| **Kaynak grubu** | Maliyetleri kaynak grubuna göre ayırmanızı sağlar. | Satın alma işlemleri, aboneliklerle ilişkilendirilmemiş olan kiracı kaynakları, kaynak grubuna dağıtılmamış olan abonelik kaynakları ve klasik kaynaklar bir kaynak grubuna sahip olmadığından **diğer**, **klasik hizmetler**, **$system** veya **Geçerli değil** şeklinde gösterilir. |
| **Kaynak türü** | Maliyetleri kaynak türüne göre ayırmanızı sağlar. | Satın alma işlemleri ve klasik hizmetler bir Azure Resource Manager kaynak gürüne sahip olmadığından **diğer**, **klasik hizmetler** veya **Geçerli değil** şeklinde gösterilir. |
| **Kaynak konumu** | Maliyetleri konuma veya bölgeye göre ayırmanızı sağlar. | Satın alma işlemleri ve Market kullanımı **atanmamış**, **bilinmiyor**, **eşlenmemiş** veya **Geçerli değil** şeklinde gösterilebilir. |
| **Hizmet adı** veya **Ölçüm kategorisi** | Maliyetleri Azure hizmetine göre ayırmanızı sağlar. | Satın alma işlemleri ve Market kullanımı **Geçerli değil** veya **atanmamış** şeklinde gösterilir. |
| **Hizmet katmanı** veya **Ölçüm alt kategorisi** | Maliyetleri Azure kullanım ölçümü alt sınıflandırmasına göre ayırmanızı sağlar. | Satın alma işlemleri ve Market kullanımı **Geçerli değil** veya **atanmamış** şeklinde gösterilir. |
| **Abonelik** | Maliyetleri Azure aboneliğine ve bağlantılı AWS hesabına göre ayırmanızı sağlar. | Satın alma işlemleri ve kiracı kaynakları **Geçerli değil** şeklinde gösterilebilir. |
| **Tag** | Maliyetleri belirli bir etiket anahtarına ait etiket değerlerine göre ayırmanızı sağlar. | Etiketler; satın alma işlemleri, aboneliklerle ilişkilendirilmemiş olan kiracı kaynakları, kaynak grubuna dağıtılmamış abonelik kaynakları veya klasik kaynaklar için kullanılamaz. Bazı hizmetler kullanım verilerinde etiket içermez. [Kaynak türlerine göre etiket desteği](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-support) hakkında daha fazla bilgi edinin. |

Terimler hakkında daha fazla bilgi için bkz. [Azure kullanımı ve ücretleri dosyasında kullanılan terimleri anlama](../understand/understand-usage.md).


## <a name="saving-and-sharing-customized-views"></a>Özelleştirilmiş görünümleri kaydetme ve paylaşma

Özelleştirilmiş görünümleri kaydetmek ve başkalarıyla paylaşmak için maliyet analizini Azure portalı panosuna sabitleyebilir veya maliyet analizi bağlantısını kopyalayabilirsiniz.

Portalı kullanarak kuruluşunuzda maliyet bilgilerini paylaşma hakkında daha fazla bilgi edinmek için [Azure Maliyet Yönetimi’nde görünümleri paylaşma ve kaydetme](https://www.youtube.com/watch?v=kQkXXj-SmvQ) videosunu izleyin. Diğer videoları izlemek için [Maliyet Yönetimi YouTube kanalını](https://www.youtube.com/c/AzureCostManagement) ziyaret edin.

>[!VIDEO https://www.youtube.com/embed/kQkXXj-SmvQ]

Maliyet analizini sabitlemek için sağ üst köşedeki sabitleme simgesini seçin. Maliyet analizini sabitlediğinizde yalnızca ana grafik veya tablo görünümü kaydedilir. Panoyu kaydederek kutucuğa başkalarının da erişmesini sağlayabilirsiniz. Bu işlem yalnızca pano yapılandırmasını paylaşır ve başkalarına temel alınan veriler için erişim izni vermez. Maliyetlere erişiminiz yoksa ancak paylaşılan bir panoya erişebiliyorsanız "erişim engellendi" iletisiyle karşılaşırsınız.

Maliyet analizi bağlantısını paylaşmak için dikey pencerenin üst tarafında **Paylaş**'ı seçin. Bu belirli kapsam için belirli görünümü açan özel bir URL gösterilir. Maliyetlere erişiminiz yoksa bu URL'yi kullandığınızda "erişim engellendi" iletisiyle karşılaşırsınız.

Desteklenen kapsamlar için maliyetlere erişim izni verme hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).



## <a name="automation-and-offline-analysis"></a>Otomasyon ve çevrimdışı analiz

Ek analiz gerçekleştirmek için verileri indirmek, kendi verilerinizle birleştirmek veya kendi sistemlerinizle tümleştirmek isteyebilirsiniz. Maliyet Yönetimi birkaç farklı seçenek sunar. Başlangıç noktası olarak maliyet analizinde olduğu gibi anlık üst düzey özete ihtiyacınız varsa ihtiyacınız olan görünümü oluşturmanız gerekir. Ardından **Dışarı aktar**'ı ve ardından **Verileri CSV olarak indir** veya **Verileri Excel'e indir**'i seçerek oluşturduğunuz görünümü indirebilirsiniz. Excel dosyası kapsam, sorgu yapılandırması, toplam ve oluşturma tarihi gibi indirilen dosyayı oluşturmak için kullandığınız görünümle ilgili ek bağlam bilgisi sunar.

Tam, toplanmamış veri kümesine ihtiyacınız varsa ödeme hesabından indirebilirsiniz. Ardından, portalın sol gezinti bölmesindeki hizmetler listesinden **Maliyet Yönetimi + Faturalandırma**'yı seçin. Gerekirse ödeme hesabınızı seçin. **Kullanım + ücretler**'e gidip istediğiniz faturalama dönemine ait **İndir** simgesini seçin.

Maliyet verilerini almayı otomatikleştirmek için benzer bir yaklaşımdan faydalanabilirsiniz. [Sorgu API'sini](/rest/api/cost-management/query) kullanarak dinamik filtreleme, gruplama ve toplama işlevleriyle daha gelişmiş analiz gerçekleştirebilir veya tam, toplanmamış veri kümesi için [UsageDetails API'sini](/rest/api/consumption/usageDetails) kullanabilirsiniz. Bu API'lerin genel kullanım (GA) sürümü, 2019-01-01 sürümüdür. Bu API'lerde rezervasyon ve Market satın alma işlemlerini önizlemek için **2019-04-01-preview** sürümünü kullanın.

Örneğin aşağıdaki toplu görünümde amorti edilmiş maliyetler ücret türüne (kullanım, satın alma veya para iadesi), yayımcı türüne (Azure veya Market), kaynak grubuna (satın alma için boş) ve rezervasyona (geçerli değilse boş) göre gösterilmiştir.

```
POST https://management.azure.com/{scope}/providers/Microsoft.CostManagement/query?api-version=2019-04-01-preview
Content-Type: application/json

{
  "type": "AmortizedCost",
  "timeframe": "Custom",
  "timePeriod": { "from": "2019-04-01", "to": "2019-04-30" },
  "dataset": {
    "granularity": "None",
    "aggregation": {
      "totalCost": { "name": "PreTaxCost", "function": "Sum" }
    },
    "grouping": [
      { "type": "dimension", "name": "ChargeType" },
      { "type": "dimension", "name": "PublisherType" },
      { "type": "dimension", "name": "Frequency" },
      { "type": "dimension", "name": "ResourceGroup" },
      { "type": "dimension", "name": "SubscriptionName" },
      { "type": "dimension", "name": "SubscriptionId" },
      { "type": "dimension", "name": "ReservationName" },
      { "type": "dimension", "name": "ReservationId" },
    ]
  },
}
```

Toplamaya ihtiyacınız yoksa ve tam, ham veri kümesini tercih ediyorsanız:

```
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?metric=AmortizedCost&$filter=properties/usageStart+ge+'2019-04-01'+AND+properties/usageEnd+le+'2019-04-30'&api-version=2019-04-01-preview
```

Gerçek maliyetlerin tahakkuk eden satın alma işlemlerini göstermesini istiyorsanız **type**/**metric** değerini **ActualCost** olarak değiştirin. Bu API'ler hakkında daha fazla bilgi için [Sorgu](/rest/api/cost-management/query) ve [UsageDetails](/rest/api/consumption/usageDetails) API'si belgelerini inceleyin. Yayımlanan belgelerin genel kullanım sürümüne ait olduğunu unutmayın. Ancak yeni type/metric özniteliği ve değiştirilen özellik adları haricinde ikisi de *2019-04-01-preview* API sürümü için geçerlidir. (Özellik adları hakkında daha fazla bilgi için aşağıdaki bölümü inceleyin.)

Maliyet Yönetimi API'leri, kaynakların üzerinde ve tüm kapsamlarda çalışır: Azure RBAC erişimi aracılığıyla kaynak grubu, abonelik ve yönetim grubu; EA portalı erişimi aracılığıyla da EA ödeme hesapları (kayıtlar) departmanlar ve kayıt hesapları. Kapsam kimliğinizi belirleme veya erişimi yönetme dahil olmak üzere kapsamlar hakkında daha fazla bilgi edinmek için bkz. [Kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

## <a name="next-steps"></a>Sonraki adımlar

Bütçelerin nasıl oluşturulduğunu ve yönetildiğini öğrenmek için ilk öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Bütçe oluşturma ve yönetme](tutorial-acm-create-budgets.md)
