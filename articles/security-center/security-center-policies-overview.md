---
title: Azure Güvenlik Merkezi ayarları | Microsoft Docs
description: Azure Güvenlik Merkezi ayarlarını yapılandırın.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/03/2018
ms.author: memildin
ms.openlocfilehash: 4a7254d4ac67ee7d1bf203baf5741638dbc8f3dd
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71201618"
---
# <a name="security-center-settings"></a>Güvenlik Merkezi ayarları
Bu makalede Güvenlik Merkezi 'ndeki ayarlara bir genel bakış sunulmaktadır.

Aşağıdaki ayarlara Güvenlik Ilkesi altında ulaşılamıyor:

- **Veri toplama**: Aracı sağlamayı ve [veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) ayarlarını belirler.
- **Güvenlik ilkesi**: Güvenlik Merkezi 'nin hangi denetimlerin izlediğini ve önerdiğini belirler. Güvenlik Merkezi 'nde [güvenlik ilkesini](tutorial-security-policy.md) düzenleyebilirsiniz. [Azure ilkesini](tutorial-security-policy.md) kullanarak yeni tanımlar oluşturabilir, ek ilkeler tanımlayabilir ve yönetim grupları arasında ilkeler atayabilirsiniz. 
- **E-posta bildirimleri**: Güvenlik kişilerini ve [e-posta bildirimi](security-center-provide-security-contact-details.md) ayarlarını belirler.
- **Fiyatlandırma katmanı**: Ücretsiz veya standart [fiyatlandırma seçimini](security-center-pricing.md)tanımlar. Seçtiğiniz katman, kapsam dahilindeki kaynaklar için hangi Güvenlik Merkezi özelliklerinin kullanılabilir olduğunu belirler. Abonelikler ve çalışma alanları için bir katman belirtebilirsiniz.

> [!NOTE]
> Her abonelik için bu kadar ayarlayabilirsiniz. Çalışma alanları için yalnızca veri toplama ve fiyatlandırma katmanı ayarlayabilirsiniz.
>


## <a name="who-can-edit-security-policies"></a>Güvenlik ilkelerini kimler düzenleyebilir?
Güvenlik Merkezi, Azure 'daki kullanıcılara, gruplara ve hizmetlere atanabilen yerleşik roller sağlayan rol tabanlı Access Control (RBAC) kullanır. Kullanıcılar Güvenlik Merkezi 'ni açtıklarında yalnızca erişimi olan kaynaklarla ilgili bilgileri görürler. Bu, kullanıcılara kaynağın ait olduğu aboneliğe *sahip*, *katkıda*bulunan veya *okuyucu* rolünün atandığı anlamına gelir. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- **Güvenlik okuyucusu**: Güvenlik Merkezi 'Nde öneriler, uyarılar, ilke ve sistem durumunu içeren görünüm haklarına sahip olmakla kalmaz, ancak değişiklik yapamaz.
- **Güvenlik Yöneticisi**: *Güvenlik okuyucusu*ile aynı görünüm haklarına sahip olmak ve ayrıca güvenlik ilkesini güncelleştirebilir ve öneriler ile uyarıları kapatabilir.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi 'nde güvenlik ilkeleri hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md): Azure abonelikleriniz için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md): Güvenlik Merkezi önerilerinin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md): Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md): Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını öğrenin.
* [Azure Güvenlik MERKEZI SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): En son Azure Güvenlik haberlerini ve bilgilerini alın.
