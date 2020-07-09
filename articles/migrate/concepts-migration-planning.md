---
title: Azure geçişi ile derleme geçiş planı | Microsoft Docs
description: Azure geçişi ile geçiş planı oluşturmaya yönelik rehberlik sağlar.
author: rashi-ms
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/21/2020
ms.author: rajosh
ms.openlocfilehash: e80ec71bef8c34331fac076b7b08be48a9571c2e
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86107708"
---
# <a name="build-migration-plan-with-azure-migrate"></a>Azure Geçişi ile geçiş planı oluşturma

Bu makalede [, Azure geçişi Ile Azure](migrate-services-overview.md)'a geçiş planınızı oluşturmaya yardımcı olacak hızlı bir kılavuz sunulmaktadır.Başka sorularınız varsa şu kaynakları kontrol edin:

- Azure geçişi hakkında [genel sorular](resources-faq.md)
- [Azure geçişi](common-questions-appliance.md) gereci hakkında sorular
- [Sunucu geçişi](common-questions-server-migration.md) hakkında sorular
- [Azure geçişi forumundaki](https://aka.ms/AzureMigrateForum) soruların yanıtlarını alın

## <a name="define-the-goals-of-cloud-migration"></a>Bulut geçişinin hedeflerini tanımlama

Bir geçiş planı oluşturmadan önce, daha başarılı iş sonuçları oluşturulmasına yardımcı olabilecek bir buluta geçmek için [mosyon](/azure/cloud-adoption-framework/strategy/motivations) 'nın anlaşılması ve değerlendirilmesi önemlidir. [Azure Için bulut benimseme çerçevesi](/azure/cloud-adoption-framework) 'nin açıklandığı şekilde, işletmeniz için uygun olan farklı tetikleyiciler ve geçiş yaklaşımları olabilir:  

**Kritik iş olayları** | **Geçiş sonucu**
--- | ---
Veri merkezi çıkışı | Maliyet tasarrufları
Merger, alım veya divestiture | Satıcının veya teknik karmaşıklığın azaltılması
Sermaye masraflarını azaltma | İç işlemleri iyileştirme
Görev açısından kritik teknolojiler için destek sonu | İş çevikliği artırma
Yasal uyumluluk değişikliklerine yanıt verme | Yeni teknik yetenekler için hazırlık
Yeni veri egemenlik gereksinimleri | Pazar taleplerini karşılamak için ölçekleme
Kesintileri azaltma ve BT kararlılığını iyileştirme | Coğrafi talepleri karşılamak için ölçekleme

Geçiş motasyon, Azure 'a geçiş yaparak elde etmek istediğiniz stratejik hedefleri ve sonuçları yansıtmanıza da yardımcı olabilir. Sonraki adım, iş yükleriniz için uyarlanmış bir Azure 'a geçiş yolu tanımlamak ve planlamak. Azure geçişi: Sunucu değerlendirmesi Aracı, şirket içi iş yüklerini değerlendirmenize yardımcı olur ve geçiş yapmanıza yardımcı olacak yönergeler ve araçlar sağlar.

## <a name="understand-your-digital-estate"></a>Dijital Emlak bilgilerinizi anlayın

Azure 'a geçirmek istediğiniz iş yüklerini ve en iyi duruma getirilmiş maliyet projeksiyonlarını tanımanıza yardımcı olması için şirket içi altyapınızı, uygulamalarınızı ve bağımlılıklarınızı öğrendim ile başlayın. Sunucu değerlendirmesi Aracı, aşağıdaki soruları yanıtlamanıza yardımcı olur:

### <a name="what-workloads-are-in-use"></a>Hangi iş yükleri kullanımda?

Şirket içi VMware VM 'Leri, Hyper-V VM 'Leri ve fiziksel sunucularınızın aracısız bir bulmayı gerçekleştirmek için basit Azure geçişi gereci kullanın. Sürekli bulma, makine yapılandırmasını ve performans meta verilerini toplar ve yüklü uygulamaların envanterini ve şirket içi makinelerinizde çalışan rolleri/özellikleri almak için de kullanılabilir. Azure geçişi gereci şunları toplar:

- Makinelerin, disklerin ve NIC 'lerin meta veri ayrıntıları

- Uygulamalar ve roller/özellikler dahil yüklü uygulamalar  

- CPU ve bellek kullanımı, disk ıOPS ve aktarım hızı dahil performans verileri

Sonra, iş yüklerinizde çalışan tüm SQL Server örneklerini öğrenmek için uygulama envanteri listesini dışarı aktarın ve Azure geçişi: veritabanı değerlendirmesi aracını kullanarak hazırlığını anlayın.

 ![Portalda uygulama envanteri](./media/concepts-migration-planning/application-inventory-portal.png)

 ![Uygulama envanteri dışarı aktarma](./media/concepts-migration-planning/application-inventory-export.png)

Sunucu değerlendirmesi aracının keşif verileri ile birlikte, sunucu ve veritabanınızın görünümünü oluşturmak ve iş birimleri, uygulama sahipleri, coğrafi öğeler, vb. genelinde sunucu dağıtımını anlamak için mevcut CMDB verilerinizi kullanın.

### <a name="what-dependencies-exist-between-workloads"></a>İş yükleri arasında hangi bağımlılıklar var?

Sunucularınızı bulduktan sonra, birbirine bağlı sunucuları Azure 'a taşımak için çapraz sunucu bağımlılıklarını ve iyileştirme stratejilerini görselleştirmek ve tanımlamak üzere aracısız bağımlılık eşlemesini kullanın. Görselleştirme, belirli makinelerin kullanımda olup olmadığını veya geçirilmesi yerine kullanımdan kaldırılan olup olmayacağını anlamanıza yardımcı olur.  Herhangi bir şeyin geri ayrılmaması ve geçiş sırasında beklenmedik kesintilerden kaçınmak için bağımlılıkları analiz ettiğinizden emin olun. Uygulama envanterini ve bağımlılık eşlemesini gerçekleştirdikten sonra, yüksek güvenilirlikli gruplar oluşturabilir ve sunucularınızı değerlendirmeye başlayabilirsiniz.

 ![Bağımlılık eşlemesi](./media/concepts-migration-planning/expand-client-group.png)

### <a name="are-they-optimized-and-sized-properly"></a>İyileştirilen ve düzgün boyutlandırıldı mı?

Azure, bulut kapasitenizi zaman içinde yeniden boyutlandırmak için esneklik sağladığından, sunucularınıza ayrılan CPU ve bellek kaynaklarını iyileştirebilmeniz için bir fırsattır. İş yüklerinin performans geçmişini anlamak için önce tanımlanan grup üzerinde bir değerlendirme oluşturun; Bu işlem, Azure 'daki doğru sanal makine SKU 'Ları ve disk önerileri konusunda çok önemlidir.

## <a name="assess-your-readiness-for-migration"></a>Geçiş hazırlığını değerlendirin

### <a name="readiness-and-suitability-analysis-for-azure"></a>Azure için hazırlık ve uygunluk Analizi
Azure VM değerlendirmesi raporunu dışarı aktarın ve VM 'nin Azure için hazır olduğunu anlamak için aşağıdaki hazırlık kategorilerine göre filtreleme yapın:

- **Azure Için hazırlanma**: Bu makineleri, herhangi bir değişiklik yapmadan Azure 'a geçirebilirsiniz  

- **Azure Için koşullu**olarak kullanılabilir: Bu makineleri Azure 'a geçirebilirsiniz, ancak değerlendirmede belirtilen düzeltme yönergelerine göre bu sunucularda küçük değişikliklere ihtiyaç duyarsınız

- **Azure için hazırlanma**: Bu makineleri Azure 'a geçirmezsiniz ve geçişten önce düzeltme yönergelerine göre sorunları çözmeniz gerekir

- **Hazır olma bilinmiyor**: Azure geçişi, yetersiz meta veri nedeniyle makinenin hazır olduğunu belirleyemiyor

Veritabanı değerlendirmelerini kullanarak, SQL Server verilerinizi Azure SQL veritabanı 'na veya Azure SQL yönetilen örneklerine geçirme hazırlığını değerlendirebilirsiniz. SQL Server örneklerinizin her biri için geçiş hazırlık durumu yüzdesini görebilirsiniz. Ayrıca, örneklerin her biri için Azure 'da önerilen hedefi, olası geçiş engelleyicileri, önemli değişiklikler sayısını, Azure SQL DB/Azure SQL VM için hazırlık ve uyumluluk düzeyini görebilirsiniz. Geçiş engelleyicileri ve bunları gidermeye yönelik önerilerin etkisini anlamak için daha ayrıntılı bilgi alabilirsiniz.

 ![Veritabanı değerlendirmeleri](./media/concepts-migration-planning/database-assessment-portal.png)

### <a name="sizing-recommendations"></a>Boyutlandırma önerileri

Makine Azure için hazırlanıyor olarak işaretlendikten sonra sunucu değerlendirmesi, sanal makinelerinize yönelik Azure VM ve disk SKU 'sunu belirlemek için boyutlandırma önerileri sağlar. Performans geçmişine göre (geçiş yaparken kaynakları iyileştirirken) veya performans geçmişini dikkate almadan şirket içi yapılandırmaya bağlı olarak boyutlandırma önerisini görmeyi seçebilirsiniz. Veritabanları için veritabanı değerlendirmede veritabanı SKU 'SU, fiyatlandırma katmanı ve işlem düzeyiyle ilgili önerileri görebilirsiniz.  

### <a name="compute-assessments-to-get-estimated-costs-for-running-the-workloads-in-azure"></a>Azure 'da iş yüklerini çalıştırmaya yönelik Tahmini maliyetleri almak için değerlendirme hesaplama

Değerlendirmelerinde *performans tabanlı* sağ boyutlandırma seçeneği, Azure için iş yüklerini iyileştirmenize olanak tanır. Maliyet tasarrufu sağlamaya yardımcı olmak için birkaç başka farklı şekilde göz atabilirsiniz:

- **Ayrılmış örnekler**: ayrılmış örneklerle, Windows ve Linux (VM) ' de 1 yıl veya 3 yıllık koşullarla Kullandıkça Öde fiyatlandırmasıyla karşılaştırıldığında maliyetleri önemli ölçüde azaltabilirsiniz.

- **Azure hibrit avantajları**: Yazılım Güvencesi kapsamındaki Windows Server şirket Içi lisanslarını Azure 'a getirebilir ve ayrılmış örnekler seçenekleriyle birleştirebilirsiniz

- **Kurumsal anlaşma teklif (EA)**: kurumsal anlaşma, aboneliğiniz için geçerli olan yerleşik tasarruflar sunar

- **Teklifler**: birden çok Azure teklifi vardır. örneğin, Kullandıkça Öde Geliştirme ve Test ve Kurumsal Geliştirme ve test geliştirme veya test VM 'leri için daha düşük ücretler sağlar

- **VM çalışma süresi**: Azure VM 'lerinin maliyetlerinizi azaltmak için çalışacağı gün başına gün ve saat başına gün ve saat olarak bir süre (RI için uygulanamaz) belirtebilirsiniz

- **Hedef bölge**: bir coğrafya içindeki belirli bir bölgeye geçiş yapmak daha uygun maliyetli olabileceğinden karşılaştırmak için farklı bölgelerde birden fazla değerlendirme oluşturabilirsiniz

- **Performans tabanlı öneriler**: en iyi uygulama olarak, bulut maliyetlerini kaydetmenize yardımcı olacak doğru Azure VM önerilerini kullanmayı deneyin

### <a name="visualize-data"></a>Verileri görselleştirme

Sunucu değerlendirmesi raporunu portalda Azure hazırlığını ve aylık maliyet dağıtımını görebilir ve geçiş planını daha zengin hale getirmek için bulma ve değerlendirme verilerinde daha fazla görselleştirme uygulamak üzere değerlendirmeyi dışarı aktarabilirsiniz. Farklı özellik birleşimleri için birden fazla değerlendirme oluşturabilir ve işletmeniz için en iyi şekilde çalışan özellikler kümesini seçebilirsiniz.  

 ![Değerlendirmelere genel bakış](./media/concepts-migration-planning/assessment-summary.png)

### <a name="evaluate-gaps-and-potential-blockers"></a>Boşlukları ve olası engelleyicileri değerlendirin

Geçirilecek uygulamaları ve temel altyapıyı belirlerken, bu uygulamalar için kapalı kalma süresi kısıtlamalarını belirleyin ve uygulamalarınız ile temel alınan altyapı arasındaki işlem bağımlılıklarını arayın. Bu analiz, kurtarma süresi hedefini (RTO) karşılayan geçişleri planlamanız ve sıfır veri kaybını en aza düşürbaşlamanıza yardımcı olabilir. Geçirmeden önce, sunucu değerlendirmesi raporunun ve Azure geçişi: veritabanı değerlendirmesi önerilerinin yardımıyla sunucularınızı ve SQL veritabanlarınızı taşımaya engel olabilecek uyumluluk sorunlarını veya desteklenmeyen özellikleri incelemenizi ve azaltmanız önerilir.

### <a name="first-workloads-to-target-and-approach"></a>Hedeflenecek ilk iş yükleri ve yaklaşım

Geçiş kararınızı almak için tüm önemli bilgilere sahip olduğunuza göre, öncelikle hangi uygulamaların ve iş yüklerinin geçirilmesi gerektiğini önceliklendirmelisiniz. Tam ölçekli bir geçiş üzerinde alanlarında önce, bu stratejide herhangi bir kusurları bir arada bırakmak için, hedeflenen uygulamalarınızı sistematik ve denetlenebilir bir şekilde geçirmeye yönelik bir "uygulama ve öğrenme" yaklaşımı geliştirin. Ayrıca, hangi uygulama gruplarının geçirileceğiyle ilgili öncelik vermek için karmaşıklıklar ve geçiş süresi gibi stratejik faktörleri de kullanabilirsiniz; Örneğin;

Önerilen bazı geçiş stratejileri şunlardır:

- **Hızlı WINS 'Nizi önceliklendirin**: değerlendirme raporlarını, tam olarak kullanıma yönelik sunucular ve veritabanları ve Azure 'a geçiş için en az çaba gerektiren düşük asılı meyve 'yi belirlemek için kullanabilirsiniz:
    - Azure İçin Hazır: değerlendirme raporunuzu dışarı aktarın ve "Azure için hazırlanma" olan tüm makineleri filtreleyin. Bu, Azure geçişi: sunucu geçiş aracı ile kaldırıp kaydırabileceğiniz ilk makine grubunuz olabilir.
    - İşletim sistemi destek bitişi: değerlendirme raporunuzu dışarı aktarın ve Windows Server 2008 ve Windows Server 2008 R2 işletim sistemlerini çalıştıran tüm makineleri filtreleyin. Bu SKU 'Lar destek bitleridir ve yalnızca Azure, Azure 'a geçirdiğinizde ücretsiz 3 yıllık güvenlik güncelleştirmeleri sağlar. Ayrılmış örnekleri birleştirdiğinizde, Azure Hibrit Avantajı ve kullandığınızda, kaydetme çok daha yüksek olabilir.
    - SQL Server Geçişi: Azure geçişi 'ni kullanarak Azure SQL veritabanlarına yönelik veritabanlarını geçirmek için veritabanı değerlendirmesi önerilerini kullanın: veritabanı geçişi ve Azure geçişi: sunucu geçişi kullanarak Azure SQL VM için kullanıma yönelik veritabanları.
    - Yazılım desteği bitişi: uygulama envanterinizi dışarı aktarın ve destek sonuna ulaşmakta olabilecek yazılım/uzantılara filtre uygulayın. Bu uygulamaları önceliklendirmelisiniz.
    - Aşırı sağlanan VM 'Ler: değerlendirme raporunuzu dışarı aktarın ve düşük CPU kullanımı (%) kullanarak makineleri filtreleyin. ve bellek kullanımı (%).  Bu fırsatı kullanarak Azure 'da doğru bir sanal makineye geçiş yapabilir ve az kullanılan kaynaklar için ödeme yaptığınız şeyleri kaydedebilirsiniz.
    - Kapasite kısıtlamaları: yüksek CPU kullanımı (%) ile değerlendirme raporunuzu dışarı aktarın ve makineleri filtreleyin. ve bellek kullanımı (%).  Fazla kullanılabilir VM 'Lerin kesilmesini önler ve Azure 'a taşıyarak performansı artırabilir ve talebi karşılamak için otomatik ölçeklendirme özelliğinden yararlanabilirsiniz. Ayrıca, disk ıOPS ve aktarım hızını çözümleyerek ve gereksinimlerinize en uygun olan önerilen disk türünü bularak depolama kısıtlamalarını anlamak için değerlendirme raporuna göz atabilirsiniz.

- **Küçük bir başlangıç yapın ve ardından büyük**bir risk ve daha az karmaşık olan uygulamaları ve iş yüklerini, geçiş stratejinizde güvenle oluşturmak için kullanmaya başlayın. Ayrıca, pilot geçişlerinizde geliştirme ve test ortamı iş yüklerini bulmak ve geçirmek için kuruluşların CMDB depolarıyla Azure geçişi değerlendirme önerilerinizi de kesişeedebilirsiniz. Üretim iş yüklerini geçirirken, bu pil'lerden gelen dersleri kullanılabilir.  

- **Mevzuata/sektör gereksinimlerinizle uyumlu**: Azure, en büyük uyumluluk portföyünü sektörde ve tekliflerin derinliğine göre sektör genelinde tutar. Azure 'a geçişlerinizi önceliklendirmenize ve ulusal, bölgesel ve sektöre özgü standartlarla ve yasalarınıza uyum sağlamak için bunu bir fırsat olarak kullanın. Bu özellikle, iş açısından kritik veya hassas bilgileri tutan ya da belirli durumlarda yapılan standartlar ve yönetmelikler en fazla değişebilir ve bu durumda, daha fazla değişiklik yapılmasını zorlaştırıyor.  

## <a name="finalize-the-migration-planandprepare-formigration"></a>Geçiş planını sonlandırma ve geçiş için hazırlanma

Geçiş planınızı sonuçlandırmadan önce, bu anahtar geçişi konularının geçiş planınıza hindrance oynamadığından emin olun:

- Beklenmedik gecikmelere neden olabilecek ve geçiş çoğaltma hızını kesintiye uğratan ağ bant genişliği ve gecikme süresi kısıtlamalarını değerlendirin.

- Geçirilen uygulamalar üzerinde performans ve Kullanıcı kabul testi gerçekleştirme ya da veritabanı bağlantı dizelerini ve Web sunucusu yapılandırmalarının güncelleştirilmesi, tam geçişi ve temizleme gibi işlemler gibi geçiş sonrası uygulama tslarını yürütmek için zaman ara.

- Önerilen Azure izinlerini ve geçiş için gereken sunucu/veritabanı erişim rollerinizi ve izin modelinizi gözden geçirin.

- Kuruluşunuzu hazırlayın ve dijital dönüştürmeye iş gücü hizalaması sağlayın. Sağlam bir eğitim altyapısı, başarılı bir kurumsal değişiklik için önemlidir. Azure temelleri, çözüm mimarisi ve güvenlik kursları dahil olmak üzere [Microsoft Learn](/learn/azure/?ocid=CM_Discovery_Checklist_PDF)sunulan ücretsiz eğitime göz atın. Takımınızı [Azure sertifikası](https://www.microsoft.com/learning/certification-overview.aspx?ocid=CM_Discovery_Checklist_PDF)'nı keşfetmeye teşvik edin   .  

- Gerekirse uygulamanız için destek alın. Birçok kuruluş, bulut geçişini desteklemeye yönelik yardım dışarıdan tercih eder. Kişiselleştirilmiş yardım ile hızlı ve sorunsuz bir şekilde Azure 'a geçmek için bir [Azure Expert yönetilen hizmet sağlayıcısı](https://www.microsoft.com/solution-providers/search?cacheId=9c2fed4f-f9e2-42fb-8966-4c565f08f11e&ocid=CM_Discovery_Checklist_PDF)   veya [FastTrack for Azure](https://azure.microsoft.com/programs/azure-fasttrack/?ocid=CM_Discovery_Checklist_PDF)değerlendirin.  

Geçirmek istediğiniz uygulama listesi/grupları, uygulamanız ve veritabanı kullanılabilirliği ve kapalı kalma kısıtlamaları ve istediğiniz geçiş kilometre taşları hakkında ayrıntılı bilgi içeren etkili bir bulut geçiş planı oluşturun. Geçiş planınız Ayrıca veri kopyasının ne kadar süreyle alınacağını ve geçiş sonrası testi ve tam geçişi etkinlikleri için bir considerate arabelleği dahil etmeyi de dikkate almalıdır. Geçiş sonrası testi, geçirilen uygulamaların beklendiği gibi çalıştığından ve tüm veritabanı nesnelerinin ve veri ilişkilerinin buluta başarıyla aktarılmasını sağlamak için işlevsel, tümleştirme, güvenlik ve performans testi kullanım örneklerini içermelidir. Geçiş sonrası testi, geçirilen uygulamaların beklendiği gibi çalıştığından ve tüm veritabanı nesnelerinin ve veri ilişkilerinin buluta başarıyla aktarılmasını sağlamak için işlevsel, tümleştirme, güvenlik ve performans testi kullanım örneklerini içermelidir.  

Bu analizi kullanarak bir geçiş yol haritası oluşturun ve uygulamalarınızı ve veritabanlarınızı minimum ve sıfır kapalı kalma süresine geçirmek için bir bakım penceresi bildirin ve geçiş sırasında olası işletimsel/iş etkisini sınırlayın.  

Azure 'a yönelik tam ölçekli geçişleri kaldırmadan önce Azure geçişi 'nin *test geçiş* özelliğini kullanarak her zaman test etmenizi ve devam etmenizi öneririz. Bu gerçek veriler, ilgili gerçek süreyi tahmin etmenize ve geçiş planınız için gerekli olan tnak 'leri yapmanıza yardımcı olur. Test geçişi Ayrıca, geçiş planıyla ilgili olası sorunları bulmaya ve gerçek geçiş gerçekleşmeden önce bunları gidermeye yönelik bir fırsat sağlar.  

Geçişe hazırsanız, uçtan uca izleme ile sorunsuz ve tümleşik bir geçiş deneyimi için Azure geçişi 'nin *sunucu geçiş aracı* 'Nı ve Azure geçişi 'Nin *veri geçiş hizmeti* 'ni kullanın. Sunucu geçiş aracı, şirket içi şirket içinde barındırılan VM 'Ler ve sunucuların ve AWS, GCP gibi diğer özel veya genel bulutlarının sıfır kapalı kalma süresiyle birlikte geçirilmesini destekler. Azure veritabanı geçiş hizmeti, birden çok veritabanı kaynağından Azure veri platformları arasında kesintisiz geçiş sağlamak için tasarlanan, tam olarak yönetilen bir hizmettir.  

> [!NOTE]
> VMware VM 'Leri için sunucu değerlendirmesi, Konuk IŞLETIM sistemi analizini işlemek üzere vCenter Server ' de VM için belirtilen işletim sistemini kullanır. VMware üzerinde çalışan Linux VM 'Leri için şu anda Konuk işletim sisteminin tam çekirdek sürümünü tanımlamaz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure bulut benimseme çerçevesindeki [bulut geçiş yolculuğunu](/azure/architecture/cloud-adoption/getting-started/migrate)araştırın   .
- Azure geçişi ile [çalışmaya](https://youtu.be/wFfq3YPxYHE) başlayın.
- [VMware VM](tutorial-assess-vmware.md) 'Leri veya [Hyper-V VM 'leri](tutorial-assess-hyper-v.md)için bir değerlendirme oluşturun.
