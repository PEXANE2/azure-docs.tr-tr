---
title: Azure Avustralya 'da Kimlik Federasyonu
description: Avustralya Kamu politika, yönetmelikler ve yasalların belirli ihtiyaçlarını karşılamak için Avustralya bölgelerindeki Kimlik Federasyonu yapılandırma kılavuzu.
author: galey801
ms.service: azure-australia
ms.topic: quickstart
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: e161e36ee7b403381b65f52a6f416be09025d0a2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68570862"
---
# <a name="identity-federation-in-azure-australia"></a>Azure Avustralya 'da Kimlik Federasyonu

Ortak bulut teklifleriyle kimlik yönetimi ve Federasyon, bulutu kullanmaya yönelik en önemli ilk adımlardan biridir. Microsoft 'un Azure Active Directory Hizmeti, bulut hizmetlerine erişimi etkinleştirmek için Kullanıcı bilgilerini depolar ve diğer Azure hizmetlerini kullanmak için bir ön önkoşuldur.

Bu makale, Azure Active Directory uygulamaya yönelik temel tasarım noktalarını ve Active Directory Domain Services bir etki alanından eşitlemeyi ve güvenli kimlik doğrulaması uygulamayı içerir. Belirli odak, Avustralya Cyber Güvenlik Merkezi 'nin bilgi güvenliği El Ile (ıSM) ve Azure Sertifika raporlarındaki önerilere yerleştirilir.

Azure Active Directory içinde depolanan bilgilerin sınıflandırılması, nasıl tasarlandığına ilişkin kararları bilgilendirmelidir. Aşağıdaki alıntı, [ACSC sertifika raporundan sağlanır – Microsoft Azure](https://aka.ms/au-irap):

>**ACSC sertifika raporu – Microsoft Azure** Azure Active Directory (Azure AD), Active Directory Federasyon Hizmetleri ile yapılandırılmalıdır. bu varlık, Active Directory, korunan varlıkların kullanımını ve veri içeriğini sınıflandırdığında. SıNıFLANDıRıLMAYAN ayırt dağıtıcı sınırlandırma Işaretleri (UDLM) sınıflandırmasına ilişkin Active Directory veri, Federasyon gerektirirken, dışarıdan sağlanmakta olan hizmetle ilişkili riskleri azaltmak için, kurumlar varlıkları yine de Federasyon uygulayabilir Avustralya.

Bu nedenle, hangi bilgilerin eşitleme yaptığı ve kullanıcıların kimlik doğrulamasının olduğu mekanizmanın burada ele alındığı iki temel sorun vardır.

## <a name="key-design-considerations"></a>Önemli tasarım konuları

### <a name="user-synchronisation"></a>Kullanıcı eşitleme

Azure AD Connect dağıttığınızda, eşitleme yapılacak veriler hakkında birkaç karar vardır. Azure AD Connect, Microsoft Identity Manager tabanlıdır ve dizinler arasında veri [dönüştürmek](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-best-practices-changing-default-configuration) için güçlü bir özellik kümesi sağlar.

Microsoft Danışmanlık Hizmetleri, mevcut Windows Server Active Directory bir ADRAP değerlendirmesi yapmak için kullanılabilir. ADRAP, Azure Active Directory eşitlemekten önce düzeltilmesi gerekebilecek sorunları belirlemede yardımcı olur. Microsoft Premier Destek anlaşmaları genellikle bu hizmeti içerecektir.

[Iddüzeltmesini aracı](https://docs.microsoft.com/office365/enterprise/install-and-run-idfix) , şirket içi Active Directory etki ALANıNıZı Azure AD ile eşitlemeöncesinde sorunlar için tarar. Iddüzeltmesini Azure AD Connect uygulamadan önce önemli bir ilk adımdır. Bir ıddüzeltmesini taraması çok sayıda sorunu belirleyebilse de, bu sorunların çoğu betiklerle hızla çözülebilir veya Azure AD Connect veri dönüştürmeleri kullanılarak çalışabilir.

Azure AD, kullanıcıların kimlik doğrulamasını etkinleştirmek için dışarıdan yönlendirilebilir en üst düzey etki alanına sahip olmasını gerektirir. Etki alanınız dışarıdan yönlendirilebilir olmayan bir UPN sonekine sahipse, AD Connect 'teki [Alternatif oturum açma kimliğini](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-userprincipalname) kullanıcının posta özniteliğine ayarlamanız gerekir. Kullanıcılar, etki alanı oturum açma yerine kendi e-posta adresiyle Azure hizmetlerinde oturum açabilirler.

Kullanıcı hesaplarındaki UPN soneki, ancak PowerShell gibi araçlar kullanılarak da değiştirilebilir; diğer bağlı sistemler için öngörülemeyen sonuçlara sahip olabilir ve artık en iyi deneyim vermez.

Azure Active Directory hangi özniteliklerin eşitlemelerine karar verirken, tüm özniteliklerin gerekli olduğunu varsaymak en güvenli hale gelir. Bir dizinin gerçek KORUMALı verileri içermesi, ancak bir denetim yapılması önerilir. Dizinde korunan veriler bulunursa, özniteliği atlama veya dönüştürme etkisini değerlendirin. Faydalı bir kılavuz olarak, [Microsoft bulut hizmetleri için](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-attributes-synchronized)gereken özniteliklerin bir listesi vardır.

### <a name="authentication"></a>Authentication

Kullanılabilir seçeneklerin ve son kullanıcıların güvenliğini sağlamak için nasıl kullanılabilecekleri hakkında anlaşılması önemlidir.
Microsoft, Azure Active Directory karşı kullanıcıların kimliğini doğrulamak için [üç yerel çözüm](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-user-signin) sunar:

* Parola karması eşitleme-Active Directory Domain Services karma hale getirilmiş parolalar, Azure Active Directory Azure AD Connect.
* [Geçişli kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta) -parolalar Active Directory Domain Services içinde kalır. Kullanıcıların kimliği bir aracı aracılığıyla Active Directory Domain Services doğrulanır. Azure AD 'de hiçbir parola depolanmaz.
* [Federal SSO](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-whatis) -Azure Active Directory Active Directory Federasyon Hizmetleri (AD FS), oturum açma sırasında Azure, kullanıcıları kimlik doğrulamaya Active Directory Federasyon Hizmetleri (AD FS) yönlendirir. Azure AD 'de hiçbir parola depolanmaz.

Parola karması eşitleme, RESMI: hassas ve aşağıdaki verilerin dizin içinde depolandığı senaryolarda kullanılabilir. KORUNAN verilerin depolanacağı senaryolar, kalan iki seçenekten birini gerektirir.

Bu seçeneklerin üçü, [ACSC tüketici kılavuzunun](https://aka.ms/au-irap) devre dışı bırakılmakta olması Için [parola geri yazma](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback)'yı destekler. Ancak kuruluşlar, verimlilik kazançlarını karşı parola geri yazma özelliğinin devre dışı bırakılması riskini değerlendirmeli ve self servis parola sıfırlamaları için daha az destek çabasıdır.

#### <a name="pass-through-authentication-pta"></a>Geçişli kimlik doğrulaması (PTA)

Geçişli kimlik doğrulaması, ıRAP değerlendirmesi tamamlandıktan sonra serbest bırakıldı ve bu nedenle; , çözümün kuruluşunuzun risk profiline nasıl uyduğunu belirlemekte ayrı ayrı değerlendirilmelidir. Gelişmiş güvenlik durumunu nedeniyle doğrudan kimlik doğrulama, Microsoft tarafından Federasyon üzerinden tercih edilir.

![Geçişli kimlik doğrulaması](media/pta1.png)

Geçişli kimlik doğrulaması göz önünde bulundurmanız için çeşitli tasarım faktörleri sunar:

* Doğrudan kimlik doğrulama aracısının Microsoft Bulut hizmetlere giden bağlantılar kurabilmesi gerekir.
* Hizmetin yüksek oranda kullanılabilir olacağını sağlamak için birden fazla aracı yükleme. En az üç aracıyı ve en fazla 12 aracıyı dağıtmak en iyi uygulamadır.
* En iyi yöntem, aracıyı doğrudan bir Active Directory Etki Alanı Denetleyicisine yüklemekten kaçınmaktır. Varsayılan olarak, Azure AD Connect doğrudan kimlik doğrulama ile dağıttığınızda, aracıyı AD Connect sunucusuna yükler.
* Doğrudan kimlik doğrulama, adanmış sunucu altyapısı, sertifika yönetimi veya gelen güvenlik duvarı kuralları gerektirmediğinden Active Directory Federasyon Hizmetleri (AD FS) göre daha düşük bir bakım seçeneğidir.

#### <a name="active-directory-federation-services-adfs"></a>Active Directory Federasyon Hizmetleri (AD FS) (ADFS)

Active Directory Federasyon Hizmetleri (AD FS), ıRAP değerlendirmesi içine eklenmiştir ve korunan ortamlarda kullanılmak üzere onaylanır.

![Federasyon](media/federated-identity.png)

Active Directory Federasyon Hizmetleri (AD FS) göz önünde bulundurmanız için çeşitli tasarım faktörleri sunar:

* Federasyon Hizmetleri, internet 'ten veya en az Microsoft 'un hizmet uç noktalarından HTTPS trafiği için ağ girişi gerektirir.
* Federasyon Hizmetleri, devam eden Yönetim ve yenileme gerektiren PKI ve sertifikaları kullanır.
* Federasyon Hizmetleri adanmış sunucularda dağıtılmalıdır ve güvenli bir şekilde erişilebilir hale getirmek için ilgili ağ altyapısına gerek duyar.

### <a name="multi-factor-authentication-mfa"></a>Multi-Factor Authentication (MFA)

Multi-Factor Authentication 'daki ıSM bölümü, risk profilinize bağlı olarak aşağıdaki senaryolarda uygulamayı önerir:

* Standart kullanıcıların kimliğini doğrulama
* Ayrıcalıklı hesapların kimliğini doğrulama
* Kullanıcıların kimliğini doğrulama uzaktan erişim
* Ayrıcalıklı eylemler yapan kullanıcılar

Azure Active Directory, hepsi veya bir kullanıcı alt kümesi (örneğin, yalnızca ayrıcalıklı hesaplar) için etkinleştirilebilen çok faktörlü kimlik doğrulaması sağlar. Microsoft ayrıca, çok faktörlü kimlik doğrulamasının uygulanma (örneğin, yalnızca kullanıcılar uzak IP adres aralıklarından oturum açtığında) üzerinde daha ayrıntılı denetim sağlayan, koşullu erişim adlı bir çözüm de sağlar.

Azure Multi-Factor Authentication, aşağıdaki ıSM kabul edilebilir doğrulama biçimlerini destekler:

* Telefon araması
* SMS iletisi
* Microsoft Authenticator uygulaması
* Desteklenen donanım belirteçleri

Azure Active Directory bir bileşeni Privileged Identity Management, kullanıcılar, dördüncü öneriyi karşılamak üzere izinlerini yükseltirse Multi-Factor Authentication kullanımını zorlamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Rol tabanlı erişim denetimleri ve Privileged Identity Management](role-privileged.md)makalesini gözden geçirin.
