---
title: Azure Active Directory portalında risk için işaretlenen kullanıcılar | Microsoft Docs
description: Azure Active Directory portalında risk güvenliği için işaretlenmiş kullanıcılar hakkında bilgi edinin
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08305b6aec7dcd5906f4316767c4c512d3a0535e
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90600509"
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Azure Active Directory portalında riskli olarak işaretlenmiş kullanıcıları düzeltme

Azure Active Directory (Azure AD) içindeki güvenlik raporlarında, ortamınızda güvenliği aşılmış Kullanıcı hesaplarının olasılığını ölçer. Riskli olduğu için işaretlenmiş kullanıcılar, güvenliği tehlikeye girmiş olabilecek kullanıcı hesaplarının göstergesidir.

Microsoft ortamınızın güvenliğini korumak için çalışmaktadır. Bu çalışma kapsamında, Microsoft alışılmadık veya bilinen saldırı düzenleriyle tutarlı etkinlikleri sürekli izler. 

Kullanıcılarınızın hesaplarının bazılarına yetkisiz erişimi gösterebilen olağan dışı etkinlikler algılanırsa, işlem yapmanız için bildirim alırsınız. Bu, Microsoft 'un kendi sistemlerinin tehlikede olduğu anlamına gelmez.

## <a name="access-the-users-flagged-for-risk-report"></a>Riskli olarak işaretlenen kullanıcılar raporuna erişme

Risk için işaretlenen kullanıcıları Azure portal, [risk altındaki kullanıcılar raporu](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RiskyUsers) aracılığıyla gözden geçirebilirsiniz. Azure AD yoksa, adresinden ücretsiz olarak kaydolabilirsiniz [https://aka.ms/AccessAAD](https://aka.ms/AccessAAD) . 

Risk işaretli kullanıcılar raporu için, her kullanıcı için aşağıdaki eylemleri gerçekleştirebilirsiniz:

- Geçici parola oluşturma
- Kullanıcının bir sonraki defa oturum açtığında parolasını güvenli şekilde sıfırlamasını zorunlu tutma
- Herhangi bir düzeltme eylemi gerçekleştirmeden kullanıcı riskini kapatma.

Daha fazla bilgi için bkz. [risk için Işaretlenen kullanıcılar güvenlik raporu](../identity-protection/overview-identity-protection.md).

### <a name="azure-ad-subscription-for-microsoft-365-customers"></a>Microsoft 365 müşterileri için Azure AD aboneliği

**Azure yönetim merkezine**erişmek için Microsoft 365 kimlik bilgilerinizi de kullanabilirsiniz. Azure AD’ye erişiminizi etkinleştirdikten sonra, Azure AD portalına yeniden yönlendirilirsiniz. Temel abonelik düzeyinde, raporlarda sağlanan ayrıntı miktarı sınırlıdır. Ek veriler ve analizler, Azure Premium abonelerine sağlanır.

Microsoft 365 Yönetim merkezinde **risk raporları için Işaretlenen kullanıcılara** erişmek için:

1.  Sol taraftaki gezinti menüsünde, **Yönetim Merkezleri**' ni seçin. 
2.  **Azure AD**'yi seçin.
3.  **Azure Active Directory yönetim merkezi**'nde oturum açın.
4.  Sayfanın üst kısmında **Yeni portalın kullanıma hazır**olduğunu belirten bir başlık görüntüleniyorsa bağlantıyı seçin.
4.  Sol taraftaki gezinti menüsünde **Azure Active Directory**' yi seçin. 
5.  Gezinti bölmesinde, **güvenlik** bölümünde **risk olarak işaretlenen kullanıcıları** seçin.

## <a name="remediation-actions"></a>Düzeltme eylemleri

Etkilenen hesapları düzeltmeye ve ortamınızın güvenliğini sağlamaya yardımcı olmak için aşağıdaki eylemleri gerçekleştirin:

1.  Multi-Factor Authentication ve self servis parola sıfırlama için [doğru bilgileri doğrulayın](https://aka.ms/MFAValid) . 
2.  Tüm kullanıcılar için [Multi-Factor Authentication 'ı etkinleştirin](https://aka.ms/MFAuth) . 
3.  Aşağıdaki adımları otomatik olarak gerçekleştirmek için, etkilenen her hesap için bu [Düzeltme betiğini](https://aka.ms/remediate) kullanın: 

    a. Hesabı güvenli hale getirmek ve etkin oturumları sonlandırmak için parolayı sıfırlayın.

    b. Posta kutusu temsilcilerini kaldırın.

    c. Dış etki alanlarına posta iletme kurallarını devre dışı bırakın.

    d. Posta kutusunda genel posta iletme özelliğini kaldırın.

    e. Kullanıcının hesabında MFA'yı etkinleştirin.

    f. Hesapta parola karmaşıklığını yüksek düzeyde olacak şekilde ayarlayın.

    örneğin: Posta kutusu denetimini etkinleştirin.

    h. Yöneticinin gözden geçirmesi için bir denetim günlüğü oluşturun.

4. Microsoft 365 kiracınızı ve diğer BT altyapınızı araştırın. Bu, tüm kiracı ayarlarını, Kullanıcı hesaplarını ve olası değişiklikler için Kullanıcı başına yapılandırma ayarlarını gözden geçirin. Kalıcılık yöntemi göstergelerini, ayrıca yetkisiz erişim sağlayan birinin VPN kimlik bilgilerini almak veya diğer kurumsal kaynaklara erişmek için ilk tutunma noktasına işaret edebilecek göstergeleri arayın. 

5.  Araştırmanızın bir parçası olarak, hukuk zorlaması dahil olmak üzere devlet yetkililerine bildirimde bulundurmanız gerekip gerekmediğini göz önünde bulundurun.

Ayrıca şunları yapmalısınız:

- [Olağandışı etkinlikleri ele](https://aka.ms/fixaccount)almak için bu kılavuzu okuyun ve uygulayın. 
- Kiracınızdaki etkinliği çözümlemenize yardımcı olması için [Denetim ardışık düzenini etkinleştirin](https://aka.ms/improvesecurity) . Tamamlandıktan sonra, denetim depolduğunuz etkinlik günlükleri ile doldurma başlar. Bu noktada, [güvenlik ve Uyumluluk Merkezi 'nin arama ve araştırma kaynağı](https://aka.ms/sccsearch)' ndan da yararlanabilirsiniz. 
- Tüm posta kutularınız için temsilci izinlerini ve posta iletme kurallarını gözden geçirin. Bu görevi gerçekleştirmek için bu [PowerShell betiğini](https://aka.ms/delegateforwardrules) kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory Kimlik Koruması](../identity-protection/overview-identity-protection.md)
* [Riskli oldukları belirlenen kullanıcılar](../identity-protection/overview-identity-protection.md)