---
title: Azure Güvenlik denetimi-olay yanıtı
description: Azure Güvenlik denetimi olay yanıtı
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 993793d21e6253188dfc199d8701cbe117503517
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81408427"
---
# <a name="security-control-incident-response"></a>Güvenlik denetimi: olay yanıtı

Bir saldırıyı hızlı bir şekilde bulmak için bir olay yanıt altyapısı (örneğin, planlar, tanımlı roller, eğitim, iletişim, yönetim fazla gözetim) geliştirip uygulayarak ve ağ ve sistemlerin bütünlüğünü geri yükleyerek, kuruluşun bilgilerini ve bunun saygınlığını koruyun.

## <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 10.1 | 19,1, 19,2, 19,3 | Müşteri |

Kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.  

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Kendi olay yanıtı planınızı oluşturmaya yardımcı olmak için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzuyla yararlanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

## <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 10.2 | 19,8 | Müşteri |

Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır. 

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) etiketleri kullanarak Azure kaynaklarını açıkça tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun.  Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 10,3 | 19 | Müşteri |

Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıtı yeteneklerini düzenli bir temposunda test etmek üzere alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

- [NıST 'nin Yayımlama Kılavuzu, BT planları ve becerileri için programları test etme, eğitim ve alıştırma Kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-84/final)

## <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 10,4 | 19,5 | Müşteri |

Microsoft Güvenlik Yanıt Merkezi (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

## <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 10,5 | 19,6 | Müşteri |

Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

- [Sürekli dışarı aktarmayı yapılandırma](https://docs.microsoft.com/azure/security-center/continuous-export)

- [Uyarıları Azure Sentinel 'e akış](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

## <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 10,6 | 19 | Müşteri |

Azure Güvenlik Merkezi 'nde Iş akışı Otomasyonu özelliğini kullanarak, güvenlik uyarılarındaki "Logic Apps" aracılığıyla yanıtları otomatik olarak tetikleyin ve Azure kaynaklarınızı korumaya yönelik öneriler alın.

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](https://docs.microsoft.com/azure/security-center/workflow-automation)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki güvenlik denetimine bakın: [sızma testleri ve kırmızı takım alıştırmaları](security-control-penetration-tests-red-team-exercises.md)