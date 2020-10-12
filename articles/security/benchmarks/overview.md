---
title: Azure Güvenlik kıyaslaması v2 'ye Genel Bakış
description: Azure Güvenlik kıyaslaması v2 genel bakış
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 09/11/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 6628d693a8df3614097e23785ac234a451565ca3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777114"
---
# <a name="overview-of-the-azure-security-benchmark-v2"></a>Azure Güvenlik kıyaslamaya genel bakış (v2)

Azure Güvenlik kıyaslaması (ASB), Azure 'da iş yüklerinin, verilerin ve hizmetlerin güvenliğini artırmaya yardımcı olmak için önerilen en iyi uygulamalar ve öneriler sağlar.

Bu kıyaslama, ayrıca şunları da içeren bir bütünsel Güvenlik Kılavuzu kümesinin parçasıdır:

- **Bulut benimseme çerçevesi** : [strateji](/azure/cloud-adoption-framework/strategy/define-security-strategy), [Roller ve sorumluluklar](/azure/cloud-adoption-framework/organize/cloud-security), [Azure ilk 10 güvenlik en iyi uygulamaları](/azure/cloud-adoption-framework/get-started/security#step-1-establish-essential-security-practices)ve [başvuru uygulaması](/azure/cloud-adoption-framework/ready/enterprise-scale/)dahil olmak üzere güvenlikle ilgili yönergeler.
- **Azure Well-Architected Framework** : Azure 'da [iş yüklerinizi güvenli hale getirme](https://docs.microsoft.com/assessments/?mode=pre-assessment&session=local) Kılavuzu.
- **Microsoft Güvenlik En Iyi uygulamaları** – Azure 'da örneklerle ilgili [öneriler](/security/compass/microsoft-security-compass-introduction) .

 Azure Güvenlik kıyaslaması, bulut merkezli denetim bölümlerine odaklanır. Bu denetimler, Internet güvenliği (CIS) için merkezi tarafından açıklananlar, sürüm 7,1 ve ulusal standartlar ve Teknoloji Enstitüsü (NıST) SP800-53 gibi iyi bilinen güvenlik kıyaslamalarıyla tutarlıdır.
Aşağıdaki denetimler Azure Güvenlik kıyaslaması 'nda bulunur:

| ASB denetim etki alanları | Açıklama 
|--|--|
| [Ağ &nbsp; Güvenliği &nbsp; (NS)](security-controls-v2-network-security.md) | Ağ güvenliği, sanal ağların güvenliğini sağlama, özel bağlantılar oluşturma, dış saldırıları koruma ve azaltma ve DNS güvenliğini sağlama gibi Azure ağlarını güvenli hale getirmeye ve korumaya yönelik denetimleri ele alır. |
| [Kimlik &nbsp; Yönetimi &nbsp; (IM)](security-controls-v2-identity-management.md) | Kimlik yönetimi, uygulamalar, koşullu erişim ve hesap bozukluklarını izlemek için çoklu oturum açma, güçlü kimlik doğrulamaları, Yönetilen kimlikler (ve hizmet ilkeleri) kullanımı dahil olmak üzere Azure Active Directory kullanarak güvenli bir kimlik ve erişim denetimlerine yönelik denetimleri ele alır. |
| [Ayrıcalıklı &nbsp; erişim &nbsp; (PA)](security-controls-v2-privileged-access.md) | Ayrıcalıklı erişim, yönetim modelinizi, yönetim hesaplarınızı ve ayrıcalıklı erişim İş istasyonlarınızı bilinçli ve yanlışlıkla riske karşı korumak için bir dizi denetim dahil olmak üzere Azure kiracınıza ve kaynaklarınıza ayrıcalıklı erişimi korumaya yönelik denetimleri ele alır. |
| [Veri &nbsp; Koruma &nbsp; (DP)](security-controls-v2-data-protection.md) | Veri koruma, Azure 'da erişim denetimi, şifreleme ve oturum açma ile hassas veri varlıklarını bulma, sınıflandırma, koruma ve izleme dahil olmak üzere, bekleyen veri koruma denetimini ele alır. |
| [Varlık &nbsp; Yönetimi &nbsp; (Har)](security-controls-v2-asset-management.md) | Varlık yönetimi, güvenlik personeline yönelik izinler, varlık envanterine yönelik güvenlik erişimi ve hizmet ve kaynaklara yönelik onayları yönetme (envanter, izleme ve düzeltme) dahil olmak üzere Azure kaynakları üzerinde güvenlik görünürlüğü ve idare sağlamak için denetimleri ele alır. |
| [Günlüğe kaydetme &nbsp; ve &nbsp; tehdit &nbsp; algılama (lt)](security-controls-v2-logging-threat-detection.md) | Günlüğe kaydetme ve tehdit algılama, Azure 'da tehditleri algılayan ve Azure hizmetleri için Denetim günlüklerini etkinleştirme, toplama ve depolama, Azure hizmetlerinde yerel tehdit algılama ile yüksek kaliteli uyarılar oluşturmaya yönelik denetimleri kapsayan denetimleri içerir; Ayrıca Azure Izleyici ile günlüklerin toplanmasını, Azure Sentinel ile güvenlik analizini, zaman eşitlemesini ve günlük tutmayı de kapsar. |
| [Olay &nbsp; yanıtı &nbsp; (IR)](security-controls-v2-incident-response.md) | Olay yanıtı, olay yanıtı sürecini otomatik hale getirmek için Azure Güvenlik Merkezi ve Sentinel gibi Azure hizmetlerini kullanma dahil, olay yanıtı yaşam döngüsü hazırlığı, algılama ve analiz, kapsama ve olay sonrası etkinliklerindeki denetimleri ele alır. |
| [Posture &nbsp; ve &nbsp; Güvenlik Açığı &nbsp; Yönetimi &nbsp; (BD)](security-controls-v2-posture-vulnerability-management.md) | Bildirim ve güvenlik açığı yönetimi, güvenlik açığı taraması, sızma testi ve düzeltilmesi, ayrıca Azure kaynaklarında güvenlik yapılandırması izleme, raporlama ve düzeltme gibi Azure Güvenlik duruşunu değerlendirmek ve geliştirmek için denetimlere odaklanır. |
| [Uç nokta &nbsp; Güvenliği &nbsp;](security-controls-v2-endpoint-security.md) | Uç nokta güvenliği, uç nokta algılama ve yanıt (EDR) ve Azure ortamlarında uç noktalar için kötü amaçlı yazılımdan koruma hizmeti kullanımı dahil olmak üzere Endpoint Detection ve yanıtta bulunan denetimleri ele alır. |
| [Yedekleme &nbsp; ve &nbsp; Kurtarma &nbsp; (br)](security-controls-v2-backup-recovery.md) | Yedekleme ve kurtarma, farklı hizmet katmanlarındaki veri ve yapılandırma yedeklemelerinin gerçekleştirilmesini, doğrulanmasını ve korunmasını sağlamak için denetimleri ele alır. |
| [İdare &nbsp; ve &nbsp; strateji &nbsp; (g)](security-controls-v2-governance-strategy.md) | İdare ve strateji, farklı bulut güvenlik işlevlerine, Birleşik teknik stratejiye ve destekleyici ilke ve standartlara yönelik roller ve sorumluluklar oluşturma gibi güvenlik güvencesini sağlayan ve güvenlik güvencesi sağlamak için yönergeler sağlar. |

## <a name="azure-security-benchmark-recommendations"></a>Azure Güvenlik kıyaslama önerileri

Her öneri aşağıdaki bilgileri içerir:

- **Azure ID**: öneriye karşılık gelen Azure GÜVENLIK kıyaslama kimliği.
- **CIS v 7.1 ID 'Leri denetler**: CIS, bu öneriye karşılık gelen v 7.1 denetimini denetler.
- **NıST SP800-53 R4 ID**: bu öneriye KARŞıLıK gelen NIST SP800-53 R4 (orta) denetimleri.
- **Ayrıntılar**: önerinin ve nasıl uygulanacağı üzerine kılavuzluk eden bağlantılar için korvaale. Öneri Azure Güvenlik Merkezi tarafından destekleniyorsa, bu bilgiler de listelenecektir.
- **Sorumluluk**: müşterinin, hizmet sağlayıcının veya her ikisinin de bu öneriyi uygulamaktan sorumlu olup olmadığı. Güvenlik sorumlulukları genel bulutta paylaşılır. Bazı güvenlik denetimleri yalnızca bulut hizmeti sağlayıcısı tarafından kullanılabilir ve bu nedenle sağlayıcı bunların adreslenmesini sağlamaktan sorumludur. Bunlar genel gözlemlerdir: bazı hizmetler için sorumluluk Azure Güvenlik kıyaslaması ' nda listelenenlerden farklı olacaktır. Bu farklılıklar, bireysel hizmet için temel önerilerle açıklanmıştır.
- **Müşteri güvenlik paydaşları**: müşteri kuruluşunda, ilgili denetim için sorumlu veya sorumlu olabilecek [güvenlik işlevleri](/azure/cloud-adoption-framework/organize/cloud-security#security-functions) . Kuruluşunuzun güvenlik kuruluşu yapısına ve Azure güvenliğiyle ilgili ayarladığınız rol ve sorumluluklara bağlı olarak, kuruluştan kuruluşa farklı olabilir.

> [!NOTE]
> ASB ve sektör değerlendirmeleri (NıST ve CIS gibi) arasındaki denetim eşlemeleri yalnızca belirli bir Azure özelliğinin NıST veya CIS 'de tanımlanan bir denetim gereksinimini tamamen veya kısmen adreslendirdiğini gösterir. Bu tür bir uygulamanın, CIS veya NıST 'de karşılık gelen denetimin tam uyumluluğuna çevrilmeyeceğini unutmayın.

Ayrıntılı geri bildiriminiz ve etkin katılım ile Azure Güvenlik kıyaslama çabasında hoş geldiniz. Azure Güvenlik kıyaslama ekibinin doğrudan girişini sağlamak istiyorsanız, formu şu adreste doldurun: https://aka.ms/AzSecBenchmark

## <a name="download"></a>İndir

Azure Güvenlik kıyaslaması ' nı [elektronik tablo biçiminde](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Security%20Benchmark)indirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar 
- Bkz. ilk güvenlik denetimi: [ağ güvenliği](security-control-network-security.md)
- [Azure Güvenlik kıyaslaması tanıtım](introduction.md) bilgilerini okuyun
- [Azure Güvenlik temellerini](../fundamentals/index.yml) öğrenin
