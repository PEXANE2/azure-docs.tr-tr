---
title: 'Azure AD Connect eşitleme: Kullanıcıları, Grupları ve Kişileri Anlama | Microsoft Dokümanlar'
description: Azure AD Connect eşitlemesinde kullanıcıları, grupları ve kişileri açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 661747754369c17ca98ae69d477e04124b6a2942
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60245486"
---
# <a name="azure-ad-connect-sync-understanding-users-groups-and-contacts"></a>Azure AD Connect eşitleme: Kullanıcıları, Grupları ve Kişileri Anlama
Birden çok Active Directory ormanları olurdu ve birkaç farklı dağıtım topolojisi vardır neden birkaç farklı nedeni vardır. Ortak modeller arasında bir birleşme & satın alma dan sonra hesap kaynağı dağıtımı ve GAL senkronize ormanları yer alıyor. Ama saf modeller olsa bile, hibrid modeller de yaygındır. Azure AD Connect eşitlemesinde varsayılan yapılandırma belirli bir modeli kabul etmez, ancak yükleme kılavuzunda kullanıcı eşleştirmesinin nasıl seçildiğine bağlı olarak farklı davranışlar gözlemlenebilir.

Bu konuda, varsayılan yapılandırmanın belirli topolojilerde nasıl bir şekilde nasıl bir şekilde nasıl bir şekilde nasıl olduğunu inceeceğiz. Yapılandırmaüzerinden gideceğiz ve Eşitleme Kuralları Düzenleyicisi yapılandırmaya bakmak için kullanılabilir.

Yapılandırmanın varsaydÜstlenmesinin varsaydığı birkaç genel kural vardı:
* Etkin Dizinler kaynağından hangi siparişi aldığımıza bakılmaksızın, sonuç her zaman aynı olmalıdır.
* Etkin bir hesap her zaman **userPrincipalName** ve **sourceAnchor**dahil olmak üzere oturum açma bilgilerine katkıda bulunur.
* Devre dışı bırakılmış bir hesap, etkin bir hesap bulunamıyorsa, bağlı bir posta kutusu olmadığı sürece userPrincipalName ve sourceAnchor'a katkıda bulunur.
* Bağlantılı posta kutusu olan bir hesap hiçbir zaman userPrincipalName ve sourceAnchor için kullanılmaz. Etkin bir hesabın daha sonra bulunacağı varsayılır.
* İlgili kişi nesnesi Azure AD'ye kişi veya kullanıcı olarak sağlanabilir. Tüm kaynak Aktif Dizin ormanları işlenene kadar gerçekten bilemezsin.

## <a name="groups"></a>Gruplar
Active Directory'den Azure AD'ye grupları eşitlerken dikkat edilmesi gereken önemli noktalar:

* Azure AD Connect yerleşik güvenlik gruplarını dizin eşitlemesinden dışlar.

* Azure AD Connect, Birincil [Grup üyeliklerini](https://technet.microsoft.com/library/cc771489(v=ws.11).aspx) Azure AD ile eşitlemeyi desteklemez.

* Azure AD Connect, Dinamik [Dağıtım Grubu üyeliklerini](https://technet.microsoft.com/library/bb123722(v=exchg.160).aspx) Azure AD ile eşitlemeyi desteklemez.

* Etkin Dizin grubunu, posta özellikli bir grup olarak Azure AD ile eşitlemek için:

    * Grubun *proxyAddress* özniteliği boşsa, *posta* özniteliğinin bir değeri olmalıdır

    * Grubun *proxyAddress* özniteliği boş değilse, en az bir SMTP proxy adresi değeri içermelidir. İşte bazı örnekler:
    
      * ProxyAddress özniteliği *{"X500:/0=contoso.com/ou=users/cn=testgroup"}* değeri olan bir Etkin Dizin grubu Azure AD'de posta yla etkinleştirilmez. SMTP adresi yok.
      
      * ProxyAddress özniteliği *\@{"X500:/0=contoso.com/ou=users/cn=testgroup","SMTP:johndoe contoso.com"}* değerleri olan bir Active Directory grubu Azure AD'da posta yla etkinleştirilir.
      
      * ProxyAddress özniteliği *{"X500:/0=contoso.com/ou=users/cn=testgroup",\@"smtp:johndoe contoso.com"}* değerleri olan bir Active Directory grubu da Azure AD'de posta yla etkinleştirilir.

## <a name="contacts"></a>Kişiler
Bir GALSync çözümiki veya daha fazla Exchange ormanları köprüleme bir birleşme & satın alma sonra farklı bir ormanda bir kullanıcı temsil eden kişiler olması yaygındır. İlgili kişi nesnesi her zaman posta özniteliğini kullanarak bağlayıcı alanından metaverse katılıyor. Zaten aynı posta adresine sahip bir kişi nesnesi veya kullanıcı nesnesi varsa, nesneler birleştirilir. Bu, **AD 'den In – Contact Join**kuralında yapılandırılmıştır. **Ad'den In** adlı bir kural da vardır – Sabit **Kişi**ile metaverse öznitelik **kaynağıObjectType** bir öznitelik akışı ile Ortak İletişim . Bu kuralın önceliği çok düşüktür, bu nedenle herhangi bir kullanıcı nesnesi aynı metaverse nesneye katılırsa, **AD - User Common** kuralı kullanıcıya bu öznitelik için değer katacaktır. Bu kuralla, kullanıcı birleştirilememişse bu öznitelik, Kişi Ile Ilgili değere ve en az bir kullanıcı bulunmuşsa Kullanıcı değerine sahip olacaktır.

Bir nesneyi Azure AD'ye sağlamak için, **AAD - Contact Join'e** giden ekarte, metaverse öznitelik **kaynağıObjectType** **Kişi**olarak ayarlanmışsa bir ilgili kişi nesnesi oluşturur. Bu öznitelik **Kullanıcı**ya **ayarlanmışsa, AAD- User Join** için ekarte bunun yerine bir kullanıcı nesnesi oluşturur.
Daha fazla kaynak Etkin Dizin içe aktarıldığında ve eşitlendiğinde bir nesnenin Kişiden Kullanıcıya yükseltilmesi mümkündür.

Örneğin, galsync topolojisinde ilk ormanı ithal ettiğimizde ikinci ormandaki herkes için iletişim nesneleri bulacağız. Bu, AAD Bağlayıcısı'nda yeni iletişim nesneleri sahneleecektir. Daha sonra ikinci ormanı içe aktarıp senkronize ettiğimizde, gerçek kullanıcıları bulup mevcut metaverse nesnelere katılacağız. Daha sonra AAD'deki kişi nesnesini sileriz ve bunun yerine yeni bir kullanıcı nesnesi oluştururuz.

Kullanıcıların ilgili kişi olarak temsil edildiği bir topolojiniz varsa, yükleme kılavuzundaki posta özniteliğindeki kullanıcıları eşleştirmeyi seçtiğinizden emin olun. Başka bir seçenek seçerseniz, siparişe bağımlı bir yapılandırmanız olur. İlgili kişi nesneleri her zaman posta özniteliğine katılır, ancak kullanıcı nesneleri yalnızca yükleme kılavuzunda bu seçenek seçilirse posta özniteliğine katılır. Daha sonra, kişi nesnesi kullanıcı nesnesinden önce içe aktarıldıysa, metaverse'de aynı posta özniteliğiyle iki farklı nesne yle birlikte sona erebilirsiniz. Azure AD'ye dışa aktarma sırasında bir hata atılır. Bu davranış tasarım gereğidir ve hatalı verileri veya yükleme sırasında topolojinin doğru şekilde tanımlanmadığını gösterir.

## <a name="disabled-accounts"></a>Devre dışı bırakılan hesaplar
Devre dışı bırakılan hesaplar Azure AD ile de senkronize edilir. Devre dışı bırakılan hesaplar, örneğin konferans salonları gibi Exchange'deki kaynakları temsil etmek için yaygındır. Bunun istisnası, bağlı posta kutusu olan kullanıcılardır; Daha önce de belirtildiği gibi, bunlar hiçbir zaman Azure AD'ye bir hesap sağlamaz.

Varsayım, devre dışı bırakılmış bir kullanıcı hesabı bulunursa, daha sonra başka bir etkin hesap bulamayız ve nesne, bulunan userPrincipalName ve sourceAnchor ile Azure AD'ye verilir. Başka bir etkin hesabın aynı metaverse nesneye katılması durumunda, kullanıcıPrincipalName ve sourceAnchor kullanılacaktır.

## <a name="changing-sourceanchor"></a>Kaynak DeğiştirmeÇapa
Bir nesne Azure AD'ye dışa aktarıldığında artık kaynağı Değiştirmesine izin verilmez. Nesne dışa aktarıldığında metaverse öznitelik **cloudSourceAnchor,** Azure AD tarafından kabul edilen **kaynakÇapa** değeriyle ayarlanır. **SourceAnchor** değiştirilir ve **cloudSourceAnchor**eşleşmiyorsa , **AAD için** kural Out - Kullanıcı Join hata **kaynağıÇapa özniteliği değişti**atacaktır . Bu durumda, nesne yeniden senkronize edilmeden önce aynı kaynak Anchor metaverse tekrar mevcut böylece yapılandırma veya veri düzeltilmelidir.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure AD Connect Eşitleme: Senkronizasyon seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

