---
title: Azure Ayırmaları nedir?
description: Sanal makinelerinize, SQL veritabanlarına, Azure Cosmos DB ve diğer kaynak maliyetlerine kaydedilecek Azure ayırmaları ve fiyatlandırma hakkında bilgi edinin.
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/19/2019
ms.author: banders
ms.openlocfilehash: c63447ff0a3329e0cc0dc0605984ae4f26e9c25f
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359221"
---
# <a name="what-are-azure-reservations"></a>Azure Ayırmaları nedir?

Azure ayırmaları, bir yıllık veya üç yıllık sanal makine, SQL veritabanı işlem kapasitesi, Azure Cosmos DB üretilen iş veya diğer Azure kaynakları için ön ödeme yaparak para tasarrufu yapmanıza yardımcı olur. Ön ödeme, kullandığınız kaynaklara ilişkin bir indirim elde etmenizi sağlar. Ayırmalar, Kullandıkça Öde fiyatlarında% 72 ' e varan sanal makinenizi, SQL veritabanı işlem, Azure Cosmos DB veya diğer kaynak maliyetlerini önemli ölçüde azaltabilir. Rezervasyonlar bir faturalandırma indirimi sağlar ve kaynaklarınızın çalışma zamanı durumunu etkilemez.

[Azure Portal](https://ms.portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade)bir rezervasyon satın alabilirsiniz.

## <a name="why-buy-a-reservation"></a>Neden rezervasyon satın alınır?

Uzun süreler boyunca çalışan sanal makinelere, Azure Cosmos DB veya SQL veritabanlarına sahipseniz, bir ayırma satın almak size en uygun maliyetli seçeneği sağlar. Örneğin, bir hizmetin dört örneğini rezervasyon olmadan sürekli olarak çalıştırdığınızda Kullandıkça Öde tarifesine göre ücretlendirilirsiniz. Bu kaynaklar için bir ayırma satın aldığınızda, rezervasyon indirimi hemen alınır. Kaynaklar artık Kullandıkça Öde tarifelerine göre ücretlendirilir.

## <a name="charges-covered-by-reservation"></a>Rezervasyon tarafından kapsanan ücretler

Hizmet planları:

- **Ayrılmış sanal makine örneği** -bir ayırma yalnızca sanal makine işlem maliyetlerini içerir. Ek yazılım, ağ veya depolama ücretleri kapsamaz.
- **Ayrılmış kapasite Azure Cosmos DB** -bir ayırma, kaynaklarınız için sağlanan aktarım hızını içerir. Depolama ve ağ ücretlerini kapsamaz.
- **SQL veritabanı ayrılmış sanal çekirdek** -yalnızca işlem maliyetleri bir ayırmaya dahildir. Lisans ayrı olarak faturalandırılır.

Windows sanal makineler ve SQL veritabanı için [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)ile lisanslama maliyetlerini de kapsayacaktır.

## <a name="whos-eligible-to-purchase-a-reservation"></a>Rezervasyon satın alma hakkına sahip misiniz?

Bir planı satın almak için, bir kuruluşta (MS-AZR-0017P veya MS-AZR-0148P) veya Kullandıkça Öde aboneliğine (MS-AZR-003P veya MS-AZR-0023P) bir abonelik sahibi rolüne sahip olmanız gerekir. Bulut çözümü sağlayıcıları Azure Portal veya [iş ortağı merkezi](/partner-center/azure-reservations) 'ni kullanarak Azure ayırmaları satın alabilir.

Kurumsal Anlaşma (EA) müşterileri, satınalmaları EA yöneticileri ile sınırlayabilir. Bunu, EA portalındaki **ayrılmış örnekleri Ekle** seçeneğini devre dışı bırakarak yapabilirler. EA yöneticileri, bir ayırma satın almak için en az bir EA aboneliğine sahip bir abonelik sahibi olmalıdır. Bu seçenek, merkezi bir ekibin farklı maliyet merkezleri için rezervasyonlar satın almasını isteyen kuruluşlar için yararlıdır. Satın alma işleminden sonra merkezi takımlar, rezervasyonların maliyet merkezi sahiplerini ekleyebilirler. Sahipleri daha sonra, rezervasyonlarını aboneliklerine göre kapsamlarına alabilir. Merkezi ekibin, rezervasyonun satın alındığı yerde abonelik sahibi erişimi olması gerekmez.

Rezervasyon indirimi yalnızca kurumsal, bulut çözümü sağlayıcısı (CSP) ile satın alınan abonelikler ve kullandıkça öde tarifeleri ile bireysel planlar ile ilişkili kaynaklar için geçerlidir.

## <a name="scope-reservations"></a>Kapsam rezervasyonları

Bir rezervasyonu bir abonelik veya kaynak grubu için kapsamını belirleyebilirsiniz. Bir ayırma kapsamının ayarlanması, rezervasyon tasarruflarının nereye uygulanacağını seçer. Ayırmayı bir kaynak grubuna göre kapsamınızda, rezervasyon iskontoları yalnızca kaynak grubu için geçerlidir — aboneliğin tamamı değildir.

### <a name="reservation-scoping-options"></a>Ayırma kapsamı seçenekleri

Kaynak grubu kapsamı ile, gereksinimlerinize bağlı olarak bir ayırmayı kapsama almak için üç seçeneğiniz vardır:

- **Tek kaynak grubu kapsamı** — yalnızca seçili kaynak grubundaki eşleşen kaynaklara rezervasyon iskontosunu uygular.
- **Tek abonelik kapsamı** — seçili abonelikteki eşleşen kaynaklara rezervasyon iskontosunu uygular.
- **Paylaşılan kapsam** — fatura bağlamındaki uygun aboneliklerde bulunan kaynaklara karşılık gelen rezervasyon iskontosunu uygular. Kurumsal Anlaşma müşteriler için, faturalandırma bağlamı kayıt olur. Kullandıkça Öde tarifelerine sahip bireysel abonelikler için faturalandırma kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.

Kullanım için rezervasyon indirimleri uygulanırken, Azure ayırmayı aşağıdaki sırayla işler:

1. Kaynak grubu kapsamındaki ayırmalar
2. Tek kapsam rezervasyonları
3. Paylaşılan kapsam ayırmaları

Tek bir kaynak grubu, rezervasyonların kapsamını nasıl kapsamdığınıza bağlı olarak birden çok ayırmasından rezervasyon iskontoları alabilir.

### <a name="scope-a-reservation-to-a-resource-group"></a>Bir kaynak grubuna ayırmayı kapsam

Ayırmayı satın aldığınızda ya da kapsamı satın aldıktan sonra ayarlarsanız bir kaynak grubuna ayırmayı kapsam içinde oluşturabilirsiniz. Bir kaynak grubuna ayırmayı kapsamı için bir abonelik sahibi olmanız gerekir.

Kapsamı ayarlamak için Azure portal [satın alma ayırma](https://ms.portal.azure.com/#blade/Microsoft\_Azure\_Reservations/CreateBlade/referrer/Browse\_AddCommand) sayfasına gidin. Satın almak istediğiniz rezervasyon türünü seçin. Satın almak istediğiniz **ürünü seçin** seçim formunda, kapsam değerini tek kaynak grubuyla değiştirin. Ardından, bir kaynak grubu seçin.

![VM ayırma satın alma seçimini gösteren örnek](./media/billing-save-compute-costs-reservations/select-product-to-purchase.png)

Sanal makine rezervasyonuna ait kaynak grubu için satın alma önerileri gösterilir. Öneriler, son 30 gün boyunca kullanımınız analiz edilirken hesaplanır. Bir satın alma önerisi, ayrılmış örneklerle çalışan kaynakların maliyeti, Kullandıkça Öde tarifesine sahip kaynakları çalıştırmanın maliyetinden daha fazla olduğunda yapılır. Rezervasyon satın alma önerileri hakkında daha fazla bilgi için bkz. [kullanım düzenine göre ayrılmış örnek satın alma önerileri alma](https://azure.microsoft.com/blog/get-usage-based-reserved-instance-recommendations).

Ayırmayı satın aldıktan sonra her zaman kapsamı güncelleştirebilirsiniz. Bunu yapmak için, rezervasyona gidin, **yapılandırma**' ya tıklayın ve ayırmayı yeniden sağlayın. Ayırmayı yeniden oluşturma işlemi ticari bir işlem değildir. Rezervasyon teriminizi değiştirmedi. Kapsamı güncelleştirme hakkında daha fazla bilgi için bkz. [bir ayırma satın aldıktan sonra kapsamı güncelleştirme](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

![Bir ayırma kapsamı değişikliğini gösteren örnek](./media/billing-save-compute-costs-reservations/rescope-reservation-resource-group.png)

### <a name="monitor-and-optimize-reservation-usage"></a>Ayırma kullanımını izleme ve iyileştirme

Rezervasyon kullanımınızı Azure portal, API 'Ler aracılığıyla veya kullanım verileri aracılığıyla birden çok şekilde izleyebilirsiniz. Erişiminiz olan tüm rezervasyonları görmek için Azure portal **ayırmalar** ' a gidin. Rezervasyonlar Kılavuzu, ayırma için en son kaydedilen kullanım yüzdesini gösterir. Rezervasyonun uzun süreli kullanımını görmek için ayırmaya tıklayın.

Ayrıca, bir kurumsal anlaşma müşterisiyseniz [API 'leri](billing-reservation-apis.md#see-reservation-usage) ve [kullanım verilerinizi](billing-understand-reserved-instance-usage-ea.md#common-cost-and-usage-tasks) kullanarak rezervasyon kullanımı da alabilirsiniz.

Kaynak grubu kapsamlı rezervasyonunuzun kullanımının düşük olduğunu fark ederseniz, ayırma kapsamını tek bir aboneliğe güncelleştirebilir veya faturalandırma bağlamı genelinde paylaşabilirsiniz. Ayrıca ayırmayı bölebilir ve elde edilen rezervasyonları farklı kaynak gruplarına uygulayabilirsiniz.

### <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Kaynak grubunda eşleşen kaynaklarınız yoksa, ayırma az kullanılır. Ayırma, düşük kullanımın kullanıldığı farklı bir kaynak grubuna veya aboneliğe otomatik olarak uygulanmaz.

Kaynak grubunu bir abonelikten diğerine taşırsanız bir ayırma kapsamı otomatik olarak güncelleştirmez. Kaynak grubunu silerseniz kapsam günceldeğildir. [Ayırmayı](billing-manage-reserved-vm-instance.md#change-the-reservation-scope)yeniden oluşturmanız gerekecektir. Aksi takdirde, ayırma az kullanılır.

## <a name="discounted-subscription-and-offer-types"></a>İndirimli abonelik ve teklif türleri

Rezervasyon iskontoları, aşağıdaki uygun abonelikler ve teklif türleri için geçerlidir.

- Kurumsal Anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P)
- Kullandıkça Öde tarifeleri olan bireysel planlar (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P)
- CSP abonelikleri

Başka teklif türlerine sahip bir abonelikte çalışan kaynaklar rezervasyon iskontosunu almaz.

## <a name="how-is-a-reservation-billed"></a>Rezervasyon nasıl faturalandırılır?

Rezervasyon, aboneliğe bağlı ödeme yöntemine göre ücretlendirilir. Kurumsal aboneliğiniz varsa, rezervasyon maliyeti parasal taahhüt bakiyesinden düşülür. Parasal taahhüt bakiyeniz rezervasyon maliyetini kapsamadığı zaman fazla kullanım üzerinden faturalandırılırsınız. Kullandıkça Öde tarifesine sahip tek bir plandan aboneliğiniz varsa, hesabınızda bulunan kredi kartı hemen faturalandırılır. Fatura ile faturalandırdığınızda, ücretleri bir sonraki faturanızda görürsünüz.

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon iskontosunun uygulanma şekli

Rezervasyon indirimi, ayırmayı satın alırken seçtiğiniz özniteliklerle eşleşen kaynak kullanımı için geçerlidir. Öznitelikler, eşleşen VM 'Lerin, SQL veritabanlarının, Azure Cosmos DB veya diğer kaynakların çalıştırıldığı kapsamı içerir. Örneğin, Batı ABD bölgesinde dört standart D2 sanal makinesi için bir rezervasyon indirimi istiyorsanız, VM 'Lerin çalıştığı aboneliği seçin.

Bir rezervasyon indirimi "*kullanım-BT-veya-kaybetme*" dır. Herhangi bir saat için eşleşen kaynaklarınız yoksa, bu saat için bir rezervasyon miktarı kaybedersiniz. Kullanılmayan ayrılmış saatlerin ileri alınamaz.

Bir kaynağı kapattığınızda, rezervasyon iskontosu otomatik olarak belirtilen kapsamdaki başka bir eşleşen kaynak için geçerli olur. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler *kaybedilir*.

Örneğin, daha sonra bir kaynak oluşturabilir ve daha az kullanılan eşleşen bir rezervasyon olabilir. Rezervasyon iskontosu, yeni eşleşen kaynağa otomatik olarak uygulanır.

Sanal makineler, kaydınız/hesabınız içindeki farklı aboneliklerde çalışıyorsa, kapsamı paylaşılan olarak seçin. Paylaşılan kapsam, rezervasyon indirimlerinin abonelikler arasında uygulanmasını sağlar. Bir ayırmayı satın aldıktan sonra kapsamı değiştirebilirsiniz. Daha fazla bilgi için bkz. [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).

Rezervasyon indirimi yalnızca, Kullandıkça Öde tarifesine sahip kurumsal, CSP veya aboneliklerle ilişkili kaynaklar için geçerlidir. Başka teklif türlerine sahip bir abonelikte çalışan kaynaklar rezervasyon iskontosunu almaz.

## <a name="when-the-reservation-term-expires"></a>Rezervasyon dönemi sona erdiğinde

Rezervasyon döneminin sonunda, fatura iskontosunun süresi dolar. Sanal makine, SQL veritabanı, Azure Cosmos DB veya başka bir kaynak Kullandıkça Öde fiyatı üzerinden faturalandırılır. Azure ayırmaları otomatik olarak yenilemez. Fatura iskontosunu almaya devam etmek için uygun hizmetler ve yazılımlar için yeni bir ayırma satın almanız gerekir.

## <a name="discount-applies-to-different-sizes"></a>İndirim farklı boyutlarda geçerlidir

Bir ayırma satın aldığınızda, indirim aynı boyut grubundaki özniteliklere sahip diğer örneklere uygulanabilir. Bu özellik örnek boyutu esnekliği olarak bilinir. İndirim kapsamının esnekliği, rezervasyon türüne ve ayırmayı satın alırken seçtiğiniz özniteliklere göre değişir.

Hizmet planları:

- Ayrılmış VM örnekleri: Ayırmayı satın alıp **örnek boyutu esnekliği Için En Iyi duruma getirilmiş**seçeneğini belirlediğinizde, indirimli kapsam seçtiğiniz VM boyutuna bağlıdır. Ayırma, aynı boyut serisi grubundaki sanal makineler (VM) boyutlarına uygulanabilir. Daha fazla bilgi için bkz. [ayrılmış VM örnekleri Ile sanal makine boyutu esnekliği](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- SQL veritabanı ayrılmış kapasitesi: İndirim kapsamı, seçtiğiniz performans katmanına bağlıdır. Daha fazla bilgi için bkz. [Azure rezervasyon iskontosunun nasıl uygulandığını anlama](billing-understand-reservation-charges.md).
- Ayrılmış kapasite Azure Cosmos DB: İndirim kapsamı, sağlanan işleme göre değişir. Daha fazla bilgi için bkz. [Azure Cosmos DB rezervasyon iskontosunun nasıl uygulandığını anlama](billing-understand-cosmosdb-reservation-charges.md).

## <a name="reservation-notifications"></a>Rezervasyon bildirimleri

Azure aboneliğiniz için nasıl ödeme yaptığınıza bağlı olarak, kuruluşunuzdaki aşağıdaki kullanıcılara rezervasyon bildirimleri e-posta ile göndereceğiz. Bildirimleri aşağıdakiler dahil çeşitli olaylar için gönderilir:

- Satın Alın
- Yaklaşan rezervasyon süre sonu
- Expiry
- Yenilemesini
- Kin
- Kapsam değişikliği

EA abonelikleri olan müşteriler için:
- Satın alma bildirimi, Satınalmacı ve EA Notification kişilerine gönderilir.
- Diğer rezervasyon yaşam döngüsü bildirimleri yalnızca EA Notification kişilerine gönderilir.
- RBAC (ıAM) izni kullanılarak bir ayırmaya eklenen kullanıcılar herhangi bir e-posta bildirimi almaz.

Bireysel abonelikleri olan müşteriler için:
- Satınalmacı bir satın alma bildirimi alır.
- Satın alma sırasında, abonelik faturalandırma hesabı sahibi bir satın alma bildirimi alır.
- Hesap sahibi diğer tüm bildirimleri alır.


## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Aşağıdaki makalelerle Azure ayırmaları hakkında daha fazla bilgi edinin:
    - [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
    - [Kullandıkça Öde tarifesine sahip aboneliğiniz için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md)
    - [Kurumsal kaydınız için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage-ea.md)
    - [Windows yazılım maliyetleri rezervasyonlar içermez](billing-reserved-instance-windows-software-costs.md)
    - [Iş Ortağı Merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmaları](/partner-center/azure-reservations)

- Hizmet planları için rezervasyonlar hakkında daha fazla bilgi edinin:
    - [Azure ayrılmış VM örneklerine sahip sanal makineler](../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Azure Cosmos DB ayrılmış kapasiteye sahip kaynakları Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Azure SQL veritabanı ayrılmış kapasitesi Ile SQL veritabanı işlem kaynakları](../sql-database/sql-database-reserved-capacity.md) Yazılım planları için ayırmalar hakkında daha fazla bilgi edinin:
    - [Azure ayırmalarının Red Hat yazılım planları](../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Azure ayırmalarının sunduğu SUSE yazılım planları](../virtual-machines/linux/prepay-suse-software-charges.md)
