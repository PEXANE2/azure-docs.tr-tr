---
title: Azure Active Directory | Microsoft Dokümanlar
description: BT Pro için yazılmış olan bu makalede, Azure uygulamalarını Active Directory ile tümleştirmek için yönergeler sağlanmaktadır.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: article
ms.date: 10/30/2018
ms.author: mimart
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: ba54f8042c20a00f8d559ddce28e007a93afaace
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67108294"
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Azure Active Directory için iş yeri uygulamaları geliştirin
Bu kılavuz, Azure Etkin Dizin (AD) için iş çizgisi (LoB) uygulamaları geliştirmeye genel bir bakış sağlar. Amaçlanan hedef kitle Active Directory/Office 365 global yöneticilerdir.

## <a name="overview"></a>Genel Bakış
Azure AD ile tümleşik uygulamalar oluşturmak, kuruluşunuzdaki kullanıcılara Office 365 ile tek oturum açma olanağı sağlar. Uygulamanın Azure AD'de olması, uygulamanın kimlik doğrulama ilkesi üzerinde denetim sağlar. Koşullu Erişim ve çok faktörlü kimlik doğrulamasına (MFA) sahip uygulamaların nasıl korunup korunabildiğini öğrenmek için [erişim kurallarını yapılandırmaya](../conditional-access/app-based-mfa.md)bakın.

Azure Active Directory'yi kullanmak için uygulamanızı kaydedin. Uygulamayı kaydetmek, geliştiricilerinizin kullanıcıların kimliğini doğrulamak için Azure AD'yi kullanabileceği ve e-posta, takvim ve belgeler gibi kullanıcı kaynaklarına erişim isteyebileceği anlamına gelir.

Dizininizin herhangi bir üyesi (misafirler değil) bir *uygulama nesnesi oluşturma*olarak bilinen bir uygulamayı kaydedebilir.

Bir uygulamayı kaydetmek, herhangi bir kullanıcının aşağıdakileri yapmasına olanak tanır:

* Azure AD'nin tanıdığı uygulamaları için bir kimlik alın
* Uygulamanın kendisini AD'ye doğrulamak için kullanabileceği bir veya daha fazla sırlar/anahtarlar alın
* Uygulamayı Azure portalında özel bir ad, logo vb. ile markalaştırın.
* Azure AD yetkilendirme özelliklerini uygulama özelliklerine şunları dahil olmak üzere uygulamalarını uygulayın:

  * Rol Tabanlı Erişim Denetimi (RBAC)
  * OAuth yetkilendirme sunucusu olarak Azure Active Directory (uygulama tarafından açığa çıkarılan bir API'yi emniyete alın)
* Aşağıdakiler de dahil olmak üzere, uygulamanın beklendiği gibi çalışması için gerekli izinleri bildirin:

     - Uygulama izinleri (yalnızca genel yöneticiler). Örneğin: Azure Kaynak, Kaynak Grubu veya Abonelik'e göre başka bir Azure REKLAM uygulamasında veya rol üyeliğinde rol üyeliği
     - Yetkin izinler (herhangi bir kullanıcı). Örneğin: Azure AD, Oturum Açma ve Okuma Profili

> [!NOTE]
> Varsayılan olarak, herhangi bir üye bir uygulamayı kaydedebilir. Uygulamaları belirli üyelere kaydetme izinlerini nasıl kısıtlarız öğrenmek için, [uygulamaların Azure AD'ye nasıl eklenmelerine](../develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance)bakın.
>
>

İşte siz, küresel yönetici, geliştiricilerin uygulamalarını üretime hazır hale getirmelerine yardımcı olmak için yapmanız gerekenler:

* Erişim kurallarını yapılandırma (erişim ilkesi/MFA)
* Uygulamayı kullanıcı ataması gerektirecek şekilde yapılandırın ve kullanıcıları atayın
* Varsayılan kullanıcı onayı deneyimini bastırma

## <a name="configure-access-rules"></a>Erişim kurallarını yapılandırma
Uygulama başına erişim kurallarını SaaS uygulamalarınızda yapılandırın. Örneğin, MFA gerektirebilir veya yalnızca güvenilen ağlardaki kullanıcılara erişime izin verebilirsiniz. Bunun [ayrıntıları, erişim kurallarını yapılandırma](../conditional-access/app-based-mfa.md)belgesinde mevcuttur.

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Uygulamayı kullanıcı ataması gerektirecek şekilde yapılandırın ve kullanıcıları atayın
Varsayılan olarak, kullanıcılar atanmadan uygulamalara erişebilir. Ancak, uygulama rolleri ortaya çıkarırsa veya uygulamanın kullanıcının erişim panelinde görünmesini istiyorsanız, kullanıcı ataması gerekir.

Azure AD Premium veya Enterprise Mobility Suite (EMS) abonesiyseniz, grupları kullanmanızı şiddetle öneririz. Uygulamaya grup atama, devam eden erişim yönetimini grubun sahibine devretmenizi sağlar. Grubu oluşturabilir veya kuruluşunuzdaki sorumlu tarafa grup yönetim tesisinizi kullanarak grubu oluşturmasını isteyebilirsiniz.

[Kullanıcıları ve grupları bir uygulamaya atama](methods-for-assigning-users-and-groups.md)  


## <a name="suppress-user-consent"></a>Kullanıcı onayLarını bastırma
Varsayılan olarak, her kullanıcı oturum açma izni deneyimiyaşar. Kullanıcılardan bir uygulamaya izin vermelerini isteyen onay deneyimi, bu tür kararlar almaya yabancı olan kullanıcılar için rahatsız edici olabilir.

Güvendiğiniz uygulamalariçin, kuruluşunuz adına uygulamayı onaylayarak kullanıcı deneyimini basitleştirebilirsiniz.

Azure'daki kullanıcı onayı ve onay deneyimi hakkında daha fazla bilgi [için](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)bkz.

## <a name="related-articles"></a>İlgili Makaleler
* [Azure AD Application Proxy ile şirket içi uygulamalara güvenli uzaktan erişim sağlama](application-proxy.md)
* [Azure AD ile uygulamalara erişimi yönetme](what-is-access-management.md)

