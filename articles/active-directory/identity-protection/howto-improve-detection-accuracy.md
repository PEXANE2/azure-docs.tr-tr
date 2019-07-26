---
title: Azure Active Directory Kimlik Koruması (YENİLENDİ) ' de algılama doğruluğunu geliştirme | Microsoft Docs
description: Azure Active Directory Kimlik Koruması (YENİLENDİ) ' de algılama doğruluğunu geliştirme.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32bb8de7970fc167a6a95e9d9c3c71e4e1dc0150
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333944"
---
# <a name="how-to-improve-the-detection-accuracy"></a>Nasıl Yapılır: Algılama doğruluğunu geliştirme 

Kimlik koruması, ortamınızdaki risk algılamaları hakkında Azure AD 'ye geri bildirim verme mekanizmaları sağlar. Geri bildirim sağlamak için, algılanan riskli Kullanıcı veya oturum açma olayının durumunu doğrulayabilirsiniz. Microsoft, bu geri bildirimde bulunmak için geçerli risk algılamalarını ve gelecekteki algılamaların doğruluğunu geliştirir. 

## <a name="what-is-detection"></a>Algılama nedir?

Algılama, şüpheli etkinlikleri kullanıcı hesaplarınızla birlikte tanımlama işlemidir. Azure AD 'nin algılayabildiği şüpheli etkinlikler [risk olayı](../reports-monitoring/concept-risk-events.md)olarak adlandırılır. Algılama işlemi, uyarlamalı makine öğrenimi algoritmalarını ve kullanıcılara yönelik risk olaylarını algılamak için buluşsal yöntemler ' i temel alır.

Algılama sonuçları, kullanıcıların ve oturum açma işlemlerinin risk altında olup olmadığını saptamak için kullanılır. 

## <a name="how-can-i-improve-the-detection-accuracy"></a>Algılama doğruluğunu nasıl geliştirebilirim?

Algılama otomatik bir işlem olduğundan, Azure AD 'nin hatalı pozitif sonuçlar raporlamasında olması mümkündür. Algılama sonuçlarıyla ilgili olarak Azure AD 'ye geri bildirim sağlayarak algılama doğruluğunu geliştirebilirsiniz.

Algılama doğruluğunu geliştirmenin üç yolu vardır: güvenliği aşılmış oturum açma işlemini onaylayın, güvenli oturum açma işlemini onaylayın ve Kullanıcı riskini kapatın. Bunu aşağıdaki raporlardan yapabilirsiniz:

- **Riskli oturum açma işlemleri raporu-** Riskli oturum açma işlemleri raporunda, oturum açma işlemlerinin güvenli veya güvenliği aşılmış olduğunu doğrulayabilirsiniz
- **Riskli kullanıcılar raporu-** Riskli kullanıcılar raporunda, Kullanıcı riskini ortadan kaldırabilirsiniz 

Geri bildiriminiz, algılama sonuçlarının doğruluğunu artırmak için Azure AD tarafından işlenir. Genellikle, bir Kullanıcı riski veya oturum açma riski araştırmasının parçası olarak geri bildirim sağlarsınız. Daha fazla bilgi için bkz. [riskli kullanıcıları ve oturum açma işlemlerini araştırma](howto-investigate-risky-users-signins.md).

## <a name="confirm-compromised"></a>Güvenliğin tehlikeye girdiğini onaylayın

Azure AD 'ye, oturum açma işleminin kimlik sahibi tarafından yetkilendirilmediğini bildiren bir oturum açma olayını onaylama. "Tehlikeyi Onayla" yı seçtiğinizde Azure AD,

- Etkilenen kullanıcının Kullanıcı riskini yüksek olarak artırın.
- Risk olaylarını algılayan makine öğrenimini iyileştirmek için yardım
- Kuruluşunuzu daha fazla korumak için ek ölçüler gerçekleştirin

Güvenliği aşılmış bir oturum açma doğrulamak için:

- **Riskli oturum açma işlemleri raporu** -Bu seçenek, bir veya daha fazla oturum açma olayı için güvenliği aşılmış bir oturum açmayı doğrulamanıza olanak sağlar.

   ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/07.png)

- **Riskli oturum açma işlemleri raporunun Ayrıntılar görünümü** -Bu seçenek, riskli oturum açma işlemleri raporundaki seçili oturum açma etkinliği için güvenliği aşılmış bir hesabı doğrulamanıza olanak sağlar. 

   ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/04.png)
 
## <a name="confirm-safe"></a>Güvenli olduğunu onayla

Oturum açma olayını, Azure AD 'ye, oturum açma 'nın ilgili kimlik sahibi tarafından yetkilendirildiği  güvenli sinyaller olarak onaylama. "Güvenli Onayla" yı seçtiğinizde Azure AD şu şekilde olur:

- Seçilen oturum açma işlemlerinin Kullanıcı risk katkısını döndürür
- Temeldeki risk olaylarını kapatma
- Risk olaylarını algılayan makine öğrenimini iyileştirmek için yardım
- Kuruluşunuzu daha fazla korumak için ek ölçüler gerçekleştirin
 
Uygulamasında güvenli bir oturum açma olduğunu doğrulamak için:

- **Riskli oturum açma işlemleri raporu** -Bu seçenek, bir veya daha fazla oturum açma olayı için güvenli bir oturum açmayı doğrulamanıza olanak sağlar.

   ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/08.png)

- **Riskli oturum açma işlemleri raporunun Ayrıntılar görünümü** -Bu seçenek, riskli oturum açma işlemleri raporunda, seçilen oturum açma etkinliği için güvenli bir oturum açmayı doğrulamanıza olanak sağlar. 

   ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/05.png)

## <a name="dismiss-user-risk"></a>Kullanıcı riskini kapat

Bir risk kullanıcısına yönelik düzeltme eylemleri zaten gerçekleştirdiyse veya özellikle riskli olarak işaretlendiğine inanıyorsanız, bir kullanıcının riskini ortadan kaldırabilirsiniz. Bir kullanıcının riski yok, kullanıcıyı riskli olmayan bir duruma geri yükler. Seçilen Kullanıcı için geçmişteki tüm riskli oturum açma işlemleri ve risk olayları kapatılır.

Bildirilen Kullanıcı riskini şu şekilde yok sayabilirsiniz:

- **Riskli kullanıcılar raporu** -Bu seçenek, seçilen bir veya daha fazla kullanıcının Kullanıcı riskini ortadan seçmenize olanak sağlar.

   ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/02.png)

- **Ayrıntılar görünümü** -Bu seçenek, Kullanıcı risk raporundaki seçili kullanıcı için Kullanıcı riskini ortadan seçmenize olanak sağlar. 

   ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/01.png)

**Bilmeniz gerekenler:**

- Bu eylemi döndüremezsiniz.
- Bu eylemin tamamlanması birkaç dakika sürebilir, bu nedenle isteğinizi yeniden gönderememelisiniz.
- Bu eylemi yalnızca Kullanıcı kimlik bilgilerini AD yönettiğinde gerçekleştirebilirsiniz. 

## <a name="best-practices"></a>En iyi uygulamalar

Kullanıcının riskini ortadan kaldırmak, Kullanıcı risk ilkesi tarafından engelleniyorsa ve parola sıfırlama ve/veya MFA 'nın etkin olmaması nedeniyle kendini düzeltemedi. Bu durumda, kullanıcının gelecekteki risk olaylarını kendiliğinden düzeltebilmeleri için parola sıfırlama ve MFA 'ya kaydolmasını sağlamak en iyisidir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Kimlik Koruması genel bakış almak için bkz. [Azure AD kimlik koruması genel bakış](overview-v2.md).
