---
title: Azure Güvenlik kıyaslama v2-olay yanıtı
description: Azure Güvenlik kıyaslaması v2 olay yanıtı
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 2dbdb1af139472d5c7f4537399d434e045bb05cb
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059401"
---
# <a name="security-control-incident-response"></a>Güvenlik denetimi: olay yanıtı

Olay yanıtı, olay yanıtı yaşam döngüsü hazırlama, algılama ve analiz, kapsama ve olay sonrası etkinliklerin denetimlerini içerir. Bu, olay yanıtı sürecini otomatikleştirmek için Azure Güvenlik Merkezi ve Sentinel gibi Azure hizmetlerinin kullanılmasını içerir.

## <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: hazırlık – Azure için olay yanıtı işlemini güncelleştirme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| IR-1 | 19 | IR-4, ıR-8 |

Kuruluşunuzun güvenlik olaylarına yanıt vermek için işlem yapıldığından emin olun, Azure için bu işlemlerin güncelleştirilmesini sağlayın ve hazırlık sağlamak için bunları düzenli olarak yeniden tasarlanmıştır.

- [Kurumsal ortam genelinde güvenliği uygulama](https://aka.ms/AzSec4)

- [Olay yanıtı başvuru kılavuzu](https://aka.ms/IRRG)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik işlemleri (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: hazırlık – olay bildirimini kurma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| IR-2 | 19,5 | IR-4, ıR-5, ıR-6, ıR-8 |

Azure Güvenlik Merkezi 'nde güvenlik olayı iletişim bilgilerini ayarlayın. Bu iletişim bilgileri, Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. Ayrıca, olay uyarı gereksinimlerinize göre farklı Azure hizmetlerinde olay uyarısını ve bildirimi özelleştirme seçenekleriniz vardır. 

- [Azure Güvenlik Merkezi güvenlik ilgili kişisini ayarlama](../../security-center/security-center-provide-security-contact-details.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik işlemleri (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: algılama ve analiz – yüksek kaliteli uyarılara göre olaylar oluşturun

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| IR-3 | 19,6 | IR-4, ıR-5 |

Yüksek kaliteli uyarılar oluşturmaya ve uyarı kalitesini ölçmeye yönelik bir işleminiz olduğundan emin olun. Bu, geçmiş olaylardan dersler öğrenmenize ve analistlere yönelik olarak bu uyarıların önceliklendirmesine izin verir, böylelikle hatalı pozitif sonuçlar üzerinde zaman harcanmazlar. 

Yüksek kaliteli uyarılar, geçmiş olaylar, doğrulanan topluluk kaynakları ve uyarılar oluşturmak ve temizlemek için tasarlanan araçların yanı sıra çeşitli sinyal kaynaklarını kullanarak oluşturulabilir. 

Azure Güvenlik Merkezi, birçok Azure varlığı arasında yüksek kaliteli uyarılar sağlar. Azure Sentinel 'e uyarı akışı sağlamak için ASC Data bağlayıcısını kullanabilirsiniz. Azure Sentinel, bir araştırma için olayları otomatik olarak oluşturmak üzere gelişmiş uyarı kuralları oluşturmanıza olanak sağlar. 

Azure kaynakları için riskleri belirlemenize yardımcı olmak üzere dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışarı aktarın.

- [Dışarı aktarmayı yapılandırma](../../security-center/continuous-export.md)

- [Uyarıları Azure Sentinel 'e akış](../../sentinel/connect-azure-security-center.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik işlemleri (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: algılama ve analiz – bir olayı araştırın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| IR-4 | 19 | IR-4 |

Analistlerin, olası olayları araştırarak, ne olduğunu bir tam görünüm oluşturmak için farklı veri kaynaklarını sorgulayabileceği ve kullanabilmesi için analist emin olun. Öngörüler ve dersleri diğer analistler için yakalandığından ve gelecekteki geçmiş başvurusuyla emin olun. 

Araştırmaya yönelik veri kaynakları, zaten kapsam içi hizmetlerden ve çalışan sistemlerde toplanmakta olan Merkezi günlük kaynakları içerir, ancak şunları da içerebilir:

Ağ verileri – ağ akışı günlüklerini ve diğer analiz bilgilerini yakalamak için ağ güvenlik grupları ' akış günlükleri, Azure ağ Izleyicisi ve Azure Izleyicisi ' ni kullanın. Çalışan sistemlerin anlık görüntüleri: 

-   Çalışan sistemin diskinin anlık görüntüsünü oluşturmak için Azure sanal makinesinin anlık görüntü özelliğini kullanın. 

-   Çalışan sistem belleğinin bir anlık görüntüsünü oluşturmak için işletim sisteminin yerel bellek dökümü özelliğini kullanın.

-   Çalışan sistemlerin anlık görüntülerini oluşturmak için Azure hizmetlerinin anlık görüntü özelliğini veya yazılımınızın kendi özelliğini kullanın.

Azure Sentinel, olayların tam yaşam döngüsünü yönetmek için neredeyse tüm günlük kaynakları ve bir servis talebi yönetim portalı genelinde kapsamlı veri analizi sağlar. İnceleme sırasında zeka bilgileri, izleme ve raporlama amaçları için bir olayla ilişkilendirilebilir. 

- [Windows makinenin diskinin anlık görüntüsünü alma](../../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Linux makinesinin diskinin anlık görüntüsünü alma](../../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure tanılama bilgilerini ve bellek dökümü toplamayı destekler](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Azure Sentinel ile olayları araştırın](../../sentinel/tutorial-investigate-cases.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik işlemleri (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: algılama ve analiz – olayların önceliklerini belirleme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| IR-5 | 19,8 | CA-2, ıR-4 |

Uyarı önem derecesine ve varlık duyarlığına göre ilk olarak odaklanabilecek olayların analistlerine bağlamını sağlayın. 

Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, güvenlik merkezi 'nin uyarıyı vermek için kullanılan bulma veya analitik ile ne kadar önemli olduğunu ve uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu belirten güven düzeyini temel alır.

Ayrıca, abonelikleri kullanarak abonelikleri işaretleyin ve Azure kaynaklarını tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun.  Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik işlemleri (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: kapsama, eradleme ve kurtarma – olay işlemeyi otomatikleştirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| IR-6 | 19 | IR-4, ıR-5, ıR-6 |

Yanıt süresini hızlandırmak ve analistlerin yükünü azaltmak için el ile yinelenen görevleri otomatikleştirin. El ile gerçekleştirilen görevlerin yürütülmesi daha uzun sürer, her olayı yavaşlatarak analist 'nin işleyebileceği olayların sayısını azaltır. El ile gerçekleştirilen görevler Ayrıca analist yükselini artırır ve bu da gecikme ve analistlerin karmaşık görevlere etkin bir şekilde odaklanmasına olanak tanır. İşlemleri otomatik olarak tetiklemek veya gelen güvenlik uyarılarına yanıt vermek üzere bir PlayBook çalıştırmak için Azure Güvenlik Merkezi 'nde ve Azure Sentinel 'de iş akışı Otomasyonu özelliklerini kullanın. PlayBook, bildirimler gönderme, hesapları devre dışı bırakma ve sorunlu ağları yalıtma gibi işlemleri gerçekleştirir. 

- [Güvenlik Merkezi 'nde iş akışı Otomasyonu 'nu yapılandırma](../../security-center/workflow-automation.md)

- [Azure Güvenlik Merkezi 'nde otomatik tehdit yanıtlarını ayarlama](../../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Azure Sentinel 'de otomatik tehdit yanıtlarını ayarlama](../../sentinel/tutorial-respond-threats-playbook.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik işlemleri (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

