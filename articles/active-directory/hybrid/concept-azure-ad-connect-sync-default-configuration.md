---
title: 'Azure AD Connect eşitleme: Varsayılan yapılandırmayı anlama | Microsoft Dokümanlar'
description: Bu makalede, Azure AD Connect eşitlemesinde varsayılan yapılandırma açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ed876f22-6892-4b9d-acbe-6a2d112f1cd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c2886b842aab81732beec0fdd7957aab8e2b4f5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76548875"
---
# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect Eşitleme: Varsayılan yapılandırmayı anlama
Bu makalede, kullanıma açık yapılandırma kuralları açıklanmaktadır. Kuralları ve bu kuralların yapılandırmayı nasıl etkilediğini belgeler. Ayrıca Azure AD Connect eşitlemevarsayılan yapılandırması ile size yol gösterir. Amaç, okuyucunun bildirimsel sağlama adlı yapılandırma modelinin gerçek dünya örneğinde nasıl çalıştığını anlamasıdır. Bu makalede, yükleme sihirbazını kullanarak Azure AD Connect eşitlemeyi zaten yüklediğinizi ve yapılandırdığınızvarsa.

Yapılandırma modelinin ayrıntılarını anlamak için, [Bildirimsel Hükmü Anlama'yı](concept-azure-ad-connect-sync-declarative-provisioning.md)okuyun.

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Şirket içi kutudan Azure AD'ye kadar kutudan çıkma kuralları
Aşağıdaki ifadeler kutudan çıkan yapılandırmada bulunabilir.

### <a name="user-out-of-box-rules"></a>Kullanıcı out-of-box kuralları
Bu kurallar iNetOrgPerson nesne türü için de geçerlidir.

Bir kullanıcı nesnesinin eşitlemesi için aşağıdakileri karşılaması gerekir:

* Bir kaynağı olmalı Anchor.
* Nesne Azure AD'de oluşturulduktan sonra sourceAnchor değiştirilemez. Değer şirket içinde değiştirilirse, kaynak Çapa önceki değerine geri değiştirilene kadar nesne eşitlemeyi durdurur.
* AccountEnabled (userAccountControl) özniteliği nin doldurulması gerekir. Şirket içi Active Directory ile bu özellik her zaman mevcuttur ve doldurulur.

Aşağıdaki kullanıcı nesneleri Azure AD ile **eşitlenmez:**

* `IsPresent([isCriticalSystemObject])`. Etkin Dizin'de yerleşik yönetici hesabı gibi birçok kutu dan sıcmış nesnenin eşitlenmediğinden emin olun.
* `IsPresent([sAMAccountName]) = False`. SAMAccountName özniteliği olmayan kullanıcı nesnelerinin senkronize olmadığından emin olun. Bu durumda sadece pratik bir etki NT4 yükseltilmiş olur.
* `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Azure AD Connect eşitlemesi ve önceki sürümleri tarafından kullanılan hizmet hesabını eşitlemeyin.
* Exchange Online'da çalışmayan Exchange hesaplarını senkronize etmeyin.
  * `[sAMAccountName] = "SUPPORT_388945a0"`
  * `Left([mailNickname], 14) = "SystemMailbox{"`
  * `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
  * `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
* Exchange Online'da çalışmayan nesneleri eşitlemeyin.
  `CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
  Bu bitmaskesi (&H21C07000) aşağıdaki nesneleri filtreler:
  * Posta özellikli Ortak Klasör (Sürüm 1.1.524.0 itibariyle Önizlemede)
  * Sistem Görevlisi Posta Kutusu
  * Posta Kutusu Veritabanı Posta Kutusu (Sistem Posta Kutusu)
  * Evrensel Güvenlik Grubu (bir kullanıcı için geçerli olmaz, ancak eski nedenlerle mevcut)
  * Evrensel Olmayan Grup (kullanıcı için geçerli değildir, ancak eski nedenlerle mevcuttur)
  * Posta Kutusu Planı
  * Discovery Posta Kutusu
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Çoğaltma kurbanı nesneleri eşitleme yatmayın.

Aşağıdaki öznitelik kuralları geçerlidir:

* `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. SourceAnchor özniteliği bağlı bir posta kutusundan katkıda bulunulmadı. Bağlantılı bir posta kutusu bulunursa, gerçek hesabın daha sonra birleştiği varsayılır.
* Exchange ile ilgili öznitelikler yalnızca öznitelik **mailNickName'nin** bir değeri varsa eşitlenir.
* Birden çok orman olduğunda, öznitelikler aşağıdaki sırayla tüketilir:
  1. Oturum açma yla ilgili öznitelikler (örneğin userPrincipalName) etkin bir hesapla birlikte ormandan katkıda bulunulur.
  2. Exchange GAL'de (Genel Adres Listesi) bulunabilecek özniteliklere bir Exchange Posta Kutusu ile birlikte ormandan katkıda bulunulr.
  3. Posta kutusu bulunamıyorsa, bu öznitelikler herhangi bir ormandan gelebilir.
  4. Değişimle ilgili öznitelikler (GAL'de görünmeyen teknik `mailNickname ISNOTNULL`öznitelikler) ormandan katkıda bulunulmaktadır.
  5. Bu kurallardan birini karşılayacak birden çok orman varsa, bağlayıcıların (ormanların) oluşturma sırası (tarih/saat) hangi ormanın özniteliklere katkıda bulunacağını belirlemek için kullanılır. Bağlı ilk orman eşitlenen ilk orman olacaktır. 

### <a name="contact-out-of-box-rules"></a>Kutudan çıkan kurallarla iletişim kurun
Bir ilgili kişi nesnesi eşitlenecek aşağıdakileri karşılamalıdır:

* İlgili kişi posta yla etkin leştirilmiş olmalıdır. Aşağıdaki kurallarla doğrulanır:
  * `IsPresent([proxyAddresses]) = True)`. ProxyAdres özniteliği doldurulmalıdır.
  * Birincil e-posta adresi proxyAddresses özniteliği veya posta özniteliği bulunabilir. Bir'in \@ varlığı, içeriğin bir e-posta adresi olduğunu doğrulamak için kullanılır. Bu iki kuraldan biri True olarak değerlendirilmelidir.
    * `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. "SMTP:" ile bir giriş var mı ve \@ varsa, dize bir bulunabilir?
    * `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Posta özniteliği doldurulur ve varsa, dizede bir şey \@ bulunabilir mi?

Aşağıdaki kişi nesneleri Azure AD ile **eşitlenmez:**

* `IsPresent([isCriticalSystemObject])`. Kritik olarak işaretlenmiş kontak tnesneleri eşitlenmediğinden emin olun. Varsayılan yapılandırmaile herhangi olmamalıdır.
* `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
* `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Bu nesneler Exchange Online'da çalışmaz.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Çoğaltma kurbanı nesneleri eşitleme yatmayın.

### <a name="group-out-of-box-rules"></a>Kutudan çıkan kuralları gruplandırma
Bir grup nesnesi eşitlenecek aşağıdakileri karşılamalıdır:

* 50.000'den az üyesi olmalı. Bu sayı, şirket içi gruptaki üye sayısıdır.
  * Eşitleme ilk kez başlamadan önce daha fazla üyesi varsa, grup eşitlenmez.
  * Üye sayısı ilk oluşturulduğu andan itibaren artarsa, 50.000 üyeye ulaştığında, üyelik sayısı tekrar 50.000'in altındakine kadar eşitlenmeyi durdurur.
  * Not: 50.000 üyelik sayısı Azure AD tarafından da uygulanır. Bu kuralı değiştirseniz veya kaldırsanız bile grupları daha fazla üyeyle eşitleme yapamazsınız.
* Grup bir **Dağıtım Grubuysa,** posta yla da etkin leştirilmiş olmalıdır. Bkz. Bu kural için [kutudan çıkan kişi kuralları](#contact-out-of-box-rules) uygulanır.

Aşağıdaki grup nesneleri Azure AD ile **eşitlenmez:**

* `IsPresent([isCriticalSystemObject])`. Yerleşik yöneticiler grubu gibi Etkin Dizini'nde kutudan çıkan birçok nesnenin eşitlenmediğinden emin olun.
* `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. DirSync tarafından kullanılan eski grup.
* `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Rol Grubu.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Çoğaltma kurbanı nesneleri eşitleme yatmayın.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out-of-box kuralları
FSP'ler metaverse 'deki\*"any" ( ) nesnesine birleştirilir. Gerçekte, bu birleştirme yalnızca kullanıcılar ve güvenlik grupları için olur. Bu yapılandırma, ormanlar arası üyeliklerin Azure AD'de doğru şekilde çözülmesini ve temsil edilmesini sağlar.

### <a name="computer-out-of-box-rules"></a>Bilgisayar out-of-box kuralları
Bir bilgisayar nesnesinin eşitlemesi için aşağıdakileri karşılaması gerekir:

* `userCertificate ISNOTNULL`. Bu özelliği yalnızca Windows 10 bilgisayarlarda doldurulur. Bu öznitelikte bir değere sahip tüm bilgisayar nesneleri eşitlenir.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Kutudan çıkma kuralları senaryosunu anlama
Bu örnekte, bir hesap ormanı (A), bir kaynak ormanı (R) ve bir Azure REKLAM dizinine sahip bir dağıtım kullanıyoruz.

![Senaryo açıklaması ile resim](./media/concept-azure-ad-connect-sync-default-configuration/scenario.png)

Bu yapılandırmada, hesap ormanında etkin bir hesap ve bağlı bir posta kutusu olan kaynak ormanında devre dışı bırakılmış bir hesap olduğu varsayılır.

Varsayılan yapılandırma ile amacımız:

* Oturum açmayla ilgili öznitelikler, ormandan etkin hesapla eşitlenir.
* GAL'de (Genel Adres Listesi) bulunabilecek öznitelikler, ormandan posta kutusuyla eşitlenir. Posta kutusu bulunamazsa, başka bir orman kullanılır.
* Bağlantılı bir posta kutusu bulunursa, nesnenin Azure AD'ye dışa aktarılabilmek için bağlantılı etkin hesap bulunması gerekir.

### <a name="synchronization-rule-editor"></a>Eşitleme Kuralı Düzenleyicisi
Yapılandırma, aracı Eşitleme Kuralları Düzenleyicisi (SRE) ile görüntülenebilir ve değiştirilebilir ve başlangıç menüsünde bunun için bir kısayol bulunabilir.

![Eşitleme Kuralları Düzenleyici simgesi](./media/concept-azure-ad-connect-sync-default-configuration/sre.png)

SRE bir kaynak kiti aracıdır ve Azure AD Connect eşitleme ile yüklenir. Başlatabilmek için ADSyncAdmins grubunun bir üyesi olmalısınız. Başladığında, şöyle bir şey görürsünüz:

![Eşitleme Kuralları Gelen](./media/concept-azure-ad-connect-sync-default-configuration/syncrulesinbound.png)

Bu bölmede, yapılandırmanız için oluşturulan tüm Eşitleme Kuralları'nı görürsünüz. Tablodaki her satır bir Eşitleme Kuralı'dır. Kural Türleri altında sola, iki farklı tür listelenir: Gelen ve Giden. Gelen ve Giden metaverse görünümünden. Bu genel bakışta gelen kurallara ağırlıklı olarak odaklanın. Eşitleme Kuralları'nın gerçek listesi AD'de algılanan şema bağlıdır. Yukarıdaki resimde, hesap ormanı (fabrikamonline.com) Exchange ve Lync gibi herhangi bir hizmete sahip değildir ve bu hizmetler için Eşitleme Kuralları oluşturulmamıştır. Ancak, kaynak ormanında (res.fabrikamonline.com) bu hizmetler için Eşitleme Kuralları'nı bulabilirsiniz. Kuralların içeriği algılanan sürüme bağlı olarak farklıdır. Örneğin, Exchange 2013 ile yapılandırılan bir dağıtımda Exchange 2010/2007'den daha fazla öznitelik akışı yapılandırılmıştır.

### <a name="synchronization-rule"></a>Eşitleme Kuralı
Eşitleme Kuralı, bir koşul karşılandığında akan bir öznitelik kümesine sahip bir yapılandırma nesnesidir. Ayrıca, bağlayıcı alanındaki bir nesnenin metaverse'deki birleştirme **veya** **eşleşme**olarak bilinen bir nesneyle nasıl ilişkili olduğunu açıklamak için de kullanılır. Eşitleme Kuralları'nın birbirleriyle nasıl ilişkili olduklarını gösteren bir öncelik değeri vardır. Daha düşük sayısal değere sahip bir Eşitleme Kuralı daha yüksek bir önceliğe sahiptir ve öznitelik akışı çakışmasında, daha yüksek öncelik çakışma çözümünü kazanır.

Örnek olarak, AD ' den Gelen Eşitleme Kuralına bakın **– Kullanıcı Hesabı Etkin**. Bu satırı SRE'de işaretleyin ve **Düzenle'yi**seçin.

Bu kural kutudan çıkmış bir kural olduğundan, kuralı açtığınızda bir uyarı alırsınız. Kutunun [dışında kurallarda](how-to-connect-sync-best-practices-changing-default-configuration.md)herhangi bir değişiklik yapmamalısınız, bu nedenle niyetinizin ne olduğu sorulur. Bu durumda, yalnızca kuralı görüntülemek istiyorsunuz. **Hayır**'ı seçin.

![Eşitleme Kuralları uyarısı](./media/concept-azure-ad-connect-sync-default-configuration/warningeditrule.png)

Eşitleme Kuralı'nın dört yapılandırma bölümü vardır: Açıklama, Kapsam denetimi, Birleştirme kuralları ve Dönüşümler.

#### <a name="description"></a>Açıklama
İlk bölümde ad ve açıklama gibi temel bilgiler sağlar.

![Eşitleme kuralı düzenleyicisinde Açıklama sekmesi](./media/concept-azure-ad-connect-sync-default-configuration/syncruledescription.png)

Ayrıca, bu kuralın hangi bağlı sistemle ilişkili olduğu, bağlı sistemde hangi nesne türüne uygulandığı ve metaverse nesne türü hakkında bilgi bulabilirsiniz. Metaverse nesne türü, kaynak nesne türü bir kullanıcı, iNetOrgPerson veya kişi olduğunda her zaman kişidir. Metaverse nesne türü genel bir tür olarak oluşturulan bu yüzden asla değişmemelidir. Bağlantı Türü Join, StickyJoin veya Provision olarak ayarlanabilir. Bu ayar, Birleştirme Kuralları bölümüyle birlikte çalışır ve daha sonra kapsanılır.

Bu eşitleme kuralının parola eşitleme için kullanıldığını da görebilirsiniz. Bir kullanıcı bu eşitleme kuralı için kapsamdaysa, parola şirket içinde buluta eşitlenir (parola eşitleme özelliğini etkinleştirdiğiniz varsayılsın).

#### <a name="scoping-filter"></a>Kapsam filtresi
Eşitleme Kuralı'nın ne zaman uygulanacağı konusunda yapılandırmak için Kapsam Filtresi bölümü kullanılır. Baktığınız Eşitleme Kuralı'nın adı yalnızca etkin kullanıcılar için uygulanması gerektiğini gösterdiğinden, kapsam, AD özniteliği **userAccountControl** bit 2 kümesine sahip olmaması için yapılandırılır. Eşitleme altyapısı AD'de bir kullanıcı bulduğunda, **userAccountControl** ondalık değer 512 (etkin normal kullanıcı) olarak ayarlandığında bu eşitleme kuralını uygular. Kullanıcı 514 (devre dışı normal kullanıcı) olarak ayarlanmış **userAccountControl** olduğunda kural geçerli değildir.

![Eşitleme kuralı düzenleyicisinde kapsam sekmesi](./media/concept-azure-ad-connect-sync-default-configuration/syncrulescopingfilter.png)

Kapsam filtresi, iç içe geçilebilen Gruplar ve Yan Tümceler'e sahiptir. Bir Eşitleme Kuralı'nın uygulanabilmesi için grup içindeki tüm tüm yan tümcelerin karşılanması gerekir. Birden çok grup tanımlandığında, kuralın uygulanabilmesi için en az bir grubun tatmin edilmesi gerekir. Diğer bir şey, mantıksal or gruplar arasında değerlendirilir ve bir mantıksal VE bir grup içinde değerlendirilir. Bu yapılandırmanın bir örneği, Giden Eşitleme Ekarte'inde **Bulunabilir AAD - Grup Join**. Birkaç eşitleme filtre grupları vardır, örneğin bir`securityEnabled EQUAL True`güvenlik grupları için (`securityEnabled EQUAL False`) ve bir dağıtım grupları için ( ).

![Eşitleme kuralı düzenleyicisinde kapsam sekmesi](./media/concept-azure-ad-connect-sync-default-configuration/syncrulescopingfilterout.png)

Bu kural, Hangi Grupların Azure AD'ye sağlanması gerektiğini tanımlamak için kullanılır. Dağıtım Gruplarının Azure AD ile eşitlenebilmek için posta yla etkinleştirilmiş olması gerekir, ancak güvenlik grupları için bir e-posta gerekmez.

#### <a name="join-rules"></a>Birleştirme kuralları
Üçüncü bölüm, bağlayıcı alanındaki nesnelerin metaverse'deki nesnelerle nasıl ilişkili olduğunu yapılandırmak için kullanılır. Daha önce baktığınız kuralın Join Rules için herhangi bir yapılandırması yoktur, bu nedenle bunun yerine **AD – User Join'e In'e**bakabilirsiniz.

![Eşitleme kuralı düzenleyicisinde kuralları birleştirme sekmesi](./media/concept-azure-ad-connect-sync-default-configuration/syncrulejoinrules.png)

Birleştirme kuralının içeriği yükleme sihirbazında seçilen eşleşen seçene bağlıdır. Gelen bir kural için değerlendirme kaynak bağlayıcı alanındaki bir nesneyle başlar ve birleştirme kurallarındaki her grup sırayla değerlendirilir. Bir kaynak nesne birleştirme kurallarından birini kullanarak metaverse tam olarak bir nesne eşleşecek şekilde değerlendirilirse, nesneler birleştirilir. Tüm kurallar değerlendirildiyse ve eşleşme yoksa, açıklama sayfasındaki Bağlantı Türü kullanılır. Bu yapılandırma **Provision**olarak ayarlanırsa, birleştirme ölçütlerinde en az bir öznitelik varsa (bir değeri varsa) hedefte, metaverse'de yeni bir nesne oluşturulur. Metaverse yeni bir nesne sağlamak için de metaverse bir nesne **proje** olarak bilinir.

Birleştirme kuralları yalnızca bir kez değerlendirilir. Bağlayıcı uzay nesnesi ve metaverse nesnesi birleştirildiğinde, Eşitleme Kuralı'nın kapsamı hala tatmin olduğu sürece birleştirilir.

Eşitleme Kuralları değerlendirilirken, birleştirme kuralları tanımlanan yalnızca bir Eşitleme Kuralı kapsamında olmalıdır. Bir nesne için birleştirme kuralları olan birden çok Eşitleme Kuralı bulunursa, bir hata atılır. Bu nedenle, en iyi uygulama, birden çok Eşitleme Kuralı bir nesne için kapsam dayken tanımlanan birleştirme ile yalnızca bir Eşitleme Kuralı olmasıdır. Azure AD Connect eşitlemi için hazır olmayan yapılandırmada, bu kurallar ada bakarak ve adın sonunda **Join** sözcüğüne sahip olanları bularak bulunabilir. Tanımlanan birleştirme kuralları olmayan bir Eşitleme Kuralı, başka bir Eşitleme Kuralı nesneleri birleştirdiğunda veya hedefte yeni bir nesne sağlandığında öznitelik akışlarını uygular.

Yukarıdaki resme bakarsanız, kuralın **objectSID'e** **msExchMasterAccountSid** (Exchange) ve **msRTCSIP-OriginatorSid** (Lync) ile katılmaya çalıştığını görebilirsiniz, ki bu da bir hesap kaynağı orman topolojisinde beklediğimiz şeydir. Tüm ormanlarda aynı kuralı bulursunuz. Varsayım, her ormanın bir hesap veya kaynak ormanı olabileceğidir. Bu yapılandırma, tek bir ormanda yaşayan ve birleştirilmesi gerekmeyen hesaplarınız varsa da çalışır.

#### <a name="transformations"></a>Dönüşümler
Dönüştürme bölümü, nesneler birleştiğinde ve kapsam filtresi karşılandığında hedef nesneye uygulanan tüm öznitelik akışlarını tanımlar. AD – User AccountEnabled Eşitleme **Kuralı'ndan In'e** geri dönersek, aşağıdaki dönüşümleri bulabilirsiniz:

![Eşitleme kuralı düzenleyicisinde dönüşümler sekmesi](./media/concept-azure-ad-connect-sync-default-configuration/syncruletransformations.png)

Bu yapılandırmayı bağlam içine koymak için, Hesap-Kaynak orman dağıtımında, hesap ormanında etkin bir hesap ve Exchange ve Lync ayarlarıyla kaynak ormanında devre dışı bırakılmış bir hesap bulması beklenir. Baktığınız Eşitleme Kuralı oturum açma için gereken öznitelikleri içerir ve bu öznitelikler, etkin bir hesap olan ormandan akmalıdır. Tüm bu öznitelik akışları tek bir Eşitleme Kuralı'nda bir araya getirilir.

Bir dönüştürmenin farklı türleri olabilir: Sabit, Doğrudan ve İfade.

* Sabit bir akış her zaman sabit kodlanmış bir değer akar. Yukarıdaki durumda, her zaman **hesapEnabled**adlı metaverse özniteliği **True** değerini ayarlar.
* Doğrudan akış her zaman kaynaktaki özniteliğin değerini hedef özniteliğe olduğu gibi akar.
* Üçüncü akış türü Expression'dır ve daha gelişmiş yapılandırmalara izin verir.

İfade dili VBA (Visual Basic for Applications) olduğundan, Microsoft Office veya VBScript deneyimi olan kişiler biçimi tanıyacaktır. Öznitelikler kare ayraçlar, [öznitelikName] içinde eklenir. Öznitelik adları ve işlev adları büyük/küçük harf duyarlıdır, ancak Eşitleme Kuralları Düzenleyicisi ifadeleri değerlendirir ve ifade geçerli değilse bir uyarı sağlar. Tüm ifadeler iç içe işlevler ile tek bir satırda ifade edilir. Yapılandırma dilinin gücünü göstermek için, burada pwdLastSet için akış, ancak ek yorumlar eklenen:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .NET datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Öznitelik akışlarıiçin ifade dili hakkında daha fazla bilgi için [Bildirimsel Sağlama İfadelerini Anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) konusuna bakın.

### <a name="precedence"></a>Önceliği
Şimdi bazı bireysel Eşitleme Kuralları baktınız, ancak kurallar yapılandırmada birlikte çalışır. Bazı durumlarda, birden çok eşitleme kuralından aynı hedef özniteliğe bir öznitelik değeri katkıda bulunulr. Bu durumda, öznitelik önceliği hangi öznitelik kazanır belirlemek için kullanılır. Örnek olarak, öznitelik kaynağıAnchor'a bakın. Bu öznitelik, Azure AD'de oturum açabilmek için önemli bir özelliktir. Bu öznitelik için ad **- Kullanıcı Hesabı Etkin** ve AD ' den - User Common olmak üzere iki farklı Eşitleme Kuralı'nda bir öznitelik akışı bulabilirsiniz. **In from AD – User Common** Eşitleme Kuralı önceliği nedeniyle, metaverse nesneye birkaç nesne birleştiğinde sourceAnchor özniteliği önce etkin bir hesapla ormandan katkıda bulunular. Etkin hesap yoksa, eşitleme motoru AD ' den catch-all Synchronization Kuralını kullanır **– Kullanıcı Ortak**. Bu yapılandırma, devre dışı bırakılan hesaplar için bile bir kaynakÇapa olmasını sağlar.

![Eşitleme Kuralları Gelen](./media/concept-azure-ad-connect-sync-default-configuration/syncrulesinbound.png)

Eşitleme Kuralları'nın önceliği yükleme sihirbazı tarafından gruplar halinde ayarlanır. Bir gruptaki tüm kurallar aynı ada sahiptir, ancak bunlar farklı bağlı dizinlere bağlıdır. Yükleme sihirbazı **AD' den In - User Join** en yüksek önceliği verir ve bağlı tüm AD dizinleri üzerinde yineler. Daha sonra önceden tanımlanmış bir sırada sonraki kural grupları ile devam ediyor. Bir grubun içinde, kurallar Bağlayıcılar sihirbaza eklendi sırasına eklenir. Sihirbaz aracılığıyla başka bir Bağlayıcı eklenirse, Eşitleme Kuralları yeniden sıralanır ve her gruba yeni Bağlayıcı kuralları en son eklenir.

### <a name="putting-it-all-together"></a>Hepsini bir araya getirme
Yapılandırmanın farklı Eşitleme Kuralları ile nasıl çalıştığını anlamak için Eşitleme Kuralları hakkında yeterince şey biliyoruz. Bir kullanıcıya ve metaverse katkıda bulunan özniteliklere bakarsanız, kurallar aşağıdaki sırada uygulanır:

| Adı | Açıklama |
|:--- |:--- |
| AD'den Içeri – Kullanıcı Join |Bağlayıcı uzay nesnelerini metaverse ile birleştirmek için kural. |
| AD'den – UserAccount Etkin |Azure AD ve Office 365'te oturum açma için gereken öznitelikler. Bu öznitelikleri etkin hesaptan istiyoruz. |
| AD'den In – Exchange'den Kullanıcı Ortak |Genel Adres Listesinde bulunan öznitelikler. Kullanıcının posta kutusunu bulduğumuz ormanda veri kalitesinin en iyisi olduğunu varsayıyoruz. |
| AD'den içeri – Kullanıcı Ortak |Genel Adres Listesinde bulunan öznitelikler. Posta kutusu bulamadığımız durumlarda, birleştirilmiş başka bir nesne öznitelik değerine katkıda bulunabilir. |
| AD'den – Kullanıcı Değişimi |Yalnızca Exchange algılanmışsa bulunur. Tüm altyapı Exchange öznitelikleri akar. |
| AD'den – Kullanıcı Lync |Yalnızca Lync algılanmışsa var olur. Tüm altyapı Lync öznitelikleri akar. |

## <a name="next-steps"></a>Sonraki adımlar
* [Bildirimsel Hükmü Anlama'da](concept-azure-ad-connect-sync-declarative-provisioning.md)yapılandırma modeli hakkında daha fazla bilgi edinin.
* [Bildirimsel Hüküm Veren İfadeleri Anlama'da](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)ifade dili hakkında daha fazla bilgi edinin.
* Kullanıcıları ve Kişileri Anlama'da kutudan çıkma yapılandırmasının nasıl çalıştığını okumaya devam [edin](concept-azure-ad-connect-sync-user-and-contacts.md)
* [Varsayılan yapılandırmada değişiklik yapma](how-to-connect-sync-change-the-configuration.md)konusunda bildirimsel hükmü kullanarak pratik bir değişiklik yapma şekline bakın.

**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

