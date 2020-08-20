---
title: Uygulamanızı kullanmak için kaydedin Azure Active Directory | Microsoft Docs
description: BT uzmanı için yazılan bu makalede, Azure uygulamalarını Active Directory ile tümleştirme yönergeleri sunulmaktadır.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d662689d44bcdbd468c882de0679fd60c9ab4f4
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88640455"
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Azure Active Directory için iş kolu uygulamaları geliştirin
Bu kılavuz, Azure Active Directory (AD) için iş kolu (LoB) uygulamaları geliştirmeye genel bir bakış sağlar. Hedef kitle Active Directory/Office 365 genel yöneticileridir.

## <a name="overview"></a>Genel Bakış
Azure AD ile tümleştirilmiş uygulamalar oluşturmak, kuruluşunuzdaki kullanıcılara Office 365 ile çoklu oturum açma olanağı sunar. Uygulamanın Azure AD 'de olması, uygulamanın kimlik doğrulama ilkesi üzerinde denetim sahibi olmanızı sağlar. Koşullu erişim ve çok faktörlü kimlik doğrulamasıyla (MFA) uygulamaları koruma hakkında daha fazla bilgi edinmek için bkz. [erişim kurallarını yapılandırma](../conditional-access/app-based-mfa.md).

Azure Active Directory kullanmak için uygulamanızı kaydedin. Uygulamanın kaydedilmesi, geliştiricilerinizin kullanıcıların kimliğini doğrulamak ve e-posta, takvim ve belgeler gibi Kullanıcı kaynaklarına erişim istemek için Azure AD 'yi kullanabileceği anlamına gelir.

Dizininizdeki herhangi bir üye (konuknot), *uygulama nesnesi oluşturma*olarak da bilinen bir uygulamayı kaydedebilir.

Bir uygulamayı kaydetmek, herhangi bir kullanıcının şunları yapmasına izin verir:

* Azure AD 'nin tanıdığı uygulamaları için bir kimlik alın
* Uygulamanın AD için kimliğini doğrulamak için kullanabileceği bir veya daha fazla gizli dizi/anahtar alın
* Uygulamayı bir özel ad, logo vb. ile Azure portal marka halinde yapın.
* Azure AD yetkilendirme özelliklerini uygulamasına uygulayın, örneğin:

  * Rol Tabanlı Erişim Denetimi (RBAC)
  * OAuth yetkilendirme sunucusu olarak Azure Active Directory (uygulama tarafından sunulan bir API 'YI güvenli hale getirme)
* Uygulamanın beklendiği gibi çalışması için gerekli izinleri bildirin, örneğin:

     - Uygulama izinleri (yalnızca genel Yöneticiler). Örneğin: bir Azure kaynağına, kaynak grubuna veya aboneliğe göre başka bir Azure AD uygulamasında rol üyeliği veya rol üyeliği
     - Temsilci izinleri (herhangi bir Kullanıcı). Örneğin: Azure AD, oturum açma ve okuma profili

> [!NOTE]
> Varsayılan olarak, herhangi bir üye bir uygulamayı kaydedebilir. Belirli üyelere uygulama kaydetme izinlerini kısıtlama hakkında bilgi edinmek için bkz. [uygulamaların Azure AD 'ye nasıl eklendiği](../develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

Geliştiricilerin uygulamayı üretime hazır hale getirmenize yardımcı olmak için, genel yönetici için yapmanız gerekenler şunlardır:

* Erişim kurallarını yapılandırma (erişim ilkesi/MFA)
* Uygulamayı kullanıcı ataması ve kullanıcı atama gerektirecek şekilde yapılandırma
* Varsayılan Kullanıcı onay deneyimini gösterme

## <a name="configure-access-rules"></a>Erişim kurallarını yapılandırma
SaaS uygulamalarınıza uygulama başına erişim kuralları yapılandırın. Örneğin, MFA gerektirebilir veya yalnızca güvenilen ağlardaki kullanıcılara erişime izin verebilirsiniz. Bu belgenin ayrıntıları, [erişim kurallarını yapılandırma](../conditional-access/app-based-mfa.md)belgesinde mevcuttur.

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Uygulamayı kullanıcı ataması ve kullanıcı atama gerektirecek şekilde yapılandırma
Varsayılan olarak, kullanıcılar atanana gerek kalmadan uygulamalara erişebilir. Ancak, uygulama rolleri açığa alıyorsa veya uygulamanın bir kullanıcının uygulamamda görünmesini istiyorsanız, Kullanıcı ataması yapmanız gerekir.

Azure AD Premium veya Enterprise Mobility Suite (EMS) abonesiyseniz, grupların kullanılması önemle önerilir. Uygulamaya grupları atamak, devam eden erişim yönetimini grubun sahibine atamanıza olanak tanır. Grup Yönetimi özelliğinizi kullanarak grubu oluşturabilir veya kuruluşunuzdaki sorumlu tarafın grubu oluşturmasını isteyebilirsiniz.

[Bir uygulamaya Kullanıcı ve Grup atama](methods-for-assigning-users-and-groups.md)  


## <a name="suppress-user-consent"></a>Kullanıcı onayını gösterme
Varsayılan olarak, her kullanıcı oturum açmak için bir onay deneyiminden geçer. İzin deneyimi, kullanıcılardan bir uygulamaya izin vermesini isteyen, bu tür kararlar vererek bilmediğiniz kullanıcılar için disconcerting olabilir.

Güvendiğiniz uygulamalar için, kuruluşunuz adına uygulamaya yarışarak Kullanıcı deneyimini basitleştirebilirsiniz.

Azure 'daki Kullanıcı onayı ve onay deneyimi hakkında daha fazla bilgi için bkz. [uygulamaları Azure Active Directory tümleştirme](../develop/quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="related-articles"></a>İlgili Makaleler
* [Azure AD Uygulama Ara Sunucusu ile şirket içi uygulamalara güvenli uzaktan erişimi etkinleştirme](application-proxy.md)
* [Azure AD ile uygulamalara erişimi yönetme](what-is-access-management.md)

