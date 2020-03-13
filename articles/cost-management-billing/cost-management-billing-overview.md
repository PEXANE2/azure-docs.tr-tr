---
title: Azure Maliyet Yönetimi ve Faturalama'ya Genel Bakış | Microsoft Docs
description: Azure Maliyet Yönetimi ve Faturalandırma özelliklerini kullanarak faturalarla ilgili yönetim görevlerini gerçekleştirebilir ve maliyetler için faturalandırma erişimini yönetebilirsiniz. Ayrıca bu özelliği kullanarak Azure harcamalarını izleyip denetleyebilir ve Azure kaynaklarının kullanımını iyileştirebilirsiniz.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/24/2020
ms.topic: overview
ms.service: cost-management-billing
ms.custom: ''
ms.openlocfilehash: c2d039dbf2ac7cfaeaad7ad038eb669337243cce
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78970115"
---
# <a name="what-is-azure-cost-management-and-billing"></a>Azure Maliyet Yönetimi ve Faturalandırma nedir?

Azure ürün ve hizmetlerinde sadece kullandığınız kadar ödersiniz. Yalnızca oluşturup kullandığınız Azure kaynakları için ücretlendirilirsiniz. Azure Maliyet Yönetimi ve Faturalandırma özelliklerini kullanarak faturalarla ilgili yönetim görevlerini gerçekleştirebilir ve maliyetler için faturalandırma erişimini yönetebilirsiniz. Ayrıca bu özellikleri kullanarak Azure harcamalarını izleyip denetleyebilir ve Azure kaynaklarının kullanımını iyileştirebilirsiniz.

## <a name="understand-azure-billing"></a>Azure Faturalandırma özelliklerini anlama

Azure Faturalandırma özellikleri, faturalandırılan maliyetlerinizi gözden geçirmenizi ve ödeme bilgilerinize kimlerin erişebileceğini yönetmenizi sağlar. Büyük çaplı kuruluşlarda faturalandırma görevleri genellikle tedarik ve finans takımları tarafından yürütülür.

Azure’ı kullanmak için kaydolduğunuzda bir ödeme hesabı oluşturulur. Faturalarınızı, ödemelerinizi yönetmek ve maliyetleri izlemek için ödeme hesabınızı kullanırsınız. Birden çok ödeme hesabına erişiminiz olabilir. Örneğin, kişisel projeleriniz için Azure’a kaydolmuş olabilirsiniz. Bu nedenle faturalandırma hesabına sahip ayrı bir Azure aboneliğiniz mevcut olabilir. Ayrıca kuruluşunuzun Kurumsal Anlaşması veya Microsoft Müşteri Sözleşmesi aracılığıyla da erişiminiz olabilir. Bu senaryoların her biri için ayrı bir ödeme hesabınız olur.

### <a name="billing-accounts"></a>Ödeme hesapları

Azure portalı şu anda aşağıdaki ödeme hesapları türünü destekler:

- **Microsoft Online Services Programı**: Azure web sitesi üzerinden Azure’a kaydolduğunuzda, Microsoft Online Services Programı için ayrı bir ödeme hesabı oluşturulur. Örneğin, bir [Ücretsiz Azure Hesabına](https://azure.microsoft.com/offers/ms-azr-0044p/), [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Visual Studio abonesi](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) olarak kaydolduğunuzda.

- **Kurumsal Anlaşma**: Kuruluşunuz Azure’ı kullanmak için bir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) imzaladığında Kurumsal Anlaşma için bir ödeme hesabı oluşturulur.

- **Microsoft Müşteri Sözleşmesi**: Kuruluşunuz bir Microsoft Müşteri Sözleşmesi imzalamak için Microsoft temsilcisiyle çalıştığında Microsoft Müşteri Sözleşmesi için bir ödeme hesabı oluşturulur. Azure web sitesinden [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) kaydolan veya [Ücretsiz Azure Hesabını](https://azure.microsoft.com/offers/ms-azr-0044p/) yükselten, belirli bölgelerdeki bazı müşterilerin Microsoft Müşteri Sözleşmesi için ödeme hesabı da olabilir. Daha fazla bilgi için bkz. [Microsoft Müşteri Sözleşmesi için ödeme hesabınızı kullanmaya başlama](./understand/mca-overview.md).

### <a name="scopes-for-billing-accounts"></a>Ödeme hesapları için kapsamlar
Kapsam, faturalamayı görüntülemek ve yönetmek için kullandığınız ödeme hesabı içindeki bir düğümdür. Bu kapsam dahilinde faturalama verilerini, ödemeleri, faturaları yönetir ve genel hesap yönetimi gerçekleştirirsiniz.

#### <a name="microsoft-online-services-program"></a>Microsoft Online Services Programı

|Kapsam  |Tanım  |
|---------|---------|
|Fatura hesabı     | Bir veya daha fazla Azure aboneliği için tek bir sahibi (Hesap yöneticisi) temsil eder. Hesap Yöneticisi, abonelik oluşturma, faturaları görüntüleme veya aboneliklerin faturalamasını değiştirme gibi çeşitli faturalama görevlerini gerçekleştirme yetkisine sahiptir.  |
|Abonelik     |  Bir Azure kaynakları gruplamasını temsil eder. Abonelik kapsamında fatura oluşturulur. Faturasını ödemek için kullanılan kendi ödeme yöntemlerine sahiptir.|


#### <a name="enterprise-agreement"></a>Kurumsal Anlaşma

|Kapsam  |Tanım  |
|---------|---------|
|Fatura hesabı    | Bir Kurumsal Anlaşma kaydını temsil eder. Fatura, ödeme hesabı kapsamında oluşturulur. Bölümler ve kayıt hesapları kullanılarak yapılandırılır.  |
|Bölüm     |  İsteğe bağlı kayıt hesapları gruplaması.      |
|Kayıt hesabı     |  Tek bir hesap sahibini temsil eder. Azure abonelikleri kayıt hesabı kapsamı altında oluşturulur.  |


#### <a name="microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi

|Kapsam  |Görevler  |
|---------|---------|
|Fatura hesabı     |   Birden çok Microsoft ürünü ve hizmeti için bir müşteri sözleşmesini temsil eder. Ödeme hesabı, faturalama profilleri ve fatura bölümleri kullanılarak yapılandırılır.   |
|Faturalama profili     |  Bir faturayı ve ödeme yöntemlerini temsil eder. Fatura bu kapsamda oluşturulur. Faturalama profili birden çok fatura bölümüne sahip olabilir.      |
|Fatura bölümü     |   Faturadaki bir maliyet grubunu temsil eder. Abonelikler ve diğer satın alma işlemleri fatura bölümünün kapsamıyla ilişkilidir.    |


## <a name="understand-azure-cost-management"></a>Azure Maliyet Yönetimi’ni anlama
Maliyet yönetimi, işlerinizle ilişkili maliyetleri etkili bir şekilde planlama ve denetleme işlemidir. Maliyet yönetimi görevleri normalde finans, yönetim ve uygulama takımları tarafından gerçekleştirilir. Azure Maliyet Yönetimi + Faturalandırma, kuruluşların maliyet planlaması yapmasını sağlar. Ayrıca, maliyetleri etkili bir şekilde analiz etmeye ve bulut harcamalarını iyileştirmek için gerçekleştirilecek eylemlere de katkı sağlar. Kuruluş olarak maliyet yönetimine nasıl yaklaşmak gerektiği hakkında daha fazla bilgi edinmek için, [Azure Maliyet Yönetimi en iyi yöntemleri](./costs/cost-mgt-best-practices.md) makalesini gözden geçirin.

Azure Maliyet Yönetimi'nin Azure'da tasarruf sağlamanıza nasıl yardımcı olacağı hakkında bilgi edinmek için [Azure Maliyet Yönetimi'ne genel bakış videosunu](https://www.youtube.com/watch?v=el4yN5cHsJ0) izleyin.

>[!VIDEO https://www.youtube.com/embed/el4yN5cHsJ0]

Faturalandırma, maliyet yönetimiyle ilgili olsa da aynı şey değildir. Faturalandırma, müşterilere mal veya hizmetler karşılığı fatura hazırlama ve ticari ilişkileri yönetme işlemidir.

Maliyet yönetimi, gelişmiş analizle kurumsal maliyet ve kullanım düzenlerini gösterir. Maliyet Yönetimi raporları, Azure hizmetleri ve üçüncü taraf Market teklifleri için kullanıma bağlı ücretleri gösterir. Ücretler, anlaşmalı fiyatlara göre hesaplanır ve rezervasyon ile Azure Hibrit Avantajı indirimleri dikkate alınır. Raporlar hep birlikte, kullanımla ilişkili iç ve dış maliyetlerinizi ve Azure Market ücretlerinizi ortaya koyar. Rezervasyon satın almaları, destek ve vergiler gibi diğer ücretler henüz raporlarda gösterilmemektedir. Raporlar, harcamalarınızı ve kaynak kullanımınızı anlamanıza, ayrıca harcama anomalilerini bulmanıza yardımcı olur. Tahmine dayalı analiz de sağlanır. Maliyet Yönetimi harcamalarınızın nasıl düzenlendiğini ve maliyetleri nasıl düşürebileceğinizi net bir şekilde göstermek için Azure yönetim gruplarını, bütçelerini ve önerilerini kullanır.

Azure portalı veya çeşitli API'leri kullanarak dışarı aktarma otomasyonu yapabilir ve bu yolla maliyet verilerini dış sistemler ve süreçlerle tümleştirebilirsiniz. Otomatik faturalandırma verilerini dışarı aktarma özelliği ve zamanlanmış raporlar da sağlanır.

### <a name="plan-and-control-expenses"></a>Giderleri planlama ve denetleme

Maliyet Yönetimi, maliyetlerinizi planlama ve denetleme konusunda şu şekilde yardımcı olabilir: Maliyet analizi, bütçeler, öneriler ve maliyet yönetimi verilerinin dışarı aktarılması.

Kurumsal maliyetlerinizi incelemek ve analiz etmek için maliyet analizini kullanırsınız. Maliyetlerin nerede tahakkuk ettiğini anlamak ve harcama eğilimlerini belirlemek için kuruluşa göre toplu maliyetleri görüntüleyebilirsiniz. Bütçeye göre aylık, üç aylık, hatta yıllık maliyet eğilimlerini tahmin etmek için, zaman içinde tahakkuk eden maliyetleri de görebilirsiniz.

Bütçeler kuruluşunuzda finansal sorumluluğu planlamanıza ve buna uymanıza yardımcı olur. Maliyet eşiklerinin veya sınırlarının aşılmasını önlemeye yardım ederler. Bütçeler, maliyetleri proaktif olarak yönetmek için diğer kişileri harcamalarıyla ilgili bilgilendirmenize de katkıda bulunabilir. Bunlardan yararlanarak, zaman içinde harcamaların nasıl bir ilerleme gösterdiğini görebilirsiniz.

Öneriler, boşta olan ve az kullanılan kaynakları belirleyerek verimliliği nasıl iyileştirebileceğinizi ve geliştirebileceğinizi gösterir. Öte yandan daha ucuz kaynak seçeneklerini de gösterebilirler. Önerilere uyarak önlem aldığınızda, kaynakları kullanım yönteminizi değiştirerek tasarruf edebilirsiniz. Önlem almak için, önce maliyet iyileştirme önerilerini görüntüleyip olası kullanım verimsizliklerini görürsünüz. Ardından, öneriye göre harekete geçip Azure kaynak kullanımınızı daha uygun maliyetli bir seçeneği kullanacak şekilde değiştirirsiniz. Sonra da yaptığınız değişikliğin başarısından emin olmak için işleminizi doğrularsınız.

Maliyet yönetimi verilerine erişmek veya bu verileri incelemek için dış sistemleri kullanıyorsanız, verileri Azure'dan kolayca dışarı aktarabilirsiniz. Ayrıca CSV biçiminde günlük zamanlanmış dışarı aktarmalar ayarlayabilir ve veri dosyalarını Azure depolama alanında depolayabilirsiniz. Sonra bu verileri dış sisteminizden erişebilirsiniz.

### <a name="consider-cloudyn"></a>Cloudyn'i göz önünde bulundurun

[Cloudyn](./cloudyn/overview.md), Maliyet Yönetimi ile ilgili bir Azure hizmetidir. Cloudyn ile, Azure kaynaklarınız için bulut kullanımını ve harcamalarını izleyebilirsiniz. Ayrıca AWS ve Google gibi diğer bulut sağlayıcılarını da destekler. Anlaşılması kolay pano raporları, maliyet ayırma ve ücret hesaplama/yansıtma konusunda yardımcı olur. Şu anda, Maliyet Yönetimi'nin geri gösterme/geri ödeme ve diğer bulut hizmeti sağlayıcıları için desteği yoktur. Öte yandan, Cloudyn bunları _destekleyen_ bir seçenektir. Maliyet Yönetimi şu an için Microsoft Bulut Hizmeti Sağlayıcısı (CSP) hesaplarını desteklememektedir ancak bu destek Cloudyn'de mevcuttur. CSP hesaplarınız varsa veya ücreti yansıtma/geri ödeme özelliklerini kullanmak istiyorsanız maliyet yönetimi için Cloudyn'i kullanabilirsiniz.

İşletmenizin ihtiyaçlarına göre Azure Maliyet Yönetimi veya Cloudyn kullanmanız gereken durumlarla ilgili öneriler için [Azure Maliyet Yönetimi ve Cloudyn videosunu](https://www.youtube.com/watch?v=PmwFWwSluh8) izleyin.

>[!VIDEO https://www.youtube.com/embed/PmwFWwSluh8]

### <a name="additional-azure-tools"></a>Ek Azure araçları

Azure'un, Azure Maliyet Yönetimi ve Faturalandırma özellik kümesi kapsamında yer almayan başka araçları da vardır. Bununla birlikte, bu araçlar maliyet yönetimi işleminde önemli bir rol oynar. Söz konusu araçlar hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara bakın.

- [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) - Önceden bulut maliyetlerinizi tahmin etmek için bu aracı kullanın.
- [Azure Geçişi](../migrate/migrate-overview.md) - Azure yedek çözümünden neler gerektiği hakkında içgörüler elde etmek için geçerli veri merkezi yükünüzü değerlendirin.
- [Azure Danışmanı](../advisor/advisor-overview.md) - Kullanılmayan sanal makineleri belirleyin ve Azure ayrılmış örnek satın almalarıyla ilgili öneriler alın.
- [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) - Tasarruf etmek için geçerli şirket içi Windows Server veya SQL Server lisanslarınızı Azure'da sanal makineler için kullanın.


## <a name="next-steps"></a>Sonraki adımlar

Artık Maliyet Yönetimi ve Faturalandırma ile tanıştığınıza göre, bir sonraki adımda hizmeti kullanmaya başlayacaksınız.

- Azure Maliyet Yönetimi'ni kullanarak [maliyetleri analiz etmeye](./costs/quick-acm-cost-analysis.md) başlayın.
- Ayrıca, [Azure Maliyet Yönetimi en iyi yöntemlerini](./costs/cost-mgt-best-practices.md) de okuyabilirsiniz.
