---
title: Azure Güvenlik Merkezi uyarıları için e-posta bildirimlerini yapılandırma
description: Güvenlik uyarıları için Azure Güvenlik Merkezi tarafından gönderilen e-posta türlerinin nasıl ince ayarlanacağını öğrenin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2020
ms.author: memildin
ms.openlocfilehash: 72ded01b141aafb7fd3e4d761882a10eaf0c4b33
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98920418"
---
# <a name="configure-email-notifications-for-security-alerts"></a>Güvenlik uyarıları için e-posta bildirimlerini yapılandırma 

Güvenlik uyarılarının kuruluşunuzdaki doğru kişilere ulaşması gerekir. Varsayılan olarak, Güvenlik Merkezi, abonelikleri için yüksek öneme sahip bir uyarı tetiklendiğinde her seferinde abonelik sahibini e-posta ile gönderir. Bu sayfada bu bildirimlerin nasıl özelleştirileceği açıklanır.

Bildirim e-postalarınız için kendi tercihlerinizi tanımlamak üzere Azure Güvenlik Merkezi 'nin **e-posta bildirimleri** ayarları sayfası şunları seçmenizi sağlar:

- **_Kime_ bildirilmesi gereken** kişiler-e-postalar, bir abonelik için belirli bir Azure rolüne sahip kişileri seçmek üzere gönderilebilir. 
- **_ne_ bildirilmesi gerekir?** Güvenlik Merkezi 'nin bildirim gönderebilmesi için önem düzeylerini değiştirin.

Uyarı fatıg ' y i önlemek için, güvenlik merkezi giden e-posta hacminin miktarını sınırlandırır. Güvenlik Merkezi, her abonelik için şunu gönderir:

- **yüksek öneme sahip** uyarılar için **6 saat** başına en fazla bir e-posta (günde 4 e-posta)
- **Orta öneme sahip** uyarılar için **12 saat** başına en fazla bir e-posta (günde 2 e-posta)
- **düşük önem derecesine** sahip uyarılar için **24 saat** başına en fazla bir e-posta

:::image type="content" source="./media/security-center-provide-security-contacts/email-notification-settings.png" alt-text="Güvenlik uyarıları hakkında e-posta alacak kişinin ayrıntılarını yapılandırma." :::
 
## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel kullanılabilirlik (GA)|
|Fiyat|Ücretsiz|
|Gerekli roller ve izinler:|**Güvenlik Yöneticisi**<br>**Abonelik sahibi** |
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||


## <a name="customize-the-security-alerts-email-notifications"></a>Güvenlik uyarıları e-posta bildirimlerini özelleştirme<a name="email"></a>

Kişilere veya belirli Azure rollerine sahip tüm kullanıcılara e-posta bildirimleri gönderebilirsiniz.

1. Güvenlik Merkezi 'nin **fiyatlandırma & ayarları** alanından ilgili aboneliği seçin ve **e-posta bildirimleri**' ni seçin.

1. Bildirimlerin alıcılarını, bu seçeneklerden birini veya her ikisini de tanımlayın:

    - Açılan listeden, kullanılabilir roller ' i seçin.
    - Virgülle ayrılmış belirli e-posta adreslerini girin. Girebilmeniz gereken e-posta adresi sayısı için bir sınır yoktur.

1. Güvenlik iletişim bilgilerini aboneliğinize uygulamak için **Kaydet**' i seçin.


## <a name="see-also"></a>Ayrıca bkz.
Güvenlik uyarıları hakkında daha fazla bilgi edinmek için aşağıdaki sayfalara bakın:

- [Güvenlik uyarıları-bir başvuru kılavuzu](alerts-reference.md)-Azure Güvenlik Merkezi 'Nin tehdit koruması modülünde görebileceğiniz güvenlik uyarılarını öğrenin
- [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)--güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin
- [Iş akışı Otomasyonu](workflow-automation.md)--özel bildirim mantığı ile uyarılara olan yanıtları otomatikleştirin