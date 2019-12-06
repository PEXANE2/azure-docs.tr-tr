---
title: Hızlı başlangıç-maliyet analizi ile Azure maliyetlerini araştırma | Microsoft Docs
description: Bu hızlı başlangıç, Azure kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 12/04/2019
ms.topic: quickstart
ms.service: cost-management-billing
manager: micflan
ms.custom: seodec18
ms.openlocfilehash: 36b8a3bba82a338f3aab526d505c4d1e96061ddf
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74849131"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Hızlı başlangıç: maliyet analizi ile maliyetleri araştırma ve analiz etme

Azure maliyetlerinizi düzgün bir şekilde denetlemeden ve iyileştirmeden önce maliyetlerin kuruluşunuzun neresinden kaynaklandığını anlamanız gerekir. Ayrıca, hizmet maliyetlerinizin ne kadar parasal olduğunu ve hangi ortamları ve sistemleri destekleyeceğinizi de yararlı olur. Maliyetlerin tüm kapsamıyla görünür olması kuruluşun harcama desenlerini doğru anlamak için önemlidir. Bütçeler gibi maliyet denetimi mekanizmalarını zorlamak için harcama desenleri kullanabilirsiniz.

Bu hızlı başlangıçta, kurumsal maliyetlerinizi keşfetmek ve analiz etmek için maliyet analizini kullanırsınız. Maliyetlerin zaman içinde nerede oluştuğunu anlamak ve harcama eğilimlerini tanımlamak için kuruluşa göre toplanmış maliyetleri görüntüleyebilirsiniz. Bütçeye göre aylık, üç aylık, hatta yıllık maliyet eğilimlerini tahmin etmek için zaman içinde tahakkuk eden maliyetleri görüntüleyebilirsiniz. Bütçe, mali kısıtlamalara uymaya yardımcı olur. Bütçe, harcama düzensizliklerini yalıtmak amacıyla günlük veya aylık maliyetleri görüntülemek için de kullanılır. Dahası, geçerli raporun verilerini daha fazla analiz etmek için veya dış sistemde kullanmak üzere indirebilirsiniz.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

- Maliyet analizinde maliyetleri gözden geçirme
- Maliyet görünümlerini özelleştirme
- Maliyet analizi verilerini indirme


## <a name="prerequisites"></a>Önkoşullar

Maliyet analizi, farklı türlerde Azure hesap türlerini destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için bkz. [maliyet yönetimi verilerini anlama](understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için, Azure hesabınız için en azından okuma erişiminizin olması gerekir.

Azure maliyet yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [verilere erişim atama](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="review-costs-in-cost-analysis"></a>Maliyet analizinde maliyetleri gözden geçirme

Maliyet analizinde maliyetlerinizi gözden geçirmek için Azure portal kapsamı açın ve menüden **Maliyet Analizi** ' ni seçin. Örneğin, **abonelikler**' e gidin, listeden bir abonelik seçin ve ardından menüde **Maliyet Analizi** ' ni seçin. Maliyet analizinde farklı bir kapsama geçiş yapmak için **Kapsam** hapını kullanın. Kapsamlar hakkında daha fazla bilgi için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

Seçtiğiniz kapsam, maliyet yönetimi genelinde veri birleştirme ve maliyet bilgilerine erişim denetimi sağlamak için kullanılır. Kapsamları kullandığınızda, birden çok kapsam seçemezsiniz. Bunun yerine, başkalarının daha büyük bir kapsam seçersiniz ve daha sonra ihtiyacınız olan iç içe kapsamlar için filtre uygulayın. Bu yaklaşım, bazı kişilerin birden çok iç içe kapsam kapsayan tek bir üst kapsama erişimi olmadığı için anlaşılması önemlidir.

İlk maliyet analizi görünümü aşağıdaki alanlara sahiptir.

**Birikmiş maliyet görünümü**: önceden tanımlanmış maliyet analizi görünümü yapılandırmasını temsil eder. Her görünümde tarih aralığı, ayrıntı düzeyi, gruplandırma ölçütü ve filtre ayarları yer alır. Varsayılan görünümde geçerli faturalama dönemi içinde birikmiş maliyetler gösterilir ama başka yerleşik görünümlere geçebilirsiniz. Daha fazla bilgi için bkz. [Maliyet görünümlerini özelleştirme](#customize-cost-views).

**Gerçek maliyet**: geçerli ay tahakkuk edildiği ve faturanızda gösterileceği için toplam kullanım ve satın alma maliyetlerini gösterir.

**Tahmin**: seçtiğiniz zaman döneminin toplam tahmini maliyetlerini gösterir. (Tahmin önizlemededir.)

**Bütçe**: varsa, seçili kapsam için planlanan harcama limitini gösterir.

**Birikmiş ayrıntı düzeyi**: fatura döneminin başından itibaren toplam günlük maliyetleri gösterir. Fatura hesabınız veya aboneliğiniz için [bütçe oluşturduktan](tutorial-acm-create-budgets.md) sonra, bütçeye göre harcama eğiliminizi hemen görebilirsiniz. Tarihin üzerine gelerek o gün için birikmiş maliyeti görüntüleyebilirsiniz.

**Pivot (halka) grafikler**: ortak bir dizi standart özellik tarafından toplam maliyetten oluşan dinamik özet 'ler sağlar. Geçerli ayın en yüksekten en düşüğe doğru maliyetlerini gösterirler. İstediğiniz zaman farklı bir özet seçerek özet grafikleri değiştirebilirsiniz. Maliyetler varsayılan olarak hizmet (ölçüm kategorisi), konum (bölge) ve alt kapsam kategorilerine ayrılır. Örneğin, kayıt hesapları fatura hesaplarının altında, kaynak gruplarının aboneliklerin altında ve kaynaklar kaynak gruplarının altındadır.

![Azure portalında maliyet analizi başlangıç görünümü](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Maliyet görünümlerini özelleştirme

Maliyet analizi, en yaygın hedefler için en iyi duruma getirilmiş dört yerleşik görünüme sahiptir:

Görüntüle | Şunun gibi sorular yanıtlayın
--- | ---
Toplam maliyet | Bu ay şimdiye kadar ne kadar harcama yaptım? Bütçemin dışına çıkar mıyım?
Günlük maliyet | Son 30 günde günlük maliyetlerde herhangi bir artış oldu mu?
Hizmete göre maliyet | Aylık kullanım son üç faturaya göre değişiyor mu?
Kaynağa göre maliyet | Bu ay şimdiye kadar en yüksek maliyetli kaynaklar hangileriydi?

![Bu ay için örnek bir seçim gösteren seçiciyi görüntüle](./media/quick-acm-cost-analysis/view-selector.png)

Öte yandan, birçok durumda daha derin analizler gerekir. Özelleştirme, seçilen tarihle sayfanın en üstünde başlatılır.

Maliyet analizi, varsayılan olarak geçerli ayın verilerini gösterir. Yaygın tarih aralıklarına hızla geçmek için tarih seçiciyi kullanın. Örnekler arasında son yedi gün, geçen ay, geçerli yıl veya özel tarih aralığı sayılabilir. Kullandıkça öde abonelikleri de faturalama döneminiz temelinde tarih aralıkları içerir. Geçerli faturalama dönemi veya son fatura gibi faturalama döneminiz, takvim ayına bağlı değildir. Önceki veya sonraki döneme geçmek için menünün en üstündeki ÖNCEKI ve **sonraki >** bağlantılarını **<** kullanın. Örneğin, **önceki <** **son 7 günden** **8-14 gün önce** veya **15-21 gün önce**değişir.

![Bu ay için bir örnek seçimi gösteren tarih seçici](./media/quick-acm-cost-analysis/date-selector.png)

Maliyet analizi varsayılan olarak **birikmiş** maliyetleri gösterir. Birikmiş maliyetler, her günün yanı sıra önceki günlerin de tüm maliyetlerini içerdiğinden, günlük birikmiş maliyetlerinizin sürekli büyüyen bir görünümü elde edilir. Bu görünüm, seçilen zaman aralığı için bütçeye göre nasıl bir eğilim gösterdiğinizi ortaya koymak için iyileştirilmiştir.

Olası bütçe aşımlarını belirlemek için tahmin grafik görünümünü kullanın. Potansiyel bir bütçe ihlali olduğunda, öngörülen fazla harcama kırmızı renkle gösterilir. Grafikte bir de gösterge sembolü yer alır. Sembolün üzerine geldiğinizde bütçe aşımının tahmini tarihi gösterilir.

![Olası bütçe ihlalini gösteren örnek](./media/quick-acm-cost-analysis/budget-breach.png)

Ayrıca, her günün maliyetlerini gösteren bir **günlük** görünüm vardır. Günlük görünüm büyüme eğilimini göstermez. Görünüm, günden güne maliyet sıçrama yaptığında veya iyice düştüğünde ortaya çıkan düzensizlikleri gösterecek şekilde tasarlanmıştır. Bütçe seçtiyseniz, günlük görünüm günlük bütçenizin tahminini de gösterir.

Günlük maliyetleriniz tutarlı olarak tahmini günlük bütçenin üzerinde olduğunda aylık bütçenizin aşılacağını öngörebilirsiniz. Tahmini günlük bütçe, bütçenizi alt düzeyde görselleştirmeye yardımcı olan bir araçtır. Günlük maliyetlerinizde dalgalanmalar olduğunda tahmini günlük bütçenin aylık bütçeyle karşılaştırılması daha az kesinlik sağlar.

İşte harcama tahminini açık olan en son harcamanın günlük görünümü.
Günlük görünüm ![geçerli ay için günlük maliyetleri gösterir](./media/quick-acm-cost-analysis/daily-view.png)

Harcama tahminini devre dışı bırakırsanız, gelecekteki tarihler için öngörülen harcama görmezsiniz. Ayrıca, geçmişteki bir dönemdeki maliyetlere baktığınızda maliyet tahmini maliyetleri göstermez.

Genellikle, tüketilen kaynaklar için verileri veya bildirimleri 8-12 saat içinde görmeyi bekleyebilir.


Maliyetleri bölmek ve en iyi katılımcıları belirlemek için ortak özelliklere **göre gruplandırın** . Örneğin, kaynak etiketlerine göre gruplandırmak için, gruplandırmak istediğiniz etiket anahtarını seçin. Bu etiketin uygulanmadığı kaynaklar için ek kesimle, maliyetler her bir etiket değeri tarafından bölünür.

Birçok [Azure kaynağı etiketlemeyi destekler](../azure-resource-manager/tag-support.md). Ancak, bazı Etiketler maliyet yönetimi ve Faturalandırma bölümünde kullanılamaz. Ayrıca, kaynak grubu etiketleri desteklenmez. Etiket desteği, etiket kaynağa uygulandıktan *sonra* bildirilen kullanım için geçerlidir. Etiketler, maliyet toplamaları için geriye dönük olarak uygulanmaz.

Maliyet veri görünürlüğünü artırmak üzere Azure etiket ilkesi 'ni kullanma hakkında bilgi edinmek için [Azure maliyet yönetimi videosu ile etiket ilkelerini gözden geçirme](https://www.youtube.com/watch?v=nHQYcYGKuyw) konusunu izleyin.

Geçerli ay için Azure hizmet maliyetlerinin bir görünümü aşağıda verilmiştir.

![Örnek Azure hizmet maliyetlerini geçen aya ait gösteren gruplandırılmış günlük birikmiş görünümü](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Varsayılan olarak, maliyet analizi tahakkuk ettikleri ve faturanızda **gerçek maliyet**olarak da bilinen tüm kullanım ve satın alma maliyetlerini gösterir. Gerçek maliyeti görmek, faturanızda mutabakat sağlamak için idealdir. Öte yandan, harcama anomalilerini ve maliyetteki diğer değişiklikleri bulmaya çalışırken maliyetteki ani satın alma artışları endişe verici olabilir. Rezervasyon satın alma maliyetlerinin neden olduğu ani artışları düzleştirmek için, **itfası maliyeti**' ne geçin.

![Rezervasyonların dönem genelinde yayıldığını ve ayırmayı kullanan kaynaklara ayrıldığını görmek için gerçek ve itfası maliyeti arasında geçiş yapın](./media/quick-acm-cost-analysis/metric-picker.png)

İtfa edilen maliyet rezervasyon satın almalarını günlük öbeklere ayırır ve bunları rezervasyon süresi boyunca yayar. Örneğin, 1 Ocak 'ta $365 satın alma yerine, 1 Ocak 'tan 31 Aralık 'a kadar her gün bir $1 satın alma görürsünüz. Temel amortismana ek olarak, bu maliyetler rezervasyonu kullanan belirli kaynaklar kullanılarak yeniden ayrılır ve ilişkilendirilir. Örneğin, iki sanal makine arasında $1 günlük ücret bölündüğünde, gün için $2 0,50 ücreti görürsünüz. Rezervasyonun bir kısmı gün boyunca kullanılmaz, geçerli sanal makine ile ilişkili $1 0,50 ücreti ve `UnusedReservation`ücret türü ile başka bir $0,50 ücreti görürsünüz. Kullanılmayan ayırma maliyetlerinin yalnızca itfası maliyeti görüntülenirken görünebileceğini unutmayın.

Maliyetlerin gösterilme biçimindeki değişiklikten dolayı, gerçek maliyet ile itfa edilen maliyet görünümlerinin farklı toplam rakamları gösterdiğini aklınızda bulundurmanız önemlidir. Genel olarak, itfa edilen maliyetler görüntülenirken satın alınan rezervasyonu olan ayların toplam maliyeti düşer ve satın alınan rezervasyonu izleyen aylarda toplam maliyet artar. İtfası yalnızca rezervasyon satın alma işlemleri için kullanılabilir ve şu anda Azure Marketi satın alımlara uygulanmaz.

Aşağıdaki görüntüde kaynak grubu adları gösterilmektedir. Etiket başına toplam maliyeti görüntülemek için etikete göre gruplandırma yapabilir veya belirli bir kaynağın tüm etiketlerini görmek için **kaynak maliyeti** görünümünü kullanabilirsiniz.

![Kaynak grubu adları gösteren geçerli görünüm için tam veri](./media/quick-acm-cost-analysis/full-data-set.png)

Maliyetleri belirli bir özniteliğe göre gruplandırdığınızda, ilk 10 maliyet katkı sağlayanlar en yüksekten en düşüğe gösterilir. 10 ' dan fazla varsa, en yüksek maliyetli katkıda bulunanlar, kalan tüm grupları temsil eden **diğer** bir grupla gösterilir. Etiketlere göre gruplandırma yaparken, etiket anahtarı uygulanamayan maliyetler için **etiketlenmemiş** bir grup görünür. Etiketlenmemiş maliyetler etiketli maliyetlerden yüksek olsa da **etiketsiz** , her zaman son ' a sahip olur. 10 veya daha fazla etiket değeri varsa, etiketlenmemiş maliyetler **Diğerlerinin**bir parçası olur. En yüksek değerden en düşük maliyetle derecelendirilmesini sağlamak için tablo görünümüne geçin ve ayrıntı düzeyini **none** olarak değiştirin.

Klasik sanal makineler, ağ ve depolama kaynakları ayrıntılı faturalandırma verilerini paylaşmaz. Maliyetler gruplandırılırken **Klasik hizmetler** olarak birleştirilir.

Ana grafiğin altındaki Özet grafikler, seçili zaman aralığı ve filtreleri için genel maliyetlerinizin daha geniş bir resmini sunan farklı gruplandırmaları gösterir. Bir özellik ya da herhangi bir boyuta göre toplanmış maliyetleri görüntülemek üzere etiketi seçin.

![Özet grafikleri gösteren örnek](./media/quick-acm-cost-analysis/pivot-charts.png)

Tüm görünüm için tam veri kümesini görüntüleyebilirsiniz. Uyguladığınız seçimler veya filtreler, sunulan verileri etkiler. Tam veri kümesini görmek için **grafik türü** listesini seçip **tablo** görünümü ' nü seçin.

![Bir tablo görünümündeki geçerli görünüm için veriler](./media/quick-acm-cost-analysis/chart-type-table-view.png)


## <a name="understanding-grouping-and-filtering-options"></a>Gruplandırma ve filtreleme seçeneklerini anlama

Aşağıdaki tabloda, en yaygın gruplandırma ve filtreleme seçeneklerinin bazıları ve ne zaman kullanmanız gerektiği listelenmektedir.

| Özellik | Kullanılması gereken durumlar |
| --- | --- |
| **Fatura dönemi** | Maliyetleri fatura ayına göre bölün. Bu seçenek, takvim ayları ile bağlantılı olmayan Kullandıkça öde ve geliştirme/test abonelikleri için önemlidir. EA/MCA hesapları, aynı hedefi gerçekleştirmek için tarih seçicide veya aylık ayrıntı düzeyinde takvim ayları kullanabilir. |
| **Ücret türü** | Kullanım, satın alma, iade ve kullanılmayan rezervasyon maliyetlerini kesin. Rezervasyon satın alımları ve para iadesi yalnızca, işlem maliyetleri kullanılırken, itfası maliyeti kullanılırken kullanılabilir. Kullanılmayan rezervasyon maliyetleri yalnızca itfası maliyetlerine bakmaya hazır olduğunda kullanılabilir. |
| **Bulut** | AWS ve Azure 'un maliyetlerini azaltın. AWS maliyetleri yalnızca yönetim gruplarından, dış faturalandırma hesaplarından ve dış aboneliklerden kullanılabilir. |
| **Departman** / **Fatura bölümü** | Maliyetleri EA Departmanı veya MCA Invoice bölümüne göre bölün. Bu seçenek yalnızca EA/MCA faturalandırma hesapları ve MCA faturalandırma profilleri için kullanılabilir. |
| **Kayıt hesabı** | Maliyetleri EA hesap sahibine göre bölün. Bu seçenek yalnızca EA faturalandırma hesapları ve departmanları için kullanılabilir. |
| **Sıklık** | Kullanım tabanlı, bir kerelik ve yinelenen maliyetleri kesin. |
| **Si** | Azure kullanım ölçünün maliyetlerini azaltın. Bu seçenek yalnızca Azure kullanımı için kullanılabilir. Tüm satın alma ve Market kullanımı **belirtilmemiş** veya **atanmamış**olarak gösterilir. |
| **Yayımcı türü** | AWS, Azure ve Market maliyetlerini bölün. |
| **La** | Maliyetleri rezervasyona göre bölün. Ayırma içermeyen kullanımlar **belirtilmemiş**olarak gösterilir. |
| **Kaynak** | Maliyetleri kaynağa göre bölün. Her satın alma, bir EA/PAYG faturalandırma hesabında veya MCA Faturalandırma profili düzeyinde uygulandığından, **belirtilen**şekilde görünür.  |
| **Kaynak grubu** | Maliyetleri kaynak grubuna göre bölün. Bu seçenek yalnızca klasik olmayan kullanım için kullanılabilir. Klasik kaynak kullanımı **diğer**gibi görünür ve satın alma işlemleri **belirtilmemiş**olarak gösterilir. |
| **Kaynak türü** | Maliyetleri kaynak türüne göre bölün. Bu seçenek yalnızca klasik olmayan kullanım için kullanılabilir. Klasik kaynak kullanımı **diğer**gibi görünür ve satın alma işlemleri **belirtilmemiş**olarak gösterilir. |
| **Hizmet adı** veya **ölçüm kategorisi** | Azure hizmeti 'nin maliyetini azaltın. Bu seçenek yalnızca Azure kullanımı için kullanılabilir. Tüm satın alma ve Market kullanımı **belirtilmemiş** veya **atanmamış**olarak gösterilir. |
| **Hizmet katmanı** veya **ölçüm alt kategorisi** | Azure kullanım ölçümü alt sınıflandırmasına göre maliyeti sonlandırın. Bu seçenek yalnızca Azure kullanımı için kullanılabilir. Tüm satın alma ve Market kullanımı **belirtilmemiş** veya **atanmamış**olarak gösterilir. |
| **Abonelik** | Maliyetleri aboneliğe göre bölün. Tüm satın alımlar **belirtilmemiş**olarak gösterilir. |
| **Tag** | Belirli bir etiket anahtarı için etiket değerlerine göre maliyetleri bölün. |

Şartlar hakkında daha fazla bilgi için bkz. [Azure kullanımı ve ücretler dosyasında kullanılan koşulları anlayın](../billing/billing-understand-your-usage.md).


## <a name="saving-and-sharing-customized-views"></a>Özelleştirilmiş görünümleri kaydetme ve paylaşma

Maliyet analizini Azure portal panosuna sabitleyerek veya maliyet analizine bir bağlantı kopyalayarak özelleştirilmiş görünümleri başkalarıyla kaydedip paylaşabilirsiniz.

Maliyet analizini sabitlemek için sağ üst köşedeki raptiye simgesini seçin. Maliyet analizini sabitleme yalnızca ana grafik veya tablo görünümünü kaydeder. Başkalarının kutucuğa erişmesine izin vermek için panoyu paylaşabilirsiniz. Bunun yalnızca Pano yapılandırmasını paylaştığı ve kullanıcılara temel alınan verilere erişim vermediği unutulmamalıdır. Maliyetlerine erişiminiz yoksa ancak paylaşılan bir panoya erişiminiz varsa, "erişim reddedildi" iletisi görürsünüz.

Maliyet analizine bir bağlantı paylaşmak için dikey pencerenin en üstünde bulunan **paylaşma** ' yı seçin. Bu belirli bir kapsam için bu görünümü açan özel bir URL görünür. Maliyet erişiminiz yoksa ve bu URL 'YI alırsanız "erişim reddedildi" iletisi görürsünüz.

Desteklenen her kapsam için maliyetlere erişim verme hakkında daha fazla bilgi edinmek için bkz. kapsamları gözden geçirin [ve kapsamlar ile çalışın](understand-work-scopes.md).

## <a name="automation-and-offline-analysis"></a>Otomasyon ve çevrimdışı analiz

Daha fazla analiz için verileri indirmeniz, kendi verilerinize birleştirmek veya kendi sistemlerinizle tümleştirmeniz gerektiğinde zaman vardır. Maliyet yönetimi birkaç farklı seçenek sunar. Başlangıç noktası olarak, maliyet analizinde aldığınız gibi bir geçici üst düzey özete ihtiyacınız varsa, ihtiyacınız olan görünümü oluşturun. Ardından **Dışa Aktar** ' ı SEÇIP **verileri CSV 'ye indir** ' i seçerek veya **verileri Excel 'e**indirerek indirin. Excel indirme, indirme oluşturmak için kullandığınız görünümde kapsam, sorgu yapılandırması, toplam ve oluşturulma tarihi gibi ek bağlam sağlar.

Tam, toplanmış veri kümesine ihtiyacınız varsa, faturalandırma hesabından indirin. Ardından, portalın sol gezinti bölmesindeki hizmetler listesinden **maliyet yönetimi + faturalandırma**' a gidin. Varsa Faturalandırma hesabınızı seçin. **Kullanım + ücretler**' e gidin ve ardından istediğiniz fatura dönemi için **İndir** simgesini seçin.

Maliyet verilerini almayı otomatikleştirmek için benzer bir yaklaşım alın. Dinamik filtreleme, gruplama ve toplama ile daha zengin analize yönelik [sorgu API](/rest/api/cost-management/query) 'sini kullanın veya tam, toplanmış veri kümesi Için [UsageDetails API](/rest/api/consumption/usageDetails) 'sini kullanın. Bu API 'lerin genel kullanılabilirlik (GA) sürümü 2019-01-01 ' dir. Bu API 'Lerde rezervasyon ve Market satın alımlarınızın önizlemesine erişim sağlamak için **2019-04-01-Preview** kullanın.

Örneğin, aşağıda, ücretlendirilmiştir (kullanım, satın alma veya geri ödeme), Yayımcı türü (Azure ya da Market), kaynak grubu (satın alma için boş) ve rezervasyon (geçerli değilse boş) ile ayrılmış düşük maliyetlerden oluşan toplu bir görünüm yer alır.

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

Toplama gerekmiyorsa ve tam ve ham veri kümesini tercih ediyorsanız:

```
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?metric=AmortizedCost&$filter=properties/usageStart+ge+'2019-04-01'+AND+properties/usageEnd+le+'2019-04-30'&api-version=2019-04-01-preview
```

Harcamaları tahakkuk ettikleri gibi göstermek için gerçek maliyetleriniz varsa, **tür**/**ölçümünü** **gerçek maliyetle**değiştirin. Bu API 'Ler hakkında daha fazla bilgi için bkz. [sorgu](/rest/api/cost-management/query) ve [UsageDetails](/rest/api/consumption/usageDetails) API 'si belgeleri. Yayımlanan docs 'ın GA sürümüne göre olduğunu unutmayın. Ancak, her ikisi de yeni tür/ölçüm özniteliği dışında *2019-04-01-Preview* API sürümü için aynı ve özellik adlarını değiştirdi. (Aşağıdaki özellik adları hakkında daha fazla bilgi edinin.)

Maliyet yönetimi API 'Leri kaynakların üzerindeki tüm kapsamlar arasında çalışır: Azure RBAC erişimi, EA faturalandırma hesapları (kayıtları), departmanlar ve EA Portal erişimi aracılığıyla kayıt hesapları aracılığıyla kaynak grubu, abonelik ve yönetim grubu. Kapsam KIMLIĞINIZI belirleme veya erişimi yönetme dahil olmak üzere kapsamlar hakkında daha fazla bilgi edinin [ve kapsamları anlayın ve bunlarla çalışın](understand-work-scopes.md).

## <a name="next-steps"></a>Sonraki adımlar

Bütçelerin nasıl oluşturulduğunu ve yönetildiğini öğrenmek için ilk öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Bütçe oluşturma ve yönetme](tutorial-acm-create-budgets.md)
