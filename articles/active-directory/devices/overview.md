---
title: Azure Active Directory'de aygıt kimliği nedir?
description: Aygıt kimlik yönetiminin ortamınızdaki kaynaklara erişen aygıtları yönetmenize nasıl yardımcı olabileceğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: overview
ms.date: 03/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: c895a13eb9b2bed9e3a8a5a250c4e925dfa834c5
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80331804"
---
# <a name="what-is-a-device-identity"></a>Cihaz kimliği nedir?

Tüm şekil ve boyutlardaki cihazların çoğalması ve Kendi Cihazını Getir (BYOD) konsepti yle, BT uzmanları iki rakip hedefle karşı karşıyadır:

- Son kullanıcıların her yerde ve her zaman üretken olmasını sağlar
- Kuruluşun varlıklarını koruma

Bu varlıkları korumak için BT personelinin önce aygıt kimliklerini yönetmesi gerekir. BT personeli, güvenlik ve uyumluluk standartlarının karşılandığından emin olmak için Microsoft Intune gibi araçlarla aygıt kimliğini geliştirebilir. Azure Active Directory (Azure AD), bu aygıtlar aracılığıyla her yerden cihazlara, uygulamalara ve hizmetlere tek oturum açma olanağı sağlar.

- Kullanıcılarınız kuruluşunuzun ihtiyaç duydukları varlıklarına erişebilir. 
- BT personeliniz, kuruluşunuzun güvenliğini sağlamak için ihtiyaç duydukları denetimleri alır.

Aygıt kimlik yönetimi, [aygıt tabanlı Koşullu Erişim'in](../conditional-access/require-managed-devices.md)temelidir. Aygıt tabanlı Koşullu Erişim ilkeleriyle, ortamınızdaki kaynaklara erişimin yalnızca yönetilen aygıtlarla mümkün olduğundan emin olabilirsiniz.

## <a name="getting-devices-in-azure-ad"></a>Azure AD'de aygıt alma

Azure AD'de bir aygıt almak için birden çok seçeneğiniz vardır:

- **Azure AD kayıtlı**
   - Azure AD kayıtlı aygıtlar genellikle kişisel veya mobil aygıtlardır ve kişisel bir Microsoft hesabı veya başka bir yerel hesapla oturum açmaktadır.
      - Windows 10
      - iOS
      - Android
      - macOS
- **Azure AD'ye katılanlar**
   - Azure AD'ye katılan aygıtlar bir kuruluşa aittir ve bu kuruluşa ait bir Azure REKLAM hesabıyla oturum açmaktadır. Sadece bulutta varolurlar.
      - Windows 10 
      - Windows Server 2019 (Sunucu çekirdeği desteklenmiyor)
- **Hibrit Azure AD'ye katılmış**
   - Karma Azure AD'si bir kuruluşa ait olan aygıtlar bir kuruluşa aittir ve bu kuruluşa ait bir Azure AD hesabıyla oturum açmaktadır. Bulutta ve şirket içinde varlar.
      - Windows 7, 8.1 veya 10
      - Windows Server 2008 veya daha yeni

![Azure AD Aygıtları bıçaklarında görüntülenen aygıtlar](./media/overview/azure-active-directory-devices-all-devices.png)

## <a name="device-management"></a>Cihaz yönetimi

Azure AD'deki aygıtlar Microsoft Intune, Microsoft Endpoint Configuration Manager, Grup İlkesi (karma Azure AD join), Mobil Uygulama Yönetimi (MAM) araçları veya diğer üçüncü taraf araçları gibi Mobil Aygıt Yönetimi (MDM) araçları kullanılarak yönetilebilir.

## <a name="resource-access"></a>Kaynak erişimi

Aygıtları Azure AD'ye kaydetme ve birleştirme, kullanıcılarınıza bulut kaynaklarına Kesintisiz Oturum Açma (SSO) sağlar. Bu işlem, yöneticilerin erişilen aygıta göre kaynaklara Koşullu Erişim ilkelerini uygulama olanağı da sağlar. 

> [!NOTE]
> Aygıt tabanlı Koşullu Erişim ilkeleri, karma Azure AD'ye katılan aygıtlar veya uyumlu Azure AD'nin katılması veya Azure AD'ye kayıtlı aygıtlar gerektirir.

Azure AD'ye katılan veya azure AD'yi karma olarak katılan aygıtlar, SSO'dan kuruluşunuzun şirket içi kaynaklarının yanı sıra bulut kaynaklarına da yarar sağlar. Makalede daha fazla bilgi bulunabilir, [SSO'nun azure AD'ye katılan cihazlarda şirket içi kaynaklarnasıl çalıştığı.](azuread-join-sso.md)

## <a name="device-security"></a>Cihaz güvenliği

- **Azure AD kayıtlı aygıtlar** son kullanıcı tarafından yönetilen bir hesap kullanır, bu hesap ya bir Microsoft hesabı ya da aşağıdakilerden biri veya daha fazlası ile güvenli başka bir yerel olarak yönetilen kimlik bilgisidir.
   - Parola
   - PIN
   - Desen
   - Windows Hello
- **Azure AD'ye katılan veya karma Azure AD'si,** Azure AD'de aşağıdakilerden biri veya birkaçı yla güvenli bir kuruluş hesabı kullanır.
   - Parola
   - İş İçin Windows Hello

## <a name="provisioning"></a>Sağlama

Aygıtları Azure AD'ye almak self servis bir şekilde veya yöneticiler tarafından kontrollü bir sağlama işlemiyle yapılabilir.

## <a name="summary"></a>Özet

Azure AD'deki aygıt kimlik yönetimi yle şunları yapabilirsiniz:

- Azure AD'de aygıt getirme ve yönetme işlemini basitleştirin
- Kullanıcılarınıza kuruluşunuzun bulut tabanlı kaynaklarına kullanımı kolay bir erişim sağlayın

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD kayıtlı cihazlar](concept-azure-ad-register.md) hakkında daha fazla bilgi edinin
- [Azure AD'ye katılan cihazlar](concept-azure-ad-join.md) hakkında daha fazla bilgi edinin
- [Karma Azure AD'ye katılan aygıtlar](concept-azure-ad-join-hybrid.md) hakkında daha fazla bilgi edinin
- Azure portalında aygıt kimliklerininasıl yöneteceğine genel bir bakış almak için Azure [portalını kullanarak aygıt kimliklerini yönetme bölümüne](device-management-azure-portal.md)bakın.
- Aygıt tabanlı Koşullu Erişim hakkında daha fazla bilgi edinmek için [Azure Etkin Dizin ini aygıt tabanlı Koşullu Erişim ilkelerini yapılandırma](../conditional-access/require-managed-devices.md)'ya bakın.
