---
title: Azure'daki Cloudyn raporlarını kullanma | Microsoft Docs
description: Bu makalede Cloudyn portalındaki raporları verimli bir şekilde kullanmanıza yardımcı olmak için bu raporların amaçları anlatılmaktadır.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/24/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: benshy
ms.custom: seodec18
ms.openlocfilehash: b3563e08963b0b32542dcbece2529c350981f557
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77200019"
---
# <a name="reports-available-in-the-cloudyn-portal"></a>Cloudyn portalında bulunan raporlar

Bu makalede Cloudyn portalındaki raporların amaçları anlatılmaktadır. Ayrıca raporları nasıl verimli bir şekilde kullanabileceğiniz de açıklanmaktadır. Raporlar genellikle sezgiseldir ve aynı görünüme sahiptir. Bir raporda yapabileceğiniz eylemlerin çoğunu genellikle diğer raporlarda da gerçekleştirmek mümkündür. Cloudyn raporlarını özelleştirme, kaydetme veya zamanlama gibi eylemler dahil olmak üzere raporlar hakkında genel bakış için bkz. [Maliyet raporlarını anlama](understanding-cost-reports.md).

Azure Maliyet Yönetimi, Cloudyn'e benzer işlevler sunar. Azure Maliyet Yönetimi, yerel Azure maliyet yönetimi çözümüdür. Maliyet analizi yapmanıza, bütçe oluşturup yönetmenize, verileri dışarı aktarmanıza ve tasarruf önerilerini gözden geçirip gerekli eylemleri gerçekleştirmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure Maliyet Yönetimi](../cost-management-billing-overview.md).

## <a name="report-types"></a>Rapor türleri

Cloudyn raporlarının üç farklı türü vardır:

- Zaman içindeki değişimlerin yer aldığı raporlar. Örneğin, Zaman İçinde Maliyet raporu. Zaman içindeki değişimlerin yer aldığı raporlar, seçilen bir zaman aralığı içindeki zaman serisi verilerini önceden tanımlanmış bir filtreye göre gösterir ve ayrıca son iki ay için haftalık düzeydeki verileri sunar. Gruplandırma ve filtreleme işlevlerini kullanarak farklı veri noktalarını yakınlaştırabilirsiniz.
  - Zaman içindeki değişimleri gösteren raporlar, eğilimleri görüntülemenize ve ani artışları veya anormal durumları tespit etmenize yardımcı olabilir.
- Analiz raporları. Örneğin, Maliyet Analizi raporu. Bu raporlar, belirlediğiniz süre boyunca toplanan verileri gösterir ve bu verileri gruplandırmanızı ve filtrelemenizi sağlar.
  - Verilerinizin ayrıntılı dökümünü sunan analiz raporları, ani artışları görüntülemenize ve anomali kök nedenlerini tespit etmenize yardımcı olabilir.
- Tablo raporları. Tüm raporları tablo olarak görüntüleyebilirsiniz ancak bazı raporlar yalnızca tablo olarak görüntülenebilir. Bu raporlar, ayrıntılı bir öğe listesi sunar.
  - Öneriler tablo raporlarıdır, bunlar için herhangi bir görselleştirme sunulmaz. Ancak öneri sonuçlarını görselleştirebilirsiniz. Örneğin, zaman içindeki tasarruf.
  - Tablo raporları, eylem listesi olarak veya ek işlem gerçekleştirmek üzere verileri dışarı aktarmak için kullanışlıdır. Örneğin, geri ödeme raporu.

Maliyet raporları _gerçek_ veya _amorti edilmiş_ maliyetleri gösterir.

Gerçek maliyet raporları, seçili zaman dilimi içinde yapılan ödemeleri görüntüler. Örneğin ayrılmış örnek (RI) satın alma işlemleri gibi tek seferlik ücretler, gerçek maliyet raporlarında ani artış olarak gösterilir.

Amorti edilmiş maliyetler, tek seferlik ücretleri geçerli oldukları zaman aralığına yayar. Örneğin RI satın alma işlemlerine ait tek seferlik ücretler, rezervasyon süresine yayılır ve ani artış olarak gösterilmez. Amorti edilmiş görünüm, gerçek eğilimleri görmenin ve maliyetlerle ilgili tahminlerde bulunmanın tek yoludur.

Bazı durumlarda amorti edilmiş ücretler ayrı bir rapor halinde sunulur. Örnek olarak Maliyet Analizi ve Amorti Edilmiş Maliyet Analizi raporları verilebilir. Diğer durumlarda amorti etme, Maliyet Ayırma ve Maliyet Analizi raporlarında olduğu gibi bir rapor ilkesidir.

Tüm raporları düzenli teslim edilecek şekilde zamanlayabilirsiniz. Maliyet raporları, eşik belirleme imkanı verdiğinden uyarılar açısından kullanışlıdır.

## <a name="cost-analysis-vs-cost-allocation"></a>Maliyet analizi ve maliyet ayırma karşılaştırması

_Maliyet analizi_ raporları, bulut sağlayıcılarınızla ilgili faturalandırma verilerini görüntüler. Raporları kullanarak faturalandırma dosyasındaki farklı veri segmentlerini gruplandırabilir ve detayına inebilirsiniz. Raporlar, bulut satıcınızın ham faturalandırma verilerinde ayrıntılı maliyet takibi yapmanızı sağlar.

Bazı _maliyet analizi_ raporları, maliyetleri kaynak etiketlerine göre gruplamaz. Ayrıca etiket tabanlı ödeme bilgileri yalnızca [360 Maliyet Dağıtma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) kullanarak bir maliyet modeli oluşturup maliyet ayrımı yaptıktan sonra raporlarda görüntülenir.

_Maliyet ayırma_ raporları, [360 Maliyet Dağıtma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) kullanarak maliyet modeli oluşturduktan sonra kullanılabilir. Cloudyn, maliyet ve faturalandırma verilerini işleyerek verileri kullanımla _eşleştirir_ ve bulut hesaplarınızın verilerini etiketler. Verileri eşleştirmek için Cloudyn'in kullanım verilerinize erişmesi gerekir. Kimlik bilgisi bulunmayan hesaplarınız, _kategorilere ayrılmamış kaynaklar_ olarak etiketlenir.

## <a name="dashboards"></a>Panolar

Cloudyn'deki panolar, raporların üst düzey görünümünü sunar. Panolar, pencere öğelerinden oluşur ve her bir pencere öğesi aslında bir raporun küçük resmidir. [Raporları özelleştirdiğinizde](understanding-cost-reports.md#save-and-schedule-reports) Raporlarım'a kaydetmiş olursunuz ve bu raporlar panoya eklenir. Panolar hakkında daha fazla bilgi için bkz. [Panolarla temel maliyet ölçümlerini görüntüleme](dashboards.md).

## <a name="budget-information-in-reports"></a>Raporlardaki bütçe bilgileri

Çoğu Cloudyn raporu el ile oluşturduğunuz bütçe bilgilerini gösterir. Bu nedenle siz bir bütçe oluşturana kadar raporlarda bütçe bilgileri gösterilmez. Daha fazla bilgi için bkz. [Bütçe Yönetimi ayarları](#budget-management-settings).

## <a name="reports-and-reporting-features"></a>Raporlar ve raporlama özellikleri

Cloudyn aşağıdaki raporları ve raporlama özelliklerini içerir.

### <a name="cost-navigator-report"></a>Maliyet Gezgini raporu

Maliyet Gezgini raporu, pano görünümünü kullanarak faturalandırma kullanımınızı görüntülemenin hızlı bir yoludur. Kuruluşun maliyetlerinin hızlı bir özet görünümünü sunmak için filtre ve temel görünüm alt kümesine sahiptir. Maliyetler tarihe göre gösterilir. Rapor, maliyetlerinizin başlangıç görünümü olarak sunulduğundan sizin oluşturduğunuz diğer birçok rapor veya özel pano kadar esnek ve kapsamlı değildir.

Varsayılan olarak rapordaki ana görünümlerde şu veriler gösterilir:

- Çalışma haftası çubuk grafik görünümünde zaman içindeki maliyet. **Tarih Aralığı** ile tarih aralığı çubuk grafiğini değiştirebilirsiniz.
- Pasta grafik görünümünde hizmete göre harcamalar.
- Pasta grafik görünümünde etiketlere göre kaynak kategorileri.
- Pasta grafik görünümünde maliyet varlıklarına göre harcamalar.
- Liste görünümünde tarihe göre toplam maliyet.

### <a name="cost-analysis-report"></a>Maliyet Analizi raporu

Maliyet Analizi raporu, ilkenize göre hesaplanan ücreti yansıtma ve geri ödeme durumlarını gösterir. Maliyetinize tüm ayırma kurallarını uyguladıktan sonra, seçilen bir zaman dilimindeki bulut tüketiminizi toplar. Örneğin maliyetleri etiketlere göre hesaplar, etiketlenmemiş kaynakların maliyetlerini yeniden atar ve isteğe bağlı olarak ayrılmış örneklerin kullanımını ayırır.

[360 Maliyet Dağıtma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ile belirlenen ilkeler, Maliyet Analizi raporunda kullanılır ve sonuçlar, bulut satıcınızın ham verilerindeki bilgilerle birleştirilir.

Bu rapor nasıl hesaplanır? Cloudyn hizmeti, _hesap benzeşimi_ uygulayarak ayırma verilerinde bağlı hesapların her birinin bütünlüğünün korunmasını sağlar. Benzeşim, belirli bir hizmeti kullanmayan bir hesabın bu hizmet tarafından ayrılan bir maliyete sahip olmamasını sağlar. Bir hesaba tahakkuk eden maliyetler o hesapta kalır ve ayırma ilkelerine göre hesaplanmaz. Örneğin bağlı beş hesabınız olabilir. Bu hesapların yalnızca üçünün depolama hizmetlerini kullanması durumunda depolama hizmetlerinin maliyeti yalnızca bu üç hesaptaki etiketlere ayrılır.

Maliyet Analizi raporunu kullanarak şunları yapabilirsiniz:

- Kuruluşunuzun geri ödeme/ücreti yansıtma durumunu hesaplama
- Tüm maliyetlerinizi kategorilere ayırma
- Belirli bir zaman dilimi için tüm dağıtımınızın toplu görünümünü görüntüleme.
- Maliyetleri, maliyet modelinde oluşturulan ilkeleri temel alan etiket kategorilerine göre görüntüleme.

Maliyet Analizi raporunu kullanmak için:

1. Bir tarih aralığı seçin.
2. Gerekirse etiket ekleyin.
3. Grupları ekleyin.
4. Daha önce oluşturduğunuz maliyet modellerinden birini seçin.

### <a name="cost-over-time-report"></a>Zaman İçinde Maliyet raporu

Zaman İçinde Maliyet raporu, maliyet ayırma sonuçlarını zaman serisi olarak görüntüler. Bu rapor, dağıtımınızdaki eğilimleri incelemenizi ve düzensiz durumları tespit etmenizi sağlar. Rapor temelde maliyetlerin tanımlanan süre içindeki dağılımını gösterir. Rapor, sürekli maliyetler dahil olmak üzere maliyet açısından katkıda bulunan temel noktaları ve belirli bir zaman dilimi içinde harcanan tek seferlik ayrılmış örnek ücretlerini içerir. Bu raporda [360 Maliyet Dağıtma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) bölümünde belirlenen ilkeler kullanılır.

Zaman İçinde Maliyet raporunu kullanarak şunları yapabilirsiniz:

- Zaman içindeki değişimleri ve günlük olarak (veya tarih aralığında) değişen etkileri görüntüleme.
- Belirli bir örnek için zaman içindeki maliyetleri analiz etme.
- Belirli bir örnek için neden maliyet artışı olduğunu anlama.

Zaman İçinde Maliyet raporunu kullanmak için:

1. Bir tarih aralığı seçin.
2. Gerekirse etiket ekleyin.
3. Grupları ekleyin.
4. Daha önce oluşturduğunuz maliyet modellerinden birini seçin.
5. Gerçek maliyetleri veya amorti edilmiş maliyetleri seçin.
6. Ham faturalama verileri görünümünü veya yeniden hesaplanmış maliyet görünümünü görüntülemek için ayırma kurallarının uygulanıp uygulanmayacağını belirtin.

### <a name="actual-cost-analysis-report"></a>Gerçek Maliyet Analizi raporu

Gerçek Maliyet Analizi raporu, sağlayıcı maliyetlerini hiçbir değişiklik yapmadan gösterir. Devam eden maliyetler ve tek seferlik ücretler dahil olmak üzere maliyetlere katkıda bulunan temel öğeleri gösterir.

Aboneliklerinize ilişkin maliyet bilgilerini görüntülemek için raporu kullanabilirsiniz. Raporda Azure abonelikleri **hesap adı** ve **hesap numarası** olarak gösterilir. **Bağlı hesaplar** AWS aboneliklerini gösterir. Her bir hesabın dökümünü içeren abonelik başına maliyetleri görüntülemek için **Gruplar** bölümünde sahip olduğunuz aboneliğin türünü seçin.

Gerçek Maliyet Analizi raporunu kullanarak şunları yapabilirsiniz:

- Belirli bir zaman dilimi içinde harcanan ham sağlayıcı maliyetlerini analiz etme ve izleme.
- Eşik uyarısı zamanlama.
- Hesaplarınız ve varlıklarınız tarafından oluşturulan değiştirilmemiş maliyetleri analiz etme.

### <a name="actual-cost-over-time-report"></a>Zamana Göre Gerçek Maliyet raporu

Zamana Göre Gerçek Maliyet raporu, maliyeti tanımlanan zaman aralığına dağıtan standart bir maliyet analizi raporudur. Bu rapor, zaman içindeki harcamaları göstererek eğilimleri incelemenizi ve düzensiz harcamaları tespit etmenizi sağlar. Bu rapor, sürekli maliyetler dahil olmak üzere maliyet açısından katkıda bulunan temel noktaları ve belirli bir zaman dilimi içinde harcanan tek seferlik ayrılmış örnek ücretlerini gösterir.

Zamana Göre Gerçek Maliyet raporunu kullanarak şunları yapabilirsiniz:

- Zaman içinde maliyet eğilimlerini görüntüleme.
- Düzensiz maliyetleri tespit etme.
- Bulut sağlayıcılarıyla ilgili tüm maliyet sorularını bulma.

### <a name="amortized-cost-reports"></a>Amorti edilmiş maliyet raporları

Bu amorti edilmiş maliyet raporu kümesi, doğrusal kullanıma dayalı olmayan hizmet ücretlerini veya tek seferlik maliyetleri gösterir ve bu maliyetleri ait oldukları zamana yayar. Örneğin tek seferlik ücretler şunları içerebilir:

- Yıllık destek ücretleri
- Yıllık güvenlik bileşeni ücretleri
- Ayrılmış Örnek satın alma ücretleri
- Bazı Azure Market öğeleri

Faturalandırma dosyasında tek seferlik ücretler, hizmet tüketimi başlangıç ve bitiş tarihleri (zaman damgası) eşit olacak şekilde gösterilir. Cloudyn hizmeti bunları amorti edilen tek seferlik ücretler olarak tanımlar. İsteğe bağlı kullanım maliyetlerine sahip olan diğer tüketime dayalı hizmetler amorti edilmez.

Amorti edilmiş maliyet raporları şunları içerir:

- Amorti edilmiş maliyet analizi
- Zamana göre amorti edilmiş maliyet

### <a name="cost-analysis-report"></a>Maliyet Analizi raporu

Maliyet Analizi raporu, seçilen zaman dilimi içindeki bulut tüketiminiz ve harcamalarınız konusunda içgörüler sunar. Maliyet Analizi raporu için [360 Maliyet Dağıtma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ile belirlenen ilkeler kullanılır.

Cloudyn bu raporu nasıl hesaplar?

Cloudyn, _hesap benzeşimi_ uygulayarak ayırma verilerinde bağlı hesapların her birinin bütünlüğünün korunmasını sağlar. Benzeşim, belirli bir hizmeti kullanmayan bir hesabın bu hizmet tarafından ayrılan bir maliyete de sahip olmamasını sağlar. Bir hesaba tahakkuk eden maliyetler o hesapta kalır ve ayırma ilkelerine göre hesaplanmaz. Örneğin bağlı beş hesabınız olabilir. Bu hesapların yalnızca üçünün depolama hizmetlerini kullanması durumunda depolama hizmetlerinin maliyeti yalnızca bu üç hesaptaki etiketlere ayrılır.

Maliyet Analizi raporunu kullanarak şunları yapabilirsiniz:

- Belirli bir zaman dilimi için tüm dağıtımınızın toplu görünümünü görüntüleme.
- Maliyetleri, maliyet modelinde oluşturulan ilkeleri temel alan etiket kategorilerine göre görüntüleme.

### <a name="cost-over-time-report"></a>Zaman İçinde Maliyet raporu

Zaman İçinde Maliyet raporu, dağıtımınızdaki eğilimleri ve düzensiz durumları belirlemeniz için zaman içindeki harcamaları gösterir. Rapor temelde maliyetlerin tanımlanan süre içindeki dağılımını gösterir. Rapor, sürekli maliyetler dahil olmak üzere maliyet açısından katkıda bulunan temel noktaları ve belirli bir zaman dilimi içinde harcanan tek seferlik ayrılmış örnek ücretlerini içerir. Bu raporda [360 Maliyet Dağıtma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) bölümünde belirlenen ilkeler kullanılır.

Zaman İçinde Maliyet raporunu kullanarak şunları yapabilirsiniz:

- Zaman içindeki değişimleri ve günlük olarak (veya tarih aralığında) değişen etkileri görüntüleme.
- Belirli bir örnek için zaman içindeki maliyetleri analiz etme.
- Belirli bir örnek için neden maliyet artışı olduğunu anlama.

### <a name="custom-charges-report"></a>Özel Ücretler raporu

Kurumsal ve CSP kullanıcıları genellikle kendi bulut kaynağı kullanımlarına ek olarak dış veya iç müşterilerine ek hizmetler de sunmaktadır. Müşterinin faturasına veya geri ödeme raporlarına özel satır öğesi olarak eklenen ek hizmetler veya indirimler için özel ücretler belirlenir.

Özel hizmet ücretleri, normalde faturada gösterilmeyen hizmetlere aittir. Bu durumda oluşturduğunuz özel ücretler, Maliyet raporlarında gösterilir.

*Özel ücretler ile özel fiyatlandırma aynı şey değildir*. Özel ücretler listesi, tahsil ettiğiniz farklı ücretleri göstermez. Örneğin AWS faturalandırma ücretleri, tahsil edildiği şekilde görüntülenir.

Özel ücret oluşturmak için:

1. **Özel Ücretler** bölümünde **Yeni Ekle**'ye tıklayın. _Yeni Özel Ücret Ekle_ iletişim kutusu görüntülenir.
2. **Sağlayıcı Adı** alanına sağlayıcının adını girin.
3. **Hizmet Adı** alanına hizmet türünü girin.
4. **Açıklama** alanına özel ücret için bir açıklama ekleyin.
5. **Tür** alanında **Yüzde**'yi seçtikten sonra Hizmetler açılır menüsünde maliyet raporlarına özel ücret olarak eklemek istediğiniz hizmetleri seçin.
6. **Ödeme** alanında Tek Seferlik Ücret veya Yinelenen Ücret arasından seçim yapın. Ücretin Yinelenen Ücret olması halinde amorti edilmesini istiyorsanız Amorti edilmiş'i seçip ay sayısını belirtin.
7. **Tarihler** alanında tek seferlik ücretler için **Geçerli Tarih** bölümüne ücretin ödendiği tarihi girin. Yinelenen Ücret'in seçilmiş olması halinde ücret için başlangıç tarihi ve bitiş tarihi dahil olmak üzere tarih aralığını girin.
8. **Varlıklar ağacı** bölümünde ücreti uygulamak istediğiniz varlıkları belirledikten sonra **Açık**'ı seçin.

_Kullanıcılar, varlığa atanmış olan ücretleri değiştiremez. Yöneticiler tarafından bir üst varlığa eklenmiş olan ücretler salt okunur özelliktedir._

Özel ücretleri görüntülemek için:

Özel ücretler, Maliyet raporlarında gösterilir. Örneğin Gerçek Maliyet Analizi raporunu açtıktan sonra **Genişletilmiş Filtreler** bölümünde **Bağımsız**'ı seçin. Ardından **Özel Ücretler** için filtreleyin.

### <a name="cost-allocation-360"></a>360 Maliyet Dağıtma

360 Maliyet Dağıtma ile tüketilen bulut kaynaklarına maliyet ayırmak için özel maliyet ayırma modelleri oluşturabilirsiniz. Çoğu raporda özel maliyet modelleri ile oluşturduğunuz özel maliyet modelleri hakkında bilgiler görüntülenir. Bazı raporlarda ise yalnızca maliyet dağıtımına sahip bir özel maliyet modeli oluşturduktan sonra bilgiler görüntülenir.

Özel maliyet modelleri oluşturma hakkında daha fazla bilgi için bkz. [Öğretici: Cloudyn kullanarak maliyetleri yönetme](tutorial-manage-costs.md).

### <a name="cost-vs-budget-over-time-report"></a>Zaman İçinde Maliyet ve Bütçe raporu

Zaman İçinde Maliyet ve Bütçe raporu, maliyet için katkıda bulunan temel öğeleri bütçenizle karşılaştırmanızı sağlar. Atanan bütçe raporda görünür ve bu sayede zaman içindeki bütçe kullanımını (fazla/az/eşit) görüntüleyebilirsiniz. Raporun en üstünde bulunan Alanları Göster/Gizle işlevini kullanarak maliyeti, bütçeyi, birikmiş maliyeti ve toplam bütçeyi görüntüleyebilirsiniz.

### <a name="current-month-projected-cost-report"></a>Geçerli Ay Öngörülen Maliyet raporu

Geçerli Ay Öngörülen Maliyet raporu, geçerli ayın başından bugüne kadar olan maliyetlerin özeti hakkında içgörü sağlar. Bu rapor, ayın başından itibaren ücretleri, önceki aya ait ücretleri ve mevcut ay için toplam öngörülen maliyeti görüntüler. Geçerli ay öngörülen maliyeti, bugüne kadar olan aylık maliyetin toplamı ile son 30 gün içinde görülen maliyeti temel alan bir tahmin olarak hesaplanır.

Geçerli Ay Öngörülen Maliyet raporunu kullanarak şunları yapabilirsiniz:

- Hizmete göre aylık maliyet tahmininde bulunma
- Hesaba göre aylık maliyet tahmininde bulunma

### <a name="annual-projected-cost-report"></a>Yıllık Öngörülen Maliyet raporu

Yıllık Öngörülen Maliyet raporu, önceki harcama eğilimlerini temel alarak yıllık öngörülen maliyetleri görüntülemenize olanak sağlar. Bir sonraki 12 ay için öngörülen maliyeti gösterir. Tahminler, son 12 ayı dikkate alan bir eğilim işlevi kullanılarak ve son 30 günlük kullanımla ilgili maliyetlerin dikkate alınmasıyla hesaplanır.

### <a name="budget-management-settings"></a>Bütçe Yönetimi ayarları

Bütçe Yönetimi, mali yılınız için bir bütçe belirlemenizi sağlar.

Bir varlığa bütçe eklemek için:

1. Bütçe Yönetimi sayfasının **Varlıklar** bölümünde bütçe oluşturmak istediğiniz varlığı seçin.
2. Bütçe yılında bütçeyi oluşturmak istediğiniz yılı seçin.
3. Her ay için bütçenizi ayarlayıp **Kaydet**'e tıklayın.

Yıllık bütçe dosyasını içeri aktarmak için:

1. **Eylemler** bölümünde **Dışarı aktar**'ı seçerek bütçe için temel olarak kullanılacak boş CSV şablonu indirin.
2. CSV dosyasına bütçe girişlerinizi ekleyip yerel olarak kaydedin.
3. **Eylemler** bölümünde **İçeri aktar**’ı seçin.
4. Kaydettiğiniz dosyayı seçin ve ardından **Tamam**'a tıklayın.

Tamamlanmış bütçenizi CSV dosyası olarak dışarı aktarmak için **Eylemler** bölümünde **Dışarı aktar**'ı seçip dosyayı indirin.

İşlem tamamlandığında bütçeniz Maliyet Analizi raporlarının yanı sıra Zaman İçinde Maliyet ve Bütçe raporunda da gösterilir. İsterseniz raporları bütçe eşiklerine göre zamanlayabilirsiniz.

### <a name="azure-resource-explorer-report"></a>Azure Kaynak Gezgini raporu

Azure Kaynak Gezgini raporu, Cloudyn'de kullanılabilen tüm Azure kaynaklarının toplu listesini gösterir. Raporu verimli bir şekilde kullanabilmeniz için Azure kaynaklarınızda genişletilmiş ölçümlerin etkinleştirilmiş olması gerekir. Genişletilmiş ölçümler, Cloudyn'in Azure VM'lerinize erişmesini sağlar. Daha fazla bilgi için bkz. [Azure sanal makineleri için genişletilmiş ölçüm ekleme](azure-vm-extended-metrics.md).

### <a name="azure-resources-over-time-report"></a>Zaman İçindeki Azure Kaynakları raporu

Zaman İçindeki Azure Kaynakları raporu, belirli bir süre boyunca çalışan tüm kaynakların dökümünü gösterir. Raporu verimli bir şekilde kullanabilmeniz için Azure kaynaklarınızda genişletilmiş ölçümlerin etkinleştirilmiş olması gerekir. Genişletilmiş ölçümler, Cloudyn'in Azure VM'lerinize erişmesini sağlar. Daha fazla bilgi için bkz. [Azure sanal makineleri için genişletilmiş ölçüm ekleme](azure-vm-extended-metrics.md).

### <a name="instance-explorer-report"></a>Örnek Gezgini raporu

Örnek Gezgini raporu, sanal makinelerinizin varlıklarına yönelik çeşitli ölçümleri görüntülemek için kullanılır. Farklı bilgileri görüntülemek için belirli örneklerin detayına inebilirsiniz; örneğin:
- Örnek çalışma aralıkları
- Seçili dönemdeki yaşam döngüsü
- CPU kullanımı
- Ağ girişi
- Giden trafik
- Etkin diskler

Örnek Gezgini raporu, belirtilen tarih aralığı içinde tüm çalışma aralıklarını bir araya getirir ve verileri uygun şekilde toplar. Tarih aralığı içindeki çalışma aralıklarını ayrı ayrı görüntülemek için ilgili örneği genişletin. Her bir örneğin maliyeti, seçilen tarih aralığı için AWS ve Azure liste fiyatları dikkate alınarak hesaplanır. İndirim uygulanmaz. Alanları Göster/Gizle işlevini kullanarak rapora farklı alanlar ekleyebilirsiniz.

Örnek Gezgini raporunu kullanarak şunları yapabilirsiniz:

- Makine başına tahmini maliyeti hesaplama.
- Belirli bir zaman aralığı boyunca etkin olan tüm makinelerin toplam çalışma saatleri de dahil olmak üzere tam bir liste oluşturma.
- Bulut hizmeti sağlayıcısına veya hesaba göre liste oluşturma.
- Belirli bir zaman aralığı boyunca oluşturulan veya sonlandırılan makineleri görüntüleme.
- Durdurulmuş olan tüm makineleri görüntüleme.
- Her bir makinenin etiketlerini görüntüleme.

### <a name="instances-over-time-report"></a>Zamana Göre Örnekler raporu

Zamana Göre Örnekler raporunu kullanarak seçili zaman aralığı boyunca etkin olan en fazla makine sayısını görüntüleyebilirsiniz. Tanımlanan çözünürlüğün hafta veya ay olması durumunda sonuçlar, o ay içindeki belirli bir günde etkin olan en fazla makine sayısını gösterir. Raporda görüntülenmesini istediğiniz filtreleri seçmek için bir tarih aralığı belirtin.

### <a name="instance-utilization-over-time-report"></a>Zamana İçindeki Örnek Kullanımı raporu

Bu rapor, tüm örnekleriniz için zaman içindeki CPU veya bellek kullanımının dökümünü gösterir.

### <a name="compute-power-cost-over-time-report"></a>Zaman İçindeki İşlem Gücü Maliyeti raporu

Zaman İçindeki İşlem Gücü Maliyeti raporu, belirli bir tarih aralığındaki işlem gücü dökümünü sunar. Diğer raporlarda çalışan makine sayısı veya çalışma süresi görüntülenirken bu raporda Çekirdek saatleri, İşlem birimi saatleri veya GB RAM saatleri gösterilir.

Raporu kullanarak şunları yapabilirsiniz:

- Belirtilen tarih aralığı içindeki işlem gücünü denetleme.
- Maliyet ayırma modellerine göre işlem sürelerini görüntüleme.

Bu rapor, [360 Maliyet Dağıtma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ilkelerinize bağlı olduğundan sonuçlar, seçilen maliyet ilkenizdeki tanımlanmış etiketlemeye ve ilkelere göre gösterilir. İlke oluşturulmadıysa sonuçlar gösterilmez.

### <a name="compute-power-average-cost-over-time-report"></a>Zaman İçindeki İşlem Gücü Ortalama Maliyeti raporu

Zaman İçindeki İşlem Gücü Ortalama Maliyeti raporunu kullanarak her bir makinenin maliyetinden daha fazlasını görüntüleyebilirsiniz. Rapor örnek saati, çekirdek saati, işlem birimi saati ve GB RAM saati başına ortalama maliyetinizi gösterir. Rapor, dağıtımınızın verimliliğiyle ilgili içgörüler sunar.

Bu rapor, [360 Maliyet Dağıtma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ilkelerinize bağlı olduğundan sonuçlar, seçilen maliyet ilkenizdeki tanımlanmış etiketlemeye ve ilkelere göre görüntülenir. İlke oluşturulmadıysa sonuçlar gösterilmez.

### <a name="s3-cost-over-time-report"></a>Zaman İçindeki S3 Maliyeti raporu

Zaman İçindeki S3 Maliyeti raporu, belirli bir zaman aralığı için klasör başına Amazon Simple Storage Service (S3) maliyetlerinin dökümünü sunar. Rapor, maliyete en çok katkıda bulunan klasörleri tespit etmenize yardımcı olmasının yanı sıra S3 kullanımınız ve harcamalarınızla ilgili eğilimleri gösterir.

### <a name="s3-distribution-of-cost-report"></a>S3 Maliyet Dağıtımı raporu

Bu raporu kullanarak önceki aya ait S3 maliyetlerinizi klasöre ve depolama sınıfına göre analiz edebilirsiniz. Görünürlük eşiğini ayarlamak için pasta grafik görünümünü kullanabilirsiniz. Ya da tablo görünümü ile alt toplamları inceleyebilirsiniz.

### <a name="s3-bucket-properties-report"></a>S3 Klasör Özellikleri raporu

Bu raporu kullanarak S3 klasörlerinin özelliklerini görüntüleyebilirsiniz. Görünürlük eşiğini ayarlamak için pasta grafik görünümünü kullanabilirsiniz. Ya da tablo görünümü ile alt toplamları inceleyebilirsiniz.

### <a name="rds-instances-over-time-report"></a>Zaman İçindeki RDS Örnekleri raporu

Bu raporu kullanarak belirtilen süre boyunca çalışan tüm Amazon Relational Database Service (RDS) örneklerinin dökümünü görüntüleyebilirsiniz.

### <a name="rds-active-instances-report"></a>RDS Etkin Örnekler raporu

Bu raporu kullanarak etkin RDS örneklerini analiz edebilirsiniz. Raporda satır öğelerini genişleterek ek bilgileri görüntüleyebilirsiniz.

### <a name="azure-reserved-instances-report"></a>Azure Ayrılmış Örnekleri raporu

Azure Ayrılmış Örnekleri raporu, tüm Azure ayrılmış örneklerinizin tek bir görünümünü sağlar. Bu rapor, her bir satın alma işlemini ayrı bir satırda gösterir. Rapor ayrıca satın alan hesap, satın alma ve örnek türü, kalan gün sayısı gibi satın alma hakkında ayrıntılı bilgiler de gösterir. Alanları Göster/Gizle işlevini kullanarak rapor verilerini gösterebilir veya gizleyebilirsiniz.

Azure Ayrılmış Örnekleri raporunu kullanarak şunları görüntüleyebilirsiniz:

- Satın alma tarihine göre tüm rezervasyonların listesi.
- RI süresinin dolması için kalan süre.
- Tek seferlik ücretler.
- RI satın alan hesap ve satın alma zamanı.

### <a name="aws-reserved-instances-report"></a>AWS Ayrılmış Örnekleri raporu

AWS Ayrılmış Örnekleri raporu, tüm AWS ayrılmış örneklerinizin tek bir görünümünü sağlar. Bu rapor her bir satın alma işlemini ayrı bir satırda görüntüler ve ayrıca satın alan hesap, satın alma ve örnek türü, kalan gün sayısı gibi satın alma hakkında ayrıntılı bilgiler de gösterir. Alanları Göster/Gizle işlevini kullanarak rapor verilerini gösterebilir veya gizleyebilirsiniz.

AWS Ayrılmış Örnekleri raporunu kullanarak şunları görüntüleyebilirsiniz:

- Satın alma tarihine göre tüm rezervasyonların listesi.
- RI süresinin dolması için kalan süre.
- Tek seferlik ücretler.
- Özgün satın alma kimliği (rezervasyon kimliği).
- RI satın alan hesap ve satın alma zamanı.

### <a name="ec2-ri-buying-recommendations-report"></a>EC2 RI Satın Alma Önerileri raporu

Buluttaki kaynakların kullanılmasının temelinde, kaynakların yalnızca kullanıldığında ücretlendirildiği istek üzerine kullanım modeli yer alır. Peşin ödeme yapmanıza gerek yoktur, yalnızca kullandığınız kaynaklar için ve kullandığınız zaman ödeme yaparsınız.

AWS, ayrılmış örnek (RI) hizmeti olan Elastic Compute Cloud (EC2) hizmetlerinde alternatif bir fiyatlandırma modeli sunmaktadır. Bu fiyatlandırma modeli, RI boyunca kullanıcılara ihtiyaç duydukları kapasiteyi sağlama garantisi verir. RI, isteğe bağlı fiyatlandırmaya kıyasla önemli indirimler sunar. Kullanıcılar bunun karşılığında sanal örneklerin kullanılması için peşin taahhütte bulunur. Taahhüt, kullanım süresi boyunca (bir veya üç yıl) belirli bir aile, boyut, kullanılabilirlik alanı (AZ) ve işletim sistemi için geçerlidir. RI, AWS'nin geleceğe dönük kapasitesini verimli bir şekilde planlamasına ve müşterilerin hizmetlerini kullanmaya devam etmesini sağlamasına imkan tanır.

RI için tamamı peşin yapılan üç farklı ödeme seçeneği vardır:

- En yüksek indirimin sunulduğu, ilk günden toplam tutarı ödeme seçeneği
- En düşük indirimin sunulduğu ve peşin ödeme yapılmadan RI maliyetinin RI boyunca aylık taksitler halinde ödendiği seçenek
- İndirim oranı daha düşük olsa da tamamının peşin ödendiği orana yakın olduğu ve ¼ - ½ oranında peşin ödeme yapıldıktan sonra kalan tutarın aylık taksitler halinde ödendiği kısmi peşin ödeme seçeneği

Cloudyn, her bir makinenin son 30 gün içindeki çalışma süresini değerlendirir. Cloudyn, makinenin geçerli çalışma süresi düzeyinde bir RI ile çalıştırılmasının maliyet açısından daha verimli olduğunu belirlemesi halinde RI satın almayı önerir.

Raporda önerileri desteklemek için son bir yıl dikkate alındığında en çok tasarruf sağlayabilecek olanları gösterir. Önerilerde isteğe bağlı örneklerin RI ile değiştirilmesi önerilir. RI'leri doğrudan rapordan satın alabilirsiniz.

Her sekme tam bir rapor olacak açılır. Sekmelerdeki önemli bölümler şunlardır:

- **EC2 RI Satın Alma Etkisi**: Bu bölüm, isteğe bağlı örneklerle ayrılmış örnekler arasındaki farkın benzetimini gösterir. **Yakınlaştır**'a tıklayarak önerinize göre tanımlanmış filtrelere sahip tam kapsamlı EC2 RI Satın Alma Etkisi raporunu görebilirsiniz. Bu rapor, olası RI satın alma işlemlerinin tamamının satın alma etkisini gösterir. EC2 Ayrılmış Örneklerini satın aldığınızda elde edeceğiniz olası tasarrufu görmek için beklenen ortalama çalışma süresini ayarlayabilirsiniz.

- **Tasarruf Analizi**: Bu bölümde takip eden Cloudyn önerileri dikkate alındığında gerçekleşecek olası tasarruf miktarı ve bu tasarrufun elde edileceği ay gösterilir. Gerçek tasarruf tutarı ve tasarruf yüzdesi kırmızı renkte gösterilir.

- **EC2 RI Türü Karşılaştırması**: Bu bölümde Cloudyn tarafından önerilen dağıtımın yatırım getirisi vurgulanır ve ilgili tüm seçenekler dahil edilir. Bunun sonucunda rapor, makinenin %100 çalıştığını kabul eder. Ayrıntılı raporu açmak için **Yakınlaştır**'a tıklayın.

- **Zamana Göre Örnekler**: Bu bölümde OnDemand, Ayrılmış Örnekler ve Spot gibi öneri ile ilişkilendirilmiş olan tüm örneklerin dökümü görüntülenir. Ayrıntılı raporu açmak için **Yakınlaştır**'a tıklayın.
- **Eşitleme Noktaları**: Bu bölümde önerilen tüm dağıtımların ve yatırım getirisinin yanı sıra yatırım getirisinin gerçekleştiği ayın yer aldığı bir tablo görüntülenir. Ayrıntılı raporu açmak için **Yakınlaştır**'a tıklayın.

### <a name="ec2-reservations-over-time-report"></a>Zaman İçindeki EC2 Ayırmaları raporu

Zaman İçindeki EC2 Ayırmaları raporu, satın alınan EC2 RI'lerinin kullanımı durumunu izler. Raporun çözünürlüğünü saat, gün veya hafta olarak ayarlayabilirsiniz.

Raporu kullanarak şunları yapabilirsiniz:

- Kullanılan ve kullanılmayan, satın alınmış rezervasyonları görüntüleme.
- Saat başına RI kullanımını görmek için saate göre görüntüleme.

### <a name="savings-over-time-report"></a>Zaman İçinde Tasarruf raporu

Zaman İçinde Tasarruf raporu, ayrılmış örnekler ve spot örnekler kullanarak elde edilen tasarrufu gösterir. Raporda satın alınan RI ile zaman içinde elde edilen yatırım getirisi de görüntülenir.

RI tasarruflarını görüntülemek için sonuçları **Fiyat Modeli** ölçütüne göre gruplayın ve **Rezervasyon**'u seçin. Belirli bir hesap veya örnek türü ile elde edilen RI tasarrufunu görüntülemek için ilgili gruplamayı ekleyin ve hesap veya örnek türüne filtre uygulayın.

Spot örnek kullanımıyla elde edilen tasarrufları görmek için **Fiyat Modeli** alanını **Spot** ile filtreleyin. Bu rapordaki varsayılan filtre RI ve Spot Örnekler olarak belirlenmiştir.

### <a name="rds-ri-buying-recommendations-report"></a>RDS RI Satın Alma Önerileri raporu

RDS RI Satın Alma Önerileri raporu, isteğe bağlı örnekler yerine RDS RI'lerini kullanmanızın önerildiği noktaları gösterir.

Her sekme tam bir rapor olacak açılır. Sekmelerdeki önemli bölümler şunlardır:

- **RDS RI Satın Alma Etkisi**: Bu bölüm, isteğe bağlı örneklerle ayrılmış örnekler arasındaki farkın benzetimini gösterir. **Yakınlaştır**'a tıklayarak önerinize göre tanımlanmış filtrelere sahip tam kapsamlı RDS RI Satın Alma Etkisi raporunu görebilirsiniz. Bu rapor, olası RI satın alma işlemlerinin tamamının satın alma etkisini görmenizi sağlar.  Beklenen çalışma süresini ayarlayabilir ve RI satın alarak elde edebileceğiniz tasarrufu görebilirsiniz.
- **Tasarruf Analizi**: Bu bölümde takip eden Cloudyn önerileri dikkate alındığında gerçekleşecek olası tasarruf miktarı ve bu tasarrufun elde edileceği ay gösterilir. Gerçek tasarruf tutarı ve tasarruf yüzdesi kırmızı renkte gösterilir.

- **RDS RI Türü Karşılaştırması**: Bu bölümde önerilen dağıtımın yatırım getirisi vurgulanır ve ilgili tüm seçenekler dahil edilir. Bunun sonucunda rapor, makinenin %100 çalıştığını kabul eder. Seçilen makineye ait ayrıntılı raporu açmak için **Yakınlaştır**'a tıklayın.
- **Zamana Göre Örnekler**: Bu bölümde OnDemand, Ayrılmış Örnekler ve Spot gibi öneri ile ilişkilendirilmiş olan tüm örneklerin dökümü görüntülenir. Ayrıntılı raporu açmak için **Yakınlaştır**'a tıklayın.

- **Eşitleme Noktaları**: Bu bölümde önerilen tüm dağıtımların ve yatırım getirisinin yanı sıra yatırım getirisinin gerçekleştiği ayın yer aldığı bir tablo görüntülenir. Ayrıntılı raporu açmak için **Yakınlaştır**'a tıklayın.

### <a name="rds-reservations-over-time-report"></a>Zaman İçindeki RDS Ayırmaları raporu

Zaman İçindeki RDS Ayırmaları raporunu kullanarak belirtilen dönem içindeki kullanılan ve kullanılmayan rezervasyonların dökümünü görüntüleyebilirsiniz.

### <a name="reserved-instance-purchase-impact-report"></a>Ayrılmış Örnek Satın Alma Etkisi raporu

EC2 RI Satın Alma Etkisi raporu, zaman içindeki ayrılmış örnek maliyetleri ile isteğe bağlı örnek maliyetlerini karşılaştırmanızı sağlar. Bu karşılaştırma, daha iyi kararlar vermenize yardımcı olabilir. RI satın alma konusunda daha bilinçli kararlar vermenize yardımcı olmak için ortalama çalışma süresi, süre, platform ve diğer filtreleri ayarlayın.

### <a name="cost-effective-sizing-recommendations-report"></a>Uygun Maliyetli Boyutlandırma Önerileri raporu

Uygun Maliyetli Boyutlandırma Önerileri raporu, AWS ve Azure için sonuçlar sunar. AWS kullanıcılarının RI satın alma işlemleri dikkate alınır ve RI olarak çalışan makineler sonuçlara dahil edilmez. Bu rapor, boyutu küçültülebilecek az kullanılan örneklerin bir listesini sunar. Öneriler, son 30 güne ait kullanım ve performans verilerini temel alır. Her bir öneride boyutu küçültebilecek adayların listesi, boyutunu küçültme nedeni ve örneğin tüm ayrıntılarını ve performans ölçümlerini görüntülemenizi sağlayacak bir bağlantı bulunur. Ayrıca yeni nesil örnek türlerine geçiş yapılmasıyla ilgili öneriler de sunulur.

Bu raporda boyutunun küçültülmesi önerilen örneklerin kimliğinin yer aldığı listeyi indiremezsiniz. Örnek kimliklerini indirmek için Tüm Boyutlandırma Önerileri raporunu kullanın.

Aşağıdaki boyut küçültme örneğini inceleyin:

Çalışan altı adet m3.xlarge örneğiniz var. Cloudyn analizi, bunların beş tanesinde CPU kullanımının düşük olduğunu gösteriyor. Bunların boyutunu küçültebilirsiniz.

Maliyet Etkisi bölümünde bu işlemin maliyet etkisi hesaplanır. Bu örnekte satır öğesini genişleterek bir adet m3.xlarge örneğinin (Linux/Unix) saatlik maliyetinin 0,266 ABD doları, bir adet m3.large örneğinin (Linux/Unix) saatlik maliyetinin ise 0,133 ABD doları olduğunu görebilirsiniz. Buna göre %100 kullanım oranına sahip beş adet m3.xlarge örneğinin yıllık maliyeti 11.651 ABD dolarıdır. %100 kullanım oranına sahip beş adet m3.large örneğinin yıllık maliyeti ise 5.825 ABD dolarıdır. Burada 5.825 ABD doları tasarruf sağlayabilirsiniz.

Maliyet açısından verimli boyutlandırma gerekçeleri için + simgesine tıklayarak satır öğesini genişletin. **Ayrıntılar** bölümünde:

- **Önerilen Gerekçe** bölümünde geçerli dağıtım ve boyutunun küçültülmesi önerilen örnek sayısı gösterilir.
- **Maliyet Etkisi** bölümünde olası tasarrufun belirlenmesi için kullanılan hesaplama gösterilir.
- **Yıllık Olası Tasarruf** bölümünde Cloudyn önerilerine göre boyut küçültme işlemi gerçekleştirildiğinde yıllık olası tasarruf miktarı gösterilir.

### <a name="all-sizing-recommendations-report"></a>Tüm Boyutlandırma Önerileri raporu

Bu rapor, boyutu küçültülebilecek az kullanılan örneklerin bir listesini sunar. Öneriler, son 30 güne ait kullanım ve performans verilerini temel alır. Her bir öneride ölçeğin tüm ayrıntılarını ve performans ölçümlerini görüntüleyebilirsiniz.

AWS ayrılmış örnekleri satın aldıysanız bu raporda RI olarak çalışan örnekler dahil olmak üzere çalışan tüm örneklere ait sonuçlar yer alır.

Tüm Boyutlandırma Önerileri raporunu kullanarak şunları yapabilirsiniz:

- Boyutunu küçültebileceğiniz tüm örneklerin listesini görüntüleme.
- Örnek adlarını ve kimliklerini içeren bir listeyi dışarı aktarma.

Belirli bir örneğe ait önerilerin ayrıntılarını görüntülemek için **+** simgesine tıklayarak ayrıntıları genişletin. Öneri Ayrıntıları bölümünde öneriyle ilişkin genel bakış sunulur.

**Etiketler** bölümünde seçilen örneğe ait etiketlerin anahtarlarını ve değerlerini içeren liste sağlanır. Sol taraftaki bölmede yer alan etiketleri kullanarak bölümü filtreleyebilirsiniz.

**CPU Kullanımı** bölümünde günlere göre önceki ay içindeki örnek CPU kullanımı sunulur.

Grafiğe tıklayarak detaya gidebilir ve Zaman İçindeki Örnek CPU'su Raporu'nu açarak örneklerin dökümünü görebilirsiniz.

- Alan eklemek veya kaldırmak için **Alanları Göster/Gizle** seçeneğini kullanın: Zaman damgası, Ortalama CPU, En Az CPU, En Fazla CPU.
- **Tarih Aralığı**'nı kullanarak bir tarih veya tarih aralığı girebilir, belirli bir örnek kimliğinin detayına gidebilirsiniz.
- **Genişletilmiş Filtreler**'i kullanarak tüm örnek kimliklerini veya belirli kimlikleri gösterebilirsiniz
- **Yakınlaştır**'a tıklayarak CPU Kullanımı Raporunu açabilirsiniz

Örneğin 30 gün boyunca izlenmemiş olması durumunda eksik veriler gösterilir.

**Bellek Kullanımı (GB)** bölümünde kullanılan bellekle ilgili bilgiler yer alır. AWS kullanıcıları için bellek ölçümleri otomatik olarak sunulmaz ve AWS üzerinden her bir örnek için ayrıca eklenmesi gerekir. AWS, EC2 örnekleri için bellek ölçümlerini ücret karşılığında sunmaktadır.

**Bellek Kullanımı (%)** bölümünde kullanılan bellek yüzdesi gösterilir.

**Ağ Giriş Trafiği** bölümünde seçilen örnek için belirli bir zaman aralığı içindeki ortalama ve en fazla ağ trafiği gösterilir. Tarihi ve o tarihe ait en fazla trafiği görmek için satırların üzerinde gezinin. Ağ Giriş Trafiği Raporunu açmak için **Yakınlaştır**'a tıklayın.

**Ağ Çıkış Trafiği** bölümünde seçilen örnek için ağ trafiğinin anlık görüntüsü gösterilir. Tarihi ve o tarihe ait en fazla trafiği görmek için satırların üzerinde gezinin. Ağ Çıkış Trafiği raporunu açmak için **Yakınlaştır**'a tıklayın.

### <a name="instance-metrics-explorer-report"></a>Örnek Ölçüm Gezgini raporu

Örnek Ölçüm Gezgini raporu, örnek başına bulutlar arası performans ölçümlerini gösterir. Bu raporu kullanarak CPU, bellek ve ağ ölçümü eşiklerine göre fazla veya az kullanılan örnekleri görüntüleyebilirsiniz.

Örnek başına bulutlar arası performansı görüntülemek için:

1. **Tarih Aralığı** bölümünde performansı görüntülemek istediğiniz tarih aralığını seçin.
2. **Etiketler** bölümünde görüntülemek istediğiniz etiketleri seçin.
3. **Filtreler** bölümünde raporda görüntülemek istediğiniz filtreleri seçin.
4. **Genişletilmiş Filtreler** bölümünde şunlara ait rapor eşiklerini ayarlayın:
    - Ortalama CPU
    - En Fazla CPU
    - Ortalama Bellek
    - En Fazla Bellek
5. **Genişletilmiş Filtreler** bölümünde **Göster**'e tıklayıp görüntülemek istediğini örnek türünü seçin.

Belirli bir örneğin zaman içindeki ölçümlerini görüntülemek için:

- Örnek Ölçüm Gezgini raporuna gidin ve ayrıntıları görüntülemek için **+** simgesine tıklayın.

### <a name="rds-sizing-recommendations-report"></a>RDS Boyutlandırma Önerileri raporu

RDS Boyutlandırma Önerileri raporu, bulut kullanımınızı iyileştirmeye yönelik RDS boyutlandırma önerileri sağlar. Boyutu küçültülebilecek az kullanılan örneklerin bir listesini sunar. Cloudyn önerileri, son 30 güne ait kullanım ve performans verilerini temel alır. Önerileri Hesap Adı, Bölge, Örnek Türü ve Durum ölçütlerine göre filtreleyebilirsiniz.

### <a name="sizing-threshold-manager-report"></a>Boyutlandırma Eşiği Yöneticisi raporu

Cloudyn'deki yerleşik boyutlandırma önerileri, doğru boyutlandırma önerileri sağlamak için karmaşık bir algoritma kullanılarak hesaplanır. Boyut küçültme önerilerinde kullanılacak eşikleri ayarlayabilirsiniz.

Eşik boyutlandırma önerilerini el ile ayarlamak için:

1. Boyutlandırma Eşiği Yöneticisi'nde aşağıdaki eşik değerlerini istediğiniz gibi ayarlayın:
    - Ortalama CPU %
    - En fazla CPU %
    - Ortalama Bellek %
    - En Fazla Bellek %
3. Değişiklikleri kaydetmek için **Uygula**'ya tıklayın.
4. Değişiklikler tüm önerilerinize anında uygulanır.

Varsayılan eşikleri geri yüklemek için:

- Boyutlandırma Eşiği Yöneticisi'nde **Varsayılanları Geri Yükle**'ye tıklayın.

### <a name="compute-instance-types-report"></a>İşlem Örneği Türleri raporu

İşlem Örneği Türleri raporunu kullanarak şunları yapabilirsiniz:

- Örnek türlerini Hizmet, Aile, API Adı ve Ad ölçütlerine göre görüntüleme.
- CPU, ECU, RAM ve Ağ gibi ayrıntıları görüntüleme.

**Ara** işlevini kullanarak belirli satır öğelerini bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Özelleştirme, kaydetme ve zamanlama dahil olmak üzere raporları nasıl kullanacağınız hakkında bilgi edinmek için bkz. [Maliyet raporlarını anlama](understanding-cost-reports.md).
- Cloudyn'deki panolar hakkında bilgi edinmek ve kendi özel panolarınızı nasıl oluşturabileceğinizi öğrenmek için bkz. [Panolarla temel maliyet ölçümlerini görüntüleme](dashboards.md).
