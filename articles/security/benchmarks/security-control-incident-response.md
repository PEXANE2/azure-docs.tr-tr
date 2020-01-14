---
title: Azure Güvenlik denetimi-olay yanıtı
description: Güvenlik denetimi olay yanıtı
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: fb3560aa2d3fbf48ab63c4da4d3a8d69cb677209
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75934411"
---
# <a name="security-control-incident-response"></a>Güvenlik denetimi: olay yanıtı

Bir saldırıyı hızlı bir şekilde bulmak için bir olay yanıt altyapısı (örneğin, planlar, tanımlı roller, eğitim, iletişim, yönetim gözetim) geliştirip uygulayarak kuruluşun bilgilerini ve bunların saymasını koruyun hasarı, saldırganın varlığını geri yükleme ve ağ ve sistemlerin bütünlüğünü geri yükleme gibi etkin bir şekilde.

## <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 10.1 | 19,1, 19,2, 19,3 | Müşteri |

Kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

Azure Güvenlik Merkezi 'nde Iş akışı Otomatiklamalar nasıl yapılandırılır:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın Anatomisi:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu de kullanabilir:

https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

## <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 10.2 | 19,8 | Müşteri |

Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

## <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 10,3 | 19 | Müşteri |

Sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin:

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

## <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 10,4 | 19,5 | Müşteri |

Microsoft Güvenlik Yanıt Merkezi (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

## <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 10.5 | 19,6 | Müşteri |

Sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

Sürekli dışarı aktarmayı yapılandırma:

https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel 'e aktarma:

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

## <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 10,6 | 19 | Müşteri |

Güvenlik uyarıları ve önerilerinde&quot; &quot;Logic Apps aracılığıyla yanıtları otomatik olarak tetiklemek için Azure Güvenlik Merkezi 'nde Iş akışı Otomasyonu özelliğini kullanın.

Iş akışı otomasyonunu yapılandırma ve Logic Apps:

https://docs.microsoft.com/azure/security-center/workflow-automation

## <a name="next-steps"></a>Sonraki adımlar

Sonraki güvenlik denetimine bakın: [sızma testleri ve kırmızı takım alıştırmaları](security-control-penetration-tests-red-team-exercises.md)