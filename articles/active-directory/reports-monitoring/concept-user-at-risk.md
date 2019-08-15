---
title: Azure Active Directory portalında risk güvenliği raporu için işaretlenmiş kullanıcılar | Microsoft Docs
description: Azure Active Directory portalında risk güvenliği için işaretlenmiş kullanıcılar hakkında bilgi edinin
services: active-directory
author: cawrites
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 01/17/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 950d0dc2f4a1a88e0c317b3398c7492cc6c52e2a
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68989831"
---
# <a name="users-flagged-for-risk-report-in-the-azure-portal"></a>Azure portal risk için işaretlenen kullanıcılar raporu

Azure Active Directory (Azure AD), kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılar. Algılanan her eylem için, [risk olayı](concept-risk-events.md) adlı bir kayıt oluşturulur.

[Azure portal](https://portal.azure.com) **Azure Active Directory** dikey penceresini seçip **güvenlik** bölümüne giderek güvenlik raporlarına erişebilirsiniz. 

Algılanan risk olayları aşağıdakileri hesaplamak için kullanılır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. 

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

Bu risk olaylarını tetikleyen ilkelerin nasıl yapılandırılacağını öğrenmek için bkz. [Kullanıcı risk ilkesini yapılandırma](../identity-protection/howto-user-risk-policy.md). 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/10.png)


## <a name="what-azure-ad-license-do-you-need-to-access-the-users-at-risk-report"></a>Risk altındaki kullanıcılara erişmek için hangi Azure AD lisansının olması gerekir?  

Azure Active Directory'nin tüm sürümlerinde size riskli oldukları belirlenen kullanıcılar ve risk raporları sağlanır. Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- **Azure Active Directory ücretsiz ve temel sürümlerde**, risk için işaretlenen kullanıcıların bir listesini alırsınız. 

- Ayrıca, **Azure Active Directory Premium 1** sürümü, her rapor için algılanan temeldeki bazı risk olaylarını incelemenizi sağlar. 

- **Azure Active Directory Premium 2** sürümü, tüm temel risk olayları hakkında en ayrıntılı bilgileri sağlar ve ayrıca, yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkeleri yapılandırmanıza da olanak tanır.


## <a name="users-at-risk-report-for-azure-ad-free-and-basic-editions"></a>Azure AD ücretsiz ve temel sürümleri için risk altındaki kullanıcılar raporu

Azure AD ücretsiz ve temel sürümlerinde risk için işaretlenmiş kullanıcılar raporu, tehlikeye girmiş olabilecek Kullanıcı hesaplarının bir listesini sağlar. 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/03.png)

Bir kullanıcının seçilmesi, oturum açma bilgilerini sağlar. Risk altındaki kullanıcılarla ilgili olarak kullanıcının oturum açma geçmişini gözden geçirebilir ve gerekirse parolasını sıfırlayabilirsiniz.

Bu iletişim kutusu size şu seçeneği sunar:

- Raporu indirme
- Kullanıcı ara

    ![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/16.png)

Daha ayrıntılı bilgi için Premium lisansa sahip olmanız gerekir.

## <a name="users-at-risk-report-for-azure-ad-premium-editions"></a>Azure AD Premium sürümleri için risk altındaki kullanıcılar raporu

Azure AD Premium sürümlerinde risk için işaretlenmiş kullanıcılar raporu şunları sağlar:

- Tehlikeye girmiş olabilecek kullanıcı hesaplarının listesi 

- Algılanan [risk olayı türleri](concept-risk-events.md) hakkında toplu bilgiler

- Raporu indirme seçeneği

- [Kullanıcı riskini azaltma ilkesi](../identity-protection/howto-user-risk-policy.md) yapılandırma seçeneği  

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/71.png)

Bir kullanıcıyı seçtiğinizde bu kullanıcıya ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Tüm oturum açma işlemleri görünümünü açabilirsiniz.

- Kullanıcının parolasını sıfırlayabilirsiniz.

- Tüm olayları kapatabilirsiniz.

- Kullanıcıya ilişkin bildirilmiş risk olaylarını araştırabilirsiniz. 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/324.png)

Bir risk olayını araştırmak için listeden bir olay seçerek bu olayın **Ayrıntılar** dikey penceresini açın. **Ayrıntılar** dikey penceresinde, risk olayını elle kapatma ve elle kapatılmış risk olayını yeniden etkinleştirme seçenekleri sunulur. 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/325.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı risk ilkesini yapılandırma](../identity-protection/howto-user-risk-policy.md)
- [Risk düzeltme ilkesini yapılandırma](../identity-protection/howto-user-risk-policy.md)
- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)

