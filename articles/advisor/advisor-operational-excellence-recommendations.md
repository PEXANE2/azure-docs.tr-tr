---
title: Azure Danışmanı ile Azure abonelikleriniz için operasyonel bir işlem için geliştirme yapın
description: Azure abonelikleriniz için en üstün işlemsel mükemmellik sağlamak için Advisor 'ı kullanın
ms.topic: article
ms.date: 10/24/2019
ms.openlocfilehash: f34284ba62bd5dea98345ebe73365b332d38ee78
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443065"
---
# <a name="achieve-operational-excellence-with-azure-advisor"></a>Azure Danışmanı ile işlemsel üstün üstün ulaşın

Azure Advisor operasyonel üstün öneriler, işlem ve iş akışı verimliliği, kaynak yönetilebilirlik ve dağıtım en iyi uygulamaları ile ilgili müşteri yardımı. Danışman panosunun **Işletimsel üstün** sekmesinde bu önerileri danışmandan edinebilirsiniz.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Azure sorunları sizi etkiliyorsa bildirim almak için Azure hizmet durumu uyarıları oluşturun

Azure hizmet sorunları sizi etkiliyorsa bildirim almak için Azure hizmet durumu uyarılarını ayarlamayı öneririz. [Azure hizmet durumu](https://azure.microsoft.com/features/service-health/) , bir Azure hizmet sorunundan etkilendiğiniz zaman kişiselleştirilmiş rehberlik ve destek sağlayan ücretsiz bir hizmettir. Advisor, uyarıları yapılandırılmamış ve bir tane oluşturulmasını öneren abonelikleri tanımlar.

## <a name="design-your-storage-accounts-to-prevent-hitting-the-maximum-subscription-limit"></a>Maksimum abonelik sınırına vurmasını engellemek için depolama hesaplarınızı tasarlayın

Bir Azure bölgesi, abonelik başına en fazla 250 depolama hesabını destekleyebilir. Sınıra ulaşıldığında, bu bölgede/abonelik birleşiminde daha fazla depolama hesabı oluşturmayacak. Danışman, aboneliklerinizi ve yüzey önerilerinizi, en yüksek sınıra ulaşılmaya yakın olan herhangi biri için daha az depolama hesabı tasarlamanızı denetlayacaktır.

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>İhtiyacınız olduğunda Azure bulut uzmanlarıyla erişime sahip olduğunuzdan emin olun

İş açısından kritik bir iş yükü çalıştırırken, gerektiğinde teknik desteğe erişebilmek önemlidir. Danışman destek planına dahil olmayan ve teknik destek içeren bir seçeneğe yükseltmeyi öneren potansiyel iş açısından kritik abonelikleri tanımlar.

## <a name="repair-invalid-log-alert-rules"></a>Geçersiz günlük uyarısı kurallarını Onar

Azure Danışmanı, durumu bölümünde geçersiz sorguları belirtilmiş olan uyarı kurallarını algılar. Günlük uyarı kuralları Azure İzleyici'de oluşturulur ve belirtilen zaman aralıklarında analiz sorguları çalıştırmak için kullanılır. Sorgu sonuçları, uyarıyı tetiklemenin gerekip gerekmediğini belirler. Başvurulan kaynaklarda, tablolarla veya komutlarda zamanla değişiklik oldukça analiz sorguları geçersiz hale gelir. Danışman, otomatik devre dışı bırakılmasını ve Azure 'da kaynaklarınızın kapsamını izlemeyi güvence altına almak için uyarı kuralındaki sorguyu düzeltmeniz önerilir. [Uyarı kuralları sorunlarını giderme hakkında daha fazla bilgi edinin](https://aka.ms/aa_logalerts_queryrepair)

## <a name="follow-best-practices-using-azure-policy"></a>Azure Ilkesini kullanarak en iyi uygulamaları izleyin

Azure İlkesi, ilkelerinizi oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir. Bu ilkeler farklı kuralları zorunlu kılar ve kaynaklarınızı etkiler. Aşağıda, işletimsel yardım için Azure ilke önerileri yer almanıza yardımcı olur: 
1. Azure Ilkesini kullanarak etiketleri yönetme: Bu ilke, herhangi bir kaynak oluşturulduğunda veya güncelleştirilirken belirtilen etiketi ve değeri ekler veya değiştirir. Mevcut kaynaklar bir düzeltme görevi tetiklenerek düzeltilebilir. Ayrıca, bu, kaynak gruplarındaki etiketleri değiştirmez.
2. Azure Ilkesi 'ni kullanarak coğrafi uyumluluk gereksinimlerini zorla: ilke, kuruluşunuzun kaynakları dağıttığınızda belirlediği konumları kısıtlamanıza olanak sağlar. 
3. Dağıtımlar için izin verilen sanal makine SKU 'Larını belirtin: Bu ilke, kuruluşunuzun dağıtabileceğiniz bir sanal makine SKU 'Ları kümesi belirtmenizi sağlar.
4. Azure ilkesi 'ni kullanarak ' yönetilen diskleri kullanmayan VM 'Leri denetle ' öğesini zorlama
5. Azure ilkesi kullanarak ' kaynak gruplarından etiket devralma ' kullanın: ilke, herhangi bir kaynak oluşturulduğunda veya güncelleştirilirken belirtilen etiketi ve değeri üst kaynak grubundan ekler veya değiştirir. Mevcut kaynaklar bir düzeltme görevi tetiklenerek düzeltilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi edinmek için bkz.:
* [Advisor 'a giriş](advisor-overview.md)
* [Kullanmaya Başlama](advisor-get-started.md)
* [Danışman maliyet önerileri](advisor-cost-recommendations.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Danışman yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Danışman güvenlik önerileri](advisor-security-recommendations.md)
