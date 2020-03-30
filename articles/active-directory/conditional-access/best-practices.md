---
title: Azure Etkin Dizini'nde Koşullu Erişim için en iyi uygulamalar | Microsoft Dokümanlar
description: Koşullu Erişim ilkelerini yapılandırırken bilmeniz gereken şeyler ve ne yapmaktan kaçınmanız gerektiği hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: f4560a514ddb9949c8cc07864b2319a5878b245e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295351"
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Azure Etkin Dizini'nde Koşullu Erişim için en iyi uygulamalar

[Azure Active Directory (Azure AD) Koşullu Erişim](../active-directory-conditional-access-azure-portal.md)ile yetkili kullanıcıların bulut uygulamalarınıza nasıl erişebileceğini kontrol edebilirsiniz. Bu makalede, hakkında bilgi sağlar:

- Bilmeniz gerekenler 
- Koşullu Erişim ilkelerini yapılandırırken yapmaktan kaçınmanız gereken şey nedir? 

Bu makalede, [Azure Etkin Dizini'nde Koşullu Erişim nedir?](../active-directory-conditional-access-azure-portal.md)

## <a name="whats-required-to-make-a-policy-work"></a>Bir politikanın işe yaraması için ne gerekir?

Yeni bir ilke oluşturduğunuzda, kullanıcı, grup, uygulama veya erişim denetimi seçilmez.

![Bulut uygulamaları](./media/best-practices/02.png)

İlkenizin çalışması için şunları yapılandırmanız gerekir:

| Nesne           | Nasıl                                  | Neden |
| :--            | :--                                  | :-- |
| **Bulut uygulamaları** |Bir veya daha fazla uygulama seçin.  | Koşullu Erişim ilkesinin amacı, yetkili kullanıcıların bulut uygulamalarına nasıl erişebileceğini denetlemenizi sağlamaktır.|
| **Kullanıcılar ve gruplar** | Seçili bulut uygulamalarınız için yetkilendirilen en az bir kullanıcı veya grup seçin. | Kullanıcı ve grup atanmamış koşullu erişim ilkesi hiçbir zaman tetiklenmez. |
| **Erişim denetimleri** | En az bir erişim denetimi seçin. | Koşullarınız tatmin olduysa, ilke işlemcinizin ne yapacağını bilmesi gerekir. |

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="how-are-conditional-access-policies-applied"></a>Koşullu Erişim ilkeleri nasıl uygulanır?

Bir bulut uygulamasına erişdiğinizde birden fazla Koşullu Erişim ilkesi uygulanabilir. Bu durumda, uygulanan tüm ilkeler tatmin edilmelidir. Örneğin, bir ilke çok faktörlü kimlik doğrulama (MFA) ve başka bir uyumlu aygıt gerektiriyorsa, MFA'yı tamamlamanız ve uyumlu bir aygıt kullanmanız gerekir. 

Tüm ilkeler iki aşamada uygulanır:

- 1. Aşama: 
   - Ayrıntı toplama: Zaten tatmin olacak ilkeleri tanımlamak için ayrıntıları toplayın.
   - Bu aşamada, aygıt uyumluluğu Koşullu Erişim ilkelerinizin bir parçasıysa, kullanıcılar bir sertifika istemi görebilir. Bu komut istemi, aygıt işletim sistemi Windows 10 olmadığında tarayıcı uygulamaları için oluşabilir.
   - İlke değerlendirmesinin 1. [report-only mode](concept-conditional-access-report-only.md)
- 2. Aşama:
   - Uygulama: Faz 1'de toplanan ayrıntıları hesaba katarak, kullanıcıdan karşılanmayan ek gereksinimleri yerine getirmesini isteyin.
   - Sonuçları oturuma uygulayın. 
   - Etkin ilkeler değerlendirmenin 2.

### <a name="how-are-assignments-evaluated"></a>Atamalar nasıl değerlendirilir?

Tüm atamalar mantıksal olarak **ANDed**vardır. Yapılandırılan birden fazla atamanız varsa, bir ilkeyi tetiklemek için tüm atamaların karşılanması gerekir.  

Kuruluşunuzun ağı dışından yapılan tüm bağlantılar için geçerli olan bir konum koşulu yapılandırmanız gerekiyorsa:

- **Tüm konumları** dahil et
- **Tüm güvenilen IP'leri** hariç tut

### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Azure AD yöneticisi portalının dışında ysanız ne yapabilirsiniz?

Koşullu Erişim ilkesindeki yanlış ayar nedeniyle Azure AD portalının dışında kalırsanız:

- Denetimi niz, kuruluşunuzda henüz engellenmemiş başka yöneticiler var mı? Azure portalına erişimi olan bir yönetici, oturum açmanızı etkileyen ilkeyi devre dışı kılabilir. 
- Kuruluşunuzdaki yöneticilerin hiçbiri ilkeyi güncelleştiremezse, bir destek isteği göndermeniz gerekir. Microsoft desteği, erişimi engelleyen Koşullu Erişim ilkelerini gözden geçirip güncelleyebilir.

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Azure klasik portalında ve Azure portalında yapılandırılan ilkelerinvarsa ne olur?  

Her iki ilke de Azure Etkin Dizin tarafından uygulanır ve kullanıcı yalnızca tüm gereksinimler karşılandığında erişebedilir.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Intune Silverlight portalında ve Azure portalında ilkelerinvarsa ne olur?

Her iki ilke de Azure Etkin Dizin tarafından uygulanır ve kullanıcı yalnızca tüm gereksinimler karşılandığında erişebedilir.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Aynı kullanıcı için yapılandırılan birden çok ilkem varsa ne olur?  

Azure Etkin Dizini, her oturum açma için tüm ilkeleri değerlendirir ve kullanıcıya erişim izni verilmeden önce tüm gereksinimlerin karşılanmasını sağlar. Engelleme erişimi diğer tüm yapılandırma ayarlarından üstündür. 

### <a name="does-conditional-access-work-with-exchange-activesync"></a>Koşullu Erişim Exchange ActiveSync ile çalışır mı?

Evet, Exchange ActiveSync'i Koşullu Erişim ilkesinde kullanabilirsiniz.

SharePoint Online ve Exchange Online gibi bazı bulut uygulamaları da eski kimlik doğrulama protokollerini destekler. Bir istemci uygulaması bir bulut uygulamasına erişmek için eski bir kimlik doğrulama protokolü kullanabiliyorsa, Azure AD bu erişim girişiminde Koşullu Erişim ilkesiuygulayamaz. Bir istemci uygulamasının ilkelerin uygulanmasını atlamasını önlemek için, yalnızca etkilenen bulut uygulamalarında modern kimlik doğrulamayı etkinleştirmenin mümkün olup olmadığını kontrol etmelisiniz.

### <a name="how-should-you-configure-conditional-access-with-office-365-apps"></a>Office 365 uygulamalarıyla Koşullu Erişimi nasıl yapılandırmanız gerekir?

Office 365 uygulamaları birbirine bağlı olduğundan, ilkeler oluştururken sık kullanılan uygulamaları birlikte atamanızı öneririz.

Birbirine bağlı yaygın uygulamalar arasında Microsoft Flow, Microsoft Planner, Microsoft Teams, Office 365 Exchange Online, Office 365 SharePoint Online ve Office 365 Yammer yer almaktadır.

Erişim bir oturumun veya görevin başında denetlendiğinde, çok faktörlü kimlik doğrulama gibi kullanıcı etkileşimleri gerektiren ilkeler için önemlidir. Bunu yapmazsanız, kullanıcılar bir uygulama içindeki bazı görevleri tamamlayamazsınız. Örneğin, yönetilmeyen aygıtlarda sharepoint'e erişmek için çok faktörlü kimlik doğrulaması gerektirir, ancak e-posta göndermezseniz, e-postalarında çalışan kullanıcılar Bir iletiye SharePoint dosyalarını ekemez. Makalede daha fazla bilgi bulunabilir, [Azure Active Directory Koşullu Erişim hizmet bağımlılıkları nelerdir?](service-dependencies.md).

## <a name="what-you-should-avoid-doing"></a>Yapmaktan kaçınmanız gerekenler

Koşullu Erişim çerçevesi size mükemmel bir yapılandırma esnekliği sağlar. Ancak, büyük esneklik, istenmeyen sonuçları önlemek için yayımlamadan önce her yapılandırma ilkesini dikkatle gözden geçirmeniz gerektiği anlamına da gelir. Bu bağlamda, tüm kullanıcılar / gruplar / bulut **uygulamaları**gibi komple setleri etkileyen atamalara özel dikkat etmelisiniz.

Ortamınızda aşağıdaki yapılandırmalardan kaçınmalısınız:

**Tüm kullanıcılar için, tüm bulut uygulamaları:**

- **Erişimi engelle** - Bu yapılandırma, kesinlikle iyi bir fikir olmayan tüm kuruluşunuzu engeller.
- **Uyumlu aygıt gerektirir** - Aygıtlarını henüz kaydetmemiş kullanıcılar için bu ilke, Intune portalına erişim de dahil olmak üzere tüm erişimi engeller. Kayıtlı bir aygıtı olmayan bir yöneticiyseniz, bu ilke, ilkeyi değiştirmek için Azure portalına geri dönmenizi engeller.
- **Etki alanı birleştirme gerektirir** - Bu ilke bloğu erişimi, henüz etki alanına bağlı bir aygıtınız yoksa kuruluşunuzdaki tüm kullanıcılara erişimi engelleme potansiyeline de sahiptir.
- **Uygulama koruma ilkesi ni gerektirme** - Bu ilke bloğu erişimi, bir Intune ilkeniz yoksa kuruluşunuzdaki tüm kullanıcılar için erişimi engelleme potansiyeline de sahiptir. Intune uygulama koruma ilkesine sahip istemci uygulaması olmayan bir yöneticiyseniz, bu ilke, Intune ve Azure gibi portallara geri girmenizi engeller.

**Tüm kullanıcılar için, tüm bulut uygulamaları, tüm cihaz platformları:**

- **Erişimi engelle** - Bu yapılandırma, kesinlikle iyi bir fikir olmayan tüm kuruluşunuzu engeller.

## <a name="how-should-you-deploy-a-new-policy"></a>Yeni bir ilkeyi nasıl dağıtmalısınız?

İlk adım olarak, ["eğer" aracını](what-if-tool.md)kullanarak ilkenizi değerlendirmelisiniz.

Ortamınız için yeni ilkeler hazır olduğunda, bunları aşamalar halinde dağıtın:

1. Küçük bir kullanıcı kümesine bir ilke uygulayın ve beklendiği gibi şekilde şekilde olduğunu doğrulayın. 
1. Bir ilkeyi daha fazla kullanıcı içerecek şekilde genişletdiğinizde. Hala erişimleri olduğundan ve bir değişiklik gerektiğinde bir ilkeyi güncelleştirebildiklerinden emin olmak için tüm yöneticileri ilkeden hariç tutma.
1. Yalnızca gerekirse tüm kullanıcılara bir ilke uygulayın. 

En iyi uygulama olarak, şu şekilde bir kullanıcı hesabı oluşturun:

- Politika yönetimine adanmış 
- Tüm ilkelerinizden hariç tutuldu

## <a name="policy-migration"></a>İlke geçişi

Azure portalında oluşturmadığınız ilkeleri geçirmeyi düşünün, çünkü:

- Artık daha önce işlenemediğiniz senaryoları ele alabilirsiniz.
- Bunları birleştirerek yönetmeniz gereken ilke sayısını azaltabilirsiniz.   
- Tüm Koşullu Erişim ilkelerinizi tek bir merkezi konumda yönetebilirsiniz.
- Azure klasik portalı kullanımdan kaldırıldı.   

Daha fazla bilgi için bkz. [Azure portalında klasik ilkeleri geçirme](policy-migration.md).

## <a name="next-steps"></a>Sonraki adımlar

Bilmek isterseniz:

- Koşullu Erişim ilkesini yapılandırma, [Azure Etkin Dizin Koşullu Erişim'e sahip belirli uygulamalar için MFA'yı zorunlu kılmasını nasıl](app-based-mfa.md)görebilirsiniz.
- Koşullu Erişim ilkelerinizi nasıl planlarınızda, [Azure Etkin Dizini'nde Koşullu Erişim dağıtımınızı nasıl planlarıizlene](plan-conditional-access.md)bakın.
