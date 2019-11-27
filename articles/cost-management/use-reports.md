---
title: Azure'da Cloudyn raporlarını kullanma | Microsoft Docs
description: Bu makalede, Cloudyn portalında, etkili bir şekilde kullanmanıza yardımcı olması için dahil edilen Cloudyn raporlarını amacını açıklar.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: f838091f4b4cdcb0535f58926a724074ef080463
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74218867"
---
# <a name="reports-available-in-the-cloudyn-portal"></a>Cloudyn portalında kullanılabilir raporlar

Bu makalede, Cloudyn portalında bulunan Cloudyn raporlarını amacını açıklar. Ayrıca, raporları etkili bir şekilde nasıl kullanabileceğinizi açıklar. Raporların çoğu, sezgisel ve Tekdüzen bir görünümüne sahip. Çoğu bir raporda gerçekleştirebileceğiniz eylemleri, diğer raporları da yapabilirsiniz. Bkz. Cloudyn raporlarının nasıl özelleştirileceği ve kaydedileceği ve zamanlanarak raporların nasıl kullanılacağı hakkında genel bir bakış için bkz. [Maliyet raporlarını anlama](understanding-cost-reports.md).

Azure Maliyet Yönetimi, Cloudyn'e benzer işlevler sunar. Azure Maliyet Yönetimi, yerel Azure maliyet yönetimi çözümüdür. Maliyet analizi yapmanıza, bütçe oluşturup yönetmenize, verileri dışarı aktarmanıza ve tasarruf önerilerini gözden geçirip gerekli eylemleri gerçekleştirmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure Maliyet Yönetimi](overview-cost-mgt.md).

## <a name="report-types"></a>Rapor türleri

Cloudyn raporlarını üç tür vardır:

- Aşırı zaman raporlar. Örneğin, zaman içinde Maliyet raporu. Aşırı zaman raporlar önceden tanımlanmış bir çözümleme seçili bir aralık boyunca bir zaman serisi verilerinin Göster ve son iki ay boyunca haftalık bir çözüm göster. Gruplandırma ve filtreleme, çeşitli veri noktalarına yakınlaştırmak için kullanabilirsiniz.
  - Aşırı zaman raporlar, eğilimleri görüntülemek ve ani artış ve anomalileri algılayın yardımcı olabilir.
- Analiz raporları. Örneğin, maliyet analizi raporu. Bu raporlar tanımlayın ve verileri filtreleme ve gruplandırma izin bir dönem boyunca toplanan verileri gösterir.
  - Analiz raporları görüntüleyin ani ve anomali nedenlerini belirlemek yardımcı olabilir ve verilerinizi ayrıntılı bir kesme aşağı göstermek için.
- Tablo raporlar. Tablo olarak herhangi bir raporu görüntüleyebilirsiniz, ancak bazı raporlar yalnızca tablo olarak görüntülenir. Bu raporlar, öğe listeleri içeren ayrıntılı sağlar.
  - Öneriler misiniz tablosal raporları — önerileri için herhangi bir görselleştirmenin vardır. Ancak, öneri sonuçları görselleştirebilirsiniz. Örneğin, zaman tasarrufu.
  - Tablo listeleri eylemlerin veya daha fazla işleme için verileri dışarı aktarma için olarak yararlı raporlardır. Örneğin, bir geri ödeme raporu.

Maliyet raporlarında _gerçek_ veya _itfası_ maliyeti gösterilir.

Seçilen bir zaman çerçevesinde yapılan ödemeleri gerçek maliyet raporlarını görüntüleyin. (RI) ayrılmış örnek satın alma gibi gibi tüm tek seferlik ücretler gerçek maliyet raporlarında maliyet ani olarak gösterilir.

Amorti edilmiş maliyet raporlarında, tek seferlik ücretler uygulandıkları bir döneme yayılır. Örneğin, RI satın alma işlemleri için tek seferlik ücretler ayırma dönemi üzerinden yayılan ve bir depo gösterilmez. Amorti edilmiş görünümü true eğilimleri görmek ve maliyet tahminleri yapmak için tek yoludur.

Bazı durumlarda, itfa ayrı bir rapor olarak sunulur. Maliyet analizi ve amorti edilmiş maliyet analizi raporları örneklerindendir. Diğer durumlarda, itfa, maliyet ayırma ve maliyet analizi raporları gibi bir rapor ilkesidir.

Herhangi bir rapordaki düzenli teslim için zamanlayabilirsiniz. Maliyet raporları, uyarılar için yararlı oldukları için bir eşik ayarı izin verir.

## <a name="cost-analysis-vs-cost-allocation"></a>Maliyet ayırma ve maliyet analizi

_Maliyet Analizi_ raporları, bulut sağlayıcılarınızdaki faturalandırma verilerini görüntüler. Raporları kullanarak, Grup ve fatura dosyasından dökümü çeşitli veri kesimlerinde ayrıntısına. Raporları bulut satıcınızın ham faturalama verileri üzerinde ayrıntılı maliyet gezinmeyi etkinleştirme.

Bazı _Maliyet Analizi_ raporları kaynak etiketlerine göre maliyetleri gruplandırmayın. Ve etiket tabanlı faturalandırma bilgileri, [Maliyet tahsisi 360](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs)kullanarak maliyet modeli oluşturarak maliyetleri ayırdıktan sonra raporlarda görüntülenir.

Maliyet tahsisi 360, maliyet [tahsisi](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs)kullanarak bir maliyet modeli oluşturduktan sonra kullanılabilir. Cloudyn maliyet ve faturalandırma verilerini işler ve verileri bulut hesaplarınızın kullanımı ve etiketleyerek verilerle _eşleştirir_ . Verileri eşleştirmek için Cloudyn kullanım verilerinize erişim gerektirir. Kimlik bilgileri eksik olan hesaplarınız varsa, bunlar _kategorilere ayrılmamış kaynaklar_olarak etiketlenir.

## <a name="dashboards"></a>Panolar

Cloudy panoları, raporları üst düzey bir görünümünü sağlar. Pano pencere öğelerinizi yapılır ve aslında bir rapor küçük resim her pencere öğesi ise. [Raporları özelleştirirken](understanding-cost-reports.md#save-and-schedule-reports), bunları Raporlarım kaydeder ve panoya eklenir. Panolar hakkında daha fazla bilgi için bkz. [panolarla ana maliyet ölçümlerini görüntüleme](dashboards.md).

## <a name="budget-information-in-reports"></a>Raporlardaki bilgilerin bütçe

El ile bir oluşturduktan sonra birçok Cloudyn raporlarını bütçe bilgileri gösterir. Bu nedenle bir bütçe oluşturana kadar raporlar bütçe bilgileri gösterilmez. Daha fazla bilgi için bkz. [bütçe yönetimi ayarları](#budget-management-settings).

## <a name="reports-and-reporting-features"></a>Raporlar ve raporlama özellikleri

Cloudyn aşağıdaki raporları ve raporlama özelliklerini içerir.

### <a name="cost-navigator-report"></a>Gezgin rapor maliyeti

Gezgin Maliyet raporu, Pano görünümü kullanılarak fatura tüketiminiz görüntülemek için hızlı bir yoludur. Bir alt kümesini filtreler ve hemen bir Özet görünümü kuruluşun maliyetleri göstermek için temel görünümleri var. Tarihe göre maliyetleri gösterilir. Raporun ilk bir maliyetlerinizi görünüm olarak tasarlandığından kadar esnek veya olarak kapsamlı değil diğer birçok raporlar veya kendi oluşturduğunuz özel panolar.

Varsayılan olarak, ana görünümleri raporunda göster:

- Bir çalışma haftası çubuk grafik görünümü gösteren zaman içinde maliyet. Tarih aralığı çubuk grafiğini değiştirmek için **Tarih aralığını** değiştirebilirsiniz.
- Bir pasta grafiğini kullanarak hizmeti tarafından masrafı.
- Bir pasta grafiğini kullanarak etiketleriyle kaynak kategori.
- Bir pasta grafiğini kullanarak maliyet varlıkları tarafından masrafı.
- Liste görünümünde tarihine göre toplam maliyeti.

### <a name="cost-analysis-report"></a>Maliyet Analizi raporu

Maliyet analizi raporunda, hesaplama ve Ücret yansıtma, ilkesini temel alarak, hesaplamasıdır. Bu, bulut tüketimi bir seçili saat için maliyet ayırma kuralların uygulandıktan sonra çerçevesi sırasında toplar. Örneğin, maliyetleri etiketlere göre hesaplar, maliyetleri etiketlenmemiş kaynakların yeniden atar ve isteğe bağlı olarak ayrılmış örnekler'ın kullanımı ayırır.

[Maliyet tahsisi 360](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ' de ayarlanan Ilkeler maliyet analizi raporunda kullanılır ve sonuçlar daha sonra bulut satıcınızın ham verilerinin bilgileriyle birleştirilir.

Bu rapor nasıl hesaplanır? Cloudyn hizmeti, ayrılan her hesabın bütünlüğünü, _Hesap benzeşimi_uygulayarak korur. Belirli bir hizmete kullanmayan bir hesap maliyetlerin ayrılmış bu hizmetin yok benzeşimi sağlar. Hesap bu hesabında kalır ve ayırma ilkeleri tarafından hesaplanmaz maliyetleri tahakkuk. Örneğin, beş bağlantılı hesaplar olabilir. Yalnızca depolama hizmetleri üç tanesi kullanın, ardından depolama hizmetleri maliyetini yalnızca üç hesaplarında etiketleri arasında ayrılır.

Maliyet analizi raporu kullanın:

- Kuruluş geri ödeme/Ücret hesaplama
- Tüm maliyetlerinizi kategorilere ayırma
- Belirli bir zaman çerçevesi için tüm dağıtımınızı toplu bir görünümünü görüntüler.
- Maliyetleri maliyet modelinde oluşturulan ilkelerine bağlı olarak etiket kategorilere göre görüntüleyin.

Maliyet analizi raporunda kullanmak için:

1. Bir tarih aralığı seçin.
2. Etiketler, gerektiği gibi ekleyin.
3. Grupları ekleyin.
4. Daha önce oluşturduğunuz maliyet modelini seçin.

### <a name="cost-over-time-report"></a>Zaman İçinde Maliyet raporu

Maliyet süresi raporu üzerinden zaman serisi maliyet dağıtma sonuçlarını görüntüler. Eğilimleri inceleyin ve dağıtımınızda sürdürmenin algılamak sağlar. Aslında, tanımlı bir dönem boyunca dağıtılmış maliyetleri gösterir. Rapor eden maliyetler ve seçilen bir zaman çerçevesinde harcanan tek seferlik ayrılmış örnek ücretleri dahil olmak üzere, ana maliyete katkıda içerir. [Maliyet tahsisi 360](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ' de ayarlanan ilkeler bu raporda kullanılır.

Zaman içinde Maliyet raporu kullanın:

- Zaman ve hangi etkiler sonraki bir gün (veya tarih aralığını) değiştirme üzerinden değişiklikleri görebilirsiniz.
- Belirli bir örneği için zaman içinde maliyet çözümleyin.
- Anlamak oluştu neden belirli bir örneği için bir maliyet artışı.

Zaman içinde Maliyet raporu kullanmak için:

1. Bir tarih aralığı seçin.
2. Etiketler, gerektiği gibi ekleyin.
3. Grupları ekleyin.
4. Daha önce oluşturduğunuz maliyet modelini seçin.
5. Gerçek maliyet veya amorti edilmiş maliyet seçin.
6. Görünüm veri görünümü faturalama ham ayırma kuralları uygulanıp uygulanmayacağını seçin veya için maliyet görünümü yeniden hesaplanır.

### <a name="actual-cost-analysis-report"></a>Gerçek maliyet analizi raporu

Gerçek maliyet analizi raporunda herhangi bir değişiklik yapılmadıysa sağlayıcısı maliyetleri gösterir. Bu, devam eden maliyetler ve tek seferlik ücretler dahil olmak üzere ana maliyet bulunanların gösterir.

Abonelikleriniz için maliyet bilgilerini görüntülemek için raporu kullanabilirsiniz. Raporda, Azure abonelikleri **Hesap adı** ve **hesap numarası**olarak gösterilir. **Bağlı hesaplar** AWS aboneliklerini gösterir. Her hesap için bir döküm olan abonelik başına maliyetleri, **gruplar**altında, sahip olduğunuz abonelik türünü seçin.

Gerçek maliyet analizi raporu kullanın:

- Analiz ve belirtilen bir zaman çerçevesi sırasındaki harcanan ham sağlayıcısı maliyetleri izleyin.
- Bir eşiği uyarı zamanlayın.
- Varlıklar ve hesapları değiştirilmemiş maliyetleri analiz edin.

### <a name="actual-cost-over-time-report"></a>Gerçek zaman içinde Maliyet raporu

Zaman içinde gerçek maliyet raporunu üzerinde tanımlanan zaman çözüm maliyet dağıtma standart maliyet analizi raporunu ' dir. Harcama eğilimleri inceleyin ve harcama sürdürmenin algılamak olanak tanımak için zaman içinde rapor görüntüler. Bu rapor, devam eden maliyetler ve seçilen bir zaman çerçevesinde harcanan tek seferlik ayrılmış örnek ücretleri dahil olmak üzere, ana maliyete katkıda gösterir.

Zaman içinde gerçek maliyet raporu kullanın:

- Zaman içinde maliyet eğilimlerinizi görmenin.
- Sürdürmenin maliyetine bulun.
- Bulut sağlayıcıları ile ilgili tüm maliyet ile ilgili soruları bulun.

### <a name="amortized-cost-reports"></a>Amorti edilmiş maliyet raporlarında

Amorti edilmiş maliyet raporları doğrusal duruma gösterir olmayan kullanım bu dizi, hizmet ücretlerini veya tek seferlik borç maliyetleri ve bunların kullanım ömrü boyunca eşit olarak zaman içinde maliyetlerine yayılabilir. Örneğin, tek seferlik ücretler şunlar olabilir:

- Yıllık destek ücretleri
- Yıllık güvenlik bileşen ücretleri
- Ayrılmış örnekler satın alma ücreti
- Bazı Azure Market öğeleri

Tek seferlik ücretler fatura dosyasında belirlenir, hizmet tüketimi başlangıç ve bitiş tarihleri (zaman damgası) eşit değerlere sahip. Cloudyn hizmet daha sonra bunları amorti edilmiş gibi tek seferlik ücretler tanır. İsteğe bağlı kullanım maliyetleri tüketim tabanlı diğer hizmetlerle amorti edilmiş değil.

Amorti edilmiş maliyet raporlarında şunlardır:

- Amorti edilmiş maliyet analizi
- Zaman içinde amorti edilmiş maliyet

### <a name="cost-analysis-report"></a>Maliyet Analizi raporu

Maliyet analizi raporunda, bulut tüketimi ve seçilen bir zaman çerçevesinde harcama öngörü sağlar. [Maliyet tahsisi 360](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ' de ayarlanan Ilkeler, maliyet analizi raporunda kullanılır.

Cloudyn, bu rapor nasıl hesaplar?

Cloudyn, _hesabın benzeşimini_uygulayarak her bir bağlı hesabın bütünlüğünü korumasını sağlar. Belirli bir hizmete kullanmayan bir hesabı da maliyetlerin ayrılmış bu hizmetin yok benzeşimi sağlar. Hesap bu hesabında kalır ve ayırma ilkeleri tarafından hesaplanan olmayan maliyet tahakkuk. Örneğin, beş bağlantılı hesaplar olabilir. Yalnızca depolama hizmetleri üç tanesi kullanın, ardından depolama hizmetleri maliyetini yalnızca üç hesaplarında etiketleri arasında ayrılır.

Maliyet analizi raporu kullanın:

- Belirli bir zaman çerçevesi için tüm dağıtımınızı toplu bir görünümünü görüntüler.
- Maliyetleri maliyet modelinde oluşturulan ilkelerine bağlı olarak etiket kategorilere göre görüntüleyin.

### <a name="cost-over-time-report"></a>Zaman İçinde Maliyet raporu

Zaman içinde Maliyet raporu, dağıtımınızdaki eğilimleri ve bildirim sürdürmenin nokta için zaman içinde harcama görüntüler. Aslında, tanımlı bir dönem boyunca dağıtılmış maliyetleri gösterir. Rapor eden maliyetler ve seçilen bir zaman çerçevesinde harcanan tek seferlik ayrılmış örnek ücretleri dahil olmak üzere, ana maliyete katkıda içerir. [Maliyet tahsisi 360](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ' de ayarlanan ilkeler bu raporda kullanılır.

Zaman içinde Maliyet raporu kullanın:

- Zaman ve hangi etkiler sonraki bir gün (veya tarih aralığını) değiştirme üzerinden değişiklikleri görebilirsiniz.
- Belirli bir örneği için zaman içinde maliyet çözümleyin.
- Anlamak oluştu neden belirli bir örneği için bir maliyet artışı.

### <a name="custom-charges-report"></a>Özel ücretleri rapor

Enterprise ve CSP kullanıcılar kendilerine eklenen Hizmetleri kendi bulut kaynak tüketimi ek olarak, dış veya iç müşterilere sunuyor genellikle bulun. Ek hizmetler veya Müşteri faturalama veya geri ödeme raporlarını özel satır öğeleri olarak eklenir indirimleri özel ücretleri tanımlarsınız.

Normalde bir fatura gösterilmeyen Hizmetleri özel hizmet ücretlerini yansıtır. Oluşturduğunuz özel ücretleri, daha sonra maliyet raporlarında gösterilir.

*Özel ücretler özel fiyatlandırma değildir*. Özel ücretleri listesinde, ücretlendirme, farklı oranları göstermez. Örneğin, AWS faturalandırma ücretleri, yalnızca ücretlendirilir olarak gösterilir.

Özel bir ücret oluşturmak için:

1. **Özel ücretler**' de **Yeni Ekle**' ye tıklayın. _Yeni özel ücret ekle_ iletişim kutusu görüntülenir.
2. **Sağlayıcı adı**' nda sağlayıcının adını girin.
3. **Hizmet adı**alanına hizmet türünü girin.
4. **Açıklama**' da, özel ücret için bir açıklama ekleyin.
5. **Tür**' de, **yüzdeyi** seçin ve ardından hizmetler açılan menüsünde, maliyet raporlarında özel ücretler olarak dahil edilecek hizmetleri seçin.
6. **Ödeme**' de, masrafın tek seferlik bir ücret mi yoksa yineleme ücreti mi olduğunu seçin. Yinelenen bir ücret ücreti ise amorti edilmiş ve kaç aya ilişkin seçmek için ücret istiyorsanız Amortized belirleyin.
7. **Tarihler**' de, bir kerelik ücret seçilirse, **geçerlilik tarihi**' nde, masrafın ödeneceği tarihi girin. Yinelenen ücret seçili ise, başlangıç tarihi ve ücretsiz olarak bitiş tarihi dahil olmak üzere tarih aralığı girin.
8. **Varlıklar ağacında**, ücreti uygulamak istediğiniz varlıkları seçin ve ardından **Açık '** ı seçin.

_Ücretler bir varlığa atandığında, kullanıcılar bunları değiştiremezler. Bir yönetici tarafından bir üst varlığa eklenen ücretler salt okunurdur._

Özel harcamalarını görüntülemek için:

Özel ücretleri maliyet raporlarında gösterilir. Örneğin, gerçek maliyet analizi raporunu açın ve **genişletilmiş filtreler**altında **tek başına**' yı seçin. Ardından **özel ücretleri**göstermek için filtreleyin.

### <a name="cost-allocation-360"></a>360 maliyet ayırma

360 maliyet, maliyetleri tüketilen bulut kaynaklarına atamak için özel bir maliyet ayırma modelleri oluşturmak için kullanın. Birçok raporlar ile özel bir maliyet modelleri oluşturduğunuz özel maliyet modelleri bilgileri gösterir. Ayrıca, maliyet ayırma ile özel bir maliyet modeli oluşturduktan sonra bazı raporlar yalnızca bilgileri gösterir.

Özel maliyet modelleri oluşturma hakkında daha fazla bilgi için bkz. [öğretici: Cloudyn kullanarak maliyetleri yönetme](tutorial-manage-costs.md).

### <a name="cost-vs-budget-over-time-report"></a>Maliyet ve zaman Içinde bütçe raporu

Zamana göre maliyet ve zaman Içinde bütçe raporu, bütçenize göre ana maliyet katkılarını karşılaştırmanızı sağlar. Zaman içinde (üzerinden/altında/nominal) bütçe tüketiminiz görüntüleyebilmesi için atanan bütçe raporda görünür. Raporun üst kısmında alanları Göster/Gizle kullanarak görünüm maliyeti, bütçe, birikmiş maliyeti ve toplam bütçe seçebilirsiniz.

### <a name="current-month-projected-cost-report"></a>Geçerli ay öngörülen maliyet raporu

Geçerli ay öngörülen maliyet raporu, geçerli ayın başından bu yana maliyet özeti hakkında Öngörüler sağlar. Bu rapor önceki ay, ayın başından itibaren maliyetlerinizi görüntüler ve toplamı için geçerli ay öngörülen maliyet. Geçerli ay öngörülen maliyet güncel aylık maliyet ve son 30 gün içinde izlenen maliyeti temel bir projeksiyon toplamı olarak hesaplanır.

Geçerli ay öngörülen maliyet raporu kullanın:

- Proje hizmeti tarafından aylık maliyetler
- Proje aylık maliyetlerini hesaba göre

### <a name="annual-projected-cost-report"></a>Yıllık öngörülen maliyet raporu

Yıllık öngörülen maliyet raporu, yıllık öngörülen maliyet önceki harcama eğilimlerine göre görüntülemenizi sağlar. Bu, sonraki 12 ay genel tahmini maliyetleri gösterir. Tahminleri, bir sonraki 12 ay boyunca kullanım son 30 gün ile ilişkili maliyetleri dayalı ortaya çıkabilecek eğilim işlevi kullanılarak yapılır.

### <a name="budget-management-settings"></a>Bütçe yönetimi ayarları

Bütçe yönetimi, mali yıl için bütçe ayarlamanıza olanak sağlar.

Bir varlığa bir bütçe eklemek için:

1. Bütçe yönetimi sayfasında, **varlıklar**' ın altında, bütçeyi oluşturmak istediğiniz varlığı seçin.
2. Bütçe yılda bütçe oluşturmak istediğiniz yıl seçin.
3. Her ay bütçesini ayarlayıp **Kaydet**' e tıklayın.

Yıllık bütçenin bir dosyayı içe aktarmak için:

1. **Eylemler**' in altında, bütçe için temel olarak kullanmak üzere boş bir CSV şablonunu Indirmek Için **dışarı aktar** ' ı seçin.
2. Bütçe girişlerinizi içeren CSV dosyası doldurun ve yerel olarak kaydedin.
3. **Eylemler**altında **içeri aktar**' ı seçin.
4. Kayıtlı dosyanızı seçin ve ardından **Tamam**' a tıklayın.

Tamamlanan bütçenizi bir CSV dosyası olarak dışa aktarmak için, **Eylemler**altında, dosyayı Indirmek Için **dışarı aktar** ' ı seçin.

Bu tamamlandığında, bütçeniz maliyet analizi raporlarında ve zaman Içindeki maliyet ile bütçeyi raporlamak için görüntülenir. Ayrıca, bütçe eşiklere dayanarak raporları zamanlayabilirsiniz.

### <a name="azure-resource-explorer-report"></a>Azure kaynak Gezgini raporu

Azure kaynak Gezgini rapor Cloudyn'de kullanılabilir tüm Azure kaynaklarını toplu listesini gösterir. Rapor etkili bir şekilde kullanmak için ölçümleri etkin şekilde Azure hesaplarınızı genişletilmiş. Genişletilmiş ölçümler, Azure Vm'lerinizi Cloudyn erişim sağlar. Daha fazla bilgi için bkz. [Azure sanal makineleri için genişletilmiş ölçümler ekleme](azure-vm-extended-metrics.md).

### <a name="azure-resources-over-time-report"></a>Azure kaynakları zaman içinde rapor

Azure kaynakları zaman içinde rapor, belirli bir süre içinde çalışan tüm kaynakları dökümünü gösterir. Rapor etkili bir şekilde kullanmak için ölçümleri etkin şekilde Azure hesaplarınızı genişletilmiş. Genişletilmiş ölçümler, Azure Vm'lerinizi Cloudyn erişim sağlar. Daha fazla bilgi için bkz. [Azure sanal makineleri için genişletilmiş ölçümler ekleme](azure-vm-extended-metrics.md).

### <a name="instance-explorer-report"></a>Örnek Gezgini raporu

Örnek Gezgini rapor, sanal makinelerinizin varlıklar için çeşitli ölçümleri görüntülemek için kullanılır. Aşağıdaki gibi bilgileri görüntülemek için belirli örnekler ayrıntıya yapabilecekleriniz:
- Aralıkları çalışan örneği
- Seçili dönem içinde yaşam döngüsü
- CPU utilization
- Ağ giriş
- Çıkış trafiği
- Etkin diskleri

Örnek Gezgini rapor tanımlı tarih aralığı içinde çalışan tüm aralıkları toplar ve buna uygun olarak veri toplar. Her çalışan aralıkları tarih aralığı içinde görüntülemek için örneği genişletin. Maliyet her örneğinin Seçili aralık temelinde AWS ve Azure listesi fiyatları tarihini hesaplanır. Hiçbir indirimi uygulanmaz. Ek alanları Göster/Gizle alanlarını kullanarak raporu ekleyebilirsiniz.

Örnek Gezgini rapora kullanın:

- Makine başına tahmini maliyet hesaplayın.
- Bir zaman aralığında etkin tüm makinelerin toplanmış çalışan saat dahil olmak üzere, tam bir liste oluşturur.
- Bulut hizmeti sağlayıcısı veya hesabı tarafından bir liste oluşturur.
- Makineleri görüntüle oluşturulduğunda veya bir zaman aralığı sonlandırıldı.
- Tüm şu anda durdu makinelerin görüntüleyin.
- Her makine etiketlerini görüntüleyin.

### <a name="instances-over-time-report"></a>Zaman içinde örnekleri raporu

Zamana göre örnekler raporunu kullanarak, seçili zaman aralığı sırasında her etkin olan makineler sayısı görebilirsiniz. Haftalık veya aylık tanımlanmış bir çözümlemeyi ise, sonuçlar sayısı makineler söz konusu ay sırasında belirli bir günde etkin olur. Raporda görünmesini istediğiniz filtreleri seçmek için bir tarih aralığı seçin.

### <a name="instance-utilization-over-time-report"></a>Örnek zaman içinde kullanımı raporu

Bu rapor, tüm örnekleri için zaman içinde herhangi bir CPU veya bellek kullanımı dökümünü gösterir.

### <a name="compute-power-cost-over-time-report"></a>İşlem zaman içinde Power Maliyet raporu

Zaman içinde işlem gücü rapor, belirtilen tarih aralığında dökümünü işlem gücü sağlar. Diğer raporları makine ya da çalışma zamanı saat çalışan sayısı gösterilmektedir, ancak bu rapor, çekirdek saatleri, işlem birimi saatlerini ve GB RAM saat gösterir.

Rapora kullanın:

- Belirtilen tarih aralığı içinde bilgi işlem gücü denetleyin.
- Zaman temelinde maliyet ayırma modeller üzerinde işlem görünümü.

Bu rapor, [Maliyet tahsisi 360](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ilkelerinize bağlı olduğundan, sonuçlar tanımlı etiketleme ve seçtiğiniz maliyet ilkenize göre gösterilir. Oluşturulan bir ilke yoksa, ardından sonuçların gösterilmediği.

### <a name="compute-power-average-cost-over-time-report"></a>İşlem gücü ortalama zaman içinde Maliyet raporu

İşlem gücü ortalama zaman içinde Maliyet raporu, en fazla çalışan her makine maliyeti görüntülemek için kullanın. Rapor örneği, çekirdek saat, işlem birimi saati ve GB RAM saat başına maliyet, ortalama gösterir. Bu rapor, dağıtımınızın verimliliğini öngörü sağlar.

Bu rapor, [Maliyet tahsisi 360](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs) ilkelerinize bağlı olduğundan, sonuçlar tanımlı etiketleme ve seçtiğiniz maliyet ilkenize göre görüntülenir. Oluşturulan bir ilke yoksa, ardından sonuçların gösterilmediği.

### <a name="s3-cost-over-time-report"></a>Zaman içinde S3 Maliyet raporu

Zaman içinde S3 Maliyet raporu, belirtilen bir zaman çerçevesi için zaman içinde Amazon basit depolama hizmeti (S3 için) maliyetlerini demet başına dökümünü sağlar. Rapor ana maliyet sürücülerdir demetler bulmanıza yardımcı olur ve, S3 eğilimleri gösterir, kullanımı ve harcamayı.

### <a name="s3-distribution-of-cost-report"></a>S3 dağıtım maliyeti raporu

S3 maliyetiniz son bir ay için demet ve depolama sınıfı tarafından analiz etmek için raporu kullanın. Görünürlük eşiği ayarlamak için pasta grafiği görünümü kullanabilirsiniz. Ya da alt toplamları görmek için Tablo görünümü kullanabilirsiniz.

### <a name="s3-bucket-properties-report"></a>S3 Demetini özellikleri raporu

S3 demetini özelliklerini görüntülemek için raporu kullanın. Görünürlük eşiği ayarlamak için pasta grafiği görünümü kullanabilirsiniz. Ya da alt toplamları görmek için Tablo görünümü kullanabilirsiniz.

### <a name="rds-instances-over-time-report"></a>RDS örnekleri zaman içinde rapor

Belirtilen dönemde çalıştırılan tüm Amazon ilişkisel veritabanı hizmeti (RDS) örneklerinin dökümünü görüntülemek için raporu kullanın.

### <a name="rds-active-instances-report"></a>RDS etkin örnekleri raporu

RDS etkin örnekler çözümlemek için bu raporu kullanın. Raporda ek bilgileri görüntülemek için çizgi öğesini genişletin.

### <a name="azure-reserved-instances-report"></a>Azure ayrılmış örnekleri raporu

Azure ayrılmış örnekleri raporun tüm Azure ayrılmış örnekler tek bir görünümünü sağlar. Bu rapor her satın alma, kendi satır öğesi olarak görüntüler. Rapor ayrıca, satın alma ve örnek türüne, vb. kalan gün sayısı türü satın hesabı gibi ilgili satın alma ayrıntılarını gösterir. Göster veya gizle alanları Göster/Gizle kullanarak rapor verileri.

Azure ayrılmış örnekleri raporu görüntülemek için kullanın:

- Tüm rezervasyon satın alma tarihe göre listesi.
- RI süresi dolana kadar kalan süre.
- Tek seferlik ücretler.
- Satın alınan RI'ları, hesap ve ne zaman.

### <a name="aws-reserved-instances-report"></a>AWS ayrılmış örnekleri raporu

AWS ayrılmış örnekleri bu rapor, tek bir görünümde, tüm AWS ayrılmış örnekleri sağlar. Bu rapor her satın alma, kendi satır öğesi ve ilgili satın alma, satın alma ve örnek türüne, vb. kalan gün sayısı türü satın hesabı gibi ayrıntılarını görüntüler olduğu. Göster veya gizle alanları Göster/Gizle kullanarak rapor verileri.

AWS ayrılmış örnekleri raporu görüntülemek için kullanın:

- Tüm rezervasyon satın alma tarihe göre listesi.
- RI süresi dolana kadar kalan süre.
- Tek seferlik ücretler.
- Orijinal satın alma kodu (Rezervasyon kimliği).
- RI satın hesabı ve ne zaman.

### <a name="ec2-ri-buying-recommendations-report"></a>EC2 RI satın alma önerileri raporu

Bulut kaynak tüketimi temelini burada kaynaklara yalnızca kullanıldığı zaman ücret talep üzerine modelidir. Ön taahhüt vardır — kullandığınızda yalnızca, kullandıklarınız için ödeme yaparsınız.

AWS fiyatlandırma modeli, elastik bulut bilgi işlem (EC2) Hizmetleri için bir alternatif sunar — ayrılmış örnek (RI). RI süresince gerektiğinde bu fiyatlandırma modeli kullanıcılar kapasitesini garanti eder. RI önemli fiyat indirimleri, isteğe bağlı fiyat üzerinden sunar. Buna karşılık, kullanıcıları, sanal bir örneğinin kullanılması için bir ön taahhüt yapar. Taahhüt belirli ailesi, boyut, kullanılabilirlik alanı (AZ) ve işletim sistemi için taahhüt (bir veya üç yıl) döneme bağlı. RI AWS hizmetlerini kullanarak müşteri taahhüt kazanmak için geleceğe yönelik kapasite, da verimli bir şekilde planlamak sağlar.

RI'ları, ama tüm ön üç ödeme seçenekleri:

- En yüksek indirimi sunan gün 0 ise, toplu Topla
- Hiçbir ön maliyet -, RI maliyetini aylık RI süresince taksitli, en düşük indirimi sunan
- Ön maliyet, hangi ¼ inç - fiyatının ½ Önden, ücretli kısmi ve düşük olan indirim oranı ile aylık taksitlere geri kalanı ancak, tüm ön hızını kapatın

Cloudyn, son 30 güne ait her makinenin çalışma süresi değerlendirir. Cloudyn, makine ile bir RI geçerli çalışma süresi düzeyinde çalıştırmak için daha uygun maliyetli olduğunda RI satın alma önerir.

Rapor yıldan çoğu tasarruf öneriler gerekçesi gösterir. Öneriler üzerine örnekleri RI ile değiştirme önerin. Raporu doğrudan RI satın alabilirsiniz.

Her sekme, tam bir rapor olarak açılır. Sekmelerdeki önemli bölümleri şunlardır:

- **EC2 RI satın alma etkisi** -Bu bölüm, isteğe bağlı ile ayrılmış örnekler arasındaki farkın benzetimini sağlar. Önerinizi önceden tanımlamış olan filtrelerle tam EC2 RI satın alma etkisi raporunu görmek için **Yakınlaştır**' a tıklayın. Bu rapor, satın alma etkisi, tüm olası RI satın alma işlemleri gösterir. EC2 ayrılmış örnekler satın kaydederken olası görmek için beklenen ortalama çalışma süresi ayarlayabilirsiniz.

- **Analiz kaydediliyor** -Bu bölüm, sağlanan olası tasarrufları ve Cloudyn önerilerini takip eden tasarrufunuzun elde edildiğini sağlar. Gerçek tasarruf miktarı ve yüzde kaydedilen kırmızıyla vurgulanır.

- **EC2 RI tür karşılaştırması** -bu bölümde, Cloudyn 'nin önerilen dağıtımının, tüm ilgili seçenekler de dahil olmak üzere, ROI vurgulamaları vurgulanır. Bu rapor sonuçlarında, makine % 100 açık kalma süresi çalıştırdığı varsayılmaktadır. Ayrıntılı raporu açmak için **Yakınlaştır** ' ı tıklatın.

- **Zamana göre örnekler** -Bu bölüm, öneri, OnDemand, ayrılmış örnekler ve spot ile ilişkili tüm örneklerin dökümünü görüntüler. Ayrıntılı raporu açmak için **Yakınlaştır** ' ı tıklatın.
- **Breakkeven noktaları** -bu bölümde, tüm olası önerilen dağıtımların ve ROI 'nın, ROI 'nin meydana geldiğinde ayın bir tablosu görüntülenir. Ayrıntılı raporu açmak için **Yakınlaştır** ' ı tıklatın.

### <a name="ec2-reservations-over-time-report"></a>EC2 ayırmaları zaman içinde rapor

EC2 ayırmaları zaman içinde rapor, satın alınan EC2 RI'larınızı kullanım durumunu izler. Saat, gün veya hafta çözümleme raporunun ayarlayabilirsiniz.

Rapora kullanın:

- Satın alınan kullanılan ve kullanılmayan ayırmaları görüntülenir.
- Çözümü saat / saat RI kullanımını görmek için ayrıntılara girin.

### <a name="savings-over-time-report"></a>Zaman içinde tasarruf raporu

Ayrılmış örnekler, hem de spot örnekleri kullanarak elde edilen tasarrufu görüntülemek için zaman içinde tasarruf raporu kullanın. Rapor RI gerçekleştirilen elde edilen zaman içinde elde edilen ROI gösterir.

RIS tasarrufunu görüntülemek için, sonuçları **Fiyat modeline** göre gruplayın ve **rezervasyon**' ı seçin. Belirli bir hesabı ya da örnek türü tarafından elde RI tasarrufu görüntülemek için hesap veya örnek türüne ilgili gruplandırma ve filtreleme ekleyin.

Spot örnek kullanımını tasarrufları görmek için, **Fiyat modelini** **noktaya**göre filtreleyin. Bu raporun varsayılan filtresi RI ve Spot örnekleri ' dir.

### <a name="rds-ri-buying-recommendations-report"></a>RDS RI satın alma önerileri raporu

RDS RI satın alma önerileri raporu yerine isteğe bağlı örnekleri RDS RI'ları kullanmak ne zaman önerir.

Her sekme, tam bir rapor olarak açılır. Sekmelerdeki önemli bölümleri şunlardır:

- **RDS RI satın alma etkisi** -Bu bölüm, isteğe bağlı ve ayrılmış örnekler arasındaki farkın benzetimini sağlar. Önerinizi önceden tanımlamış olan filtrelerle tam RDS RI satın alma etkisi raporunu görmek için **Yakınlaştır** ' a tıklayın. Bu rapor, tüm olası RI satın alma işlemleri, satın alma etkisi görmenizi sağlar.  Beklenen ortalama çalışma süresi ayarlama ve kaydetme RI satın alarak olası bakın.
- **Analiz kaydediliyor** – Bu bölüm, sağlanan olası tasarrufları ve Cloudyn önerilerini takip eden tasarrufunuzun elde edildiğini sağlar. Gerçek tasarruf miktarı ve yüzde kaydedilen kırmızıyla vurgulanır.

- **RDS RI tür karşılaştırması** -Bu bölüm, ilgili tüm seçenekler de dahil olmak üzere önerilen dağıtımın ROI vurgularını vurgular. Bu rapor sonuçlarında, makine % 100 açık kalma süresi çalıştırdığı varsayılmaktadır. Seçilen makine için ayrıntılı raporu açmak üzere **Yakınlaştır** ' a tıklayın.
- **Zamana göre örnekler** – Bu bölüm, öneri, OnDemand, ayrılmış örnekler ve spot ile ilişkili tüm örneklerin dökümünü görüntüler. Ayrıntılı raporu açmak için **Yakınlaştır** ' ı tıklatın.

- **Breakkeven noktaları** – bu bölümde, tüm olası önerilen dağıtımların ve ROI 'nın, ROI 'nin meydana geldiğinde ayın bir tablosu görüntülenir. Ayrıntılı raporu açmak için **Yakınlaştır** ' ı tıklatın.

### <a name="rds-reservations-over-time-report"></a>RDS ayırmaları zaman içinde rapor

RDS ayırma zaman içinde rapor, belirtilen süre, her iki kullanılan ve kullanılmayan ayırmaları dökümünü görüntülemek için kullanın.

### <a name="reserved-instance-purchase-impact-report"></a>Ayrılmış örnek satın alma etkisi raporu

EC2 RI satın alma etkisi raporu zamanla ayrılmış örnek maliyeti ile isteğe bağlı maliyetin benzetimini sağlar. Daha iyi satın alma kararları vermenize yardımcı olabilir. Ortalama çalışma zamanı, terim, platform ve başkalarının RI satın alma işlemleri göz önüne aldığınızda bilgiye dayalı kararlar gibi filtrelerini ayarlayın.

### <a name="cost-effective-sizing-recommendations-report"></a>Uygun maliyetli boyutlandırma önerileri raporu

Uygun maliyetli boyutlandırma önerileri raporu, AWS ve Azure için sonuçlar sağlar. AWS kullanıcıları için RI satın alma işlemleri dikkate alınır ve sonuçları RI'ın çalışan makineler içermez. Bu rapor, downsize için aday niteliği taşıyan az kullanılan örneklerinin bir listesini sağlar. Öneriler, son 30 güne ait kullanım ve performans verilerini temel alır. Her bir öneri downsize için aday downsize için gerekçe göstermesi ve tam ayrıntılarını görüntülemek için bir bağlantı listesi ve performans ölçümlerinden birinde bir örneği var. Ve ne zaman ilgili öneriler için yeni nesil örnek türlerini değiştirme.

Örneği bu rapordan downsize için önerilen kimlikleri listesi indirilemiyor. Örnek kimlikleri indirmek için tüm boyutlandırma önerileri raporu kullanın.

Aşağıdaki örnek downsizing göz önünde bulundurun:

Altı m3.xlarge çalışan örneklerini var. Cloudyn analiz bunları beş düşük CPU kullanımı olduğunu gösterir. Bunları downsizing göz önünde bulundurun.

Maliyet etkileri maliyet etkisi hesaplanır. Bu örnekte, satır öğesi genişleterek, geçerli fiyat (Linux/Unix) m3.xlarge örneği için saat ve bir m3.large (Linux/Unix) örneği maliyetleri başına 0.266 0.133 saatlik maliyetleri görebilirsiniz. Bu nedenle, 11,651 kullanımı % 100 çalışan beş m3.xlarge örnekler için yıllık maliyeti. 5,825 kullanımı % 100 çalışan beş m3.large örnekler için yıllık maliyetidir. Olası tasarruf $5,825 ' dir.

Uygun maliyetli boyutlandırma Gerekçeleri görüntülemek için + satır öğesi genişletin. **Ayrıntılar**:

- **Öneri gerekçe** bölümü, geçerli dağıtımı ve alt boyut için önerilen örnek sayısını görüntüler.
- **Maliyet etkisi** bölümü, olası tasarrufları belirlemede kullanılan hesaplamayı görüntüler.
- **Olası yıllık tasarruf** bölümü, Cloudyn 'in önerisi başına yeniden boyutlandırılması sırasında olası yıllık tasarrufları görüntüler.

### <a name="all-sizing-recommendations-report"></a>Tüm boyutlandırma önerileri raporu

Bu rapor, downsize için aday niteliği taşıyan az kullanılan örneklerinin bir listesini sağlar. Öneriler, son 30 güne ait kullanım ve performans verilerini temel alır. Her bir öneri tüm ayrıntılar ve performans ölçümlerinden birinde örneği görüntüleyebilirsiniz.

Bu rapor, AWS ayrılmış örnekler satın aldıysanız RI'ları çalışan örnekleri dahil olmak üzere tüm çalışan örnekler için sonuçları içerir.

Tüm boyutlandırma önerileri raporu kullanın:

- Downsize için aday niteliği taşıyan tüm örneklerinizin bir listesini görürsünüz.
- Örnek adları ve kimlikleri içeren bir rapor listesini dışarı aktarın.

Belirli bir örneğin öneri ayrıntılarını görüntülemek için, ayrıntıları genişletmek üzere **+** ' ye tıklayın. Öneri ayrıntıları bölümü, öneri genel bir bakış sağlar.

**Etiketler** bölümü, seçilen örnek için etiket anahtarlarının ve değerlerin listesini sağlar. Etiketler bölümü filtrelemek için sol bölmede kullanın.

**CPU kullanımı** bölümü, son ay içindeki örneğe göre CPU kullanımını güne göre sağlar.

Detaya gitme ve örnek CPU üzerinden zaman örnekleri dökümünü görmek için raporu açmak için grafiğe tıklayın.

- Alanları eklemek veya kaldırmak için **alanları göster/gizle** ' yi kullanın: timestamp, AVG CPU, min CPU, Max CPU.
- Tarih **aralığını** kullanarak bir tarih veya tarih aralığı girin ve belirli bir InstanceId 'ye gidin.
- Tüm veya belirli bir örnek KIMLIĞINI göstermek için **genişletilmiş filtreleri** kullanın
- CPU kullanımı raporunu açmak için **Yakınlaştır** ' a tıklayın

Örneği 30 gün boyunca izlenen edilmemiş varsa, eksik veri gösterilir.

**Bellek kullanımı (GB)** bölümünde kullanılan bellek hakkında bilgi sağlanır. AWS kullanıcıları için bellek ölçümlerini otomatik olarak bulunmaz ve AWS aracılığıyla örnek başına eklenmesi gerekir. AWS EC2 örneği için bellek ölçümleri etkinleştirmek üzere uygular.

**Bellek kullanımı (%)** bölümünde kullanılan bellek yüzdesi görüntülenir.

**Ağ girişi trafiği** bölümü, seçilen örnek için ağ trafiği, ortalama ve en yüksek zaman içindeki anlık görüntüyü görüntüler. Bu kez tarih ve en çok trafiği görmek için satırları üzerine gelin. Ağ giriş trafiği raporunu açmak için **Yakınlaştır** ' a tıklayın.

**Ağ çıkış trafiği** bölümü, seçilen örnek için ağ çıkış trafiğinin bir anlık görüntüsünü görüntüler. Bu kez tarih ve en çok trafiği görmek için satırları üzerine gelin. Ağ çıkış trafiği raporunu açmak için **Yakınlaştır** ' a tıklayın.

### <a name="instance-metrics-explorer-report"></a>Ölçüm Gezgini rapor örneği

Ölçüm Gezgini örneği rapor örnek başına Bulutlar arası performans ölçümleri gösterir. CPU, bellek ve ağ ölçüm eşikleri bağlı olarak üzerinde veya altında kullanılan örnekleri görüntülemek için raporu kullanın.

Örnek başına Bulutlar arası performansını görüntülemek için:

1. **Tarih aralığı**' nda, performansını görüntülemek istediğiniz bir tarih aralığı seçin.
2. **Etiketler**' de, görüntülemek istediğiniz herhangi bir etiketi seçin.
3. **Filtreler**' de raporda görüntülenmesini istediğiniz filtreleri seçin.
4. **Genişletilmiş filtrelerde**, için rapor eşiklerini ayarlayın:
    - Ortalama CPU
    - En fazla CPU
    - Ortalama bellek
    - En fazla bellek
5. **Genişletilmiş filtreler**' de **göster** ' e tıklayın ve ardından görüntülenecek örneklerin türünü seçin.

Zaman içinde belirli bir örneğine ait ölçümleri görüntülemek için:

- Örnek Ölçüm Gezgini raporuna gidin ve ayrıntıları görüntülemek için **+** ' a tıklayın.

### <a name="rds-sizing-recommendations-report"></a>RDS boyutlandırma önerileri raporu

RDS boyutlandırma önerileri raporu RDS boyutlandırma bulut kullanımınızı iyileştirmek için öneriler sağlar. Bu, downsize için aday niteliği taşıyan az kullanılan örneklerinin bir listesini sağlar. Cloudyn, öneriler, son 30 gün içinde kullanım ve performans verileri üzerinde temel alır. Hesap adı, bölgeye, örnek türüne ve durum göre öneriler filtreleyebilirsiniz.

### <a name="sizing-threshold-manager-report"></a>Boyutlandırma eşiği Manager raporu

Cloudyn'ın yerleşik boyutlandırma önerileri, doğru boyutlandırma önerileri sağlamak için bir karmaşık algoritma kullanılarak hesaplanır. Downsizing önerileri için eşikler ayarlayabilir.

Eşik boyutlandırma önerileri el ile ayarlamak için:

1. İstediğiniz gibi boyutlandırma eşiği Yöneticisi'nde, aşağıdaki eşikleri ayarlayın:
    - Ortalama CPU %
    - Maksimum CPU %
    - Ortalama bellek %
    - Maksimum bellek %
3. Değişiklikleri kaydetmek için **Uygula** ' ya tıklayın.
4. Değişiklikler, tüm önerilerinizi hemen uygulanır.

Varsayılan eşikler geri yüklemek için:

- Boyutlandırma eşiği Yöneticisi ' nde **Varsayılanları geri yükle**' ye tıklayın.

### <a name="compute-instance-types-report"></a>Örnek türleri rapor işlem

Örnek türleri raporu kullanın:

- Hizmet, aile, API adı ve ad örneği türlerini görüntüleyin.
- CPU, ECU, RAM gibi ayrıntılarını görüntülemek ve ağ.

Belirli satır öğelerini bulmak için **Ara** ' yı kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Raporların nasıl özelleştirileceği, kaydedileceği ve zamanlandığının yanı sıra raporların nasıl kullanılacağı hakkında bilgi edinin. bkz. [Maliyet raporlarını anlama](understanding-cost-reports.md).
- Cloudyn 'e dahil olan panolar ve kendi özel panolarınızı oluşturma hakkında bilgi edinin. bkz. [ana maliyet ölçümlerini panolar Ile görüntüleme](dashboards.md).
