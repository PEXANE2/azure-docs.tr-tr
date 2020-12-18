---
title: Beklenmeyen Azure ücretlerini analiz etme
description: Azure aboneliğiniz için beklenmeyen ücretleri analiz etmeyi öğrenin.
author: bandersmsft
ms.reviewer: adwise
ms.service: cost-management-billing
ms.subservice: cost-management
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: banders
ms.custom: contperf-fy21q1
ms.openlocfilehash: f6df8c6ff0db74798b71d57feae91e5245f2a6c2
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97028727"
---
# <a name="analyze-unexpected-charges"></a>Beklenmeyen ücretleri analiz etme

Kuruluşunuz için oluşturduğunuz bulut kaynak altyapısı büyük olasılıkla karmaşıktır. Azure kaynak türlerinin pek çoğu farklı ücret türlerine sahip olabilir. Azure kaynakları, kuruluşunuzdaki farklı ekiplere ait olabilir ve çeşitli kaynaklara uygulanan farklı faturalama modeli türlerine sahip olabilir. Ücretleri daha iyi anlamak için, aşağıdaki bölümlerde yer alan bir veya daha çok stratejiden yararlanarak analizinizi başlatın.

## <a name="review-invoice-for-resource-responsible-for-charge"></a>Ücretten sorumlu olan kaynak için faturayı gözden geçirme

Azure hizmetlerini satın alma yönteminiz, ücretle ilişkili kaynağı belirledikçe kullanabileceğiniz metodoloji ve araçlara da karar vermenize yardımcı olur. Sizin için geçerli olan metodolojiye karar vermek için öncelikle [Azure teklif türünüze karar verin](../costs/understand-cost-mgt-data.md#determine-your-offer-type). Daha sonra, [desteklenen Azure teklifleri](../costs/understand-cost-mgt-data.md#supported-microsoft-azure-offers) listesinden müşteri kategorinizi belirleyebilirsiniz.

Aşağıdaki makalelerde, müşteri türüne göre faturanızı nasıl gözden geçireceğinizi açıklayan ayrıntılı adımlar sağlanmaktadır. Her bir makalede, verilen faturalama dönemine ait kullanım ve maliyet ayrıntılarını içeren bir CSV dosyasını indirmeye ilişkin yönergeler yer alır.

- [Kullandıkça Öde fatura gözden geçirme süreci](review-individual-bill.md#charges)
- [Kurumsal Sözleşme fatura gözden geçirme süreci](review-enterprise-agreement-bill.md)
- [Microsoft Müşteri Sözleşmesini gözden geçirme süreci](review-customer-agreement-bill.md#analyze-your-azure-usage-charges)
- [Microsoft İş Ortağı Sözleşmesini gözden geçirme süreci](review-partner-agreement-bill.md#analyze-your-azure-usage-charges)

Azure faturanızda, o aya ait ücretlerinizi _ölçüm_ başına değerlendirilerek toplanır. Ölçümler, bir kaynağın zaman içinde kullanımını izlemek ve faturanızı hesaplamak için kullanılır. Örneğin, sanal makine gibi tek bir Azure kaynağı oluşturduğunuzda bu kaynağa ait bir veya birden fazla ölçüm örneği oluşturulur.

CSV biçimindeki kullanım dosyasını, analiz etmek istediğiniz fatura üzerinde gösterilen _MeterName_’e göre filtreleyip ilgili ölçüme uygulanan tüm satır öğelerini görün. Satır öğesine ait _InstanceID_, ücrete neden olan Azure kaynağına karşılık gelir.

Söz konusu kaynağı tanımladıktan sonra, kaynakla ilgili maliyetleri daha derin analiz etmek için Azure Maliyet Yönetimi’nde maliyet analizini kullanabilirsiniz. Maliyet analizini kullanma hakkında daha fazla bilgi için bkz. [Maliyetleri analiz etmeye başlama](../costs/quick-acm-cost-analysis.md).

## <a name="review-invoiced-charges-in-cost-analysis"></a>Maliyet analizinde faturalanmış ücretleri gözden geçirme

Azure portalında fatura ayrıntılarınızı görüntülemek için, analiz ettiğiniz faturayla ilişkilendirilmiş kapsamın Maliyet analizi bölümüne gidin. **Fatura ayrıntıları** görünümünü seçin. Fatura ayrıntıları, faturada göründüğü haliyle ücretleri gösterir.

[![Fatura ayrıntılarını gösteren örnek](./media/analyze-unexpected-charges/invoice-details.png)](./media/analyze-unexpected-charges/invoice-details.png#lightbox)

Fatura ayrıntılarını görüntüleyerek beklenmeyen maliyetler getiren hizmeti belirleyebilir ve hangi kaynakların Maliyet analizindeki kaynakla doğrudan ilişkili olduğunu saptayabilirsiniz. Örneğin Sanal Makineler hizmetinin ücretlerini analiz etmek istiyorsanız, **Birikmiş maliyet** görünümüne gidin. Ardından ayrıntı düzeyini **Günlük** olarak ayarlayın, sonra **Hizmet adı: Sanal makineler** için ücretleri filtreleyin ve ücretleri **Kaynak**’a göre gruplandırın.

[![Sanal makinelerin birikmiş maliyetlerini gösteren örnek](./media/analyze-unexpected-charges/virtual-machines.png)](./media/analyze-unexpected-charges/virtual-machines.png#lightbox)

## <a name="identify-spikes-in-cost-over-time"></a>Zaman içindeki maliyette oluşan ani artışları belirleme

Bazen faturalanan ücretlerinizde değişikliğe neden olan son maliyetleri bilemeyebilirsiniz. Değişiklikleri anlamak için, [zaman içindeki maliyetlerin günlük veya aylık olarak dökümünü görmek](../costs/cost-analysis-common-uses.md#view-costs-per-day-or-by-month) amacıyla maliyet analizini kullanabilirsiniz. Görünümü oluşturduktan sonra, değişiklikleri tanımlamak için ücretlerinizi **Hizmet** veya **Kaynak** seçeneklerinden biriyle gruplandırın. Dilerseniz verileri daha iyi görselleştirmek için görünümünüzü **Çizgi** grafiği olarak değiştirebilirsiniz.

![Maliyet analizinde zaman içindeki maliyeti gösteren örnek](./media/analyze-unexpected-charges/costs-over-time.png)

## <a name="determine-resource-pricing-and-billing-model"></a>Kaynak fiyatlandırma ve faturalama modelini belirleme

Tek bir kaynak, birden çok Azure ürünü ve hizmeti arasındaki ücretleri tahakkuk edebilir. Her bir Azure hizmetine ait fiyatlandırma hakkında daha fazla bilgi edinmek için [ürüne göre Azure fiyatlandırması](https://azure.microsoft.com/pricing/#product-pricing) sayfasına göz atın. Örneğin, Azure’da oluşturulan tek bir sanal makinenin (VM) kullanımını izlemek için aşağıdaki ölçümler oluşturulmuş olabilir. Her birinin fiyatlandırması farklı olabilir.

- İşlem Saatleri
- IP Adresi Saatleri
- Gelen Veri Aktarımı
- Giden Veri Aktarımı
- Standart Yönetilen Disk
- Standart Yönetilen Disk İşlemleri
- Standart GÇ-Disk
- Standart GÇ-Blok Blobu Okuma
- Standart GÇ-Blok Blobu Yazma
- Standart GÇ-Blok Blobu Silme

VM oluşturulduğunda her ölçüm kullanım kayıtları üretmeye başlar. Kullanım ve ölçümün fiyatı Azure ölçüm sisteminde izlenir. CSV biçimindeki kullanım dosyanızda faturanızın hesaplanması için kullanılan ölçümleri görebilirsiniz.

## <a name="find-people-responsible-for-the-resource-and-engage"></a>Kaynaktan sorumlu kişileri bulma ve iletişim kurma

Belirli bir kaynaktan sorumlu ekip sıklıkla bu kaynağa yapılan değişiklikler hakkında bilgi sahibidir. Ücretlerin neden oluşabileceğini belirlediğinizden, bu kişilerin katılımını sağlamak yararlıdır. Örneğin, kaynağa sahip olan ekip kaynağı yeni oluşturmuş, SKU’sunu güncelleştirmiş (doğal olarak kaynak ücretini de değiştirmiş) veya kod değişikliklerinden dolayı kaynak üzerindeki yükü artırmış olabilir. Kaynağa sahip olan kişileri belirlemeye yönelik diğer teknikler için aşağıdaki bölümleri okumaya devam edin.

### <a name="analyze-the-audit-logs-for-the-resource"></a>Kaynağa ait denetim günlüklerini analiz etme

Bir kaynağı görüntüleme iznine sahipseniz kaynağın denetim günlüklerine de erişebilmeniz gerekir. Kaynakta yapılan son değişikliklerden sorumlu olan kullanıcıyı bulmak için günlükleri inceleyin. Daha fazla bilgi edinmek için bkz. [Azure Etkinlik günlüğü olaylarını görüntüleme ve alma](../../azure-monitor/platform/activity-log.md#view-the-activity-log).

### <a name="analyze-user-permissions-to-the-resources-parent-scope"></a>Kaynağın üst kapsamına yönelik kullanıcı izinlerini analiz etme

Bir aboneliğe veya kaynak grubuna yazma erişimi olan kişiler, genellikle oluşturulan kaynaklar hakkında bilgi sahibidir. Bu kişiler, bir kaynağın amacını açıklayabilir veya bilen bir kişiye sizi yönlendirebilir. Bir Abonelik kapsamına yönelik izinlere sahip kişileri belirlemek için bkz. [Bir kullanıcının Azure kaynaklarına erişimini kontrol etme](../../role-based-access-control/check-access.md). Kaynak grupları için benzer bir süreçten yararlanabilirsiniz.

## <a name="get-help-to-identify-charges"></a>Ücretleri belirlemek için yardım alma

Önceki stratejileri kullandıysanız ve hala neden ücret aldığınızı anlamadıysanız veya faturalama sorunlarıyla ilgili başka konularda yardıma ihtiyacınız varsa lütfen [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Maliyet Yönetimi ile bulut yatırımınızı iyileştirme](../costs/cost-mgt-best-practices.md) hakkında bilgi edinin.