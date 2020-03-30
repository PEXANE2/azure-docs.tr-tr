---
title: "Azure AD Bağlantısı: Azure REKLAM'ına zaten sahipseniz | Microsoft Dokümanlar"
description: Bu konu, varolan bir Azure AD kiracınız olduğunda Bağlan'ın nasıl kullanılacağını açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3636b88b14cf7e76e4fb023434316e7ee31ded04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71336815"
---
# <a name="azure-ad-connect-when-you-have-an-existent-tenant"></a>Azure AD Connect: Varolan bir kiracınız varsa
Azure AD Connect'in nasıl kullanılacağına dair konuların çoğu, yeni bir Azure AD kiracısıyla başladığınızı ve orada kullanıcı veya başka nesne bulunmadığını varsayar. Ancak bir Azure REKLAM kiracısıyla başladıysanız, kullanıcılar ve diğer nesnelerle doldurulduysanız ve şimdi Connect'i kullanmak istiyorsanız, bu konu tam size göre.

## <a name="the-basics"></a>Temel bilgiler
Azure AD'deki bir nesne bulutta (Azure AD) veya şirket içinde ustadır. Tek bir nesne için, Azure AD'deki bazı öznitelikleri ve diğer bazı öznitelikleri şirket içinde yönetemezsiniz. Her nesnenin, nesnenin yönetildiği yeri gösteren bir bayrağı vardır.

Bazı kullanıcıları şirket içinde ve diğer kullanıcıları bulutta yönetebilirsiniz. Bu yapılandırma için ortak bir senaryo muhasebe çalışanları ve satış çalışanları nın bir karışımı olan bir kuruluştur. Muhasebe çalışanlarının şirket içi bir AD hesabı vardır, ancak satış çalışanlarının yok, Azure AD'de bir hesapları vardır. Bazı kullanıcıları şirket içinde, bazılarını da Azure AD'de yönetebilirsiniz.

Azure AD'de de şirket içi REKLAM'da bulunan kullanıcıları yönetmeye başladıysanız ve daha sonra Bağlantı'yı kullanmak istiyorsanız, göz önünde bulundurmanız gereken bazı ek endişeler vardır.

## <a name="sync-with-existing-users-in-azure-ad"></a>Azure AD'deki mevcut kullanıcılarla eşitleme
Azure AD Connect'i yüklediğinizde ve eşitlemeye başladığınızda, Azure AD eşitleme hizmeti (Azure AD'de) her yeni nesneyi denetler ve eşleşecek varolan bir nesne bulmaya çalışır. Bu işlem için kullanılan üç öznitelikleri vardır: **userPrincipalName**, **proxyAdresleri**ve **sourceAnchor**/**immutableID**. **userPrincipalName** ve **proxyAdresler** bir maç **yumuşak**bir eşleşme olarak bilinir. **sourceAnchor'daki** bir eşleşme **sert eşleşme**olarak bilinir. **ProxyAdresleri** için yalnızca **SMTP**ile değer atfetmek: , birincil e-posta adresi, değerlendirme için kullanılır.

Eşleşme yalnızca Connect'ten gelen yeni nesneler için değerlendirilir. Varolan bir nesneyi, bu özniteliklerden herhangi birini eşleştireceği şekilde değiştirirseniz, bunun yerine bir hata görürsünüz.

Azure AD, Connect'ten gelen bir nesne için öznitelik değerlerinin aynı olduğu ve Azure AD'de zaten mevcut olan bir nesne bulursa, Azure AD'deki nesne Connect tarafından devralınır. Daha önce bulut tarafından yönetilen nesne şirket içinde yönetilen olarak işaretlenir. Azure AD'deki şirket içi AD değerine sahip tüm öznitelikler şirket içi değerle birlikte üzerine yazılır. Özel durum, bir özniteliğin şirket içinde **NULL** değeri ne zaman olmasıdır. Bu durumda, Azure AD'deki değer kalır, ancak yine de şirket içinde yalnızca başka bir şeyle değiştirebilirsiniz.

> [!WARNING]
> Azure AD'deki tüm öznitelikler şirket içi değere göre üzerine yazılacağınız için, şirket içinde iyi verilere sahip olduğundan emin olun. Örneğin, yalnızca Office 365'te e-posta adresini yönettiyseniz ve şirket içi AD DS'de güncel tutmadıysanız, AD DS'de bulunmayan Azure AD/Office 365'teki tüm değerleri kaybedersiniz.

> [!IMPORTANT]
> Her zaman ekspres ayarlar tarafından kullanılan parola eşitleme kullanıyorsanız, Azure AD'deki parola şirket içi AD'deki parolayla birlikte üzerine yazılır. Kullanıcılarınız farklı parolaları yönetmek için kullanılıyorsa, Connect'i yüklediğinizde şirket içi parolayı kullanmaları gerektiğini bildirmeniz gerekir.

Önceki bölüm ve uyarı planlamanızda dikkate alınmalıdır. Azure AD'de şirket içi AD DS'de yansıtılmayan birçok değişiklik yaptıysanız, nesnelerinizi Azure AD Connect ile eşitlemeden önce AD DS'yi güncelleştirilmiş değerlerle nasıl doldurabileceğinizi planlamanız gerekir.

Nesnelerinizi yumuşak bir eşleşmeyle eşlediyseniz, **daha** sonra zor bir eşleşme kullanılabilmek için kaynak Anchor Azure AD'deki nesneye eklenir.

>[!IMPORTANT]
> Microsoft, Azure Active Directory'de şirket içi hesapları önceden varolan yönetim hesaplarıyla eşitlememenizi şiddetle önerir.

### <a name="hard-match-vs-soft-match"></a>Sert maç vs Soft-match
Connect'in yeni bir kurulumu için yumuşak ve sert eşleşme arasında pratik bir fark yoktur. Aradaki fark bir felaket kurtarma durumudur. Azure AD Connect ile sunucunuzu kaybettiyseniz, veri kaybetmeden yeni bir örneği yeniden yükleyebilirsiniz. İlk yükleme sırasında Connect'e kaynak Çapa'lı bir nesne gönderilir. Bu eşleşme daha sonra, Azure AD'de aynı şeyi yapmaktan çok daha hızlı olan istemci (Azure AD Connect) tarafından değerlendirilebilir. Zor eşleşme hem Connect hem de Azure AD tarafından değerlendirilir. Yumuşak bir eşleşme yalnızca Azure AD tarafından değerlendirilir.

### <a name="other-objects-than-users"></a>Kullanıcılar dan diğer nesneler
Posta özellikli gruplar ve kişiler için proxyAdres'leri temel alan yumuşak eşleşebilirsiniz. Yalnızca kaynakAnchor/immutableID'yi (PowerShell kullanarak) yalnızca Kullanıcılar üzerinde güncellebildiğiniz için zor eşleşme geçerli değildir. Posta yla etkin olmayan gruplar için şu anda yumuşak eşleşme veya zor eşleşme desteği yok.

### <a name="admin-role-considerations"></a>Yönetici rol değerlendirmeleri
Azure AD Connect, güvenilmeyen şirket içi kullanıcıların yönetici rolü olan bir bulut kullanıcısıyla eşleştirilmesini önlemek için şirket içi kullanıcı nesnelerini yönetici rolü olan nesnelerle eşleştirmez. Bu varsayılan olarak. Bu davranışı geçici olarak çözüme getirmek için aşağıdakileri yapabilirsiniz:

1.  Dizin rollerini yalnızca bulut kullanıcı nesnesinden kaldırın.
2.  Başarısız bir kullanıcı eşitleme denemesi varsa, buluttaki Karantina nesnesini zorsilin.
3.  Eşitlemeyi tetikle.
4.  Eşleç oluştuktan sonra dizin rollerini buluttaki kullanıcı nesnesine isteğe bağlı olarak ekleyin.



## <a name="create-a-new-on-premises-active-directory-from-data-in-azure-ad"></a>Azure AD'deki verilerden yeni bir şirket içi Active Directory oluşturun
Bazı müşteriler Azure AD ile yalnızca buluta yönelik bir çözümle başlar ve şirket içi AD'leri yoktur. Daha sonra şirket içi kaynakları tüketmek ve Azure AD verilerine dayalı bir şirket içi REKLAM oluşturmak isterler. Azure AD Connect bu senaryo için size yardımcı olamaz. Şirket içinde kullanıcı oluşturmaz ve parolayı Azure AD'dekiyle aynı şirket içinde ayarlama olanağı yoktur.

Şirket içi AD eklemeyi planlamanızın tek nedeni LOB'ları (Business'ın Satırı uygulamaları) desteklemekse, belki de bunun yerine [Azure AD etki alanı hizmetlerini](../../active-directory-domain-services/index.yml) kullanmayı düşünmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
