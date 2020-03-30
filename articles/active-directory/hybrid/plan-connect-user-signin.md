---
title: 'Azure AD Connect: Kullanıcı oturum açma | Microsoft Dokümanlar'
description: Azure AD Özel ayarlar için kullanıcı oturum aç.'ı bağlayın.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 547b118e-7282-4c7f-be87-c035561001df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/31/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a08120b98c7a08bca50453df59df313b1645c5c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331260"
---
# <a name="azure-ad-connect-user-sign-in-options"></a>Azure AD Connect kullanıcı oturum açma seçenekleri
Azure Active Directory (Azure AD) Connect, kullanıcılarınızın aynı parolaları kullanarak hem buluthem de şirket içi kaynaklarda oturum açmalarına olanak tanır. Bu makalede, Azure AD'de oturum açken kullanmak istediğiniz kimliği seçmenize yardımcı olacak her kimlik modeli için temel kavramlar açıklanmaktadır.

Azure AD kimlik modelini zaten biliyorsanız ve belirli bir yöntem hakkında daha fazla bilgi edinmek istiyorsanız, uygun bağlantıya bakın:

* [Sorunsuz Tek Oturum Açma (SSO)](how-to-connect-sso.md) ile şifre karma [senkronizasyonu](#password-hash-synchronization)
* Sorunsuz Tek Oturum [Açma (SSO)](how-to-connect-sso.md) ile [geçiş kimlik doğrulaması](how-to-connect-pta.md)
* [Federe SSO (Aktif Dizin Federasyon Hizmetleri (AD FS ile))](#federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2)
* [PingFederate ile federasyon](#federation-with-pingfederate)

> [!NOTE] 
> Azure AD için federasyon yapılandırarak Azure AD kiracınızla federe etki alanlarınız arasında güven oluşturduğunuzu unutmayın. Bu güven federe etki alanı kullanıcıları kiracı içinde Azure AD bulut kaynaklarına erişebilir.  
>

## <a name="choosing-the-user-sign-in-method-for-your-organization"></a>Kuruluşunuz için kullanıcı oturum açma yöntemini seçme
Azure AD Connect'i uygulamanın ilk kararı, kullanıcılarınızın oturum açmada hangi kimlik doğrulama yöntemini kullanacağını seçmektir. Kuruluşunuzun güvenliğini ve gelişmiş gereksinimlerini karşılayan doğru yöntemi seçtiğinizden emin olmak önemlidir. Kimlik doğrulama, buluttaki uygulamalara ve verilere erişmek için kullanıcıkimliklerini doğruladığı için önemlidir. Doğru kimlik doğrulama yöntemini seçmek için, seçiminizi uygulamanın zamanını, varolan altyapıyı, karmaşıklığı ve maliyetini göz önünde bulundurmanız gerekir. Bu etkenler her kuruluş için farklıdır ve zaman içinde değişebilir.

Azure AD aşağıdaki kimlik doğrulama yöntemlerini destekler: 

* **Bulut Kimlik Doğrulaması** - Bu kimlik doğrulama yöntemini seçtiğinizde Azure AD, kullanıcının oturum açma işlemi için kimlik doğrulama işlemini işler. Bulut kimlik doğrulaması ile iki seçenek arasından seçim yapabilirsiniz: 
   * **Parola karma eşitleme (PHS)** - Password Hash Sync, kullanıcıların Azure AD Connect dışında ek altyapı dağıtmak zorunda kalmadan şirket içinde kullandıkları kullanıcı adını ve parolayı kullanmalarını sağlar. 
   * **Geçiş kimlik doğrulaması (PTA)** - Bu seçenek parola karma eşitleme benzer, ancak güçlü güvenlik ve uyumluluk ilkeleri olan kuruluşlar için şirket içi yazılım aracıları kullanarak basit bir parola doğrulama sağlar.
* **Federe kimlik doğrulama** - Bu kimlik doğrulama yöntemini seçtiğinizde Azure AD, kullanıcının oturum açma işlemini doğrulamak için kimlik doğrulama işlemini AD FS veya üçüncü taraf federasyon sistemi gibi ayrı bir güvenilir kimlik doğrulama sistemine teslim eder. 

Office 365, SaaS uygulamaları ve diğer Azure AD tabanlı kaynaklarda kullanıcı oturum açmaolanağı nı etkinleştirmek isteyen kuruluşların çoğu için varsayılan parola karma eşitleme seçeneğini öneririz.
 
Kimlik doğrulama yöntemi seçme hakkında ayrıntılı bilgi [için](../../security/fundamentals/choose-ad-authn.md) bkz.

### <a name="password-hash-synchronization"></a>Parola karması eşitleme
Parola karma senkronizasyonu ile, kullanıcı parolalarının karmaları şirket içi Active Directory'den Azure AD'ye eşitlenir. Parolalar şirket içinde değiştirildiğinde veya sıfırlandığında, kullanıcılarınızın bulut kaynakları ve şirket içi kaynaklar için her zaman aynı parolayı kullanabilmesi için yeni parola kalıpları hemen Azure AD ile senkronize edilir. Parolalar hiçbir zaman Azure AD'ye gönderilmez veya Azure AD'de açık metin olarak depolanır. Azure AD'de self servis parola sıfırlamayı etkinleştirmek için parola karma senkronizasyonunu parola yazma yla birlikte kullanabilirsiniz.

Ayrıca, şirket ağında bulunan etki alanı birleştirilmiş makinelerdeki kullanıcılar için [Kesintisiz SSO'yu](how-to-connect-sso.md) etkinleştirebilirsiniz. Tek oturum açma özelliğiyle, etkinleştirilen kullanıcıların bulut kaynaklarına güvenli bir şekilde erişmelerine yardımcı olmak için yalnızca bir kullanıcı adı girmeleri gerekir.

![Parola karması eşitleme](./media/plan-connect-user-signin/passwordhash.png)

Daha fazla bilgi için [parola karma eşitleme](how-to-connect-password-hash-synchronization.md) makalesine bakın.

### <a name="pass-through-authentication"></a>Doğrudan kimlik doğrulama
Geçiş kimlik doğrulaması ile kullanıcının parolası şirket içi Active Directory denetleyicisi tarafından doğrulanır. Parolanın Azure AD'de herhangi bir biçimde bulunması gerekmez. Bu, oturum açma saati kısıtlamaları gibi şirket içi ilkelerin bulut hizmetlerine kimlik doğrulama sırasında değerlendirilmesine olanak tanır.

Geçiş kimlik doğrulaması, şirket içi ortamda Windows Server 2012 R2 etki alanı birleştirilmiş makinede basit bir aracı kullanır. Bu aracı, parola doğrulama isteklerini dinler. Gelen bağlantı noktalarının Internet'e açık olmasını gerektirmez.

Ayrıca, şirket ağında bulunan etki alanı birleştirilmiş makinelerde kullanıcılar için tek oturum açma olanağı da sağlayabilirsiniz. Tek oturum açma özelliğiyle, etkinleştirilen kullanıcıların bulut kaynaklarına güvenli bir şekilde erişmelerine yardımcı olmak için yalnızca bir kullanıcı adı girmeleri gerekir.
![Doğrudan kimlik doğrulama](./media/plan-connect-user-signin/pta.png)

Daha fazla bilgi için bkz.
- [Doğrudan kimlik doğrulama](how-to-connect-pta.md)
- [Çoklu oturum açma](how-to-connect-sso.md)

### <a name="federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2"></a>Windows Server 2012 R2'de AD FS ile yeni veya varolan bir çiftlik kullanan federasyon
Federe oturum açma ile kullanıcılarınız, şirket içi parolalarıyla Azure AD tabanlı hizmetlerde oturum açabilir. Şirket ağındayken şifrelerini girmelerine bile gerek yok. AD FS ile federasyon seçeneğini kullanarak, Windows Server 2012 R2'de AD FS'li yeni veya varolan bir çiftlik dağıtabilirsiniz. Varolan bir çiftlik belirtmeyi seçerseniz, Azure AD Connect, kullanıcılarınızın oturum açabilmesi için çiftliğiniz ile Azure AD'niz arasındaki güveni yapılandırır.

<center>

![Windows Server 2012 R2 AD FS ile Federasyon](./media/plan-connect-user-signin/federatedsignin.png)</center>

#### <a name="deploy-federation-with-ad-fs-in-windows-server-2012-r2"></a>Windows Server 2012 R2'de AD FS ile federasyonu dağıtma

Yeni bir çiftlik dağıtıyorsanız, şunları yapmanız gerekir:

* Federasyon sunucusu için bir Windows Server 2012 R2 sunucusu.
* Web Application Proxy için bir Windows Server 2012 R2 sunucusu.
* Amacınız federasyon hizmet adı için bir TLS/SSL sertifikasına sahip bir .pfx dosyası. Örneğin: fs.contoso.com.

Yeni bir çiftlik dağıtıyorsanız veya varolan bir çiftliği kullanıyorsanız, şunları yapmanız gerekir:

* Federasyon sunucularınızdaki yerel yönetici kimlik bilgileri.
* Web Application Proxy rolünü dağıtmayı planladığınız çalışma grubu sunucularında (etki alanı birleştirilmiş değil) yerel yönetici kimlik bilgileri.
* Windows Remote Management'ı kullanarak AD FS veya Web Application Proxy'yi yüklemek istediğiniz diğer makinelere bağlanabilmek için sihirbazı çalıştırdığınız makine.

Daha fazla bilgi için bkz: [SSO'nun AD FS ile yapılandırılma.](how-to-connect-install-custom.md#configuring-federation-with-ad-fs)

### <a name="federation-with-pingfederate"></a>PingFederate ile federasyon
Federe oturum açma ile kullanıcılarınız, şirket içi parolalarıyla Azure AD tabanlı hizmetlerde oturum açabilir. Şirket ağındayken şifrelerini girmelerine bile gerek yok.

Azure Active Directory ile kullanılmak üzere PingFederate yapılandırma hakkında daha fazla bilgi için bkz: [Azure Active Directory ve Office 365 ile PingFederate Tümleştirmesi](https://www.pingidentity.com/AzureADConnect)

PingFederate kullanarak Azure AD Connect'i ayarlama hakkında bilgi için Azure [AD Connect özel yüklemesi](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)

#### <a name="sign-in-by-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>AD FS'nin önceki bir sürümünü veya üçüncü taraf çözümünün bir sürümünü kullanarak oturum açın
Ad FS'nin (AD FS 2.0 gibi) önceki bir sürümünü veya üçüncü taraf federasyon sağlayıcısını kullanarak bulut oturum açma işlemlerini zaten yapılandırmışsanız, Azure AD Connect aracılığıyla kullanıcı oturum açma yapılandırmasını atlamayı seçebilirsiniz. Bu, oturum açma için mevcut çözümünüzü kullanmaya devam ederken Azure AD Connect'in en son eşitleme ve diğer özelliklerini elde etmenizi sağlar.

Daha fazla bilgi için [Azure AD üçüncü taraf federasyon uyumluluk listesine](how-to-connect-fed-compatibility.md)bakın.


## <a name="user-sign-in-and-user-principal-name"></a>Kullanıcı oturum açma ve kullanıcı ana adı
### <a name="understanding-user-principal-name"></a>Kullanıcı ana adını anlama
Etkin Dizin'de varsayılan kullanıcı anaadı (UPN) soneki, kullanıcı hesabının oluşturulduğu etki alanının DNS adıdır. Çoğu durumda, bu, Internet'te kurumsal etki alanı olarak kayıtlı alan adıdır. Ancak, Active Directory Etki Alanları ve Güvenleri kullanarak daha fazla UPN sonekleri ekleyebilirsiniz.

Kullanıcının UPN biçimi username@domainvardır. Örneğin, "contoso.com" adlı bir Active Directory etki alanı için, Johnjohn@contoso.comadlı bir kullanıcı UPN " "ait olabilir. Kullanıcının UPN'si RFC 822'yi temel alın. UPN ve e-posta aynı biçimi paylaşsa da, bir kullanıcı için UPN değeri kullanıcının e-posta adresiyle aynı olabilir veya olmayabilir.

### <a name="user-principal-name-in-azure-ad"></a>Azure AD'de kullanıcı adı
Azure AD Connect sihirbazı userPrincipalName özniteliğini kullanır veya Azure AD'de kullanıcı ana adı olarak şirket içinde kullanılacak özniteliği (özel yüklemede) belirtmenize olanak tanır. Bu, Azure AD'de oturum açma için kullanılan değerdir. userPrincipalName özniteliğinin değeri Azure AD'de doğrulanmış bir etki alanına karşılık gelmiyorsa, Azure AD varsayılan .onmicrosoft.com değeriyle değiştirir.

Azure Active Directory'deki her dizin, Azure veya diğer Microsoft hizmetlerini kullanmaya başlamanızı sağlayan biçimi contoso.onmicrosoft.com olan yerleşik bir etki alanı adı ile birlikte gelir. Özel etki alanlarını kullanarak oturum açma deneyimini geliştirebilir ve basitleştirebilirsiniz. Azure AD'deki özel etki alanı adları ve bir etki alanını nasıl doğruleyeceğiniz hakkında bilgi [için](../fundamentals/add-custom-domain.md)bkz.

## <a name="azure-ad-sign-in-configuration"></a>Azure AD oturum açma yapılandırması
### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD Connect ile Azure AD oturum açma yapılandırması
Azure AD oturum açma deneyimi, Azure AD dizininde doğrulanan özel etki alanlarından biriyle senkronize edilen bir kullanıcının kullanıcı ana adı sonesine eşleşip eşleşmediğine bağlıdır. Azure AD Connect, buluttaki kullanıcı oturum açma deneyiminin şirket içi deneyime benzemesi için Azure AD oturum açma ayarlarını yapılandırırken yardım sağlar.

Azure AD Connect, etki alanları için tanımlanan UPN sonekleri listeler ve bunları Azure AD'de özel bir etki alanıyla eşleştirmeye çalışır. Daha sonra yapılması gereken uygun eylem ile size yardımcı olur.
Azure AD oturum açma sayfası, şirket içi Active Directory için tanımlanan UPN sonekleri listeler ve her sonek için karşılık gelen durumu görüntüler. Durum değerleri aşağıdakilerden biri olabilir:

| Durum | Açıklama | Eylem gerekli |
|:--- |:--- |:--- |
| Doğrulandı |Azure AD Connect, Azure AD'de eşleşen doğrulanmış bir etki alanı buldu. Bu etki alanının tüm kullanıcıları şirket içi kimlik bilgilerini kullanarak oturum açabilir. |Hiçbir eylem gerekli değildir. |
| Doğrulanmadı |Azure AD Connect, Azure AD'de eşleşen bir özel etki alanı buldu, ancak doğrulanmadı. Etki alanı doğrulanmamışsa, bu etki alanının kullanıcılarının UPN soneki, eşitlemeden sonra varsayılan .onmicrosoft.com soneki olarak değiştirilir. | [Azure AD'de özel etki alanını doğrulayın.](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) |
| Eklenmez |Azure AD Connect, UPN sonekine karşılık gelen özel bir etki alanı bulamadı. Etki alanı Azure'da eklenmez ve doğrulanmazsa, bu etki alanının kullanıcılarının UPN soneki varsayılan .onmicrosoft.com soneki olarak değiştirilir. | [UPN sonekine karşılık gelen özel bir etki alanı ekleyin ve doğrulayın.](../fundamentals/add-custom-domain.md) |

Azure AD oturum açma sayfası, şirket içi Active Directory için tanımlanan UPN sonekleri ve geçerli doğrulama durumuyla Azure AD'deki ilgili özel etki alanını listeler. Özel bir yüklemede, azure **AD oturum açma** sayfasındaki kullanıcı ana adının özniteliğini seçebilirsiniz.

![Azure AD oturum açma sayfası](./media/plan-connect-user-signin/custom_azure_sign_in.png)

Azure AD'deki özel etki alanlarının en son durumunu yeniden almak için yenileme düğmesini tıklatabilirsiniz.

### <a name="selecting-the-attribute-for-the-user-principal-name-in-azure-ad"></a>Azure AD'de kullanıcı ana adının özniteliğini seçme
Öznitelik userPrincipalName, kullanıcıların Azure AD ve Office 365'te oturum açtıklarında kullandıkları özniteliktir. Kullanıcılar eşitlenmeden önce Azure AD'de kullanılan etki alanlarını (UPN sonekleri olarak da bilinir) doğrulamanız gerekir.

Varsayılan öznitelik userPrincipalName'i korumanızı şiddetle öneririz. Bu öznitelik kaydedilebilir değilse ve doğrulanamıyorsa, oturum açma kimliğini tutan öznitelik olarak başka bir öznitelik (örneğin e-posta) seçmek mümkündür. Bu, Alternatif Kimlik olarak bilinir. Alternatif Kimlik özniteliği değeri RFC 822 standardını izlemelidir. Oturum açma çözümü olarak hem parola SSO hem de federasyon SSO içeren bir Alternatif Kimlik kullanabilirsiniz.

> [!NOTE]
> Alternatif Kimlik kullanmak tüm Office 365 iş yükleriyle uyumlu değildir. Daha fazla bilgi için bkz. [Alternatif Oturum Açma Kimliğini Yapılandırma](https://technet.microsoft.com/library/dn659436.aspx).
>
>

#### <a name="different-custom-domain-states-and-their-effect-on-the-azure-sign-in-experience"></a>Farklı özel etki alanı durumları ve bunların Azure oturum açma deneyimi üzerindeki etkileri
Azure REKLAM dizininizdeki özel etki alanı durumları ile şirket içinde tanımlanan UPN sonekleri arasındaki ilişkiyi anlamak çok önemlidir. Azure AD Connect'i kullanarak senkronizasyon ayarlarken olası farklı Azure oturum açma deneyimlerini gözden geçirelim.

Aşağıdaki bilgiler için, upn'in bir parçası olarak şirket içi dizinde kullanılan UPN soneki contoso.com ile ilgilendiğimizi user@contoso.comvarsayalım.

###### <a name="express-settingspassword-hash-synchronization"></a>Ekspres ayarlar/Parola karma senkronizasyonu

| Durum | Kullanıcı Azure oturum açma deneyimi üzerindeki etkisi |
|:---:|:--- |
| Eklenmez |Bu durumda, Azure AD dizinine contoso.com için özel bir etki alanı eklenmemiştir. Sonek @contoso.com ile upn şirket içinde olan kullanıcılar Azure'da oturum açmak için şirket içi UPN'lerini kullanamaz. Bunun yerine, varsayılan Azure REKLAM dizininin sonekini ekleyerek Azure AD tarafından kendilerine sağlanan yeni bir UPN kullanmaları gerekir. Örneğin, kullanıcıları Azure AD dizinine azurecontoso.onmicrosoft.com eşitlüyorsanız, şirket içi user@contoso.com kullanıcıya bir UPN'lik user@azurecontoso.onmicrosoft.com. |
| Doğrulanmadı |Bu durumda, Azure AD dizinine eklenen özel bir etki contoso.com sahibiz. Ancak, henüz doğrulanmadı. Etki alanını doğrulamadan kullanıcıları eşitleme ile devam ederseniz, kullanıcılara "Eklenmemiş" senaryosunda olduğu gibi Azure AD tarafından yeni bir UPN atanır. |
| Doğrulandı |Bu durumda, UPN soneki için Azure AD'ye zaten eklenen ve doğrulanmış özel bir etki contoso.com sahibiz. Kullanıcılar, Azure user@contoso.comAD ile senkronize edildikten sonra Azure'da oturum açabilmek için şirket içi kullanıcı ana adlarını kullanabilirler. |

###### <a name="ad-fs-federation"></a>AD FS federasyonu
Azure AD'de varsayılan .onmicrosoft.com etki alanına veya Azure AD'de doğrulanmamış özel bir etki alanına sahip bir federasyon oluşturamazsınız. Azure AD Connect sihirbazını çalıştırırken, federasyon oluşturmak için doğrulanmamış bir etki alanı seçerseniz, Azure AD Connect,DNS'nizin etki alanı için barındırıldığı yerde oluşturulacak gerekli kayıtları ister. Daha fazla bilgi için [bkz.](how-to-connect-install-custom.md#verify-the-azure-ad-domain-selected-for-federation)

**AD FS ile**kullanıcı oturum açma seçeneği Federasyonu'nu seçtiyseniz, Azure AD'de federasyon oluşturmaya devam etmek için özel bir etki alanınız olması gerekir. Tartışmamız için bu, Azure REKLAM dizinine contoso.com özel bir etki alanı eklememiz gerektiği anlamına gelir.

| Durum | Kullanıcı Azure oturum açma deneyimi üzerindeki etkisi |
|:---:|:--- |
| Eklenmez |Bu durumda, Azure AD Connect, Azure REKLAM dizininde contoso.com UPN soneki için eşleşen bir özel etki alanı bulamadı. Kullanıcıların şirket içi UPN'leri (benzeri) user@contoso.comile AD FS kullanarak oturum açmaları gerekiyorsa, özel bir etki alanı contoso.com eklemeniz gerekir. |
| Doğrulanmadı |Bu durumda, Azure AD Connect, etki alanınızı daha sonraki bir aşamada nasıl doğrulayabileceğinizle ilgili uygun ayrıntıları ister. |
| Doğrulandı |Bu durumda, başka bir işlem yapmadan yapılandırmaya devam edebilirsiniz. |

## <a name="changing-the-user-sign-in-method"></a>Kullanıcı oturum açma yöntemini değiştirme
Sihirbazla Azure AD Connect'in ilk yapılandırmasından sonra Azure AD Connect'te kullanılabilen görevleri kullanarak kullanıcı oturum açma yöntemini federasyondan, parola karma senkronizasyonundan veya geçiş kimlik doğrulamasını değiştirebilirsiniz. Azure AD Connect sihirbazını yeniden çalıştırDığınızda gerçekleştirebileceğiniz görevlerin listesini görürsünüz. Görev listesinden **kullanıcı oturum aç'ını değiştir'i** seçin.

![Kullanıcı oturum açma'yı değiştirme](./media/plan-connect-user-signin/changeusersignin.png)

Bir sonraki sayfada, Azure AD kimlik bilgilerini sağlamanız istenir.

![Azure AD'ye Bağlanma](./media/plan-connect-user-signin/changeusersignin2.png)

Kullanıcı **oturum açma** sayfasında, istenen kullanıcı oturum açma sayfasını seçin.

![Azure AD'ye Bağlanma](./media/plan-connect-user-signin/changeusersignin2a.png)

> [!NOTE]
> Yalnızca parola karma senkronizasyonuna geçici bir geçiş yapıyorsanız, **kullanıcı hesaplarını dönüştürme onay** kutusunu seçin. Seçeneği denetlememek her kullanıcıyı federe ye dönüştürür ve birkaç saat sürebilir.
>
>

## <a name="next-steps"></a>Sonraki adımlar
- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)hakkında daha fazla bilgi edinin.
- [Azure AD Connect tasarım kavramları](plan-connect-design-concepts.md)hakkında daha fazla bilgi edinin.
