---
title: Azure Güvenlik Merkezi'nde güvenlik iletişim bilgileri sağlayın | Microsoft Dokümanlar
description: Bu belge, Azure Güvenlik Merkezi'nde güvenlik iletişim bilgilerini nasıl sağlayacağınızı gösterir.
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
ms.openlocfilehash: 64a9600a3014f7e85a7f924d38882bfadaf631db
ms.sourcegitcommit: d0fd35f4f0f3ec71159e9fb43fcd8e89d653f3f2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80387827"
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik iletişim bilgileri sağlama
Azure Güvenlik Merkezi, henüz yapmadıysanız Azure aboneliğiniz için güvenlik kişi ayrıntılarını sağlamanızı önerir. Bu bilgiler, Microsoft Güvenlik Yanıt Merkezi (MSRC) müşteri verilerinize yasadışı veya yetkisiz bir tarafın eriştiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. MSRC Azure ağı ve altyapısında seçmeli güvenlik izlemesi gerçekleştirir ve üçüncü taraflardan tehdit bilgilerini ve uygunsuz kullanım şikayetlerini alır.

Bir uyarının gün içinde ilk kez oluşması durumunda ve yalnızca yüksek önem düzeyindeki uyarılar için bir e-posta bildirimi gönderilir. E-posta tercihleri yalnızca abonelik ilkeleri için yapılandırılabilir. Bir aboneliğin altındaki kaynak grupları bu ayarları devralır. Uyarılar yalnızca Azure Güvenlik Merkezi'nin Standart katmanında kullanılabilir.

Uyarı e-posta bildirimleri şu durumlarda gönderilir:
- Uyarı türüne göre günde yalnızca tek bir e-posta alıcısına  
- Tek bir alıcıya bir günde en fazla 3 e-posta iletisi gönderilir
- Her e-posta iletisinde tek bir uyarı bulunur, uyarılar toplu gönderilmez
- Yalnızca yüksek önem derecesindeki uyarılar için

> [!TIP]
> Diğer önem düzeyi düzeylerine sahip uyarılar için, ilgili personele e-posta gönderecek bir Mantık Uygulaması kullanmak için bir [iş akışı otomasyonu](workflow-automation.md) oluşturun.
 
Örneğin bir RDP saldırısı konusunda uyarmak için bir e-posta iletisi gönderildiyse uyarı tetiklense dahi aynı gün içinde RDP saldırısıyla ilgili başka bir e-posta iletisi gönderilmez. 

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.

## <a name="set-up-email-notifications-for-alerts"></a>Uyarılar için e-posta bildirimleri ayarlama<a name="email"></a>

1. **E-posta bildirimleri** sayfasını açın:

    - Uyarılar için, **Fiyatlandırma & ayarları,** ilgili aboneliği seçin ve **E-posta bildirimlerini**seçin.

    - Bir öneri uyguluyorsanız, ardından **Öneriler**altında güvenlik **iletişim bilgilerini sağlayın'ı**seçin, iletişim bilgilerini sağlamak için Azure aboneliğini seçin. Bu **E-posta bildirimleri**açar.

   ![Güvenlik ilgili kişi bilgilerini belirtin][2]

1. Güvenlik kişisi e-posta adresini veya virgülle ayrılmış adresleri girin. Girebileceğiniz e-posta adreslerinin sayısı için bir sınır yoktur.

1. Yüksek önem uyarıları hakkında e-posta almak için, **uyarılar hakkında bana e-posta gönderme**seçeneğini açın. Diğer önem düzeyleri için iş akışı [otomasyonunda](workflow-automation.md)açıklandığı gibi bir Mantık Uygulaması kullanın.

1. Abonelik sahiplerine e-posta bildirimleri (klasik Servis Yöneticisi ve Yardımcı Yöneticiler ve abonelik kapsamında RBAC Sahibi rolü) gönderebilirsiniz.

1. Güvenlik iletişim bilgilerini aboneliğinize uygulamak için **Kaydet'i**seçin.

## <a name="see-also"></a>Ayrıca bkz.
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -- Önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumu izleme](security-center-monitoring.md) -- Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetme ve yanıtla](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını nasıl yönetip yanıtlaştak yapılacağını öğrenin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) -- İş ortağı çözümlarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
