---
title: Azure AD'de etkin olmayan kullanıcı hesapları nasıl yönetilir | Microsoft Dokümanlar
description: Azure AD'de eskimiş kullanıcı hesaplarını nasıl algılayıp işleyeceğiniz hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/07/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56e44059268037cfd839fc7c877c5d6c972dead8
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886050"
---
# <a name="how-to-manage-inactive-user-accounts-in-azure-ad"></a>Nasıl Olun: Azure AD'de etkin olmayan kullanıcı hesaplarını yönetme

Büyük ortamlarda, çalışanlar bir kuruluştan ayrıldıklarında kullanıcı hesapları her zaman silinmez. BT yöneticisi olarak, bu eski kullanıcı hesaplarını algılamak ve işlemek istersiniz, çünkü bunlar bir güvenlik riski oluşturur.

Bu makalede, Azure AD'deki eski kullanıcı hesaplarını işlemek için bir yöntem açıklanmaktadır. 

## <a name="what-are-inactive-user-accounts"></a>Etkin olmayan kullanıcı hesapları nelerdir?

Etkin olmayan hesaplar, kuruluşunuz üyeleri tarafından artık kaynaklarınıza erişim sağlamak için gerekli olmayan kullanıcı hesaplarıdır. Etkin olmayan hesaplar için önemli bir tanımlayıcı, ortamınızda oturum açmaiçin *bir süredir* kullanılmamış olmalarıdır. Etkin olmayan hesaplar oturum açma etkinliğine bağlı olduğundan, bunları algılamak için başarılı olan son oturum açma nın zaman damgasını kullanabilirsiniz. 

Bu yöntemin zorluğu, ortamınızın durumunda *bir süre için* ne anlama geldiğini tanımlamaktır. Örneğin, kullanıcılar tatilde oldukları için *bir*ortamda bir süre oturum açamayabilir. Etkin olmayan kullanıcı hesapları için deltanızın ne olduğunu tanımlarken, ortamınızda oturum açmamanız için tüm meşru nedenleri hesaba katmalısınız. Birçok kuruluşta, etkin olmayan kullanıcı hesapları için delta 90 ile 180 gün arasındadır. 

Son başarılı oturum açma, kullanıcının kaynaklara sürekli erişim gereksinimine ilişkin potansiyel öngörüler sağlar.  Grup üyeliğine veya uygulama erişimine hala ihtiyaç olup olmadığını veya kaldırılıp kaldırılabileceğini belirlemede yardımcı olabilir. Dış kullanıcı yönetimi için, harici bir kullanıcının kiracı içinde hala etkin olup olmadığını veya temizlenmesi gerektiğini anlayabilirsiniz. 

    
## <a name="how-to-detect-inactive-user-accounts"></a>Etkin olmayan kullanıcı hesapları nasıl algılatır?

**Microsoft Graph** API'nin **signInActivity** kaynak türüne göre ortaya çıkarılan **son SignInDateTime** özelliğini değerlendirerek etkin olmayan hesapları algılarsınız. Bu özelliği kullanarak, aşağıdaki senaryolar için bir çözüm uygulayabilirsiniz:

- **Adlarına göre kullanıcılar**: Bu senaryoda, belirli bir kullanıcıyı ada göre ararsınız, bu da son SignInDate'i değerlendirmenizi sağlar:`https://graph.microsoft.com/beta/users?$filter=startswith(displayName,'markvi')&$select=displayName,signInActivity`

- **Tarihe göre kullanıcılar**: Bu senaryoda, belirli bir tarihten önce lastSignInDateTime olan kullanıcıların listesini isteyin:`https://graph.microsoft.com/beta/users?filter=signInActivity/lastSignInDateTime le 2019-06-01T00:00:00Z`






## <a name="what-you-need-to-know"></a>Bilmeniz gerekenler

Bu bölümde, son SignInDateTime özelliği hakkında bilmeniz gerekenler listelenebileniz.

### <a name="how-can-i-access-this-property"></a>Bu tesise nasıl erişebilirim?

**SonSignInDateTime** özelliği [Microsoft Graph REST API](https://docs.microsoft.com/graph/overview?view=graph-rest-beta#whats-in-microsoft-graph) [signInActivity kaynak türü](https://docs.microsoft.com/graph/api/resources/signinactivity?view=graph-rest-beta) tarafından ortaya çıkar.   

### <a name="is-the-lastsignindatetime-property-available-through-the-get-azureaduser-cmdlet"></a>Son SignInDateTime özelliği Get-AzureAdUser cmdlet üzerinden kullanılabilir mi?

Hayır.

### <a name="what-edition-of-azure-ad-do-i-need-to-access-the-property"></a>Özellik için Azure AD'nin hangi sürümüne erişmem gerekir?

Bu özelliğe Azure AD'nin tüm sürümlerinde erişebilirsiniz.

### <a name="what-permission-do-i-need-to-read-the-property"></a>Mülkü okumak için ne gibi izinlere ihtiyacım var?

Bu özelliği okumak için aşağıdaki hakları vermeniz gerekir: 

- AuditLogs.Read.All
- Organizasyon.Read.All  


### <a name="when-does-azure-ad-update-the-property"></a>Azure AD özelliği ne zaman günceller?

Temel veri deposunun güncelleştirmesinde başarılı sonuçlar elde edilen her etkileşimli oturum açma. Genellikle, başarılı oturum açma lar ilgili oturum açma raporunda 10 dakika içinde ortaya çıkar.
 

### <a name="what-does-a-blank-property-value-mean"></a>Boş bir özellik değeri ne anlama gelir?

Son SignInDateTime zaman damgası oluşturmak için başarılı bir oturum açmanız gerekir. LastSignInDateTime özelliği yeni bir özellik olduğundan, aşağıdaki durumlarda son SignInDateTime özelliğinin değeri boş olabilir:

- Bir kullanıcının son başarılı oturum açma özelliği bu özellik yayımlanmadan önce gerçekleşmişti (1 Aralık 2019).
- Etkilenen kullanıcı hesabı başarılı bir oturum açma için hiçbir zaman kullanılmadı.

## <a name="next-steps"></a>Sonraki adımlar

* [Sertifikalarla Azure Active Directory raporlama API’sini kullanarak veri alma](tutorial-access-api-with-certificates.md)
* [Denetim API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma faaliyet raporu API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
