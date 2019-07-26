---
title: Azure Active Directory Kimlik Koruması bildirimleri | Microsoft Docs
description: Bildirimlerin araştırma etkinliklerinizi nasıl desteklediğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3dcc7d2d03b3c4a3f4c8c772f38aa3785080d986
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335355"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Kimlik Koruması bildirimleri

Azure AD Kimlik Koruması, Kullanıcı riskini ve risk olaylarını yönetmenize yardımcı olmak üzere iki tür otomatik bildirim e-postası gönderir:

- Risk altındaki kullanıcılar e-posta algıladı
- Haftalık Özet e-postası

Bu makalede her iki bildirim e-postası için bir genel bakış sunulmaktadır.

## <a name="users-at-risk-detected-email"></a>Risk altındaki kullanıcılar e-posta algıladı

Risk altında algılanan bir hesaba yanıt olarak Azure AD Kimlik Koruması, **risk altındaki kullanıcılar** konu olarak algılanan bir e-posta uyarısı oluşturur. E-posta, **[risk için Işaretlenmiş kullanıcılar](../reports-monitoring/concept-user-at-risk.md)** raporu için bir bağlantı içerir. En iyi uygulama olarak, risk altındaki kullanıcıları hemen araştırmanız gerekir.

Bu uyarının yapılandırması, uyarının oluşturulmasını istediğiniz Kullanıcı risk düzeyini belirtmenize olanak tanır. Bu e-posta, kullanıcının risk düzeyi belirtdiklerinize ulaştığında oluşturulacaktır; Bununla birlikte, risk bu Kullanıcı risk düzeyine geçtikten sonra bu kullanıcı için e-posta uyarılarını tespit eden yeni kullanıcıları almazsınız. Örneğin, ilkeyi orta Kullanıcı riski üzerine uyarı olarak ayarlarsanız ve Kullanıcı John, orta riske geçerse, kullanıcılardan John için algılanan risk için e-posta olduğunu görürsünüz. Ancak, John daha sonra yüksek riske geçerse veya ek risk olayları varsa, risk tespit eden ikinci bir Kullanıcı almazsınız.

![Risk altındaki kullanıcılar e-posta algıladı](./media/notifications/01.png)

### <a name="configuration"></a>Yapılandırma

Yönetici olarak şunları yapabilirsiniz:

- **Bu e-postanın oluşturulmasını tetikleyen Kullanıcı risk düzeyi** -varsayılan olarak, risk düzeyi "yüksek" riskli olarak ayarlanır.
- **Bu e-postanın alıcıları** varsayılan olarak tüm genel yöneticileri içerir. Genel Yöneticiler, diğer genel Yöneticiler, güvenlik yöneticileri, güvenlik okuyucularını da alıcı olarak ekleyebilir.  

İlgili iletişim kutusunu açmak için **kimlik koruması** sayfasının **Ayarlar** bölümünde **Uyarılar** ' a tıklayın.

![Risk altındaki kullanıcılar e-posta algıladı](./media/notifications/05.png)

## <a name="weekly-digest-email"></a>Haftalık Özet e-postası

Haftalık Özet e-postası yeni risk olaylarının bir özetini içerir.  
Şunları içerir:

- Risk altındaki kullanıcılar
- Şüpheli etkinlikler
- Algılanan güvenlik açıkları
- Kimlik koruması 'nda ilgili raporların bağlantıları

    ![Düzeltme](./media/notifications/400.png "Düzeltme")

### <a name="configuration"></a>Yapılandırma

Yönetici olarak, haftalık bir Özet e-postası gönderilmesini kapatabilirsiniz.

![Kullanıcı riskleri](./media/notifications/62.png "Kullanıcı riskleri")

İlgili iletişim kutusunu açmak için **kimlik koruması** sayfasının **Ayarlar** bölümünde **Haftalık Özet** ' e tıklayın.

![Risk altındaki kullanıcılar e-posta algıladı](./media/notifications/04.png)

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
