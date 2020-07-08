---
title: Azure AD Connect eşitleme hizmeti gölge öznitelikleri | Microsoft Docs
description: Azure AD Connect eşitleme hizmetinde gölge özniteliklerinin nasıl çalıştığını açıklar.
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
ms.topic: how-to
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c57ab03f72f0f59dd18a873ddc7cd98d3c36ef9c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85356670"
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Azure AD Connect eşitleme hizmeti gölge öznitelikleri
Çoğu öznitelik, Azure AD 'de şirket içi Active Directory olduklarından aynı şekilde temsil edilir. Ancak bazı özniteliklerin bazı özel işlemesi vardır ve Azure AD 'de öznitelik değeri Azure AD Connect eşitlenenden farklı olabilir.

## <a name="introducing-shadow-attributes"></a>Gölge öznitelikleri tanıtımı
Bazı özniteliklerin Azure AD 'de iki temsili vardır. Hem şirket içi değer hem de hesaplanmış bir değer depolanır. Bu ek özniteliklere gölge öznitelikleri denir. Bu davranışı gördüğünüz en yaygın iki öznitelik **userPrincipalName** ve **ProxyAddress**' dir. Öznitelik değerlerindeki değişiklik, bu özniteliklerde doğrulanmamış etki alanlarını temsil eden değerler olduğunda gerçekleşir. Ancak Connect 'teki eşitleme altyapısı, gölge özniteliğinde değeri, perspektiften bu şekilde okur, öznitelik Azure AD tarafından onaylandı.

Azure portal veya PowerShell 'i kullanarak gölge öznitelikleri göremezsiniz. Ancak kavramı anlamak, özniteliğin şirket içinde ve bulutta farklı değerlere sahip olduğu belirli senaryoları gidermenize yardımcı olur.

Davranışı daha iyi anlamak için Fabrikam ' dan bu örneğe bakın:  
![Etki Alanları](./media/how-to-connect-syncservice-shadow-attributes/domains.png)  
Şirket içi Active Directory birden çok UPN soneki vardır, ancak yalnızca bir tane doğrulanır.

### <a name="userprincipalname"></a>userPrincipalName
Kullanıcının doğrulanmamış bir etki alanında aşağıdaki öznitelik değerleri vardır:

| Öznitelik | Değer |
| --- | --- |
| Şirket içi userPrincipalName | lee.sperry@fabrikam.com |
| Azure AD shadowUserPrincipalName | lee.sperry@fabrikam.com |
| Azure AD userPrincipalName | lee.sperry@fabrikam.onmicrosoft.com |

UserPrincipalName özniteliği, PowerShell kullanırken gördüğünüz değerdir.

Gerçek şirket içi öznitelik değeri Azure AD 'de depolandığından, fabrikam.com etki alanını doğruladıktan sonra Azure AD, userPrincipalName özniteliğini shadowUserPrincipalName değeri ile güncelleştirir. Bu değerlerin güncelleştirilmesini sağlamak için Azure AD Connect yapılan değişiklikleri eşitlemeniz gerekmez.

### <a name="proxyaddresses"></a>proxyAddresses
Yalnızca doğrulanmış etki alanları da dahil olmak üzere aynı işlem, proxyAddresses için de, ancak bazı ek mantığlarla oluşur. Doğrulanan etki alanlarını denetleme yalnızca posta kutusu kullanıcıları için gerçekleşir. Posta etkin bir kullanıcı veya iletişim, başka bir Exchange kuruluşundaki bir kullanıcıyı temsil eder ve bu nesnelere proxyAddresses içinde herhangi bir değer ekleyebilirsiniz.

Şirket içi veya Exchange Online 'da posta kutusu kullanıcısı için yalnızca doğrulanmış etki alanı değerleri görünür. Şuna benzeyebilir:

| Öznitelik | Değer |
| --- | --- |
| Şirket içi proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

Bu durumda, bu etki alanı doğrulanmadığından, **SMTP: Abbie. Spencer \@ fabrikam.com** kaldırılmıştır. Ancak Exchange, **SIP: Abbie. Spencer \@ fabrikamonline.com**öğesini de ekledi. Fabrikam, şirket içi Lync/Skype kullanmadı, ancak Azure AD ve Exchange Online 'ı BT için hazırla.

ProxyAddresses için bu mantık, **Proxycalc**olarak adlandırılır. ProxyCalc, şu durumlarda bir Kullanıcı üzerindeki her değişiklik ile çağrılır:

- Kullanıcı Exchange için lisanslı olmasa bile Exchange Online 'ı içeren bir hizmet planı atandı. Örneğin, kullanıcıya Office E3 SKU 'SU atanmışsa ancak SharePoint Online atanırsa. Bu, posta kutunuz hala şirket içi olsa bile geçerlidir.
- MsExchRecipientTypeDetails özniteliği bir değere sahip.
- ProxyAddresses veya userPrincipalName ' de bir değişiklik yaparsınız.

ProxyCalc, Kullanıcı üzerinde bir değişikliği işlemek için biraz zaman alabilir ve Azure AD Connect dışa aktarma işlemiyle zaman uyumlu değildir.

> [!NOTE]
> ProxyCalc mantığının, bu konuda belgelenmeyen gelişmiş senaryolar için bazı ek davranışları vardır. Bu konu, davranışı anlamanız ve tüm iç mantığın belgeleyeceği için verilmiştir.

### <a name="quarantined-attribute-values"></a>Karantinaya alınan öznitelik değerleri
Gölge öznitelikleri, yinelenen öznitelik değerleri olduğunda da kullanılır. Daha fazla bilgi için bkz. [yinelenen öznitelik dayanıklılığı](how-to-connect-syncservice-duplicate-attribute-resiliency.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory Ile tümleştirme](whatis-hybrid-identity.md).
