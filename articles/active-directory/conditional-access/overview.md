---
title: Azure Active Directory Koşullu erişim nedir? | Microsoft Docs
description: Azure Active Directory ' deki Koşullu erişimin, yalnızca bir kaynağa erişmeye çalışan, ancak kaynağa nasıl erişildiğine yönelik otomatik erişim kararları uygulamanıza nasıl yardımcı olduğunu öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 02/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 99d17b354e267d003e23e507ca190b951e3ed4a0
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68608084"
---
# <a name="what-is-conditional-access"></a>Koşullu Erişim nedir?

Bulutu kullanan kuruluşlar için en önemli konu güvenliktir. Bulut kaynaklarınızı yönetmek söz konusu olduğunda, bulut güvenliğinin temel etmenlerinden biri güvenlik ve kimliktir. Mobil ve bulut öncelikli bir dünyada kullanıcılar, kuruluşunuzun kaynaklarına erişmek için farklı konumlardan farklı cihazlar ve uygulamalar kullanabilir. Bunun sonucunda artık yalnızca bir kaynağa erişim sağlayabilecek kullanıcılara odaklanmak yeterli değildir. Güvenlik ve üretkenlik arasındaki dengeyi kurmak için, erişim denetimi kararı sırasında bir kaynağa erişim şeklini de dikkate almanız gerekir. Azure Active Directory (Azure AD) koşullu erişim ile, bu gereksinime göre adres sağlayabilirsiniz. Koşullu erişim Azure Active Directory bir yetenektir. Koşullu erişimle, koşullara göre bulut uygulamalarınıza erişmek için otomatik erişim denetimi kararları uygulayabilirsiniz.

Koşullu erişim ilkeleri, ilk faktör kimlik doğrulaması tamamlandıktan sonra zorlanır. Bu nedenle, koşullu erişim hizmet reddi (DoS) saldırıları gibi senaryolar için birinci hat savunma olarak tasarlanmamıştır, ancak erişimi anlamak için bu olaylardaki sinyalleri (örneğin, oturum açma risk düzeyi, isteğin konumu vb.) kullanabilir.  

![Denetim](./media/overview/81.png)

Bu makalede, Azure AD 'de koşullu erişime kavramsal bir genel bakış sunulmaktadır.

## <a name="common-scenarios"></a>Genel senaryolar

Mobil ve bulut öncelikli bir dünyada Azure Active Directory cihazlarda, uygulamalarda ve hizmetlerde tüm konumlardan çoklu oturum açma özelliğinin kullanılmasını sağlar. Cihaz kullanımı (KCG dahil), kuruluş ağı dışında çalışma ve üçüncü taraf SaaS uygulamaları arttıkça iki hedefle karşılaşırsınız:

- Kullanıcıların her yerde ve her zaman üretken olmasını sağlama
- Kuruluş varlıklarını her zaman koruma altında tutma

Koşullu erişim ilkelerini kullanarak, gerekli koşullarda doğru erişim denetimlerini uygulayabilirsiniz. Azure AD koşullu erişim, gerektiğinde ek güvenlik sağlar ve bu olmadığında kullanıcının bir yolu dışında kalır.

Aşağıda, Koşullu erişimin size yardımcı olabilecek bazı yaygın erişim sorunları verilmiştir:

- **[Oturum açma riski](conditions.md#sign-in-risk)** : Azure AD Kimlik Koruması oturum açma risklerini algılar. Oturum açma riskinin kötü niyetli bir kullanıcıyı işaret ettiği durumda erişimi nasıl sınırlarsınız? Oturumun açma işleminin doğru kullanıcı tarafından gerçekleştirilmiş olduğuna dair daha kuvvetli bir kanıta ihtiyaç duyarsanız ne olur? Şüpheleriniz, belirli kullanıcıların bir uygulamaya erişimini engelleyecek kadar kuvvetliyse ne yapmalısınız?  
- **[Ağ konumu](location-condition.md)** : Azure AD 'den her yerden erişilebilir. BT departmanınızın denetimi altında olmayan bir ağ konumundan erişim girişimi gerçekleştirilirse ne olur? Kullanıcı adı ve parola bileşimi, kuruluş ağınızdan yapılan erişim denemelerinde kimliğinizi kanıtlamaya yeterli olabilir. Peki dünya üzerindeki diğer beklenmeyen ülkelerden veya bölgelerden başlatılan erişim girişimleri için daha kuvvetli bir kimlik kanıtına ihtiyaç duyuyorsanız? Belirli konumlardan erişim girişimlerini tamamen engellemek isterseniz ne yapmalısınız?  
- **[Cihaz yönetimi](conditions.md#device-platforms)** : Azure AD 'de, kullanıcılar mobil ve ayrıca kişisel cihazlar dahil olmak üzere çok çeşitli cihazlardan bulut uygulamalarına erişebilirler. Peki ya erişim denemelerinin yalnızca BT departmanınız tarafından yönetilen cihazlardan gerçekleştirilmesini isterseniz? Peki ya belirli cihaz türlerinin ortamınızdaki bulut uygulamalarına erişmesini engellemek isterseniz?
- **[İstemci uygulaması](conditions.md#client-apps)** : Günümüzde, Web tabanlı uygulamalar, mobil uygulamalar veya masaüstü uygulamaları gibi farklı uygulama türlerini kullanarak birçok bulut uygulamasına erişebilirsiniz. Peki bilinen sorunlara yol açan türde bir istemci uygulaması kullanılarak erişim girişimi gerçekleştirilirse ne olur? Belirli uygulama türleri için BT departmanınız tarafından yönetilen bir cihazı şart koşmak istiyorsanız ne yapmalısınız?

Bu sorular ve ilgili yanıtlar, Azure AD koşullu erişim için genel erişim senaryolarını temsil eder.
Koşullu erişim, ilke tabanlı bir yaklaşım kullanarak erişim senaryolarını idare etmenizi sağlayan bir Azure Active Directory özelliğidir.

> [!VIDEO https://www.youtube.com/embed/eLAYBwjCGoA]

## <a name="conditional-access-policies"></a>Koşullu erişim ilkeleri

Koşullu erişim ilkesi, aşağıdaki model kullanılarak bir erişim senaryosunun tanımıdır:

![Denetim](./media/overview/10.png)


**Bu ortaya çıktığında**, ilkenizin tetiklenme nedenini tanımlar. Neden, karşılanan bir koşul grubu ile belirlenir. Azure AD koşullu erişim 'de, iki atama koşulu özel bir rol oynar:

- **[Kullanıcılar](conditions.md#users-and-groups)** : Kullanıcılar bir erişim denemesi (**kim**) gerçekleştiriyor.
- **[Bulut uygulamaları](conditions.md#cloud-apps-and-actions)** : Erişim girişiminin (**ne**) hedefi.

Bu iki koşul, koşullu erişim ilkesinde zorunludur. Bu iki zorunlu koşula ek olarak, erişim girişiminin gerçekleştirilme şeklini açıklayan ek koşullar da kullanabilirsiniz. Mobil cihazların veya kuruluş ağınızın dışındaki konumların kullanılması, yaygın örnekler arasında sayılabilir. Daha fazla bilgi için bkz. [koşullu erişim Azure Active Directory koşullar](conditions.md).

Erişim denetimleriniz ile koşulların birleşimi, koşullu erişim ilkesini temsil eder.

![Denetim](./media/overview/51.png)

Azure AD koşullu erişimi sayesinde, yetkili kullanıcıların bulut uygulamalarınıza nasıl erişebileceğini denetleyebilirsiniz. Bir koşullu erişim ilkesinin amacı, bir erişim girişiminin nasıl gerçekleştirilebileceğini temel alarak bir bulut uygulamasına erişim denemesinde ek erişim denetimleri zorlayamazdır.

Bulut uygulamalarınıza erişimi korumak için ilke tabanlı bir yaklaşım kullanmak, ortamınızın ilke gereksinimlerini işin teknik boyutu konusunda kaygılanmadan bu makalede ana hatları belirlenen yapıyı kullanarak belirlemeye başlamanızı sağlar.

## <a name="azure-ad-conditional-access-and-federated-authentication"></a>Azure AD koşullu erişim ve federal kimlik doğrulaması

Koşullu erişim ilkeleri, [federe kimlik doğrulamasıyla](../../security/fundamentals/choose-ad-authn.md#federated-authentication)sorunsuz bir şekilde çalışır. Bu destek, ilkenin [Azure AD raporlama](../reports-monitoring/concept-sign-ins.md)kullanılarak etkin kullanıcı oturum açma işlemleri için nasıl uygulandığını gösteren tüm desteklenen koşulları ve denetimleri ve görünürlüğü içerir.

*Azure AD ile federe kimlik doğrulaması*, Azure AD kullanıcı kimliği doğrulamalarının tümünün güvenilen bir kimlik doğrulama hizmeti tarafından halledilmesi anlamına gelir. Güvenilen bir kimlik doğrulama hizmeti; örneğin Active Directory Federasyon Hizmetleri (AD FS) ya da başka bir Federasyon Hizmetidir. Bu yapılandırmada birincil kullanıcı kimlik doğrulaması hizmette gerçekleştirilir, sonra bireysel uygulamalarda oturum için Azure AD kullanılır. Kullanıcının eriştiği uygulamaya erişim verilmeden önce Azure AD koşullu erişim uygulanır. 

Yapılandırılmış koşullu erişim ilkesi çok faktörlü kimlik doğrulaması gerektirdiğinde, Azure AD varsayılan olarak Azure MFA 'yı kullanmaktır. MFA için federasyon hizmetini kullanıyorsanız, [PowerShell](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings)'de `-SupportsMFA` değerini `$true` yaparak Azure AD'yi MFA gerektiğinde federasyon hizmetine yönlenecek şekilde yapılandırabilirsiniz. Bu ayar, Azure AD tarafından `wauth= http://schemas.microsoft.com/claims/multipleauthn` kullanılarak gönderilen MFA sınama isteğini destekleyen federe kimlik doğrulama hizmetleri içindir.

Kullanıcı federe kimlik doğrulama hizmetinde oturum açtıktan sonra Azure AD, cihaz uyumluluğu ya da uygulamanın onaylanmış olması gibi diğer ilke gereksinimlerini halleder.

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

[Microsoft 365 iş lisanslarına](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) sahip müşterilerin koşullu erişim özelliklerine de erişimi vardır. 

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda Koşullu erişimin nasıl uygulanacağını öğrenmek için, bkz. [Azure Active Directory Koşullu erişim dağıtımınızı planlayın](plan-conditional-access.md).
