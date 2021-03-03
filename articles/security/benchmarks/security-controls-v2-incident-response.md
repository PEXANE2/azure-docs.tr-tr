---
title: Azure Güvenlik kıyaslama v2-olay yanıtı
description: Azure Güvenlik kıyaslaması v2 olay yanıtı
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 002/22/2021
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 40a88349040482b3325e63d7035dea35df444cdb
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101698595"
---
# <a name="security-control-v2-incident-response"></a>Güvenlik denetimi v2: olay yanıtı

Olay yanıtı, olay yanıtı yaşam döngüsü hazırlama, algılama ve analiz, kapsama ve olay sonrası etkinliklerdeki denetimleri ele alır. Bu, olay yanıtı sürecini otomatikleştirmek için Azure Güvenlik Merkezi ve Sentinel gibi Azure hizmetlerinin kullanılmasını içerir.

Geçerli yerleşik Azure Ilkesini görmek için, bkz. [Azure Güvenlik kıyaslaması uyumluluğu yerleşik girişiminin ayrıntıları: olay yanıtı](../../governance/policy/samples/azure-security-benchmark#incident-response)

## <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Hazırlık: Azure için olay yanıtı sürecini güncelleştirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| IR-1 | 19 | IR-4, ıR-8 |

Kuruluşunuzun güvenlik olaylarına yanıt vermek için işlem yapıldığından emin olun, Azure için bu işlemlerin güncelleştirilmesini sağlayın ve hazırlık sağlamak için bunları düzenli olarak yeniden tasarlanmıştır.

- [Güvenliği kuruluş genelinde uygulama](/azure/cloud-adoption-framework/security/security-top-10#3-process-assign-accountability-for-cloud-security-decisions)

- [Olay yanıtı başvuru kılavuzu](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Hazırlık: Olay bildirimini ayarlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| IR-2 | 19,5 | IR-4, ıR-5, ıR-6, ıR-8 |

Azure Güvenlik Merkezi 'nde güvenlik olayı iletişim bilgilerini ayarlayın. Microsoft, Microsoft Güvenlik Yanıt Merkezi'nin (MSRC) verilerinize kanuna aykırı veya yetkisiz erişim sağlanmasını keşfetmesi durumunda sizinle iletişime geçmek için bu iletişim bilgilerini kullanır. İsterseniz farklı Azure hizmetlerindeki olay uyarılarını ve bildirimlerini olay yanıt gereksinimlerinize göre özelleştirebilirsiniz. 

- [Azure Güvenlik Merkezi güvenlik ilgili kişisini ayarlama](../../security-center/security-center-provide-security-contact-details.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: Algılama ve analiz: Yüksek kaliteli uyarıları temel alan olaylar oluşturun

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| IR-3 | 19,6 | IR-4, ıR-5 |

Yüksek kaliteli uyarılar oluşturmaya ve uyarı kalitesini ölçmeye yönelik bir işleminiz olduğundan emin olun. Bu, geçmiş olaylardan dersler öğrenmenize ve analistlere yönelik olarak bu uyarıların önceliklendirmesine izin verir, böylelikle hatalı pozitif sonuçlar üzerinde zaman harcanmazlar. 

Yüksek kaliteli uyarılar geçmiş olaylardan edinilen deneyime, doğrulanmış topluluk kaynaklarına ve çeşitli sinyal kaynaklarını birleştirerek ve aralarında bağıntı kurarak uyarılar oluşturmak ve temizlemek için tasarlanmış araçlara dayalı olarak oluşturulabilir. 

Azure Güvenlik Merkezi birçok Azure varlığı arasında yüksek kaliteli uyarılar sağlar. ASC veri bağlayıcısını kullanarak uyarıların Azure Sentinel’e akışını yapabilirsiniz. Azure Sentinel bir araştırmaya yönelik olayların otomatik olarak oluşturulması için gelişmiş uyarı kuralları oluşturur. 

Azure kaynaklarına yönelik riskleri tanımlamaya yardımcı olmak için, dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Uyarıları ve önerileri el ile veya sürekli bir biçimde dışarı aktarabilirsiniz.

- [Dışarı aktarma işlemini yapılandırma](../../security-center/continuous-export.md)

- [Uyarıların Azure Sentinel’e akışını yapma](../../sentinel/connect-azure-security-center.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: Algılama ve analiz: Olay araştırması

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| IR-4 | 19 | IR-4 |

Analistlerin, olası olayları araştırarak, ne olduğunu bir tam görünüm oluşturmak için farklı veri kaynaklarını sorgulayabileceği ve kullanabilmesi için analist emin olun. Sonlandırma zinciri boyunca olası saldırganın etkinliklerinin izlenebilmesi için çeşitli günlükler toplanmalı, kör noktaların olması önlenmelidir. Ayrıca diğer analistler için ve gelecekte başvurmak üzere içgörülerin ve çıkarılan derslerin yakalandığından da emin olmalısınız.

Araştırmaya yönelik veri kaynakları kapsam içindeki hizmetlerden ve çalışan sistemlerden zaten toplanmakta olan merkezi günlük kaynaklarının yanı sıra şunları da içerebilir:

- Ağ verileri – ağ akış günlüklerini ve diğer analiz bilgilerini yakalamak için ağ güvenlik gruplarının akış günlüklerini, Azure Ağ İzleyicisi’ni ve Azure İzleyici’ni kullanın. 

- Çalışan sistemlerin anlık görüntüleri: 

    - Çalışan sistemin diskinin anlık görüntüsünü oluşturmak için Azure sanal makinesinin anlık görüntü özelliğini kullanın. 

    - Çalışan sistemin belleğinin anlık görüntüsünü oluşturmak için işletim sisteminin yerel bellek dökümü özelliğini kullanın.

    - Çalışan sistemlerin anlık görüntülerini oluşturmak için Azure hizmetlerinin anlık görüntü özelliğini veya yazılımınızın kendi özelliğini kullanın.

Azure Sentinel hemen her veri kaynağı üzerinde kapsamlı veri analizi gerçekleştirdiği gibi, olayların tüm yaşam döngüsünü yöneten bir olay yönetim portalı da sağlar. Araştırma sırasında elde edilen inceleme bilgileri izleme ve raporlama amacıyla bir olayla ilişkilendirilebilir. 

- [Windows makinesi diskinin anlık görüntüsü](../../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Linux makinesi diskinin anlık görüntüsü](../../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure Desteği tanılama bilgileri ve bellek dökümü toplama](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Azure Sentinel ile olayları araştırma](../../sentinel/tutorial-investigate-cases.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Algılama ve analiz: Olayları önceliklendirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| IR-5 | 19.8 | CA-2, ıR-4 |

Uyarı önem derecesine ve varlık duyarlığına göre ilk olarak odaklanabilecek olayların analistlerine bağlamını sağlayın. 

Azure Güvenlik Merkezi önce hangi uyarıların araştırılması gerektiğini belirlemenize yardımcı olmak için her uyarıya bir önem derecesi atar. Önem derecesinde, Güvenlik Merkezi’nin bulguya ne kadar güvendiği veya uyarıyı verirken kullanılan analiz kadar, uyarıya yol açan etkinliğin ardında kötü bir amaç olduğuna ilişkin güvenilirlik düzeyi de temel alınır.

Buna ek olarak Azure kaynaklarını, özellikle hassas verileri işleyen kaynakları belirlemek ve kategorilere ayırmak için etiketleri kullanarak kaynakları işaretleyin ve bir adlandırma sistemi oluşturun. Azure kaynaklarının önem düzeyine ve olayın oluştuğu ortama bağlı olarak uyarıların çözümünde önceliği belirlemek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../../azure-resource-manager/management/tag-resources.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Kapsama, ortadan kaldırma ve kurtarma: Olay işleme sürecini otomatikleştirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| IR-6 | 19 | IR-4, ıR-5, ıR-6 |

Yanıt süresini hızlandırmak ve analistlerin yükünü azaltmak için el ile yinelenen görevleri otomatikleştirin. El ile gerçekleştirilen görevlerin yürütülmesi daha uzun sürer ve olay yanıt sürecini yavaşlatarak analistlerin işleyebileceği olay sayısını azaltır. Ayrıca el ile gerçekleştirilen görevler analistler açısından daha yorucudur. Dolayısıyla gecikmelere neden olan insan hatası riskini artırır ve analistlerin karmaşık görevlere etkili bir şekilde odaklanma becerisini azaltır. Eylemleri otomatik olarak tetiklemek veya gelen güvenlik uyarılarını yanıtlamak üzere bir playbook çalıştırmak için Azure Güvenlik Merkezi’nin ve Azure Sentinel’in iş akışı otomasyonu özelliklerini kullanın. Playbook bildirimleri gönderme, hesapları devre dışı bırakma ve sorunlu ağları yalıtma gibi eylemleri gerçekleştirir. 

- [Güvenlik Merkezi’nde iş akışı otomasyonunu yapılandırma](../../security-center/workflow-automation.md)

- [Azure Güvenlik Merkezi’nde otomatik tehdit yanıtlarını ayarlama](../../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Azure Sentinel’de otomatik tehdit yanıtlarını ayarlama](../../sentinel/tutorial-respond-threats-playbook.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)