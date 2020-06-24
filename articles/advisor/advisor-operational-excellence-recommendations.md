---
title: Danışman ile operasyonel Excel 'i geliştirme
description: Azure abonelikleriniz için işlemsel mükemmelliği iyileştirmek için Azure Advisor 'ı kullanın.
ms.topic: article
ms.date: 10/24/2019
ms.openlocfilehash: 1cac5a3f93df8422a3896b1251857bf552731fb4
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2020
ms.locfileid: "85125404"
---
# <a name="achieve-operational-excellence-by-using-azure-advisor"></a>Azure Advisor 'ı kullanarak işlemsel mükemmellik elde edin

Azure Advisor 'daki işlemsel mükemmellik önerileri şunları yapmanıza yardımcı olabilir: 
- İşlem ve iş akışı verimliliği.
- Kaynak yönetilebilirliği.
- Dağıtım en iyi uygulamaları. 

Bu önerileri, danışman panosunun **Işletimsel üstün çalışma** sekmesinde edinebilirsiniz.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-problems-affect-you"></a>Azure sorunları sizi etkiliyorsa bildirim almak için Azure hizmet durumu uyarıları oluşturun

Azure hizmeti sorunları sizi etkiliyorsa size bildirilecek olan Azure hizmet durumu uyarılarını ayarlamanızı öneririz. [Azure hizmet durumu](https://azure.microsoft.com/features/service-health/) , bir Azure hizmet sorunundan etkilendiğiniz zaman kişiselleştirilmiş rehberlik ve destek sağlayan ücretsiz bir hizmettir. Danışman, uyarıları yapılandırılmış olmayan ve bunları yapılandırmanızı öneren abonelikleri tanımlar.


## <a name="design-your-storage-accounts-to-prevent-reaching-the-maximum-subscription-limit"></a>Maksimum abonelik sınırına ulaşmasını engellemek için depolama hesaplarınızı tasarlayın

Bir Azure bölgesi, abonelik başına en fazla 250 depolama hesabını destekleyebilir. Bu sınıra ulaştığınızda, bu bölge/abonelik birleşiminde depolama hesapları oluşturamazsınız. Danışman aboneliklerinizi denetler ve sınıra ulaşılmaya yakın olan herhangi bir bölge/abonelik için daha az depolama hesabı tasarlamanızı sağlayacak öneriler sağlar.

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>İhtiyacınız olduğunda Azure bulut uzmanlarıyla erişime sahip olduğunuzdan emin olun

İş açısından kritik bir iş yükü çalıştırırken, ihtiyacınız olduğunda teknik desteğe erişebilmek önemlidir. Danışman, destek planına dahil olan teknik desteğe sahip olmayan iş açısından kritik abonelikleri tanımlar. Teknik destek içeren bir seçeneğe yükseltmeyi önerir.

## <a name="delete-and-re-create-your-pool-to-remove-a-deprecated-internal-component"></a>Kullanımdan kaldırılan bir iç bileşeni kaldırmak için havuzunuzu silin ve yeniden oluşturun

Havuzunuz kullanım dışı bir iç bileşen kullanıyorsa, daha iyi kararlılık ve performans için havuzu silin ve yeniden oluşturun.

## <a name="repair-invalid-log-alert-rules"></a>Geçersiz günlük uyarısı kurallarını Onar

Azure Danışmanı, durumu bölümünde geçersiz sorguları belirtilmiş olan uyarı kurallarını algılar. Azure Izleyici 'de günlük uyarı kuralları oluşturabilir ve bunları belirli aralıklarda analiz sorguları çalıştırmak için kullanabilirsiniz. Sorgu sonuçları, uyarıyı tetiklemenin gerekip gerekmediğini belirler. Başvurulan kaynaklarda, tablolarda veya komutlarda yapılan değişiklikler nedeniyle, analiz sorguları zaman içinde geçersiz hale gelebilir. Danışman, otomatik olarak devre dışı bırakılmasını ve Azure 'da kaynaklarınızın kapsamını izlemeyi güvence altına almak için uyarı kuralındaki sorguyu düzeltmenizi önerir. [Uyarı kuralları sorunlarını giderme hakkında daha fazla bilgi edinin.](https://aka.ms/aa_logalerts_queryrepair)

## <a name="use-azure-policy-recommendations"></a>Azure Ilke önerilerini kullanma

Azure Ilkesi, Azure 'da ilke oluşturmak, atamak ve yönetmek için kullanabileceğiniz bir hizmettir. Bu ilkeler, kaynaklarınızda kuralları ve etkileri uygular. Aşağıdaki Azure Ilke önerileri, işlemsel bir yardım almanıza yardımcı olabilir: 

**Etiketleri yönetin.** Bu ilke, bir kaynak oluşturulduğunda veya güncelleştirildiğinde belirtilen etiketi ve değeri ekler veya değiştirir. Bir düzeltme görevi tetikleyerek mevcut kaynakları düzeltebilirsiniz. Bu ilke, kaynak gruplarındaki etiketleri değiştirmez.

**Coğrafi uyumluluk gereksinimlerini zorunlu tutun.** Bu ilke, kuruluşunuzun kaynakları dağıtırken belirleyebileceği konumları kısıtlamanıza olanak verir. 

**Dağıtımlar için izin verilen sanal makine SKU 'Larını belirtin.** Bu ilke, kuruluşunuzun dağıtabileceği bir sanal makine SKU'ları kümesi belirtmenizi sağlar.

***Yönetilen disk kullanmayan VM 'Leri denetlemeyi*zorunlu tutun.**

***Kaynak gruplarından etiket devralmayı*etkinleştirin.** Bu ilke, bir kaynak oluşturulduğunda veya güncelleştirildiğinde üst kaynak grubunda belirtilen etiketi ve değeri ekler veya değiştirir. Bir düzeltme görevi tetikleyerek mevcut kaynakları düzeltebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi edinmek için bkz.:
* [Advisor 'a giriş](advisor-overview.md)
* [başlarken](advisor-get-started.md)
* [Danışman maliyet önerileri](advisor-cost-recommendations.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Advisor güvenilirlik önerileri](advisor-high-availability-recommendations.md)
* [Danışman güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman REST API](https://docs.microsoft.com/rest/api/advisor/)
