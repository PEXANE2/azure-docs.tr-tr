---
title: Azure Active Directory portalında risk için işaretlenmiş kullanıcılar | Microsoft Dokümanlar
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
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 30d02c5484ea4cce2953eac6b1b7b26a17c142bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68989706"
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Azure Active Directory portalında riskli olarak işaretlenmiş kullanıcıları düzeltme

Azure Etkin Dizini'ndeki (Azure AD) güvenlik raporlarıyla, ortamınızdaki gizliliği ihlal edilen kullanıcı hesapları olasılığını ölçebilirsiniz. Riskli olduğu için işaretlenmiş kullanıcılar, güvenliği tehlikeye girmiş olabilecek kullanıcı hesaplarının göstergesidir.

Microsoft ortamınızın güvenliğini korumak için çalışmaktadır. Bu çalışma kapsamında, Microsoft alışılmadık veya bilinen saldırı düzenleriyle tutarlı etkinlikleri sürekli izler. 

Bazı kullanıcılarınızın hesaplarına yetkisiz erişim olduğunu gösteren olağandışı etkinlikler algılanırsa, işlem yapmanızı sağlayan bildirimler alırsınız. Bu, Microsoft'un kendi sistemlerinin tehlikeye atıldığı anlamına gelmez.

## <a name="access-the-users-flagged-for-risk-report"></a>Riskli olarak işaretlenen kullanıcılar raporuna erişme

Azure portalında [risk altındaki kullanıcılar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RiskyUsers) aracılığıyla risk için işaretlenen kullanıcıları inceleyebilirsiniz. Azure AD'niz yoksa, 'den [https://aka.ms/AccessAAD](https://aka.ms/AccessAAD)ücretsiz olarak kaydolabilirsiniz. 

Risk raporu için işaretlenen kullanıcılardan, her kullanıcı için aşağıdaki eylemleri yapabilirsiniz:

- Geçici parola oluşturma
- Kullanıcının bir sonraki defa oturum açtığında parolasını güvenli şekilde sıfırlamasını zorunlu tutma
- Herhangi bir düzeltme eylemi gerçekleştirmeden kullanıcı riskini kapatma.

Daha fazla bilgi için risk [güvenliği raporu için işaretlenen Kullanıcılar'a](concept-user-at-risk.md)bakın.

### <a name="azure-ad-subscription-for-office-365-customers"></a>Office 365 müşterileri için Azure AD aboneliği

**Azure Yönetici Merkezi'ne**erişmek için Office 365 kimlik bilgilerinizi de kullanabilirsiniz. Azure AD’ye erişiminizi etkinleştirdikten sonra, Azure AD portalına yeniden yönlendirilirsiniz. Temel abonelik düzeyinde, raporlarda sağlanan ayrıntı miktarı sınırlıdır. Ek veriler ve analizler, Azure Premium abonelerine sağlanır.

Microsoft 365 yönetici merkezinde risk raporları **için işaretlenen Kullanıcılara** erişmek için:

1.  Sol taraftaki gezinti menüsünden **Yönetici merkezlerini**seçin. 
2.  **Azure AD'yi**seçin.
3.  **Azure Active Directory yönetim merkezi**'nde oturum açın.
4.  Yeni **portala göz atın**yazan sayfanın üst kısmında bir başlık görüntüleniyorsa, bağlantıyı seçin.
4.  Sol taraftaki gezinti menüsünde **Azure Etkin Dizini'ni**seçin. 
5.  Gezinti bölmesinde, **Güvenlik** bölümünden **risk için işaretlenen Kullanıcıları** seçin.

## <a name="remediation-actions"></a>Düzeltme eylemleri

Etkilenen hesapları düzeltmeye ve ortamınızın güvenliğini sağlamaya yardımcı olmak için aşağıdaki eylemleri gerçekleştirin:

1.  Çok faktörlü kimlik doğrulama ve self servis parola sıfırlama için [doğru bilgileri doğrulayın.](https://aka.ms/MFAValid) 
2.  Tüm kullanıcılar için [çok faktörlü kimlik doğrulamayı etkinleştirin.](https://aka.ms/MFAuth) 
3.  Etkilenen her hesap için aşağıdaki adımları otomatik olarak gerçekleştirmek için bu [düzeltme komut dosyasını](https://aka.ms/remediate) kullanın: 

    a. Hesabı güvenli hale getirmek ve etkin oturumları öldürmek için parolayı sıfırla.

    b. Posta kutusu temsilcilerini kaldırın.

    c. Dış etki alanlarına posta iletme kurallarını devre dışı bırakın.

    d. Posta kutusunda genel posta iletme özelliğini kaldırın.

    e. Kullanıcının hesabında MFA'yı etkinleştirin.

    f. Hesapta parola karmaşıklığını yüksek düzeyde olacak şekilde ayarlayın.

    g. Posta kutusu denetimini etkinleştirin.

    h. Yöneticinin gözden geçirmesi için bir denetim günlüğü oluştur.

4. Office 365 kiracınızı ve diğer BT altyapısını inceleyin; bunun için tüm kiracı ayarlarını, kullanıcı hesaplarını ve olası değişiklikler için kullanıcı başına yapılandırma ayarlarını gözden geçirin. Kalıcılık yöntemi göstergelerini, ayrıca yetkisiz erişim sağlayan birinin VPN kimlik bilgilerini almak veya diğer kurumsal kaynaklara erişmek için ilk tutunma noktasına işaret edebilecek göstergeleri arayın. 

5.  Soruşturmanızın bir parçası olarak, kolluk kuvvetleri de dahil olmak üzere hükümet yetkililerine haber verip vermemeniz gerektiğini düşünün.

Ayrıca şunları yapmalısınız:

- Olağandışı etkinlikleri [ele alma yla ilgili](https://aka.ms/fixaccount)bu kılavuzu okuyun ve uygulayın. 
- Kiracınızdaki etkinliği çözümlemenize yardımcı olması için [denetim ardışık hattını etkinleştirin.](https://aka.ms/improvesecurity) Tamamlandığında, denetim mağazanız etkinlik günlükleriyle doldurmaya başlar. Bu noktada, Güvenlik ve [Uyumluluk Merkezi'nin arama ve araştırma kaynağından](https://aka.ms/sccsearch)da yararlanabilirsiniz. 
- Tüm hesaplarınız için posta kutusu denetimi sağlamak için bu [komut dosyasını](https://aka.ms/mailboxaudit1) kullanın. 
- Tüm posta kutularınız için temsilci izinlerini ve posta iletme kurallarını gözden geçirin. Bu görevi gerçekleştirmek için bu [PowerShell betiğini](https://aka.ms/delegateforwardrules) kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md)
* [Riskli oldukları belirlenen kullanıcılar](concept-user-at-risk.md)
