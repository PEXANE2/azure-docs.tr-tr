---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 7c3887367b6365ea3577bbff6cc19bf34d178ee6
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081552"
---
### <a name="create-a-notification-hub"></a>Bildirim hub’ı oluşturma 

Bu bölümde, bir Bildirim Hub 'ı oluşturur ve **APNs**ile kimlik doğrulaması yapılandırırsınız. Bir P12 Push Sertifikası veya belirteç tabanlı kimlik doğrulaması kullanabilirsiniz. Zaten oluşturduğunuz bir Bildirim Hub 'ı kullanmak istiyorsanız, 5. adıma geçebilirsiniz.

1. [Azure](https://portal.azure.com)'da oturum açın.

1. **Kaynak oluştur ' a**tıklayın, ardından **Bildirim Hub 'ını**arayıp seçin ve ardından **Oluştur**' a tıklayın.

1. Aşağıdaki alanları güncelleştirin ve ardından **Oluştur**' a tıklayın:

    **TEMEL AYRıNTıLAR**  

    **Abonelik:** Açılan listeden hedef **aboneliği** seçin  
    **Kaynak grubu:** Yeni bir **kaynak grubu** oluştur (veya mevcut olanı seç)  

    **AD ALANı AYRıNTıLARı**  

    **Bildirim Hub 'ı ad alanı:** **Bildirim Hub 'ı** ad alanı için genel olarak benzersiz bir ad girin  

    > [!NOTE]
    > Bu alan için **Yeni oluştur** seçeneğinin seçildiğinden emin olun.

    **BILDIRIM HUB 'ı AYRıNTıLARı**  

    **Bildirim Hub 'ı:** **Bildirim Hub 'ı** için bir ad girin  
    **Konum:** Açılan listeden uygun bir konum seçin  
    **Fiyatlandırma katmanı:** Varsayılan **ücretsiz** seçeneğini tut  

    > [!NOTE]
    > Ücretsiz katmanda maksimum hub sayısına Ulaşmadığınız müddetçe.

1. **Bildirim Hub 'ı** sağlandıktan sonra, bu kaynağa gidin.
1. Yeni **Bildirim Hub 'ınıza**gidin.
1. Listeden **erişim ilkeleri** ' ni seçin ( **Yönet**altında).
1. **Ilke adı** değerlerini karşılık gelen **bağlantı dizesi** değerleriyle birlikte unutmayın.
