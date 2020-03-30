---
title: Azure AD kimlik altyapınızı güvenli hale
titleSuffix: Azure Active Directory
description: Bu belge, yöneticilerin Azure AD özelliklerini kullanarak kuruluşlarını güvence altına almalarına yardımcı olmak için uygulamaları gereken önemli eylemlerin bir listesini özetliyor
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: security
ms.subservice: security-fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 01/29/2020
ms.author: martinco
ms.openlocfilehash: e0db8edfdfa380697a1d8d7e262a7a84da2fb7d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77565545"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>Kimlik altyapınızı güvence altına almak için beş adım

Bu belgeyi okuyorsanız, güvenliğin öneminin farkındasınızdır. Büyük olasılıkla kuruluşunuzun güvenliğini sağlama sorumluluğunu zaten taşıyorsunuz. Güvenliğin önemi ne olursa olmak üzere başkalarını ikna etmeniz gerekiyorsa, en son [Microsoft Güvenlik Zekası raporunu](https://go.microsoft.com/fwlink/p/?linkid=2073747)okumaları için gönderin.

Bu belge, kuruluşunuzu siber saldırılara karşı aşılamak için beş adımlık bir denetim listesi kullanarak Azure Active Directory'nin özelliklerini kullanarak daha güvenli bir duruş elde etmenize yardımcı olur.

Bu denetim listesi, aşağıdakileri açıklayarak kuruluşunuzu korumak için önerilen kritik eylemleri hızla dağıtmanıza yardımcı olur:

* Kimlik bilgilerinizi güçlendirin.
* Saldırı yüzey alanınızı azaltın.
* Tehdit yanıtını otomatikleştirin.
* Bulut zekasından yararlanın.
* Son kullanıcı self servis etkinleştirin.

Bu denetim listesini okurken hangi özelliklerin ve adımların tamamlandığınızı takip ettiğinizden emin olun.

> [!NOTE]
> Bu belgedeki önerilerin çoğu yalnızca kimlik sağlayıcısı olarak Azure Active Directory'yi kullanacak şekilde yapılandırılan uygulamalar için geçerlidir. Tek Oturum Açma uygulamalarının yapılandırılması, kimlik bilgisi ilkelerinin, tehdit algılamanın, denetimin, günlüğe kaydetmenin ve bu uygulamalara ekleyen diğer özelliklerin avantajlarını sağlar. [Azure Active Directory üzerinden tek oturum açma,](../../active-directory/manage-apps/configure-single-sign-on-non-gallery-applications.md) tüm bu önerilerin temel aldığı temeldir.

Bu belgedeki öneriler, Azure AD kiracınızın kimlik güvenliği yapılandırmasının otomatik bir değerlendirmesi olan [Identity Secure Score](../../active-directory/fundamentals/identity-secure-score.md)ile uyumluhale getirilir. Kuruluşlar, geçerli güvenlik yapılandırmalarında boşlukları bulmak ve microsoft'un güvenlik için en iyi [uygulamalarını](identity-management-best-practices.md) izlemelerini sağlamak için Azure REKLAM portalındaki Kimlik Güvenli Puan sayfasını kullanabilir. Güvenli Puan sayfasında ki her bir önerinin uygulanması puanınızı artırır ve ilerlemenizi izlemenize olanak sağlar ve ayrıca uygulamanızı diğer benzer boyuttaki kuruluşlarla veya sektörünüzle karşılaştırmanıza yardımcı olur.

![Kimlik Güvenli Puanı](./media/steps-secure-identity/azure-ad-sec-steps0.png)

> [!NOTE]
> Burada açıklanan özelliklerin çoğu Azure AD Premium aboneliği gerektirirken, bazıları ücretsizdir. Daha fazla bilgi için lütfen [Azure Active Directory fiyatlandırmamızı](https://azure.microsoft.com/pricing/details/active-directory/) ve [Azure REKLAM Dağıtımı denetim listemizi](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2) inceleyin.

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>Başlamadan önce: MFA ile ayrıcalıklı hesapları koruyun

Bu denetim listesine başlamadan önce, bu denetim listesini okurken gizliliğinizi ihlal etmedik. Öncelikle ayrıcalıklı hesaplarınızı korumanız gerekir.

Ayrıcalıklı hesapların denetimini alan saldırganlar büyük zarar verebilir, bu nedenle önce bu hesapları korumak çok önemlidir. [Azure REKLAM Güvenliği Varsayılanları](../../active-directory/fundamentals/concept-fundamentals-security-defaults.md) veya [Koşullu Erişim'i](../../active-directory/conditional-access/plan-conditional-access.md)kullanarak kuruluşunuzdaki tüm yöneticiler için [Azure Çok Faktörlü Kimlik Doğrulaması'nı](../../active-directory/authentication/multi-factor-authentication.md) (MFA) etkinleştirin ve gereksinim ini. MFA'yı uygulamadıysanız, şimdi yapın! Bu çok önemli.

Her şey hazır mı? Kontrol listesine başlayalım.

## <a name="step-1---strengthen-your-credentials"></a>Adım 1 - Kimlik bilgilerinizi güçlendirin

Çoğu kurumsal güvenlik ihlali, parola püskürtme, ihlal tekrarı veya kimlik avı gibi bir avuç yöntemden biriyle tehlikeye atılmış bir hesaptan kaynaklanır. Bu videoda bu saldırılar hakkında daha fazla bilgi edinin (45 dk):
> [!VIDEO https://www.youtube.com/embed/uy0j1_t5Hd4]

### <a name="make-sure-your-organization-uses-strong-authentication"></a>Kuruluşunuzun güçlü kimlik doğrulaması kullandığından emin olun

Tahmin edilen, kimlik le çalınan veya yeniden kullanılan parolaların sıklığı göz önüne alındığında, parolayı bir tür güçlü kimlik bilgisiyle yedeklemek çok önemlidir – [Azure Çok Faktörlü Kimlik Doğrulama](../../active-directory/authentication/multi-factor-authentication.md)hakkında daha fazla bilgi edinin.

Temel kimlik güvenliği düzeyini kolayca etkinleştirmek için [Azure AD Güvenlik Varsayılanları](../../active-directory/fundamentals/concept-fundamentals-security-defaults.md)ile tek tıklamayla etkinleştirme yi kullanabilirsiniz. Güvenlik varsayılanları, kiracıdaki tüm kullanıcılar için Azure MFA'sını zorlar ve oturum açmaoturumlarını eski protokoller genelinde engeller.

### <a name="start-banning-commonly-attacked-passwords-and-turn-off-traditional-complexity-and-expiration-rules"></a>Yaygın olarak saldırıya uğrayan parolaları yasaklamaya başlayın ve geleneksel karmaşıklığı ve son kullanma kurallarını kapatın.

Birçok kuruluş geleneksel karmaşıklığı (özel karakterler, sayılar, büyük harf ve küçük harf gerektiren) ve parola son kullanma kurallarını kullanır. [Microsoft'un araştırması,](https://aka.ms/passwordguidance) bu ilkelerin kullanıcıların tahmin edilmesi daha kolay parolaları seçmesine neden olduğunu göstermiştir.

Azure AD'nin [dinamik yasaklı parola](../../active-directory/authentication/concept-password-ban-bad.md) özelliği, kullanıcıların kolayca tahmin edilebilen parolalar ayarlamasını önlemek için geçerli saldırgan davranışını kullanır. Bu özellik her zaman kullanıcılar bulutta oluşturulduğunda açık, ancak [windows server active directory için Azure AD parola koruması](../../active-directory/authentication/concept-password-ban-bad-on-premises.md)dağıttıklarında karma kuruluşlar için de kullanılabilir. Azure AD parola koruması, kullanıcıların bu ortak parolaları seçmesini engeller ve belirttiğiniz özel anahtar kelimeler içeren parolayı engellemek için genişletilebilir. Örneğin, kullanıcılarınızın şirketinizin ürün adlarını veya yerel bir spor takımını içeren parolaları seçmesini engelleyebilirsiniz.

Microsoft, [NIST kılavuzuna](https://pages.nist.gov/800-63-3/sp800-63b.html)dayalı aşağıdaki modern parola ilkesini benimsemenizi önerir:

1. Parolaların en az 8 karakter olması gerekir. Kullanıcıların öngörülebilir parolaları seçmesine, parolaları dosyalara kaydetmesine veya yazmasına neden olduklarından, daha uzun süre daha uzun olması şart değildir.
2. Kullanıcıları **Spring2019** gibi kolayca tahmin edilen parolalara yönlendiren son kullanma tarihini devre dışı düşürün!
3. Karakter kompozisyonu gereksinimlerini devre dışı bırakıp, kullanıcıların parolalarda öngörülebilir karakter değiştirmeleri seçmesine neden olduğundan, kullanıcıların yaygın olarak saldırıya uğrayan parolaları seçmesini engelleyin.

Doğrudan Azure AD'de kimlik oluşturursanız, kullanıcılar için [parolaların süresinin dolmasını önlemek](../../active-directory/authentication/concept-sspr-policy.md) için PowerShell'i kullanabilirsiniz. Karma kuruluşlar bu ilkeleri [etki alanı grubu ilkesi ayarlarını](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10)) veya [Windows PowerShell'i](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy)kullanarak uygulamalıdır.

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>Sızdırılan kimlik bilgilerine karşı koruyun ve kesintilere karşı esneklik ekleyin

Kuruluşunuz geçiş kimlik doğrulaması veya federasyoniçeren karma kimlik çözümlü bir çözüm kullanıyorsa, aşağıdaki iki nedenden dolayı parola karma eşitlemesini etkinleştirmelisiniz:

* Azure AD yönetiminde [sızdırılmış kimlik bilgilerine](../../active-directory/reports-monitoring/concept-risk-events.md) sahip kullanıcılar, "karanlık web"de açığa çıkan kullanıcı adı ve parola çiftleri konusunda sizi uyarır. Daha sonra ihlal edilen üçüncü taraf sitelerinde kimlik avı, kötü amaçlı yazılım ve parola yeniden kullanımı yoluyla inanılmaz miktarda parola sızdırılır. Microsoft bu sızan kimlik bilgilerinin çoğunu bulur ve bu raporda, kuruluşunuzdaki kimlik bilgileriyle eşleşirse size söyleyecektir – ancak [yalnızca parola karma eşitlemesini](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)etkinleştiriseniz!
* Şirket içi bir kesinti durumunda (örneğin, bir ransomware saldırısında) parola karma [eşitlemeyi kullanarak bulut kimlik doğrulamasını kullanmaya](choose-ad-authn.md)geçebilirsiniz. Bu yedekleme kimlik doğrulama yöntemi, Office 365 de dahil olmak üzere Azure Active Directory ile kimlik doğrulaması için yapılandırılan uygulamalara erişmeye devam etmenizi sağlar. Bu durumda, BT personelinin şirket içi kesinti çözülene kadar verileri paylaşmak için kişisel e-posta hesaplarına başvurması gerekmez.

Parola karma [eşitlemenin](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md) nasıl çalıştığı hakkında daha fazla bilgi edinin.

> [!NOTE]
> Parola karma eşitlemesini etkinleştiriyseniz ve Azure AD Etki Alanı hizmetlerini kullanıyorsanız, Kerberos (AES 256) karmaları ve isteğe bağlı olarak NTLM (RC4, tuz yok) karmaları da şifrelenir ve Azure AD ile senkronize edilir.

### <a name="implement-ad-fs-extranet-smart-lockout"></a>AD FS extranet akıllı kilitleme uygulayın

Uygulamaları doğrudan Azure AD'ye doğrulayabilmek için yapılandırılan kuruluşlar [Azure AD akıllı kilitlemeden](../../active-directory/authentication/concept-sspr-howitworks.md)yararlanır. Windows Server 2012R2'de AD FS kullanıyorsanız, AD FS [extranet kilitleme koruması](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection)uygulayın. Windows Server 2016'da AD FS kullanıyorsanız, [extranet akıllı kilitleme](https://support.microsoft.com/help/4096478/extranet-smart-lockout-feature-in-windows-server-2016)uygulayın. AD FS Smart Extranet lokavt, kullanıcıların Active Directory'de kilitlenmelerini önlerken AD FS'yi hedef alan kaba kuvvet saldırılarına karşı koruma sağlar.

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>Özünde güvenli, kullanımı daha kolay kimlik bilgileri avantajlarından yararlanın

[Windows Hello'yu](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)kullanarak, parolaları bilgisayarlarda ve mobil cihazlarda güçlü iki faktörlü kimlik doğrulamayla değiştirebilirsiniz. Bu kimlik doğrulama, bir aygıta güvenli bir şekilde bağlanan ve biyometrik veya PIN kullanan yeni bir kullanıcı kimlik bilgisi türünden oluşur.

## <a name="step-2---reduce-your-attack-surface"></a>Adım 2 - Saldırı yüzeyinizi azaltın

Parola gizliliğinin yaygınlığı göz önüne alındığında, kuruluşunuzdaki saldırı yüzeyini en aza indirmek çok önemlidir. Eski, daha az güvenli protokollerin kullanımını ortadan kaldırmak, erişim giriş noktalarını sınırlamak ve kaynaklara idari erişim üzerinde daha önemli denetim uygulamak saldırı yüzey alanını azaltmaya yardımcı olabilir.

### <a name="block-legacy-authentication"></a>Eski kimlik doğrulamasını engelleme

Azure AD ile kimlik doğrulaması yapmak ve şirket verilerine erişmek için kendi eski yöntemlerini kullanan uygulamalar, kuruluşlar için başka bir risk oluşturur. Eski kimlik doğrulaması kullanan uygulamalara örnek olarak POP3, IMAP4 veya SMTP istemcileri verilebilir. Eski kimlik doğrulama uygulamaları kullanıcı adına kimlik doğrulaması yapar ve Azure AD'nin gelişmiş güvenlik değerlendirmeleri yapmasını engeller. Alternatif, modern kimlik doğrulama, çok faktörlü kimlik doğrulama ve Koşullu Erişim destekler, çünkü güvenlik riskini azaltacaktır. Aşağıdaki üç eylemi öneririz:

1. [AD FS kullanıyorsanız eski kimlik doğrulamasını engelleyin.](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)
2. [Kurulum SharePoint Online ve Exchange Online modern kimlik doğrulaması kullanmak için](../../active-directory/conditional-access/conditional-access-for-exo-and-spo.md).
3. Azure AD Premium'unuz varsa, eski kimlik doğrulamasını engellemek için [Koşullu Erişim ilkelerini](../../active-directory/conditional-access/overview.md) kullanın, aksi takdirde [Azure REKLAM Güvenliği Varsayılanlarını](../../active-directory/fundamentals/concept-fundamentals-security-defaults.md)kullanın.

### <a name="block-invalid-authentication-entry-points"></a>Geçersiz kimlik doğrulama giriş noktalarını engelleme

İhlal zihniyetini kullanarak, tehlikeye atıldığında kullanıcı kimlik bilgilerinin etkisini azaltmalısınız. Ortamınızdaki her uygulama için geçerli kullanım durumlarını göz önünde bulundurun: hangi gruplar, hangi ağlar, hangi cihazlar ve diğer öğeler yetkilidir – sonra geri kalanını engelleyin. [Azure AD Koşullu Erişim](../../active-directory/conditional-access/overview.md)ile, tanımladığınız belirli koşullara bağlı olarak yetkili kullanıcıların uygulamalarına ve kaynaklarına nasıl erişebileceğini denetleyebilirsiniz.

### <a name="restrict-user-consent-operations"></a>Kullanıcı onayı işlemlerini kısıtlama

Çeşitli Azure AD uygulama [onam deneyimleri,](https://docs.microsoft.com/azure/active-directory/develop/application-consent-experience) [izin ve onay türleri](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent)ve bunların kuruluşunuzun güvenlik duruşu üzerindeki etkilerini anlamak önemlidir. Varsayılan olarak, Azure AD'deki tüm kullanıcılar kuruluşunuzun verilerine erişmek için Microsoft kimlik platformundan yararlanan uygulamalar verebilir. Kullanıcıların kendi rızalarına izin vermek, kullanıcıların Microsoft 365, Azure ve diğer hizmetlerle tümletkikolan yararlı uygulamaları kolayca edinmelerine olanak sağlarken, kullanılmadığı ve dikkatle izlenmediği takdirde risk teşkil edebilir.

Microsoft, yüzey alanınızı azaltmaya ve bu riski azaltmaya yardımcı olmak için [gelecekteki kullanıcı onayı işlemlerini devre dışı](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-removing-user-access#i-want-to-disable-all-future-user-consent-operations-to-any-application) bırakmanızı önerir. Son kullanıcı onayı devre dışı bırakılırsa, önceki izin hibeleri yine de onurlandırılır, ancak gelecekteki tüm onay işlemleri bir yönetici tarafından gerçekleştirilmelidir. Yönetici onayı, kullanıcılar tarafından entegre bir [yönetici onayı isteği iş akışı](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow) veya kendi destek süreçleriniz aracılığıyla talep edilebilir. Son kullanıcı onayını devre dışı bırakmadan önce, kuruluşunuzdaki bu değişikliği planlamak için [önerilerimizi](https://docs.microsoft.com/azure/active-directory/manage-apps/manage-consent-requests) kullanın. Tüm kullanıcıların erişmesine izin vermek istediğiniz uygulamalariçin, [tüm kullanıcılar adına onay vermeyi](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent)düşünün , henüz bireysel olarak izin vermemiş kullanıcıların uygulamaya erişebilmesini sağlamak. Bu uygulamaların tüm senaryolarda tüm kullanıcıların kullanımına sunulmasını istemiyorsanız, kullanıcının uygulamalara erişimini kısıtlamak için [uygulama atamasını](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups) ve [koşullu erişimi](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) kullanın.

Kullanıcıların, kullanıcı sürtünmesini azaltmak, destek hacmini en aza indirmek ve kullanıcıların Azure REKLAM kimlik bilgilerini kullanarak uygulamalara kaydolmasını önlemek için yeni uygulamalar için yönetici onayı isteyebilmesini unutmayın. Onay işlemlerinizi düzenledikten sonra, yöneticilerin uygulamayı ve izinleri düzenli olarak denetlemesi gerekir.


### <a name="implement-azure-ad-privileged-identity-management"></a>Azure AD Ayrıcalıklı Kimlik Yönetimi'ni uygulayın

"İhlal" kabul etmesinin bir diğer etkisi de, gizliliği ihlal edilen bir hesabın ayrıcalıklı bir rol ile çalışma olasılığını en aza indirme gereksinimidir.

[Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM),](../../active-directory/privileged-identity-management/pim-configure.md) size yardımcı olarak hesap ayrıcalıklarını en aza indirmenize yardımcı olur:

* Yönetim rollerine atanan kullanıcıları tanımlayın ve yönetin.
* Kaldırmanız gereken kullanılmayan veya aşırı ayrıcalık rollerini anlayın.
* İmtiyazlı rollerin çok faktörlü kimlik doğrulamayla korunduğundan emin olmak için kurallar belirleyin.
* Ayrıcalıklı rollerin yalnızca ayrıcalıklı görevi yerine getirecek kadar uzun süre verildiğinden emin olmak için kurallar belirleyin.

Azure AD PIM'yi etkinleştirin, ardından yönetim rolleri atanan kullanıcıları görüntüleyin ve bu rollerdeki gereksiz hesapları kaldırın. Kalan ayrıcalıklı kullanıcılar için, bunları kalıcıdan uygun alabilene taşıyın. Son olarak, bu ayrıcalıklı rollere erişmeleri gerektiğinden emin olmak için uygun ilkeler oluşturmak, bunu gerekli değişiklik denetimiyle güvenli bir şekilde yapabileceklerinden emin olun.

Ayrıcalıklı hesap işleminizi dağıtmanın bir parçası olarak, kendinizi kilitlerseniz Azure AD'ye erişiminiz olduğundan emin olmak [için en az iki acil durum hesabı oluşturmak için en iyi uygulamayı](../../active-directory/users-groups-roles/directory-admin-roles-secure.md) uygulayın.

## <a name="step-3---automate-threat-response"></a>Adım 3 - Tehdit yanıtını otomatikleştirin

Azure Active Directory, algılama ve yanıt arasındaki gecikmeyi kaldırmak için saldırıları otomatik olarak engelleyen birçok yetenek sahiptir. Suçluların kendilerini çevrenize yerleştirmek için kullandıkları süreyi azalttığınızda maliyetleri ve riskleri azaltabilirsiniz. İşte atabileceğiniz somut adımlar.

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>Azure AD Kimlik Koruması'nı kullanarak kullanıcı riski güvenliği ilkesini uygulayın

Kullanıcı riski, kullanıcının kimliğinin tehlikeye girme olasılığını gösterir ve kullanıcının kimliğiyle ilişkili [kullanıcı risk algılamalarına](../../active-directory/identity-protection/overview.md) göre hesaplanır. Kullanıcı risk ilkesi, risk düzeyini belirli bir kullanıcı veya gruba değerlendiren koşullu erişim ilkesidir. Düşük, Orta, Yüksek risk düzeyine bağlı olarak, bir ilke erişimi engellemek veya çok faktörlü kimlik doğrulaması kullanarak güvenli bir parola değişikliği gerektirecek şekilde yapılandırılabilir. Microsoft'un önerisi, yüksek riskli kullanıcılar için güvenli bir parola değişikliği gerektirmesidir.

![Riskli oldukları belirlenen kullanıcılar](./media/steps-secure-identity/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>Azure AD Kimlik Koruması'nı kullanarak oturum açma risk ilkesini uygulayın

Oturum açma riski, hesap sahibi nden başka birinin kimliği kullanarak oturum açmaya çalışması olasılığıdır. [Oturum açma risk ilkesi,](../../active-directory/identity-protection/overview.md) risk düzeyini belirli bir kullanıcı veya gruba değerlendiren koşullu erişim ilkesidir. Risk düzeyine (yüksek/orta/düşük) bağlı olarak, bir ilke erişimi engellemek veya çok faktörlü kimlik doğrulamayı zorlamak için yapılandırılabilir. Orta veya daha yüksek risk oturum açma larda çok faktörlü kimlik doğrulamayı zorladığınızdan emin olun.

![Anonim IP'lerden oturum açma](./media/steps-secure-identity/azure-ad-sec-steps2.png)

## <a name="step-4---utilize-cloud-intelligence"></a>Adım 4 - Bulut zekasından yararlanın

Güvenlikle ilgili olayların ve ilgili uyarıların denetlenmeve günlüğe kaydedilmesini, verimli bir koruma stratejisinin temel bileşenleridir. Güvenlik günlükleri ve raporları, şüpheli etkinliklerin elektronik bir kaydını sağlar ve ağın ve dahili saldırıların girişimini veya başarılı bir şekilde dış ağını gösteren desenleri algılamanıza yardımcı olur. Denetimi, kullanıcı etkinliğini izlemek, mevzuata uygunluğu belgelemek, adli analiz yapmak ve daha fazlasını yapmak için kullanabilirsiniz. Uyarılar güvenlik olayları bildirimleri sağlar.

### <a name="monitor-azure-ad"></a>Azure REKLAM'ı izleyin

Microsoft Azure hizmetleri ve özellikleri, güvenlik ilkelerive mekanizmalarınızdaki boşlukları belirlemenize ve ihlalleri önlemeye yardımcı olmak için bu boşlukları gidermenize yardımcı olmak için yapılandırılabilir güvenlik denetimi ve günlüğe kaydetme seçenekleri sunar. [Azure Günlük Ve Denetim'i](log-audit.md) kullanabilir ve Azure Etkin [Dizin portalında Denetim etkinlik raporlarını](../../active-directory/reports-monitoring/concept-audit-logs.md)kullanabilirsiniz.

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>Karma ortamlarda Azure AD Connect Health'i izleyin

[AD FS'yi Azure AD Connect Health ile izlemek,](../../active-directory/hybrid/how-to-connect-health-adfs.md) olası sorunlar ve AD FS altyapınıza yönelik saldırıların görünürlüğü hakkında daha fazla bilgi sağlar. Azure AD Connect Health ayrıntılar, çözüm adımları ve ilgili belgelere bağlantılar içeren uyarılar sunar; kimlik doğrulama trafiğiyle ilgili çeşitli ölçümler için kullanım analitiği; performans izleme ve raporlar.

![Azure AD Connect Health](./media/steps-secure-identity/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>Azure AD Kimlik Koruması olaylarını izleme

[Azure AD Kimlik Koruması,](../../active-directory/identity-protection/overview.md) kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılamak için kullanabileceğiniz bir bildirim, izleme ve raporlama aracıdır. Sızdırılan kimlik bilgileri, imkansız seyahat ve virüslü cihazlardan oturum açma, anonim IP adresleri, şüpheli etkinlikle ilişkili IP adresleri ve bilinmeyen konumlar gibi risk algılamalarını algılar. Risk altındaki kullanıcıların e-postalarını ve/veya haftalık özet e-postalarını almak için bildirim uyarılarını etkinleştirin.

Azure AD Kimlik Koruması, her gün izlemeniz gereken iki önemli rapor sağlar:
1. Riskli oturum açma raporları, kullanıcı oturum açma etkinliklerini araştırır, yasal sahibi oturum açma işlemi gerçekleştirmemiş olabilir.
2. Riskli kullanıcı raporları, algılanan veya kullanıcının farklı konumlardan oturum açması gibi tehlikeye atılmış olabilecek kullanıcı hesaplarını yüzeye çıkararak imkansız bir seyahat olayına neden olur.

![Riskli oldukları belirlenen kullanıcılar](./media/steps-secure-identity/azure-ad-sec-steps3.png)

### <a name="audit-apps-and-consented-permissions"></a>Denetim uygulamaları ve onaylanan izinler

Kullanıcılar, profil bilgilerine ve e-postaları gibi kullanıcı verilerine erişecek, gizliliği ihlal edilmiş bir web sitesinde veya uygulamalarında gezinmeleri için kandırılabilir. Kötü niyetli bir aktör, posta kutusu içeriğini şifrelemek ve posta kutusu verilerinizi yeniden kazanmak için fidye istemek için aldığı izinleri kullanabilir. Yöneticiler, kullanıcılar tarafından verilen izinleri [gözden geçirmeli ve denetlemeli](https://docs.microsoft.com/office365/securitycompliance/detect-and-remediate-illicit-consent-grants) veya kullanıcıların varsayılan olarak onay verme yeteneğini devre dışı bırakmalıdır.

Kullanıcılar tarafından verilen izinleri denetlemenin yanı sıra, [riskli veya istenmeyen OAuth uygulamalarını](https://docs.microsoft.com/cloud-app-security/investigate-risky-oauth) premium ortamlarda bulabilirsiniz.

## <a name="step-5---enable-end-user-self-service"></a>Adım 5 - Son kullanıcı self servisini etkinleştirme

Mümkün olduğunca güvenliği verimlilikle dengelemek isteyeceksiniz. Uzun vadede güvenliğin temelini kurduğunuz zihniyetle yolculuğunuza yaklaşırken, tetikte kalarak kullanıcılarınızı güçlendirerek kuruluşunuzdaki sürtünmeyi kaldırabilirsiniz.

### <a name="implement-self-service-password-reset"></a>Self servis parola sıfırlama uygulama

Azure AD'nin [self servis parola sıfırlama (SSPR),](../../active-directory/authentication/quickstart-sspr.md) BT yöneticilerinin kullanıcıların yardım masası veya yönetici müdahalesi olmadan parolalarını veya hesaplarını sıfırlamalarına veya hesaplarını sıfırlamalarına veya kilidini açmalarına olanak sağlaması için basit bir araç sunar. Sistem, kullanıcıların parolalarını sıvazladığında sizi kötüye kullanma veya kötüye kullanım konusunda uyaran bildirimlerle birlikte izleyen ayrıntılı raporlamaiçerir.

### <a name="implement-self-service-group-and-application-access"></a>Self servis grubu ve uygulama erişimi uygulama

Azure AD, güvenlik gruplarını, Office 365 gruplarını, uygulama rollerini ve erişim paketi kataloglarını kullanarak yönetici olmayanların kaynaklara erişimi yönetme olanağı sağlar.  [Self servis grup yönetimi,](../../active-directory/users-groups-roles/groups-self-service-management.md) grup sahiplerinin yönetimgörevi atanmasına gerek kalmadan kendi gruplarını yönetmelerini sağlar. Kullanıcılar ayrıca, yöneticilerin isteklerini işlemelerine güvenmeden Office 365 gruplarını oluşturabilir ve yönetebilir ve kullanılmayan grupların süresi otomatik olarak dolabilir.  [Azure AD yetkilendirme yönetimi,](../../active-directory/governance/entitlement-management-overview.md) kapsamlı erişim isteği iş akışları ve otomatik son kullanma süresi yle birlikte delegasyon ve görünürlük sağlar.  Yönetici olmayanlara, sahip oldukları gruplar, Takımlar, uygulamalar ve SharePoint Online siteleri için kendi erişim paketlerini yapılandırma olanağını, çalışanların erişimini yapılandırmak da dahil olmak üzere erişimi onaylaması gereken kişiler için özel ilkelerle ayin edebilirsiniz. onaylayan olarak yöneticiler ve iş ortağı sponsorları.

### <a name="implement-azure-ad-access-reviews"></a>Azure AD erişim incelemelerini uygulama

[Azure AD erişim incelemeleri](../../active-directory/governance/access-reviews-overview.md)ile, bir güvenlik standardı koruduğunuzdan emin olmak için erişim paketi ve grup üyeliklerini, kurumsal uygulamalara erişimi ve ayrıcalıklı rol atamalarıyönetebilirsiniz.  Kullanıcıların kendileri, kaynak sahipleri ve diğer gözden geçirenler tarafından düzenli olarak denetlenme, kullanıcıların artık ihtiyaç duymadıkları sürelerde erişimi korumamalarını sağlar.

## <a name="summary"></a>Özet

Güvenli bir Kimlik altyapısının birçok yönü vardır, ancak bu beş adımlı denetim listesi daha güvenli ve emniyetli bir kimlik altyapısını hızla gerçekleştirmenize yardımcı olur:

* Kimlik bilgilerinizi güçlendirin.
* Saldırı yüzey alanınızı azaltın.
* Tehdit yanıtını otomatikleştirin.
* Bulut zekasından yararlanın.
* Kendi kendine yardım la daha öngörülebilir ve tam son kullanıcı güvenliğini etkinleştirin.

Kimlik Güvenliğini ne kadar ciddiye aldığınızı takdir ediyoruz ve bu belgenin kuruluşunuz için daha güvenli bir duruş için yararlı bir yol haritası olmasını umuyoruz.

## <a name="next-steps"></a>Sonraki adımlar

Önerileri planlamak ve dağıtmak için yardıma ihtiyacınız varsa, yardım için [Azure AD proje dağıtım planlarına](https://aka.ms/deploymentplans) bakın.

Tüm bu adımların tamamlandığından eminseniz, microsoft'un [Kimlik Güvenli Puanı'nı](../../active-directory/fundamentals/identity-secure-score.md)kullanın ve bu da sizi en son en iyi uygulamalar ve güvenlik [tehditleriyle](identity-management-best-practices.md) güncel tutar.
