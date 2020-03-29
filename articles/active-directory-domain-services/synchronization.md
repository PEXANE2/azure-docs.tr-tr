---
title: Azure AD Etki Alanı Hizmetlerinde senkronizasyon nasıl çalışır | Microsoft Dokümanlar
description: Bir Azure AD kiracısından veya şirket içi Active Directory Etki Alanı Hizmetleri ortamından Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen etki alanına eşitleme işleminin nesneler ve kimlik bilgileri için nasıl çalıştığını öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 57cbf436-fc1d-4bab-b991-7d25b6e987ef
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: iainfou
ms.openlocfilehash: 7e0e904b182a57a51b5d76f0acebc13bce5902b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78944425"
---
# <a name="how-objects-and-credentials-are-synchronized-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanında nesneler ve kimlik bilgileri nasıl eşitlenir?

Azure Etkin Dizin Etki Alanı Hizmetleri (AD DS) yönetilen etki alanındaki nesneler ve kimlik bilgileri etki alanı içinde yerel olarak oluşturulabilir veya bir Azure Etkin Dizin (Azure AD) kiracısından eşitlenebilir. Azure AD DS'yi ilk dağıttığınızda, otomatik tek yönlü eşitleme yapılandırılır ve Azure AD'deki nesneleri çoğaltmaya başlar. Bu tek yönlü eşitleme, Azure AD DS yönetilen etki alanını Azure AD'deki değişikliklerle güncel tutmak için arka planda devam eder. Azure AD DS'den Azure AD'ye geri eşitleme gerçekleşmez.

Karma bir ortamda, şirket içi AD DS etki alanından gelen nesneler ve kimlik bilgileri Azure AD Connect kullanılarak Azure AD ile senkronize edilebilir. Bu nesneler Azure AD ile başarıyla eşitlendikten sonra, otomatik arka plan eşitlemi, bu nesneleri ve kimlik bilgilerini Azure AD DS yönetilen etki alanını kullanarak uygulamalarda kullanılabilir hale getirir.

Aşağıdaki diyagram, Azure AD DS, Azure AD ve isteğe bağlı şirket içi AD DS ortamı arasında eşitlemenin nasıl çalıştığını göstermektedir:

![Azure AD Etki Alanı Hizmetleri yönetilen etki alanı için senkronizasyona genel bakış](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-azure-ad-to-azure-ad-ds"></a>Azure AD'den Azure AD DS'ye eşitleme

Kullanıcı hesapları, grup üyelikleri ve kimlik bilgileri karmaları Azure AD'den Azure AD DS'ye tek bir şekilde eşitlenir. Bu eşitleme işlemi otomatiktir. Bu eşitleme işlemini yapılandırmanız, izlemeniz veya yönetmeniz gerekmez. İlk eşitleme, Azure AD dizinindeki nesnelerin sayısına bağlı olarak birkaç saat ile birkaç gün arasında sürebilir. İlk eşitleme tamamlandıktan sonra, parola veya öznitelik değişiklikleri gibi Azure AD'de yapılan değişiklikler otomatik olarak Azure AD DS'ye eşitlenir.

Senkronizasyon işlemi tek yönlü / tasarım tarafından tek yönlüdür. Azure AD DS'deki değişikliklerin Azure AD'ye geri dönmesi yoktur. Azure AD DS yönetilen etki alanı, oluşturabileceğiniz özel OS'ler dışında büyük ölçüde salt okunur. Azure AD DS yönetilen bir etki alanında kullanıcı özniteliklerinde, kullanıcı parolalarında veya grup üyeliklerinde değişiklik yapamazsınız.

## <a name="attribute-synchronization-and-mapping-to-azure-ad-ds"></a>Azure AD DS'ye eşitleme ve eşleme atfetme

Aşağıdaki tabloda bazı ortak öznitelikler ve bunların Azure AD DS ile nasıl eşitlendikleri listelenir.

| Azure AD DS'de öznitelik | Kaynak | Notlar |
|:--- |:--- |:--- |
| UPN | Azure AD kiracısında kullanıcının *UPN* özniteliği | Azure AD kiracısının UPN özniteliği, Azure AD DS'ye olduğu gibi eşitlenir. Azure AD DS yönetilen bir etki alanında oturum açmanın en güvenilir yolu UPN'yi kullanmaktır. |
| Samaccountname | Kullanıcının *mailTakma* özelliği Azure AD kiracısında veya otomatik olarak oluşturulmuş | *SAMAccountName* özniteliği, Azure AD kiracısındaki *mailNickname* özniteliğinden kaynaklanmaktadır. Birden çok kullanıcı hesabı aynı *mailNickname* özniteliğine sahipse, *SAMAccountName* otomatik olarak oluşturulur. Kullanıcının *mailNickname* veya *UPN* öneki 20 karakterden uzunsa, *SAMAccountName* *SAMAccountName* özniteliklerindeki 20 karakter sınırını karşılamak üzere otomatik olarak oluşturulur. |
| Parolalar | Azure AD kiracısından kullanıcı nın şifresi | NTLM veya Kerberos kimlik doğrulaması için gereken eski parola işlenme leri Azure AD kiracısından eşitlenir. Azure AD kiracısı Azure AD Connect kullanılarak karma eşitleme için yapılandırılırsa, bu parola karmaları şirket içi AD DS ortamından elde edilir. |
| Birincil kullanıcı/grup SID | Otomatik | Kullanıcı/grup hesapları için birincil SID, Azure AD DS'de otomatik olarak oluşturulur. Bu öznitelik, şirket içi AD DS ortamında nesnenin birincil kullanıcı/grup SID'si ile eşleşmez. Bu uyumsuzluk, Azure AD DS yönetilen etki alanının şirket içi AD DS etki alanından farklı bir SID ad alanına sahip olmasıdır. |
| Kullanıcılar ve gruplar için SID geçmişi | Şirket içi birincil kullanıcı ve grup SID | Azure AD DS'deki kullanıcılar ve gruplar için *SidHistory* özniteliği, şirket içi BIR AD DS ortamında ilgili birincil kullanıcı veya grup SID ile eşleşecek şekilde ayarlanmıştır. Bu özellik, yeniden ACL kaynaklarını yeniden almanız gerekmeden şirket içi uygulamaların Azure AD DS'ye kaldırılmasını ve kaydırmasını kolaylaştırır. |

> [!TIP]
> **UPN biçimini kullanarak yönetilen etki alanında oturum açma** *SAMAccountName* özniteliği, `AADDSCONTOSO\driley`Azure AD DS yönetilen etki alanında bazı kullanıcı hesapları için otomatik olarak oluşturulabilir. Kullanıcıların otomatik olarak oluşturulan *SAMAccountName'si* UPN önekinden farklı olabilir, bu nedenle oturum açmak için her zaman güvenilir bir yol değildir.
>
> Örneğin, birden çok kullanıcı aynı *mailNickname* özniteliğine sahipse veya kullanıcılar da aşırı uzun UPN önekleri varsa, bu kullanıcılar için *SAMAccountName* otomatik olarak oluşturulabilir. Azure AD DS yönetilen `driley@aaddscontoso.com`bir etki alanında güvenilir bir şekilde oturum açmak için UPN biçimini kullanın.

### <a name="attribute-mapping-for-user-accounts"></a>Kullanıcı hesapları için öznitelik eşleme

Aşağıdaki tablo, Azure AD'deki kullanıcı nesnelerine ait belirli özniteliklerin Azure AD DS'deki ilgili özniteliklerle nasıl eşitleniyi gösterdiğini göstermektedir.

| Azure AD'de kullanıcı özniteliği | Azure AD DS'de kullanıcı özniteliği |
|:--- |:--- |
| hesapEtkin |userAccountControl (ACCOUNT_DISABLED bitini ayarlar veya temizler) |
| city |l |
| ülke |Co |
| bölüm |bölüm |
| displayName |displayName |
| faksTelefon Numarası |faksTelefon Numarası |
| givenName |givenName |
| jobTitle |başlık |
| posta |posta |
| Mailnickname |msDS-AzureADMailNickname |
| Mailnickname |SAMAccountName (bazen otomatik olarak oluşturulabilir) |
| mobil |mobil |
| Objectıd |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |Sıdhistory |
| passwordPolitikalar |userAccountControl (DONT_EXPIRE_PASSWORD bitini ayarlar veya temizler) |
| physicalDeliveryOfficeName |physicalDeliveryOfficeName |
| Postakodu |Postakodu |
| tercihDil |tercihDil |
| durum |st |
| Streetaddress |Streetaddress |
| surname |sn |
| Telephonenumber |Telephonenumber |
| userPrincipalName |userPrincipalName |

### <a name="attribute-mapping-for-groups"></a>Gruplar için öznitelik eşleme

Aşağıdaki tablo, Azure AD'deki grup nesnelerinin belirli özniteliklerinin Azure AD DS'deki ilgili özniteliklerle nasıl eşitleniyi gösterdiğini göstermektedir.

| Azure AD'de grup özniteliği | Azure AD DS'de grup özniteliği |
|:--- |:--- |
| displayName |displayName |
| displayName |SAMAccountName (bazen otomatik olarak oluşturulabilir) |
| posta |posta |
| Mailnickname |msDS-AzureADMailNickname |
| Objectıd |msDS-AzureADObjectId |
| onPremiseSecurityIdentifier |Sıdhistory |
| securityEtkin |groupType |

## <a name="synchronization-from-on-premises-ad-ds-to-azure-ad-and-azure-ad-ds"></a>Şirket içi AD DS'den Azure AD ve Azure AD DS'ye eşitleme

Azure AD Connect, şirket içi AD DS ortamından Azure AD'ye kullanıcı hesaplarını, grup üyeliklerini ve kimlik bilgilerini eşitlemek için kullanılır. UPN ve şirket içi güvenlik tanımlayıcısı (SID) gibi kullanıcı hesaplarının öznitelikleri eşitlenir. Azure AD DS kullanarak oturum açma için NTLM ve Kerberos kimlik doğrulaması için gereken eski parola kalıpları da Azure AD ile senkronize edilir.

> [!IMPORTANT]
> Azure AD Connect yalnızca şirket içi AD DS ortamlarıyla senkronizasyon için yüklenmeli ve yapılandırılmalıdır. Azure AD Connect'i Azure AD DS yönetilen bir etki alanına yüklemek için desteklenmez.

Geri yazmayı yapılandırırsanız, Azure AD'deki değişiklikler şirket içi AD DS ortamına eşitlenir. Örneğin, bir kullanıcı Azure AD self servis parola yönetimini kullanarak parolasını değiştirirse, parola şirket içi AD DS ortamında yeniden güncelleştirilir.

> [!NOTE]
> Bilinen tüm hataların düzeltmeleri olduğundan emin olmak için her zaman Azure AD Connect'in en son sürümünü kullanın.

### <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Çok ormanlı bir şirket ortamından senkronizasyon

Birçok kuruluş, birden çok orman içeren oldukça karmaşık bir şirket içi AD DS ortamına sahiptir. Azure AD Connect, kullanıcıları, grupları ve kimlik bilgileri açıklarını çok ormanlı ortamlardan Azure AD'ye eşitlemeyi destekler.

Azure AD çok daha basit ve düz bir ad alanına sahiptir. Kullanıcıların Azure AD tarafından güvenli uygulamalara güvenilir bir şekilde erişmesini sağlamak için, farklı ormanlardaki kullanıcı hesapları arasındaki UPN çakışmalarını çözün. Azure AD DS yönetilen etki alanları, Azure AD'ye benzer düz bir OU yapısı kullanır. Şirket içinde hiyerarşik bir OU yapısını yapılandırmış olsanız bile, şirket içinde farklı etki alanlarından veya ormanlardan senkronize edilmiş olmasına rağmen tüm kullanıcı hesapları ve grupları *AADDC Kullanıcıları* konteynerinde depolanır. Azure AD DS yönetilen etki alanı hiyerarşik OU yapılarını düzleştirir.

Daha önce ayrıntılı olarak açıklandığı gibi, Azure AD DS'den Azure AD'ye geri senkronizasyon yoktur. Azure AD DS'de [özel bir Kuruluş Birimi (OU) ve](create-ou.md) ardından bu özel OSB'lerde kullanıcılar, gruplar veya hizmet hesapları oluşturabilirsiniz. Özel İş'lerde oluşturulan nesnelerin hiçbiri Azure AD ile eşitlenmez. Bu nesneler yalnızca Azure AD DS yönetilen etki alanında kullanılabilir ve Azure AD PowerShell cmdlets, Microsoft Graph API veya Azure AD yönetimi ui kullanarak görünür değildir.

## <a name="what-isnt-synchronized-to-azure-ad-ds"></a>Azure AD DS ile senkronize olmayanlar

Aşağıdaki nesneler veya öznitelikler şirket içi AD DS ortamından Azure AD veya Azure AD DS'ye eşitlenmez:

* **Dışlanmış öznitelikler:** Azure AD Connect'i kullanarak belirli öznitelikleri, şirket içi AD DS ortamından Azure AD'ye eşitleme dışında hariç tutabilirsiniz. Bu dışlanmış öznitelikler azure AD DS'de kullanılamaz.
* **Grup İlkeleri:** Şirket içi AD DS ortamında yapılandırılan Grup İlkeleri, Azure AD DS ile eşitlenmez.
* **Sysvol klasörü:** Şirket içi AD DS ortamındaki *Sysvol* klasörünün içeriği Azure AD DS ile eşitlenmez.
* **Bilgisayar nesneleri:** Şirket içi AD DS ortamına katılan bilgisayarların bilgisayar nesneleri Azure AD DS ile eşitlenmez. Bu bilgisayarların Azure AD DS yönetilen etki alanıyla güven ilişkisi yoktur ve yalnızca şirket içi AD DS ortamına aittir. Azure AD DS'de, yalnızca yönetilen etki alanına açıkça etki alanına katılan bilgisayarlar için bilgisayar nesneleri gösterilir.
* **Kullanıcılar ve gruplar için SidHistory öznitelikleri:** Şirket içi AD DS ortamındaki birincil kullanıcı ve birincil grup SID'leri Azure AD DS ile senkronize edilir. Ancak, kullanıcılar ve gruplar için varolan *SidHistory* öznitelikleri şirket içi AD DS ortamından Azure AD DS'ye eşitlenmez.
* **Organizasyon Birimleri (OU) yapıları:** Şirket içi AD DS ortamında tanımlanan kuruluş birimleri Azure AD DS ile eşitleme yapmaz. Azure AD DS'de biri kullanıcılar, diğeri bilgisayarlar için olmak üzere iki yerleşik OS'ler vardır. Azure AD DS yönetilen etki alanı düz bir OU yapısına sahiptir. Yönetilen etki [alanınızda özel bir OU oluşturmayı](create-ou.md)seçebilirsiniz.

## <a name="password-hash-synchronization-and-security-considerations"></a>Parola karma senkronizasyonu ve güvenlik konuları

Azure AD DS'yi etkinleştirdiğinizde, NTLM + Kerberos kimlik doğrulaması için eski parola işhepleri gereklidir. Azure AD açık metin parolaları depolamaz, bu nedenle bu hashes varolan kullanıcı hesapları için otomatik olarak oluşturulamaz. Bir kez oluşturulduktan ve depolandıktan sonra, NTLM ve Kerberos uyumlu parola kalıpları her zaman Azure AD'de şifreli bir şekilde depolanır.

Şifreleme anahtarları her Azure AD kiracısına özgüdir. Bu hashes şifreçözme anahtarları na yalnızca Azure AD DS erişimi vardır şekilde şifrelenir. Azure AD'deki başka hiçbir hizmet veya bileşenşifre çözme anahtarlarına erişememiştir.

Eski parola hehe'leri daha sonra Azure AD'den Azure AD tarafından yönetilen bir etki alanı için etki alanı denetleyicilerine eşitlenir. Azure AD DS'deki bu yönetilen etki alanı denetleyicilerinin diskleri istirahatte şifrelenir. Bu parola kalıpları, parolaların şirket içi AD DS ortamında depolanması na benzer şekilde bu etki alanı denetleyicilerinde depolanır ve güvenli hale gelir.

Yalnızca buluta özel Azure REKLAM ortamları için, gerekli parola hashelerinin Oluşturulabilmesi ve Azure AD'de depolanabilmesi için [kullanıcıların parolalarını sıfırlamaları/değiştirmeleri gerekir.](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds) Azure AD Etki Alanı Hizmetlerini etkinleştirdikten sonra Azure AD Ad'de oluşturulan tüm bulut kullanıcı hesapları için parola kalıpları oluşturulur ve NTLM ve Kerberos uyumlu biçimlerde depolanır. Bu yeni hesapların parolalarını sıfırlamaları veya değiştirmeleri gerekmez ve eski parola yongaları oluşturur.

Azure AD Connect'i kullanarak şirket içi AD DS ortamından senkronize edilen karma kullanıcı hesapları için, [NTLM ve Kerberos uyumlu biçimlerde parola karmalarını eşitlemek için Azure AD Connect'i yapılandırmanız](tutorial-configure-password-hash-sync.md)gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Parola eşitlemenin özellikleri hakkında daha fazla bilgi için Azure [AD Connect ile parola karma eşitlemenin nasıl çalıştığını](../active-directory/hybrid/how-to-connect-password-hash-synchronization.md?context=/azure/active-directory-domain-services/context/azure-ad-ds-context)görün.

Azure AD DS'ye başlamak için [yönetilen bir etki alanı oluşturun.](tutorial-create-instance.md)
