---
title: Azure AD Connect eşitleme hizmeti gölge öznitelikleri | Microsoft Dokümanlar
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
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10a4078f49abbdf431f42c6cde7cf882112e5848
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60384718"
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Azure AD Connect eşitleme hizmeti gölge öznitelikleri
Çoğu öznitelik, Azure AD'de şirket içi Active Directory'nizde olduğu gibi aynı şekilde temsil edilir. Ancak bazı özniteliklerin bazı özel kullanımları vardır ve Azure AD'deki öznitelik değeri Azure AD Connect'in eşitledüğünden farklı olabilir.

## <a name="introducing-shadow-attributes"></a>Gölge özniteliklerini tanıtma
Bazı özniteliklerin Azure AD'de iki gösterimi vardır. Hem şirket içi değer hem de hesaplanan değer depolanır. Bu ek özniteliklere gölge öznitelikleri denir. Bu davranışı gördüğünüz iki en yaygın öznitelikleri **userPrincipalName** ve **proxyAddress**vardır. Öznitelik değerlerindeki değişiklik, bu özniteliklerde doğrulanmamış etki alanlarını temsil eden değerler olduğunda gerçekleşir. Ancak Connect'teki eşitleme altyapısı gölge özniteliğindeki değeri okur, bu nedenle kendi bakış açısından, öznitelik Azure AD tarafından onaylanmıştır.

Azure portalını kullanarak veya PowerShell ile gölge özniteliklerini göremezsiniz. Ancak kavramı anlamak, özniteliğin şirket içinde ve bulutta farklı değerlere sahip olduğu belirli senaryoları gidermenize yardımcı olur.

Davranışı daha iyi anlamak için Fabrikam'daki şu örneğe bakın:  
![Etki Alanları](./media/how-to-connect-syncservice-shadow-attributes/domains.png)  
Şirket içi Active Directory'lerinde birden çok UPN sonekleri vardır, ancak yalnızca birini doğrulayabiliyorlar.

### <a name="userprincipalname"></a>userPrincipalName
Kullanıcı doğrulanmamış bir etki alanında aşağıdaki öznitelik değerlerine sahiptir:

| Öznitelik | Değer |
| --- | --- |
| şirket içi kullanıcıPrincipalName | lee.sperry@fabrikam.com |
| Azure AD gölgeSIKullanıcıPrincipalName | lee.sperry@fabrikam.com |
| Azure AD kullanıcıPrincipalName | lee.sperry@fabrikam.onmicrosoft.com |

UserPrincipalName özniteliği PowerShell kullanırken gördüğünüz değerdir.

Gerçek şirket içi öznitelik değeri Azure AD'de depolandığından, fabrikam.com etki alanını doğruladığınızda, Azure AD kullanıcıPrincipalName özniteliğini gölgeUserPrincipalName'deki değerle güncelleştirir. Bu değerlerin güncelleştirilemesi için Azure AD Connect'ten herhangi bir değişikliği eşitlemeniz gerekmez.

### <a name="proxyaddresses"></a>proxyAddresses
Yalnızca doğrulanmış etki alanları da dahil olmak üzere aynı işlem proxyAdresleri için de oluşur, ancak bazı ek mantık ile. Doğrulanmış etki alanları için denetim yalnızca posta kutusu kullanıcıları için olur. Posta yla etkinleştirilmiş bir kullanıcı veya kişi başka bir Exchange kuruluşunda bir kullanıcıyı temsil eder ve proxyAdreslerindeki herhangi bir değeri bu nesnelere ekleyebilirsiniz.

Bir posta kutusu kullanıcısı için şirket içi veya Exchange Online'da yalnızca doğrulanmış etki alanları için değerler görüntülenir. Şuna benziyor olabilir:

| Öznitelik | Değer |
| --- | --- |
| şirket içi proxyAdresler | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAdresleri | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

Bu durumda **\@smtp:abbie.spencer fabrikam.com** bu etki alanı doğrulanmadı ğından kaldırıldı. Ama Exchange de **SIP\@ekledi:abbie.spencer fabrikamonline.com**. Fabrikam, Lync/Skype'ı şirket içinde kullanmamıştır, ancak Azure AD ve Exchange Online buna hazırlanmıştır.

ProxyAdresler için bu mantık **ProxyCalc**olarak adlandırılır. ProxyCalc, kullanıcıüzerindeki her değişiklikle birlikte şu anda çağrılır:

- Kullanıcı, Exchange lisansı olmasa bile Exchange Online'ı içeren bir hizmet planı atanır. Örneğin, kullanıcıya Office E3 SKU atanır, ancak yalnızca SharePoint Online atanır. Posta kutunuz hala şirket içinde olsa bile bu durum geçerlidir.
- Öznitelik msExchRecipientTypeDetails bir değeri vardır.
- ProxyAdres'lerde veya userPrincipalName'de değişiklik yaparsınız.

ProxyCalc'ın bir kullanıcı üzerinde bir değişikliği işleme salması biraz zaman alabilir ve Azure AD Connect dışa aktarma işlemiyle eşzamanlı değildir.

> [!NOTE]
> ProxyCalc mantığı, bu konuda belgelenmemiş gelişmiş senaryolar için bazı ek davranışlara sahiptir. Bu konu, davranışı anlamanız ve tüm iç mantığı belgelememeniz için sağlanır.

### <a name="quarantined-attribute-values"></a>Karantinaya alınmış öznitelik değerleri
Yinelenen öznitelik değerleri olduğunda gölge öznitelikleri de kullanılır. Daha fazla bilgi için [yinelenen öznitelik esnekliğine](how-to-connect-syncservice-duplicate-attribute-resiliency.md)bakın.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Etkin Dizini ile tümleştirme.](whatis-hybrid-identity.md)
