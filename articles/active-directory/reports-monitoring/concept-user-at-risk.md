---
title: Azure Active Directory portalında risk güvenliği raporu için işaretlenmiş kullanıcılar | Microsoft Docs
description: Azure Active Directory portalında risk güvenliği için işaretlenmiş kullanıcılar hakkında bilgi edinin
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 01/17/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 894d8dfb7f870ec4a2a11f1d75ee0376b25d8c7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74014460"
---
# <a name="users-flagged-for-risk-report-in-the-azure-portal"></a>Azure portalında risk raporu için işaretlenmiş kullanıcılar

Azure Etkin Dizin (Azure AD), kullanıcı hesaplarınızla ilgili şüpheli eylemleri algılar. Algılanan her eylem için [risk algılama](concept-risk-events.md) adı verilen bir kayıt oluşturulur.

**Azure Active Directory** bıçağını seçip **Güvenlik** bölümüne giderek [Güvenlik portalından](https://portal.azure.com) güvenlik raporlarına erişebilirsiniz. 

Algılanan risk algılamaları hesaplamak için kullanılır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. 

- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

Bu risk algılamalarını tetikleyen ilkeleri nasıl yapılandırılabildiğini öğrenmek [için, kullanıcı risk ilkesini nasıl yapılandırılatırınız.](../identity-protection/howto-user-risk-policy.md) 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/10.png)


## <a name="what-azure-ad-license-do-you-need-to-access-the-users-at-risk-report"></a>Risk altındaki kullanıcılara erişmek için hangi Azure REKLAM lisansına ihtiyacınız vardır?  

Azure Active Directory'nin tüm sürümlerinde size riskli oldukları belirlenen kullanıcılar ve risk raporları sağlanır. Bununla birlikte, rapordaki ayrıntı düzeyi sürümler arasında değişiklik gösterir: 

- Azure **Etkin Dizin Ii ve Temel sürümlerinde,** risk için işaretlenmiş kullanıcıların listesini alırsınız. 

- Ayrıca, **Azure Active Directory Premium 1** sürümü, her rapor için algılanan temel risk algılamalarından bazılarını incelemenize olanak tanır. 

- **Azure Active Directory Premium 2** sürümü, temel risk algılamaları hakkında en ayrıntılı bilgileri sağlar ve ayrıca yapılandırılmış risk düzeylerine otomatik olarak yanıt veren güvenlik ilkelerini yapılandırmanızı sağlar.


## <a name="users-at-risk-report-for-azure-ad-free-and-basic-editions"></a>Azure AD ücretsiz ve temel sürümleri için risk altındaki kullanıcılar raporu

Azure AD ücretsiz ve temel sürümlerinde risk raporu için işaretlenmiş kullanıcılar, size gizliliği ihlal edilmiş olabilecek kullanıcı hesaplarının bir listesini sağlar. 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/03.png)

Bir kullanıcı seçmek oturum açma bilgileri sağlar. Risk altındaki kullanıcılarla ilgili olarak kullanıcının oturum açma geçmişini gözden geçirebilir ve gerekirse parolasını sıfırlayabilirsiniz.

Bu iletişim kutusu size şu seçeneği sunar:

- Raporu indirme
- Kullanıcılarda arama

    ![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/16.png)

Daha ayrıntılı bilgi için premium lisansa ihtiyacınız var.

## <a name="users-at-risk-report-for-azure-ad-premium-editions"></a>Azure AD premium sürümleri için risk altındaki kullanıcılar rapor

Azure AD premium sürümlerinde risk raporu için işaretlenen kullanıcılar size şunları sağlar:

- Tehlikeye girmiş olabilecek kullanıcı hesaplarının listesi 

- Algılanan [risk algılama türleri](concept-risk-events.md) hakkında toplu bilgiler

- Raporu indirme seçeneği

- [Kullanıcı riskini azaltma ilkesi](../identity-protection/howto-user-risk-policy.md) yapılandırma seçeneği  

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/71.png)

Bir kullanıcıyı seçtiğinizde bu kullanıcıya ilişkin, aşağıdakileri gerçekleştirmenize olanak tanıyan ayrıntılı bir rapor görünümü açılır:

- Tüm oturum açma işlemleri görünümünü açabilirsiniz.

- Kullanıcının parolasını sıfırlayabilirsiniz.

- Tüm olayları kapatabilirsiniz.

- Kullanıcı için bildirilen risk algılamalarını araştırın. 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/324.png)

Bir risk tespitini araştırmak için, bu risk tespiti için **Ayrıntılar** bıçağını açmak için listeden birini seçin. **Ayrıntılar** bıçağında, bir risk algılamayı el ile kapatma veya el ile kapatılan bir risk algılamayı yeniden etkinleştirme seçeneğiniz bulunmaktadır. 

![Riskli Oturum Açma İşlemleri](./media/concept-user-at-risk/325.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcı risk ilkesi nasıl yapılandırılır?](../identity-protection/howto-user-risk-policy.md)
- [Risk düzeltme ilkesi nasıl yapılandırılır?](../identity-protection/howto-user-risk-policy.md)
- [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)

