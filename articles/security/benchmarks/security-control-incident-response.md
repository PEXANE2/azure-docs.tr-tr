---
title: Azure Güvenlik Denetimi - Olay Yanıtı
description: Azure Güvenlik Denetimi Olay Yanıtı
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 993793d21e6253188dfc199d8701cbe117503517
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81408427"
---
# <a name="security-control-incident-response"></a>Güvenlik Kontrolü: Olay Yanıtı

Bir saldırıyı hızlı bir şekilde keşfetmek ve hasarı etkili bir şekilde kontrol etmek, saldırganın varlığını ortadan kaldırmak ve ağ ve sistemlerin bütünlüğünü geri kazanmak için bir olay müdahale altyapısı (örneğin, planlar, tanımlanmış roller, eğitim, iletişim, yönetim gözetimi) geliştirerek ve uygulayarak kuruluşun bilgilerini ve itibarını koruyun.

## <a name="101-create-an-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 10.1 | 19.1, 19.2, 19.3 | Müşteri |

Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Personelin tüm rollerini ve olay işleme/yönetiminin algılamadan olay sonrası incelemeye kadar olan aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.  

- [Kendi güvenlik olayı yanıt sürecinizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi'nin Bir Olayın Anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Kendi olay yanıt planınızın oluşturulmasına yardımcı olmak için NIST'nin Bilgisayar Güvenliği Olay İşlemkılavuzundan yararlanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

## <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Olay puanlama ve öncelik lendirme prosedürü oluşturma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 10.2 | 19.8 | Müşteri |

Güvenlik Merkezi, önce hangi uyarıların araştırılması gerektiğine öncelik vermenize yardımcı olmak için her uyarıya bir önem atar. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır. 

Ayrıca, abonelikleri açıkça işaretleyin (eski için. etiketleri kullanarak üretim, prod olmayan) ve Azure kaynaklarını, özellikle de hassas verileri işleyenleri net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.  Azure kaynaklarının ve olayın meydana geldiği ortamın kritikliğine bağlı olarak uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="103-test-security-response-procedures"></a>10.3: Test güvenlik yanıt yordamları

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 10.3 | 19 | Müşteri |

Azure kaynaklarınızın korunmasına yardımcı olmak için sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar gerçekleştirin. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin.

- [NIST yayını - BT Planları ve Yetenekleri için Test, Eğitim ve Egzersiz Programları Kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-84/final)

## <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Güvenlik olayı iletişim bilgilerini sağlayın ve güvenlik olayları için uyarı bildirimlerini yapılandırma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 10.4 | 19.5 | Müşteri |

Microsoft Güvenlik Yanıt Merkezi (MSRC), verilerinize yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır. Sorunların çözülmesini sağlamak için olaydan sonraki olayları gözden geçirin.

- [Azure Güvenlik Merkezi Güvenlik Kişisi nasıl ayarlanır?](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

## <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Olay yanıt sisteminize güvenlik uyarıları dahil edin

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 10,5 | 19.6 | Müşteri |

Azure kaynaklarına yönelik riskleri belirlemeye yardımcı olmak için Sürekli Dış Aktarım özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışa aktarın. Sürekli Dışa Aktarma, uyarı ve önerileri el ile veya sürekli olarak dışa aktarmanıza olanak tanır. Uyarıları Azure Sentinel'e aktarmak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

- [Sürekli dışa aktarma nasıl yapılandırılır?](https://docs.microsoft.com/azure/security-center/continuous-export)

- [Uyarıları Azure Sentinel'e akışı](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

## <a name="106-automate-the-response-to-security-alerts"></a>10.6: Güvenlik uyarılarına yanıtı otomatikleştirin

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 10.6 | 19 | Müşteri |

Azure kaynaklarınızı korumak için güvenlik uyarıları ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek için Azure Güvenlik Merkezi'ndeki İş Akışı Otomasyonu özelliğini kullanın.

- [İş Akışı Otomasyonu ve Mantık Uygulamaları nasıl yapılandırılabilen](https://docs.microsoft.com/azure/security-center/workflow-automation)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki Güvenlik Kontrolüne Bakın: [Penetrasyon Testleri ve Kırmızı Takım Egzersizleri](security-control-penetration-tests-red-team-exercises.md)