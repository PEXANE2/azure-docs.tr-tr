---
title: Karma kimlik tasarımı - benimseme stratejisi Azure | Microsoft Dokümanlar
description: Koşullu Erişim denetimi ile Azure Etkin Dizin, kullanıcının kimliğini doğrularken ve uygulamaya erişime izin vermeden önce seçtiğiniz belirli koşulları denetler. Bu koşullar yerine getirildiğinde, kullanıcı nın kimliği doğrulanır ve uygulamaya erişime izin verilir.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: b92fa5a9-c04c-4692-b495-ff64d023792c
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: e662d2c6d7939756dee6eb25ca62fef171b7d6d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109326"
---
# <a name="define-a-hybrid-identity-adoption-strategy"></a>Karma kimlik benimseme stratejisi tanımlayın
Bu görevde, tartışılan iş gereksinimlerini karşılamak için karma kimlik çözümünüz için karma kimlik benimseme stratejisini tanımlarsınız:

* [İş gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-business-needs.md)
* [Dizin eşitleme gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)
* [Çok faktörlü kimlik doğrulama gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>İş ihtiyaçları stratejisini tanımlayın
İlk görev, işletmenin gereksinimlerini belirleyen kuruluşları belirler.  Bu çok geniş olabilir ve dikkatli değilseniz kapsam sürünme oluşabilir.  Başlangıçta, basit tutmak ama her zaman karşılamak ve gelecekte değişimi kolaylaştırmak bir tasarım için planlamak unutmayın.  Basit bir tasarım veya son derece karmaşık bir tasarım ne olursa olsun, Azure Active Directory, Office 365, Microsoft Çevrimiçi Hizmetleri ve bulut farkında uygulamaları destekleyen Microsoft Identity platformudur.

## <a name="define-an-integration-strategy"></a>Entegrasyon stratejisi tanımlama
Microsoft'un bulut kimlikleri, eşitlenmiş kimlikler ve federe kimlikler olmak üzere üç ana tümleştirme senaryosu vardır.  Bu entegrasyon stratejilerinden birini benimsemeyi planlamalısınız.  Seçtiğiniz strateji değişebilir ve birini seçerken alınan kararlar, ne tür bir kullanıcı deneyimi sağlamak istediğinizi, varolan bir altyapınız var mı ve en uygun maliyetli olan nedir.  

![tümleştirme senaryoları](./media/plan-hybrid-identity-design-considerations/integration-scenarios.png)

Yukarıdaki şekilde tanımlanan senaryolar şunlardır:

* **Bulut kimlikleri**: bunlar yalnızca bulutta bulunan kimliklerdir.  Azure AD durumunda, bunlar özellikle Azure REKLAM dizininizde yer eder.
* **Senkronize**: bunlar şirket içinde ve bulutta var olan kimliklerdir.  Azure AD Connect kullanılarak, bu kullanıcılar mevcut Azure AD hesapları yla oluşturulur veya bunlara katılır.  Kullanıcının parola karması, şirket içi ortamdan buluta parola karma adı verilen bir şekilde senkronize edilir.  Senkronize kullanırken bir uyarı, bir kullanıcı şirket içi ortamda devre dışı bırakılırsa, bu hesap durumunun Azure AD'de gösterilmesinin üç saat kadar sürebileceğidir.  Bunun nedeni eşitleme zaman aralığıdır.
* **Federe**: bu kimlikler hem şirket içinde hem de bulutta bulunur.  Azure AD Connect kullanılarak, bu kullanıcılar mevcut Azure AD hesapları yla oluşturulur veya bunlara katılır.  

> [!NOTE]
> Eşitleme seçenekleri hakkında daha fazla bilgi için [şirket içi kimliklerinizi Azure Etkin Dizini ile tümleştirme 'yi](whatis-hybrid-identity.md)okuyun.
> 
> 

Aşağıdaki tablo, aşağıdaki stratejilerin her birinin avantaj ve dezavantajlarının belirlenmesinde yardımcı olur:

| Strateji | Yararları | Dezavantajlar |
| --- | --- | --- |
| **Bulut kimlikleri** |Küçük organizasyonlar için yönetmek daha kolay. <br> Şirket içinde kurulacak bir şey yok. Ek donanım gerekmez<br>Kullanıcı şirketten ayrılırsa kolayca devre dışı bırakılır |Kullanıcıların buluttaki iş yüklerine erişirken oturum açmaları gerekir <br> Parolalar bulut ve şirket içi kimlikler için aynı olabilir veya olmayabilir |
| **Eşitlenmiş** |Şirket içi parola, hem şirket içinde hem de bulut dizinlerini doğrular <br>Küçük, orta veya büyük kuruluşlar için yönetilmesi daha kolay <br>Kullanıcılar bazı kaynaklar için tek oturum açma (SSO) alabilir <br> Senkronizasyon için Microsoft tercih yöntemi <br> Yönetilmesi daha kolay |Bazı müşteriler, belirli bir şirketin polisi nedeniyle dizinlerini bulutla senkronize etmekte isteksiz olabilir |
| **Federe** |Kullanıcılar tek oturum açabilir (SSO) <br>Bir kullanıcı sonlandırılır veya ayrılırsa, hesap derhal devre dışı bırakılabilir ve erişim iptal edilebilir,<br> Eşitlenmiş ile gerçekleştirilemeyen gelişmiş senaryoları destekler |Kurmak ve yapılandırmak için daha fazla adım <br> Daha yüksek bakım <br> STS altyapısı için ek donanım gerektirebilir <br> Federasyon sunucusunu yüklemek için ek donanım gerekebilir. AD FS kullanılıyorsa ek yazılım gereklidir <br> SSO için kapsamlı kurulum gerektirir <br> Federasyon sunucusu kapalıysa, kullanıcılar kimlik doğrulaması yapamaz |

### <a name="client-experience"></a>Müşteri deneyimi
Kullandığınız strateji, kullanıcı oturum açma deneyimini belirler.  Aşağıdaki tablolar, kullanıcıların oturum açma deneyimlerinin ne olmasını beklemeleri gerektiği hakkında size bilgi sağlar.  Tüm federe kimlik sağlayıcıları tüm senaryolarda SSO'yu desteklemez.

**Doman-birleştirilmiş ve özel ağ uygulamaları:**

|  | Senkronize Kimlik | Federal Kimlik |
| --- | --- | --- |
| Web Tarayıcıları |Form tabanlı kimlik doğrulama |tek oturum açma, bazen kuruluş kimliği sağlamak için gerekli |
| Outlook |Kimlik bilgileri iste |Kimlik bilgileri iste |
| Skype kurumsal (Lync) |Kimlik bilgileri iste |Lync için tek oturum açma, Exchange için kimlik bilgileri istenir |
| OneDrive İş |Kimlik bilgileri iste |tek oturum açma |
| Office Pro Plus Aboneliği |Kimlik bilgileri iste |tek oturum açma |

**Harici veya güvenilmeyen kaynaklar:**

|  | Senkronize Kimlik | Federal Kimlik |
| --- | --- | --- |
| Web Tarayıcıları |Form tabanlı kimlik doğrulama |Form tabanlı kimlik doğrulama |
| Outlook, İşletmeler için Skype (Lync), İşletmeler için OneDrive, Office aboneliği |Kimlik bilgileri iste |Kimlik bilgileri iste |
| Exchange ActiveSync |Kimlik bilgileri iste |Lync için tek oturum açma, Exchange için kimlik bilgileri istenir |
| Mobil uygulamalar |Kimlik bilgileri iste |Kimlik bilgileri iste |

Görev 1'den üçüncü taraf bir IdP'niz olduğunu belirlediyseniz veya Azure AD ile federasyon sağlamak için bir tane kullanacaksanız, aşağıdaki desteklenen özelliklerden haberdar olmanız gerekir:

* SP-Lite profili için uyumlu olan herhangi bir SAML 2.0 sağlayıcısı Azure AD ve ilişkili uygulamalariçin kimlik doğrulamayı destekleyebilir
* OWA, SPO, vb. kimlik doğrulamayı kolaylaştıran pasif kimlik doğrulamasını destekler.
* Exchange Online istemcileri SAML 2.0 Gelişmiş İstemci Profili (ECP) ile desteklenebilir

Ayrıca hangi özelliklerin kullanılmayacağını da bilmelisiniz:

* WS-Trust/Federation desteği olmadan, diğer tüm aktif müşteriler
  * Bu, Office 2016'dan önce Lync istemcisi, OneDrive istemcisi, Office Aboneliği, Office Mobile olmadığı anlamına gelir
* Office'in pasif kimlik doğrulamasına geçişi, saf SAML 2.0 IDP'lerini desteklemelerine olanak tanır, ancak destek yine de istemci bazında olacaktır

> [!NOTE]
> En güncel liste için makaleyi okuyun [Azure AD federasyonu uyumluluk listesi](how-to-connect-fed-compatibility.md).
> 
> 

## <a name="define-synchronization-strategy"></a>Eşitleme stratejisini tanımlama
Bu görevde, kuruluşun şirket içi verilerini bulutla eşitlemek için kullanılacak araçları ve hangi topolojiyi kullanmanız gerektiğini tanımlarsınız.  Çünkü çoğu kuruluş Active Directory kullandığından, yukarıdaki soruları yanıtlamak için Azure AD Connect'i kullanma yla ilgili bilgiler bazı ayrıntılarda sağlanır.  Active Directory'si olmayan ortamlar için, bu stratejinin planlanmalarına yardımcı olmak için FIM 2010 R2 veya MIM 2016'yı kullanma hakkında bilgi vardır.  Ancak, Azure AD Connect'in gelecekteki sürümleri LDAP dizinlerini destekleyecektir, bu nedenle zaman tünelinize bağlı olarak bu bilgiler yardımcı olabilir.

### <a name="synchronization-tools"></a>Senkronizasyon araçları
Yıllar içinde, çeşitli eşitleme araçları var ve çeşitli senaryolar için kullanılır.  Şu anda Azure AD Connect, desteklenen tüm senaryolar için tercih edilen araçtır.  AAD Sync ve DirSync de hala etrafında ve hatta şimdi çevrenizde mevcut olabilir. 

> [!NOTE]
> Her aracın desteklenen yetenekleriyle ilgili en son bilgiler için [Dizin tümleştirme araçları karşılaştırma](plan-hybrid-identity-design-considerations-tools-comparison.md) makalesini okuyun.  
> 
> 

### <a name="supported-topologies"></a>Desteklenen topolojiler
Bir senkronizasyon stratejisi tanımlanırken, kullanılan topoloji belirlenmelidir. Adım 2'de belirlenen bilgilere bağlı olarak hangi topolojinin kullanılacağını belirleyebilirsiniz. Tek orman, tek Bir Azure AD topolojisi en yaygın olanıdır ve tek bir Active Directory ormanı ve tek bir Azure AD örneğinden oluşur.  Bu, senaryoların çoğunda kullanılacaktır ve aşağıdaki şekilde gösterildiği gibi Azure AD Connect Express yüklemesini kullanırken beklenen topolojidir.

![Desteklenen topolojiler](./media/plan-hybrid-identity-design-considerations/single-forest.png) Tek Orman Senaryosu Şekil 5'te gösterildiği gibi, büyük ve hatta küçük kuruluşların birden fazla ormana sahip olması yaygındır.

> [!NOTE]
> Azure AD Connect senkronizasyonuile farklı şirket içi ve Azure AD topolojileri hakkında daha fazla bilgi [için Azure AD Connect için Topolojiler](plan-connect-topologies.md)makalesini okuyun.
> 
> 

![çok ormanlı topoloji](./media/plan-hybrid-identity-design-considerations/multi-forest.png) 

Çoklu Orman Senaryosu

Bu durumda, aşağıdaki öğeler doğruysa, çok ormanlı tek Azure AD topolojisi göz önünde bulundurulmalıdır:

* Kullanıcıların tüm ormanlarda yalnızca 1 kimliği vardır – aşağıdaki benzersiz olarak tanımlayıcı kullanıcılar bölümü bunu daha ayrıntılı olarak açıklar.
* Kullanıcı, kimliğinin bulunduğu ormana kimlik doğrular
* UPN ve Kaynak Çapa (değişmez id) bu ormandan gelecek
* Tüm ormanlara Azure AD Connect tarafından erişilebilir – bu, etki alanının birleştirilmesi gerekmediği ve bunu kolaylaştırıyorsa DMZ'ye yerleştirilebileceği anlamına gelir.
* Kullanıcıların yalnızca bir posta kutusu var
* Kullanıcının posta kutusunu barındıran orman, Exchange Global Adres Listesi'nde (GAL) görünen öznitelikler için en iyi veri kalitesine sahiptir
* Kullanıcıda posta kutusu yoksa, bu değerlere katkıda bulunmak için herhangi bir orman kullanılabilir
* Bağlantılı bir posta kutunuz varsa, oturum açmaiçin kullanılan farklı bir ormanda başka bir hesap da vardır.

> [!NOTE]
> Hem şirket içinde hem de bulutta bulunan nesneler, benzersiz bir tanımlayıcı aracılığıyla "bağlanır". Dizin Eşitlemesi bağlamında, bu benzersiz tanımlayıcı kaynak çapa olarak adlandırılır. Tek Oturum Açma bağlamında, bu ImmutableId olarak adlandırılır. SourceAnchor'ın kullanımıyla ilgili daha fazla göz önünde bulundurulması gereken daha fazla konu için [Azure AD Connect için tasarım kavramları.](plan-connect-design-concepts.md#sourceanchor)
> 
> 

Yukarıdakiler doğru değilse ve birden fazla etkin hesabınız veya birden fazla posta kutunuz varsa, Azure AD Connect birini seçer ve diğerini yok sayacaktır.  Bağlı posta kutularınız varsa ancak başka bir hesabınız yoksa, bu hesaplar Azure AD'ye dışa aktarılmaz ve bu kullanıcı herhangi bir grubun üyesi olmaz.  Bu, DirSync ile geçmişte olduğundan farklıdır ve bu çok ormanlı senaryoları daha iyi desteklemek için kasıtlıdır. Aşağıdaki şekilde çok ormanlı bir senaryo gösterilmiştir.

![birden çok Azure AD kiracı](./media/plan-hybrid-identity-design-considerations/multiforest-multipleAzureAD.png) 

**Çoklu orman birden çok Azure REKLAM senaryosu**

Bir kuruluş için Azure AD'de yalnızca tek bir dizin olması önerilir, ancak azure AD Connect eşitleme sunucusu ile Azure AD dizini arasında 1:1 ilişkisi tutulur şekilde desteklenir.  Azure AD'nin her örneği için Azure AD Connect yüklemeniz gerekir.  Ayrıca, Azure AD, tasarım gereği yalıtılmış ve Azure AD'nin bir örneğindeki kullanıcılar kullanıcıları başka bir örnekte göremez.

Aşağıdaki şekilde gösterildiği gibi, Etkin Dizin'in bir şirket içi örneğini birden çok Azure REKLAM dizinine bağlamak mümkündür ve desteklenir:

![tek orman filtreleme](./media/plan-hybrid-identity-design-considerations/single-forest-flitering.png) 

**Tek orman filtreleme senaryosu**

Bunu yapmak için aşağıdakilerin doğru olması gerekir:

* Azure AD Connect eşitleme sunucularının filtreleme için yapılandırılması gerekir, böylece her biri birbirini dışlayan nesneler kümesine sahiptir.  Bu, örneğin, belirli bir etki alanı veya OU her sunucu kapsam tarafından yapılır.
* Bir DNS etki alanı yalnızca tek bir Azure REKLAM dizinine kaydedilebilir, bu nedenle şirket içi AD'deki kullanıcıların UPN'leri ayrı ad alanları kullanmalıdır
* Azure AD'nin bir örneğindeki kullanıcılar yalnızca kendi örneklerinden kullanıcıları görebilir.  Diğer durumlarda kullanıcıları göremezler
* Azure REKLAM dizinlerinden yalnızca biri şirket içi AD ile Exchange hibrit etkinleştirebilir
* Karşılıklı münhasırlık da yazma-geri için de geçerlidir.  Bu, tek bir şirket içi yapılandırma varsayalım bu topoloji ile desteklenmeyen bazı yazma özellikleri yapar.  Buna aşağıdakiler dahildir:
  * Varsayılan yapılandırma ile grup yazma geri
  * Cihaz geri yazma

Aşağıdakiler desteklenmez ve uygulama olarak seçilmemelidir:

* Aynı Azure AD dizinine bağlanan birden çok Azure AD Connect eşitleme sunucusu, birbirini dışlayacak şekilde yapılandırılacak olsa bile desteklenmez
* Aynı kullanıcıyı birden çok Azure REKLAM dizini ile eşitlemek desteklenmez. 
* Ayrıca, bir Azure REKLAM'ındaki kullanıcıların başka bir Azure REKLAM dizininde kişi olarak görünmesini sağlamak için yapılandırma değişikliği yapmak da desteklenmez. 
* Birden çok Azure REKLAM dizinine bağlanmak için Azure AD Connect eşitlemeyi değiştirmek de desteklenmez.
* Azure AD dizinleri tasarım yalıtılmış. Dizinler arasında ortak ve birleşik bir GAL oluşturmak amacıyla başka bir Azure AD dizininden verileri okumak için Azure AD Connect eşitleme yapılandırmasını değiştirmek desteklenmez. Ayrıca, Azure AD Connect eşitlemeyi kullanarak kullanıcıları şirket içi başka bir REKLAM'a kişi olarak dışa aktarmak da desteklenmez.

> [!NOTE]
> Kuruluşunuz ağınızdaki bilgisayarların Internet'e bağlanmasını kısıtlarsa, bu makalede giden izin listelerinize ve Internet Explorer Güvenilir Siteler Alanı'na eklemeniz gereken uç noktaları (FQDN'ler, IPv4 ve IPv6 adres aralıkları) listelenir bilgisayarlarınızın Office 365'i başarıyla kullanabilmesini sağlamak için bilgisayarlar. Daha fazla bilgi için [Office 365 URL'leri ve IP adres aralıklarını](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)okuyun.
> 
> 

## <a name="define-multi-factor-authentication-strategy"></a>Çok faktörlü kimlik doğrulama stratejisini tanımlama
Bu görevde kullanılacak çok faktörlü kimlik doğrulama stratejisini tanımlarsınız.  Azure Çok Faktörlü Kimlik Doğrulama iki farklı sürümde gelir.  Bunlardan biri bulut tabanlı, diğeri de Azure MFA Server kullanılarak şirket içi dir.  Yukarıda yaptığınız değerlendirmeye dayanarak stratejiniz için hangi çözümün doğru olduğunu belirleyebilirsiniz.  Hangi tasarım seçeneğinin şirketinizin güvenlik gereksinimini en iyi karşıladığınızı belirlemek için aşağıdaki tabloyu kullanın:

Çok faktörlü tasarım seçenekleri:

| Güvenli olması gereken varlık | Bulutta MFA | Şirket içi MFA |
| --- | --- | --- |
| Microsoft uygulamaları |evet |evet |
| Uygulama galerisinde SaaS uygulamaları |evet |evet |
| Azure AD Uygulaması Proxy üzerinden yayımlanan IIS uygulamaları |evet |evet |
| Azure AD App Proxy aracılığıyla yayınlanmayan IIS uygulamaları |hayır |evet |
| VPN, RDG gibi uzaktan erişim |hayır |evet |

Stratejiniz için bir çözüm üzerinde yerleşmiş olsanız bile, kullanıcılarınızın bulunduğu yerdeki değerlendirmeyi yukarıdan kullanmanız gerekir.  Bu, çözümün değişmesine neden olabilir.  Bunu belirlemenize yardımcı olmak için aşağıdaki tabloyu kullanın:

| Kullanıcı konumu | Tercih edilen tasarım seçeneği |
| --- | --- |
| Azure Active Directory |Bulutta Çoklu Faktör Doğrulaması |
| AD FS ile federasyon kullanana Azure AD ve şirket içi AD |Her ikisi de |
| Azure AD kullanarak Azure AD ve şirket içi AD Hiçbir parola eşitleme bağlama |Her ikisi de |
| Azure AD ve şirket içi, parola eşitleme yle Azure AD Connect'i kullanarak |Her ikisi de |
| Şirket içi AD |Multi-Factor Authentication Sunucusu |

> [!NOTE]
> Ayrıca, seçtiğiniz çok faktörlü kimlik doğrulama tasarım seçeneğinin tasarımınız için gerekli olan özellikleri desteklediğinden de emin olmalısınız.  Daha fazla bilgi için okuyun [Sizin için çok faktörlü güvenlik çözümseçin.](../authentication/concept-mfa-howitworks.md)
> 

## <a name="multi-factor-auth-provider"></a>Çok Faktörlü Auth Sağlayıcı
Çok faktörlü kimlik doğrulama, Azure Etkin Dizin kiracısı olan genel yöneticiler için varsayılan olarak kullanılabilir. Ancak, yönetim portalı, özel karşılamalar ve raporlar gibi özelliklerden yararlanabilmek için tüm kullanıcılarınıza ve/veya global yöneticilerinize çok faktörlü kimlik doğrulamasını genişletmek istiyorsanız, Çok Faktörlü Kimlik Doğrulama Sağlayıcısı'nı satın almanız ve yapılandırmanız gerekir.

> [!NOTE]
> Ayrıca, seçtiğiniz çok faktörlü kimlik doğrulama tasarım seçeneğinin tasarımınız için gerekli olan özellikleri desteklediğinden de emin olmalısınız. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Veri koruma gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

