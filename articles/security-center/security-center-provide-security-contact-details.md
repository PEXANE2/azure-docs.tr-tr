---
title: Azure Güvenlik Merkezi 'nde güvenlik iletişim bilgilerini sağlama | Microsoft Docs
description: Bu belgede, Azure Güvenlik Merkezi 'nde güvenlik iletişim ayrıntılarını sağlama gösterilmektedir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2019
ms.author: memildin
ms.openlocfilehash: 08ad761e81909e6ab23c7c07f5ce05865136bc47
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82204109"
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde güvenlik iletişim bilgilerini sağlama
Azure Güvenlik Merkezi, henüz yapmadıysanız Azure aboneliğiniz için güvenlik kişi ayrıntılarını sağlamanızı önerir. Bu bilgiler, Microsoft Güvenlik Yanıt Merkezi (MSRC) müşteri verilerinize yasadışı veya yetkisiz bir tarafın eriştiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. MSRC Azure ağı ve altyapısında seçmeli güvenlik izlemesi gerçekleştirir ve üçüncü taraflardan tehdit bilgilerini ve uygunsuz kullanım şikayetlerini alır.

Bir uyarının gün içinde ilk kez oluşması durumunda ve yalnızca yüksek önem düzeyindeki uyarılar için bir e-posta bildirimi gönderilir. E-posta tercihleri yalnızca abonelik ilkeleri için yapılandırılabilir. Bir aboneliğin altındaki kaynak grupları bu ayarları devralır. Uyarılar yalnızca Azure Güvenlik Merkezi 'nin standart katmanında kullanılabilir.

Uyarı e-posta bildirimleri şu durumlarda gönderilir:
- Uyarı türüne göre günde yalnızca tek bir e-posta alıcısına  
- Tek bir günde tek bir alıcıya 3 ' ten fazla e-posta iletisi gönderilmez
- Her e-posta iletisinde tek bir uyarı bulunur, uyarılar toplu gönderilmez
- Yalnızca yüksek önem derecesindeki uyarılar için

> [!TIP]
> Diğer önem düzeylerine sahip uyarılar için, ilgili personele e-posta gönderecek bir mantıksal uygulama kullanmak üzere bir [iş akışı Otomasyonu](workflow-automation.md) oluşturun.
 
Örneğin bir RDP saldırısı konusunda uyarmak için bir e-posta iletisi gönderildiyse uyarı tetiklense dahi aynı gün içinde RDP saldırısıyla ilgili başka bir e-posta iletisi gönderilmez. 

> [!IMPORTANT]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.

## <a name="set-up-email-notifications-for-alerts"></a>Uyarılar için e-posta bildirimleri ayarlama<a name="email"></a>

1. Rol güvenlik yöneticisi veya abonelik sahibine sahip bir kullanıcı olarak, **e-posta bildirimleri** sayfasını açın:

    - Uyarılar için **fiyatlandırma & ayarları**' nı açın, ilgili aboneliği seçin ve **e-posta bildirimleri**' ni seçin.

    - Öneri uygulamadıysanız, **öneriler**altında **güvenlik iletişim bilgileri sağla**' yı seçin, Iletişim bilgilerini sağlamak için Azure aboneliğini seçin. Bu, **e-posta bildirimlerini**açar.

   ![Güvenlik ilgili kişi bilgilerini belirtin][2]

1. Güvenlik ilgili kişisinin e-posta adresini veya adreslerini virgülle ayırarak girin. Girebilmeniz gereken e-posta adresi sayısı için bir sınır yoktur.

1. Yüksek önem derecesine sahip uyarılar hakkında e-posta almak için **bana uyarılar hakkında e-posta gönder**seçeneğini açın. Diğer önem düzeyleri için, [iş akışı Otomasyonu](workflow-automation.md)'nda açıklandığı şekilde bir mantıksal uygulama kullanın.

1. Abonelik sahiplerine (klasik hizmet yöneticisi ve ortak yöneticiler ve abonelik kapsamındaki RBAC sahibi rolü) e-posta bildirimleri gönderebilirsiniz.

1. Güvenlik iletişim bilgilerini aboneliğinize uygulamak için **Kaydet**' i seçin.

## <a name="see-also"></a>Ayrıca bkz.
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
* [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) --güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) -iş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
