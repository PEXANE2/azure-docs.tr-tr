---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 537c86512db4706077d0089a4f71ee945623c26b
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081546"
---
### <a name="configure-your-notification-hub-with-apns-information"></a>APNS bilgileriyle bildirim hub’ınızı yapılandırma

**Bildirim hizmetleri**altında, **Apple** ' ı seçin ve ardından [Notification Hubs için sertifika oluşturma](#creating-a-certificate-for-notification-hubs) bölümünde daha önce seçtiğiniz yaklaşıma göre uygun adımları izleyin.  

> [!NOTE]
> Yalnızca uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek istiyorsanız **uygulama modu** için **üretimi** kullanın.

### <a name="option-1-using-a-p12-push-certificate"></a>SEÇENEK 1:. p12 anında iletme sertifikası kullanma

1. **Sertifika**’yı seçin.

1. Dosya simgesini seçin.

1. Daha önce verdiğiniz. P12 dosyasını seçin ve sonra **Aç**' ı seçin.

1. Gerekirse, doğru parolayı belirtin.

1. **Korumalı alan** modunu seçin.

1. **Kaydet**’i seçin.

### <a name="option-2-using-token-based-authentication"></a>Seçenek 2: belirteç tabanlı kimlik doğrulaması kullanma

1. **Belirteç**seçin.
1. Daha önce aldığınız aşağıdaki değerleri girin:

    - **Anahtar KIMLIĞI**
    - **Paket Kimliği**
    - **Takım KIMLIĞI**
    - **Belirteç**

1. **Korumalı alan**öğesini seçin.
1. **Kaydet**’i seçin.
