---
title: Azure Danışmanı ile Azure abonelikleriniz için operasyonel mükemmelliği artırın
description: Azure aboneliklerinizde operasyonel mükemmelliği optimize etmek ve olgunlaştMak için Danışman'ı kullanın
ms.topic: article
ms.date: 10/24/2019
ms.openlocfilehash: f34284ba62bd5dea98345ebe73365b332d38ee78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443065"
---
# <a name="achieve-operational-excellence-with-azure-advisor"></a>Azure Danışmanı ile operasyonel mükemmelliğe ulaşma

Azure Danışmanı operasyonel mükemmellik önerileri, müşteriye süreç ve iş akışı verimliliği, kaynak yönetilebilirliği ve dağıtım en iyi uygulamaları konusunda yardımcı olur. Bu önerileri Danışman panosunun **Operasyonel Mükemmellik** sekmesinde Danışman'dan alabilirsiniz.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Azure sorunları sizi etkilediğinde haberdar olmak için Azure Hizmet Durumu uyarıları oluşturun

Azure hizmet sorunları sizi etkilediğinde bildirilecek Azure Hizmet Durumu uyarıları ayarlamanızı öneririz. [Azure Hizmet Durumu,](https://azure.microsoft.com/features/service-health/) bir Azure hizmet sorunundan etkilendiğinde kişiselleştirilmiş rehberlik ve destek sağlayan ücretsiz bir hizmettir. Danışman, uyarıyapılmayı olmayan abonelikleri tanımlar ve bir tane oluşturmayı önerir.

## <a name="design-your-storage-accounts-to-prevent-hitting-the-maximum-subscription-limit"></a>Maksimum abonelik sınırına çarpmayı önlemek için depolama hesaplarınızı tasarlayın

Azure bölgesi, abonelik başına en fazla 250 depolama hesabı destekleyebilir. Sınıra ulaşıldıktan sonra, o bölgede/abonelik bileşiminde başka depolama hesabı oluşturamazsınız. Danışman, maksimum limite ulaşmaya yakın olanlar için daha az depolama alanı hesabı tasarlamanız için aboneliklerinizi ve yüzey önerilerinizi kontrol edecektir.

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>İhtiyacınız olduğunda Azure bulut uzmanlarına erişebildiğinizden emin olun

İş açısından kritik bir iş yükü çalıştırırken, gerektiğinde teknik desteğe erişebilmek önemlidir. Danışman, destek planında teknik destek olmayan olası iş açısından kritik abonelikleri tanımlar ve teknik destek içeren bir seçenek için yükseltme önerir.

## <a name="repair-invalid-log-alert-rules"></a>Geçersiz günlük uyarı kurallarını onar

Azure Danışmanı, durum bölümünde belirtilen geçersiz sorguları olan uyarı kurallarını algılar. Günlük uyarı kuralları Azure İzleyici'de oluşturulur ve belirtilen zaman aralıklarında analiz sorguları çalıştırmak için kullanılır. Sorgu sonuçları, uyarıyı tetiklemenin gerekip gerekmediğini belirler. Başvurulan kaynaklarda, tablolarla veya komutlarda zamanla değişiklik oldukça analiz sorguları geçersiz hale gelir. Danışman, otomatik devre dışı kalmasını önlemek ve Azure'daki kaynaklarınızın kapsama alanını izlemeyi sağlamak için uyarı kuralındaki sorguyu düzeltmenizi önerir. [Sorun giderme uyarısı kuralları hakkında daha fazla bilgi edinin](https://aka.ms/aa_logalerts_queryrepair)

## <a name="follow-best-practices-using-azure-policy"></a>Azure İlkesi'ni kullanarak en iyi uygulamaları izleyin

Azure İlkesi, ilkelerinizi oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir. Bu ilkeler farklı kuralları zorunlu kılar ve kaynaklarınızı etkiler. Aşağıda, operasyonel mükemmellik elde etmenize yardımcı olacak Azure ilke önerileri verilmiştir: 
1. Azure İlkesini Kullanarak Etiketleri Yönet: Bu ilke, herhangi bir kaynak oluşturulduğunda veya güncelleştirildiğinde belirtilen etiket ve değeri ekler veya değiştirir. Mevcut kaynaklar bir düzeltme görevi tetiklenerek düzeltilebilir. Ayrıca, bu kaynak gruplarındaki etiketleri değiştirmez.
2. Azure İlkesi'ni kullanarak coğrafi uyumluluk gereksinimlerini uygulayın: İlke, kuruluşunuzun kaynakları dağıtırken belirleyebileceği konumları kısıtlamanızı sağlar. 
3. Dağıtımlar için izin verilen sanal makine SUS'larını belirtin: Bu ilke, kuruluşunuzun dağıtabileceği bir sanal makine SUS kümesi belirtmenize olanak tanır.
4. Azure ilkesini kullanarak 'Yönetilen diskleri kullanmayan Denetim VM'lerini' uygulayın
5. Azure ilkesini kullanarak 'Kaynak gruplarından etiket devralma' öğesini kullanın: İlke, herhangi bir kaynak oluşturulduğunda veya güncelleştirildiğinde ana kaynak grubundan belirtilen etiketi ve değeri ekler veya değiştirir. Mevcut kaynaklar bir düzeltme görevi tetiklenerek düzeltilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi edinmek için bkz:
* [Danışmana Giriş](advisor-overview.md)
* [Başlayın](advisor-get-started.md)
* [Danışman Maliyet önerileri](advisor-cost-recommendations.md)
* [Danışman Performans önerileri](advisor-performance-recommendations.md)
* [Danışman Yüksek Kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Danışman Güvenlik önerileri](advisor-security-recommendations.md)
