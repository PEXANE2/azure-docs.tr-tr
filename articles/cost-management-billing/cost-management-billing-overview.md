---
title: Azure maliyet yönetimine ve faturalandırmaya genel bakış | Microsoft Docs
description: Faturalama yönetim görevlerini gerçekleştirmek ve maliyetlere faturalandırma erişimini yönetmek için Azure maliyet yönetimi ve faturalandırma özelliklerini kullanırsınız. Ayrıca, Azure harcamayı izleyip denetleyen ve Azure Kaynak kullanımını iyileştirmek için özelliği de kullanabilirsiniz.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/20/2019
ms.topic: overview
ms.service: cost-management-billing
manager: dougeby
ms.custom: ''
ms.openlocfilehash: cadff1d83a8b47a540efe9b74ffaf6de171138b3
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75987519"
---
# <a name="what-is-azure-cost-management-and-billing"></a>Azure maliyet yönetimi ve faturalandırma nedir?

Azure ürün ve hizmetleriyle, yalnızca kullandığınız kadar ödersiniz. Azure kaynaklarını oluştururken ve kullanırken, kaynaklar için ücretlendirilirsiniz. Faturalama yönetim görevlerini gerçekleştirmek ve maliyetlere faturalandırma erişimini yönetmek için Azure maliyet yönetimi ve faturalandırma özelliklerini kullanırsınız. Ayrıca, Azure harcamayı izleyip denetleyen ve Azure Kaynak kullanımını iyileştirmek için özellikleri de kullanabilirsiniz.

## <a name="understand-azure-billing"></a>Azure Faturalandırmayı anlama

Azure Faturalandırma özellikleri, faturalanmış maliyetlerinizi gözden geçirmek ve faturalandırma bilgilerine erişimi yönetmek için kullanılır. Daha büyük kuruluşlarda, tedarik ve finans ekipleri genellikle faturalandırma görevlerini işler.

Azure’ı kullanmak için kaydolduğunuzda bir ödeme hesabı oluşturulur. Faturalarınızı, ödemelerinizi yönetmek ve maliyetleri izlemek için ödeme hesabınızı kullanırsınız. Birden çok ödeme hesabına erişiminiz olabilir. Örneğin, kişisel projeleriniz için Azure’a kaydolmuş olabilirsiniz. Bu nedenle, bir faturalandırma hesabıyla tek bir Azure aboneliğiniz olabilir. Ayrıca kuruluşunuzun Kurumsal Anlaşması veya Microsoft Müşteri Sözleşmesi aracılığıyla da erişiminiz olabilir. Her senaryo için ayrı bir faturalandırma hesabınız olur.

### <a name="billing-accounts"></a>Faturalandırma hesapları

Azure portal Şu anda aşağıdaki faturalandırma hesabı türlerini desteklemektedir:

- **Microsoft Online Services programı**: Azure için Azure Web sitesi üzerinden kaydolduğunuzda, bir Microsoft Online Services programı için bireysel faturalandırma hesabı oluşturulur. Örneğin, bir [Ücretsiz Azure Hesabına](https://azure.microsoft.com/offers/ms-azr-0044p/), [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Visual Studio abonesi](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) olarak kaydolduğunuzda.

- **Kurumsal Anlaşma**: Kuruluşunuz Azure kullanmak üzere bir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) kaydolduğunda bir kurumsal anlaşma faturalandırma hesabı oluşturulur.

- **Microsoft Müşteri Sözleşmesi**: Microsoft Müşteri Sözleşmesi için bir Microsoft temsilcisiyle birlikte çalıştırıldığında, Microsoft Müşteri Sözleşmesi için bir faturalandırma hesabı oluşturulur. Azure web sitesinden [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) kaydolan veya [Ücretsiz Azure Hesabını](https://azure.microsoft.com/offers/ms-azr-0044p/) yükselten, belirli bölgelerdeki bazı müşterilerin Microsoft Müşteri Sözleşmesi için ödeme hesabı da olabilir. Daha fazla bilgi için bkz. [Microsoft Müşteri Sözleşmesi için ödeme hesabınızı kullanmaya başlama](./understand/mca-overview.md).

### <a name="scopes-for-billing-accounts"></a>Ödeme hesapları için kapsamlar
Kapsam, faturalandırmayı görüntülemek ve yönetmek için kullandığınız faturalandırma hesabındaki bir düğümdür. Bu, faturalandırma verilerini, ödemeleri, faturaları ve genel hesap yönetimini yönettiğinizde yer alabilir.

#### <a name="microsoft-online-services-program"></a>Microsoft Online Services Programı

|Kapsam  |Tanım  |
|---------|---------|
|Fatura hesabı     | Bir veya daha fazla Azure aboneliği için tek bir sahibi (Hesap yöneticisi) temsil eder. Hesap Yöneticisi, abonelik oluşturma, faturaları görüntüleme veya aboneliklerin faturalamasını değiştirme gibi çeşitli faturalama görevlerini gerçekleştirme yetkisine sahiptir.  |
|Abonelik     |  Bir Azure kaynakları gruplamasını temsil eder. Abonelik kapsamında bir fatura oluşturulur. Faturasını ödemek için kullanılan kendi ödeme yöntemlerine sahiptir.|


#### <a name="enterprise-agreement"></a>Kurumsal Sözleşme

|Kapsam  |Tanım  |
|---------|---------|
|Fatura hesabı    | Bir Kurumsal Anlaşma kaydını temsil eder. Fatura, faturalandırma hesabı kapsamında oluşturulur. Departmanlar ve kayıt hesapları kullanılarak yapılandırılır.  |
|Bölüm     |  İsteğe bağlı kayıt hesapları gruplaması.      |
|Kayıt hesabı     |  Tek bir hesap sahibini temsil eder. Azure abonelikleri, kayıt hesabı kapsamı altında oluşturulur.  |


#### <a name="microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi

|Kapsam  |Görevler  |
|---------|---------|
|Fatura hesabı     |   Birden çok Microsoft ürünü ve hizmeti için bir müşteri sözleşmesini temsil eder. Faturalandırma hesabı, faturalandırma profilleri ve fatura bölümleri kullanılarak yapılandırılır.   |
|Faturalama profili     |  Bir faturayı ve ödeme yöntemlerini temsil eder. Fatura bu kapsamda oluşturulur. Faturalandırma profilinde birden çok fatura bölümü olabilir.      |
|Fatura bölümü     |   Faturadaki bir maliyet grubunu temsil eder. Abonelikler ve diğer satın almalar, fatura bölümü kapsamıyla ilişkilendirilir.    |


## <a name="understand-azure-cost-management"></a>Azure maliyet yönetimi 'ni anlama
Maliyet yönetimi, işlerinizle ilişkili maliyetleri etkili bir şekilde planlama ve denetleme işlemidir. Maliyet yönetimi görevleri normalde finans, yönetim ve uygulama takımları tarafından gerçekleştirilir. Azure maliyet yönetimi + faturalandırma, kuruluşların maliyeti göz önünde bulundurmasına yardımcı olur. Ayrıca maliyetleri etkin bir şekilde analiz etmenize ve bulut harcamalarını iyileştirmek için işlem yapmanıza yardımcı olur. Kuruluş olarak maliyet yönetimine nasıl yaklaşmak gerektiği hakkında daha fazla bilgi edinmek için, [Azure Maliyet Yönetimi en iyi yöntemleri](./costs/cost-mgt-best-practices.md) makalesini gözden geçirin.

Azure maliyet yönetimi 'nin Azure 'da para tasarrufu sağlamanıza nasıl yardımcı olduğu hakkında hızlı bir genel bakış için [Azure maliyet yönetimine genel bakış videosunu](https://www.youtube.com/watch?v=el4yN5cHsJ0) izleyin.

>[!VIDEO https://www.youtube.com/embed/el4yN5cHsJ0]

Faturalandırma, maliyet yönetimiyle ilgili olsa da aynı şey değildir. Faturalandırma, müşterilere mal veya hizmetler karşılığı fatura hazırlama ve ticari ilişkileri yönetme işlemidir.

Maliyet yönetimi, gelişmiş analizle kurumsal maliyet ve kullanım düzenlerini gösterir. Maliyet yönetimi raporlarında, Azure hizmetleri ve üçüncü taraf Market teklifleri tarafından tüketilen kullanım tabanlı maliyetler gösterilmektedir. Ücretler, anlaşmalı fiyatlara ve indirimle Azure Hibrit Avantajı indirimlere göre hesaplanır. Raporlar hep birlikte, kullanımla ilişkili iç ve dış maliyetlerinizi ve Azure Market ücretlerinizi ortaya koyar. Rezervasyon satın almaları, destek ve vergiler gibi diğer ücretler henüz raporlarda gösterilmemektedir. Raporlar, harcamalarınızı ve kaynak kullanımınızı anlamanıza, ayrıca harcama anomalilerini bulmanıza yardımcı olur. Tahmine dayalı analiz de sağlanır. Maliyet Yönetimi harcamalarınızın nasıl düzenlendiğini ve maliyetleri nasıl düşürebileceğinizi net bir şekilde göstermek için Azure yönetim gruplarını, bütçelerini ve önerilerini kullanır.

Azure portalı veya çeşitli API'leri kullanarak dışarı aktarma otomasyonu yapabilir ve bu yolla maliyet verilerini dış sistemler ve süreçlerle tümleştirebilirsiniz. Otomatik faturalandırma verilerini dışarı aktarma özelliği ve zamanlanmış raporlar da sağlanır.

### <a name="plan-and-control-expenses"></a>Giderleri planlama ve denetleme

Maliyet yönetimi 'nin maliyetlerinizi planlayıp denetlemenize yardımcı olma yolları şunlardır: maliyet analizi, bütçeler, öneriler ve maliyet yönetimi verilerini dışa aktarma.

Kurumsal maliyetlerinizi incelemek ve analiz etmek için maliyet analizini kullanırsınız. Maliyetlerin nerede tahakkuk ettiğini anlamak ve harcama eğilimlerini belirlemek için kuruluşa göre toplu maliyetleri görüntüleyebilirsiniz. Bütçeye göre aylık, üç aylık, hatta yıllık maliyet eğilimlerini tahmin etmek için, zaman içinde tahakkuk eden maliyetleri de görebilirsiniz.

Bütçeler kuruluşunuzda finansal sorumluluğu planlamanıza ve buna uymanıza yardımcı olur. Maliyet eşiklerinin veya sınırlarının aşılmasını önlemeye yardım ederler. Bütçeler, maliyetleri proaktif olarak yönetmek için diğer kişileri harcamalarıyla ilgili bilgilendirmenize de katkıda bulunabilir. Bunlardan yararlanarak, zaman içinde harcamaların nasıl bir ilerleme gösterdiğini görebilirsiniz.

Öneriler, boşta olan ve az kullanılan kaynakları belirleyerek verimliliği nasıl iyileştirebileceğinizi ve geliştirebileceğinizi gösterir. Öte yandan daha ucuz kaynak seçeneklerini de gösterebilirler. Önerilere uyarak önlem aldığınızda, kaynakları kullanım yönteminizi değiştirerek tasarruf edebilirsiniz. Önlem almak için, önce maliyet iyileştirme önerilerini görüntüleyip olası kullanım verimsizliklerini görürsünüz. Ardından, öneriye göre harekete geçip Azure kaynak kullanımınızı daha uygun maliyetli bir seçeneği kullanacak şekilde değiştirirsiniz. Sonra da yaptığınız değişikliğin başarısından emin olmak için işleminizi doğrularsınız.

Maliyet yönetimi verilerine erişmek veya bu verileri incelemek için dış sistemleri kullanıyorsanız, verileri Azure'dan kolayca dışarı aktarabilirsiniz. Ayrıca CSV biçiminde günlük zamanlanmış dışarı aktarmalar ayarlayabilir ve veri dosyalarını Azure depolama alanında depolayabilirsiniz. Sonra bu verileri dış sisteminizden erişebilirsiniz.

### <a name="consider-cloudyn"></a>Cloudyn'i göz önünde bulundurun

[Cloudyn](./cloudyn/overview.md), Maliyet Yönetimi ile ilgili bir Azure hizmetidir. Cloudyn ile, Azure kaynaklarınız için bulut kullanımını ve harcamalarını izleyebilirsiniz. Ayrıca AWS ve Google gibi diğer bulut sağlayıcılarını da destekler. Anlaşılması kolay pano raporları, maliyet ayırma ve ücret hesaplama/yansıtma konusunda yardımcı olur. Şu anda, Maliyet Yönetimi'nin geri gösterme/geri ödeme ve diğer bulut hizmeti sağlayıcıları için desteği yoktur. Öte yandan, Cloudyn bunları _destekleyen_ bir seçenektir. Şu anda, maliyet yönetimi Microsoft Bulut hizmet sağlayıcısı (CSP) hesaplarını desteklemez, ancak Cloudyn. CSP hesaplarınız varsa veya showback/chargeback kullanmak istiyorsanız, maliyetlerinizi yönetmeye yardımcı olması için Cloudyn kullanabilirsiniz.

Azure maliyet yönetimi [ve Cloudyn videosunu](https://www.youtube.com/watch?v=PmwFWwSluh8) , iş gereksinimlerinize göre Azure Cost Management veya Cloudyn ' i kullanmanız gereken önerileri görmek için izleyin.

>[!VIDEO https://www.youtube.com/embed/PmwFWwSluh8]

### <a name="additional-azure-tools"></a>Ek Azure araçları

Azure, Azure maliyet yönetimi ve faturalandırma özelliği kümesinin bir parçası olmayan diğer araçlara sahiptir. Ancak, maliyet yönetimi sürecinde önemli bir rol oynar. Söz konusu araçlar hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara bakın.

- [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) - Önceden bulut maliyetlerinizi tahmin etmek için bu aracı kullanın.
- [Azure Geçişi](../migrate/migrate-overview.md) - Azure yedek çözümünden neler gerektiği hakkında içgörüler elde etmek için geçerli veri merkezi yükünüzü değerlendirin.
- [Azure Danışmanı](../advisor/advisor-overview.md) - Kullanılmayan sanal makineleri belirleyin ve Azure ayrılmış örnek satın almalarıyla ilgili öneriler alın.
- [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) - Tasarruf etmek için geçerli şirket içi Windows Server veya SQL Server lisanslarınızı Azure'da sanal makineler için kullanın.


## <a name="next-steps"></a>Sonraki adımlar

Artık maliyet yönetimi ve faturalandırma hakkında bilgi sahibi olduğunuza göre, bir sonraki adım hizmeti kullanmaya başlamadır.

- Azure Maliyet Yönetimi'ni kullanarak [maliyetleri analiz etmeye](./costs/quick-acm-cost-analysis.md) başlayın.
- Ayrıca, [Azure Maliyet Yönetimi en iyi yöntemlerini](./costs/cost-mgt-best-practices.md) de okuyabilirsiniz.
