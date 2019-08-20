---
title: 'Azure Active Directory Domain Services: Yönetilen etki alanlarında eşitleme | Microsoft Docs'
description: Azure Active Directory Domain Services yönetilen bir etki alanında eşitlemeyi anlama
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 57cbf436-fc1d-4bab-b991-7d25b6e987ef
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 1c52ac967d241f31d96988fa5ead8b4e049f6f4c
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617095"
---
# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen bir etki alanında eşitleme
Aşağıdaki diyagramda, Azure AD Domain Services yönetilen etki alanlarında eşitlemenin nasıl çalıştığı gösterilmektedir.

![Azure AD Domain Services eşitleme](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Şirket içi dizininizden Azure AD kiracınıza eşitleme
Azure AD Connect eşitleme, Kullanıcı hesaplarını, grup üyeliklerini ve kimlik bilgisi karmalarını Azure AD kiracınızla eşitlemek için kullanılır. UPN ve şirket içi SID (güvenlik tanımlayıcısı) gibi kullanıcı hesaplarının öznitelikleri eşitlenir. Azure AD Domain Services kullanıyorsanız, NTLM ve Kerberos kimlik doğrulaması için gereken eski kimlik bilgisi karmaları da Azure AD kiracınızla eşitlenir.

Geri yazma 'yı yapılandırırsanız Azure AD dizininizde gerçekleşen değişiklikler şirket içi Active Directory geri eşitlenir. Örneğin, Azure AD self servis parola yönetimi kullanarak parolanızı değiştirirseniz, değiştirilen parola şirket içi AD etki alanında güncelleştirilir.

> [!NOTE]
> Bilinen tüm hatalara yönelik düzeltmelerinizi sağlamak için her zaman Azure AD Connect en son sürümünü kullanın.
>
>

## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Azure AD kiracınızdan yönetilen etki alanına eşitleme
Kullanıcı hesapları, grup üyelikleri ve kimlik bilgisi karmaları, Azure AD kiracınızdan Azure AD Domain Services yönetilen etki alanına eşitlenir. Bu eşitleme işlemi otomatiktir. Bu eşitleme işlemini yapılandırmanız, izlemeniz veya yönetmeniz gerekmez. İlk eşitleme, Azure AD dizininizdeki nesne sayısına bağlı olarak birkaç saat ile birkaç güne kadar sürebilir. İlk eşitleme tamamlandıktan sonra, Azure AD 'de yapılan değişikliklerin yönetilen etki alanında güncelleştirilmesini 20-30 dakika sürer. Bu eşitleme aralığı, parola değişiklikleri veya Azure AD 'de yapılan özniteliklerde yapılan değişiklikler için geçerlidir.

Eşitleme işlemi aynı zamanda tek yönlü/tek yönlü bir şekilde oluşturulur. Yönetilen etki alanınız büyük ölçüde salt okunurdur ve oluşturduğunuz özel OU 'Lar hariç olur. Bu nedenle, yönetilen etki alanı içindeki Kullanıcı öznitelikleri, kullanıcı parolaları veya grup üyelikleri üzerinde değişiklik yapamazsınız. Sonuç olarak, yönetilen etki alanınızı Azure AD kiracınıza geri doğru bir şekilde eşitleme yoktur.

## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Çok ormanlı şirket içi ortamdan eşitleme
Birçok kuruluş, birden çok hesap ormanlarından oluşan oldukça karmaşık, şirket içi kimlik altyapısına sahiptir. Azure AD Connect, çok ormanlı ortamlarınızdaki kullanıcıların, grupların ve kimlik bilgisi karmalarının Azure AD kiracınıza eşitlenmesini destekler.

Buna karşılık, Azure AD kiracınız daha basit ve düz bir ad alanıdır. Kullanıcıların Azure AD tarafından güvenli hale getirilmiş uygulamalara güvenilir bir şekilde erişmesini sağlamak için, farklı ormanlardaki Kullanıcı hesapları genelinde UPN çakışmalarını çözün. Azure AD Domain Services yönetilen etki alanınızı Azure AD kiracınıza benzer şekilde kapatın. Yönetilen etki alanında düz bir OU yapısı görürsünüz. Tüm Kullanıcı hesapları ve grupları, farklı şirket içi etki alanlarından veya ormanlardan eşitlenene rağmen ' AADDC Users ' kapsayıcısı içinde depolanır. Şirket içinde hiyerarşik bir OU yapısı yapılandırmış olabilirsiniz. Yönetilen etki alanınız hala basit bir düz OU yapısına sahiptir.

## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Dışlamalar-yönetilen etki alanınız ile eşitlenmemiş
Aşağıdaki nesneler veya öznitelikler Azure AD kiracınızla veya yönetilen etki alanınız ile eşitlenmez:

* **Dışlanan öznitelikler:** Azure AD Connect kullanarak, belirli özniteliklerin Azure AD kiracınıza eşitlenmesini, şirket içi etki alanından dışlanmasını seçebilirsiniz. Bu Dışlanan öznitelikler yönetilen etki alanında kullanılamaz.
* **Grup Ilkeleri:** Şirket içi etki alanında yapılandırılan Grup Ilkeleri, yönetilen etki alanınız ile eşitlenmez.
* **SYSVOL paylaşma:** Benzer şekilde, şirket içi etki alanındaki SYSVOL paylaşımının içeriği, yönetilen etki alanınız ile eşitlenmez.
* **Bilgisayar nesneleri:** Şirket içi etki alanına katılmış bilgisayarlar için bilgisayar nesneleri, yönetilen etki alanınız ile eşitlenmez. Bu bilgisayarların, yönetilen etki alanı ile bir güven ilişkisi yoktur ve yalnızca şirket içi etki alanına ait değildir. Yönetilen etki alanında, bilgisayar nesnelerini yalnızca, yönetilen etki alanına yalnızca etki alanına katılmış olan bilgisayarlar için bulabilirsiniz.
* **Kullanıcılar ve gruplar için Sıdhistory öznitelikleri:** Şirket içi etki alanındaki birincil kullanıcı ve birincil Grup SID 'Leri, yönetilen etki alanınız ile eşitlenir. Ancak, kullanıcılar ve gruplar için mevcut Sıdhistory öznitelikleri, şirket içi etki alanından yönetilen etki alanınız ile eşitlenmez.
* **Kuruluş birimleri (OU) yapıları:** Şirket içi etki alanında tanımlanan kuruluş birimleri, yönetilen etki alanınız ile eşitlenmez. Yönetilen etki alanında iki yerleşik OU vardır. Varsayılan olarak, yönetilen etki alanınız düz bir OU yapısına sahiptir. Bununla birlikte, [yönetilen etki alanında özel bır OU oluşturmayı](create-ou.md)seçebilirsiniz.

## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Belirli özniteliklerin yönetilen etki alanınız ile eşitlenmesi
Aşağıdaki tabloda bazı yaygın öznitelikler listelenmekte ve yönetilen etki alanınız ile nasıl eşitlendikleri açıklanmaktadır.

| Yönetilen etki alanındaki özniteliği | Source | Notlar |
|:--- |:--- |:--- |
| UPN |Azure AD kiracınızdaki kullanıcının UPN özniteliği |Azure AD kiracınızdan UPN özniteliği, yönetilen etki alanınız için olduğu gibi eşitlenir. Bu nedenle, yönetilen etki alanında oturum açmak için en güvenilir yol UPN 'nizi kullanmaktır. |
| Hesap |Azure AD kiracınızdaki veya otomatik olarak oluşturulan kullanıcının Mailrumuz özniteliği |SAMAccountName özniteliği Azure AD kiracınızdaki Mailrumuz özniteliğinden kaynaklıdır. Birden çok kullanıcı hesabı aynı Mailrumuz özniteliğine sahip ise, SAMAccountName otomatik olarak oluşturulur. Kullanıcının Mailrumuz veya UPN ön eki 20 karakterden uzunsa, sAMAccountName öznitelikleri üzerinde 20 karakter sınırını karşılamak için SAMAccountName otomatik olarak oluşturulur. |
| Parolalar |Azure AD kiracınızdan kullanıcının parolası |NTLM veya Kerberos kimlik doğrulaması (ek kimlik bilgileri olarak da bilinir) için gereken kimlik bilgisi karmaları Azure AD kiracınızdan eşitlenir. Azure AD kiracınız eşitlenmiş bir kiracıya sahip ise, bu kimlik bilgileri şirket içi etki alanından kaynaklıdır. |
| Birincil Kullanıcı/Grup SID 'SI |Otomatik olarak oluşturulan |Kullanıcı/Grup hesapları için birincil SID, yönetilen etki alanında otomatik olarak oluşturulur. Bu öznitelik, şirket içi AD etki alanındaki nesnenin birincil Kullanıcı/Grup SID 'SI ile eşleşmiyor. Bu uyuşmazlık, yönetilen etki alanının şirket içi etki alanınız dışında farklı bir SID Ad alanına sahip olmasından kaynaklanır. |
| Kullanıcılar ve gruplar için SID geçmişi |Şirket içi birincil kullanıcı ve Grup SID 'SI |Yönetilen etki alanındaki kullanıcılar ve gruplar için Sıdhistory özniteliği, şirket içi etki alanındaki ilgili birincil kullanıcı veya Grup SID 'SI ile eşleşecek şekilde ayarlanır. Bu özellik, kaynakları yeniden ACL 'leri ile kullanmanıza gerek olmadığından, şirket içi uygulamaların yönetilen etki alanına daha kolay bir şekilde kaymasını ve kaydırlanmasını sağlar. |

> [!NOTE]
> **Yönetilen etki alanında UPN biçimini kullanarak oturum açın:** SAMAccountName özniteliği, yönetilen etki alanındaki bazı Kullanıcı hesapları için otomatik olarak oluşturulabilir. Birden çok kullanıcının aynı Mailrumuz özniteliği varsa veya kullanıcılar aşırı uzun UPN önekleri içeriyorsa, bu kullanıcılara ait SAMAccountName otomatik olarak oluşturulabilir. Bu nedenle, SAMAccountName biçimi (örneğin, ' CONTOSO\dee ') etki alanında oturum açmak için her zaman güvenilir bir yol değildir. Kullanıcıların otomatik olarak oluşturulan SAMAccountName, UPN öneklerinden farklı olabilir. Yönetilen etki alanında güvenilir bir şekilde oturum açmak içindee@contoso.comUPN biçimini kullanın (örneğin, ' ').

### <a name="attribute-mapping-for-user-accounts"></a>Kullanıcı hesapları için öznitelik eşlemesi
Aşağıdaki tabloda, Azure AD kiracınızdaki Kullanıcı nesneleri için belirli özniteliklerin, yönetilen etki alanındaki ilgili özniteliklerle nasıl eşitlendiği gösterilmektedir.

| Azure AD kiracınızdaki Kullanıcı özniteliği | Yönetilen etki alanındaki kullanıcı özniteliği |
|:--- |:--- |
| accountEnabled |userAccountControl (ACCOUNT_DISABLED bitini ayarlar veya temizler) |
| city |m |
| ülke |Ortak |
| Bölüm |Bölüm |
| displayName |displayName |
| facsimileTelephoneNumber |facsimileTelephoneNumber |
| givenName |givenName |
| İş Unvanı |başlık |
| posta |posta |
| mailNickname |msDS-Azureadmailtakma ad |
| mailNickname |SAMAccountName (bazen otomatik olarak oluşturulabilir) |
| Mobil |Mobil |
| uzantının |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |Sıdhistory |
| passwordPolicies |userAccountControl (DONT_EXPIRE_PASSWORD bitini ayarlar veya temizler) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| posta kodu |posta kodu |
| preferredLanguage |preferredLanguage |
| state |St |
| streetAddress |streetAddress |
| Soyadı |sn |
| telephoneNumber 'dır |telephoneNumber 'dır |
| userPrincipalName |userPrincipalName |

### <a name="attribute-mapping-for-groups"></a>Gruplar için öznitelik eşlemesi
Aşağıdaki tabloda, Azure AD kiracınızdaki Grup nesneleri için belirli özniteliklerin, yönetilen etki alanındaki ilgili özniteliklerle nasıl eşitlendiği gösterilmektedir.

| Azure AD kiracınızdaki Grup özniteliği | Yönetilen etki alanındaki Grup özniteliği |
|:--- |:--- |
| displayName |displayName |
| displayName |SAMAccountName (bazen otomatik olarak oluşturulabilir) |
| posta |posta |
| mailNickname |msDS-Azureadmailtakma ad |
| uzantının |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |Sıdhistory |
| securityEnabled |groupType |

## <a name="password-hash-synchronization-and-security-considerations"></a>Parola karması eşitleme ve güvenlik konuları
Azure AD Domain Services etkinleştirdiğinizde, Azure AD dizininiz NTLM & Kerberos uyumlu biçimlerdeki parola karmalarını oluşturur ve depolar. 

Mevcut bulut Kullanıcı hesapları için, Azure AD hiçbir şekilde şifresiz metin parolalarını depoladığından, bu karmalar otomatik olarak oluşturulamaz. Bu nedenle, Microsoft, parola karmalarının Azure AD 'de oluşturulup depolanması için, [bulut kullanıcılarının parolalarını sıfırlamasını/değiştirmesini](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds) gerektirir. Azure AD Domain Services etkinleştirildikten sonra Azure AD 'de oluşturulan herhangi bir bulut Kullanıcı hesabı için, parola karmaları oluşturulup NTLM ve Kerberos uyumlu biçimlerinde depolanır. 

Azure AD Connect eşitleme kullanılarak şirket içi AD 'den eşitlenen Kullanıcı hesaplarında, [NTLM ve Kerberos uyumlu biçimlerdeki parola karmalarını eşitlemek için Azure AD Connect yapılandırmanız](active-directory-ds-getting-started-password-sync-synced-tenant.md)gerekir.

NTLM ve Kerberos uyumlu parola karmaları her zaman Azure AD 'de şifreli bir şekilde depolanır. Bu karmalar, yalnızca Azure AD Domain Services şifre çözme anahtarlarına erişime sahip olacak şekilde şifrelenir. Azure AD 'de başka bir hizmet veya bileşen şifre çözme anahtarlarına erişemez. Şifreleme anahtarları Azure AD kiracısı başına benzersizdir. Azure AD Domain Services, parola karmalarını yönetilen etki alanınız için etki alanı denetleyicileri ile eşitler. Bu parola karmaları, parolaların Windows Server AD etki alanı denetleyicilerinde nasıl depolandığı ve güvenliğinin sağlandığı gibi bu etki alanı denetleyicilerinde depolanır ve güvenlik altına alınır. Bu yönetilen etki alanı denetleyicilerinin diskleri, bekleyen olarak şifrelenir.

## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Yönetilen etki alanınızı Azure AD kiracınızla eşitlenmeyen nesneler
Bu makalenin önceki bölümünde açıklandığı gibi, yönetilen etki alanınızı Azure AD kiracınıza geri bir eşitleme yoktur. Yönetilen etki alanında [özel bir kuruluş birimi (OU) oluşturmayı](create-ou.md) tercih edebilirsiniz. Ayrıca, bu özel OU 'lar içinde başka OU 'Lar, kullanıcılar, gruplar veya hizmet hesapları oluşturabilirsiniz. Özel OU 'Lar içinde oluşturulan nesnelerden hiçbiri Azure AD kiracınıza geri eşitlenir. Bu nesneler yalnızca yönetilen etki alanınız dahilinde kullanılabilir. Bu nedenle, bu nesneler Azure AD PowerShell cmdlet 'leri, Azure AD Graph API veya Azure AD Yönetim kullanıcı arabirimi kullanılarak görülemez.

## <a name="related-content"></a>İlgili İçerik
* [Dağıtım senaryoları-Azure AD Domain Services](scenarios.md)
* [Azure AD Domain Services için ağ oluşturma konuları](network-considerations.md)
* [Azure AD Domain Services kullanmaya başlayın](tutorial-create-instance.md)
