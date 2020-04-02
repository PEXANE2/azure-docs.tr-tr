---
title: Azure kimliği & access security en iyi uygulamaları | Microsoft Dokümanlar
description: Bu makalede, Azure özelliklerinde yerleşik kullanarak kimlik yönetimi ve erişim denetimi için en iyi uygulamalar kümesi sağlanmaktadır.
services: security
documentationcenter: na
author: terrylanfear
manager: RKarlin
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2019
ms.author: terrylan
ms.openlocfilehash: ffd9919092cdf2481767e58f10ba6525d56ca4a8
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548450"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure Kimlik Yönetimi ve erişim denetimi güvenliği en iyi uygulamaları

Bu makalede, Azure kimlik yönetimi ve erişim denetimi güvenliği en iyi uygulamaları koleksiyonunu tartışıyoruz. Bu en iyi uygulamalar, [Azure AD](../../active-directory/fundamentals/active-directory-whatis.md) ile olan deneyimlerimizden ve sizin gibi müşterilerin deneyimlerinden elde edilir.

Her en iyi uygulama için, açıklarız:

* En iyi uygulama nedir
* Neden en iyi uygulamayı etkinleştirmek istiyorsunuz?
* En iyi uygulamayı etkinleştiremezseniz sonuç ne olabilir?
* En iyi uygulamaya olası alternatifler
* En iyi uygulamayı etkinleştirmeyi nasıl öğrenebilirsiniz?

Bu Azure kimlik yönetimi ve erişim denetimi güvenliği en iyi uygulamaları makalesi, bu makalenin yazıldığı sırada var oldukları gibi, bir konsensüs görüşüne ve Azure platform özelliklerine ve özellik kümelerine dayanır.

Bu makalenin yazımAmacı, bazı temel özelliklerimiz ve hizmetlerimizde size yol gösteren "[kimlik altyapınızı güvence altına almak için 5 adım](steps-secure-identity.md)" kontrol listemiz tarafından yönlendirilen dağıtım dan sonra daha sağlam bir güvenlik duruşu için genel bir yol haritası sunmaktır.

Görüşler ve teknolojiler zaman içinde değişir ve bu makale, bu değişiklikleri yansıtacak şekilde düzenli olarak güncellenir.

Azure kimlik yönetimi ve erişim denetimi güvenliği en iyi uygulamaları bu makalede tartışılan şunlardır:

* Kimliği birincil güvenlik alanı olarak ele alın
* Kimlik yönetimini merkezileştirme
* Bağlı kiracıları yönetme
* Tek oturum açmayı etkinleştirme
* Koşullu Erişimi Aç
* Rutin güvenlik iyileştirmeleri için plan
* Parola yönetimini etkinleştirme
* Kullanıcılar için çok faktörlü doğrulamayı uygulayın
* Rol tabanlı erişim denetimi kullanma
* İmtiyazlı hesapların daha düşük pozlama
* Kaynakların bulunduğu konumları denetleme
* Depolama kimlik doğrulaması için Azure AD'yi kullanma

## <a name="treat-identity-as-the-primary-security-perimeter"></a>Kimliği birincil güvenlik alanı olarak ele alın

Pek çok kişi kimliğin güvenliğin birincil alanı olduğunu düşünüyor. Bu ağ güvenliği geleneksel odak bir kaymadır. Ağ çevreleri daha gözenekli olmaya devam ediyor ve bu çevre savunması [BYOD](https://aka.ms/byodcg) cihazlarının ve bulut uygulamalarının patlamasından önceki kadar etkili olamaz.

[Azure Active Directory (Azure AD),](../../active-directory/fundamentals/active-directory-whatis.md) kimlik ve erişim yönetimi için Azure çözümüdür. Azure AD, Microsoft'un çok kiracılı, bulut tabanlı bir dizin ve kimlik yönetimi hizmetidir. Temel dizin hizmetlerini, uygulama erişimi yönetimini ve kimlik korumasını tek bir çözümde toplar.

Aşağıdaki bölümlerde Azure AD kullanarak kimlik ve erişim güvenliği için en iyi uygulamaları listelediğiniz de listelenerilmiştir.

**En iyi uygulama**: Kullanıcı ve hizmet kimlikleri etrafında güvenlik denetimleri ve algılamaları merkezi.
**Ayrıntı**: Denetimleri ve kimlikleri harmanlamak için Azure AD'yi kullanın.

## <a name="centralize-identity-management"></a>Kimlik yönetimini merkezileştirme

Karma [kimlik](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?) senaryosunda şirket içi ve bulut dizinlerinizi entegre edersiniz. Tümleştirme, BT ekibinizin, hesabın nerede oluşturulduğuna bakılmaksızın hesapları tek bir konumdan yönetmesine olanak tanır. Tümleştirme, hem bulut hem de şirket içi kaynaklara erişmek için ortak bir kimlik sağlayarak kullanıcılarınızın daha üretken hale çıkmasına da yardımcı olur.

**En iyi yöntem**: Tek bir Azure AD örneği oluşturma. Tutarlılık ve tek bir yetkili kaynaklar netliği artıracak ve insan hataları ve yapılandırma karmaşıklığı güvenlik risklerini azaltacaktır.
**Ayrıntı**: Kurumsal ve kurumsal hesaplar için yetkili kaynak olarak tek bir Azure REKLAM dizini belirleyin.

**En iyi uygulama**: Şirket içi dizinlerinizi Azure AD ile tümleştirin.  
**Ayrıntı**: Şirket içi dizininizi bulut dizininizle senkronize etmek için [Azure AD Connect'i](/azure/active-directory/connect/active-directory-aadconnect) kullanın.

> [!Note]
> Azure [AD Connect'in performansını etkileyen etkenler](../../active-directory/hybrid/plan-connect-performance-factors.md)vardır. Azure AD Connect'in düşük performans gösteren sistemlerin güvenliği ve üretkenliği engellemesini engelleyecek kapasiteye sahip olduğundan emin olun. Büyük veya karmaşık kuruluşlar (100.000'den fazla nesne sağlayan kuruluşlar) Azure AD Connect uygulamalarını optimize etmek için [önerileri](../../active-directory/hybrid/whatis-hybrid-identity.md) izlemelidir.

**En iyi uygulama**: Varolan Active Directory örneğinde yüksek ayrıcalıklara sahip hesapları Azure AD ile senkronize etmeyin.
**Ayrıntı**: Bu hesapları filtreleyen varsayılan [Azure AD Connect yapılandırmasını](../../active-directory/hybrid/how-to-connect-sync-configure-filtering.md) değiştirmeyin. Bu yapılandırma, düşmanların buluttan şirket içi varlıklara dönüşme riskini azaltır (bu da büyük bir olaya neden olabilir).

**En iyi yöntem**: Parola karma senkronizasyonunu açın.  
**Ayrıntı**: Parola karma eşitleme, kullanıcı parola karmalarını şirket içi Active Directory örneğinden bulut tabanlı Azure AD örneğine eşitlemek için kullanılan bir özelliktir. Bu eşitleme, önceki saldırılara karşı yeniden oynatılmaya karşı korumanıza yardımcı olur.

Aktif Dizin Federasyonu Hizmetleri (AD FS) veya diğer kimlik sağlayıcılarıyla federasyon kullanmaya karar verseniz bile, şirket içi sunucularınızın başarısız olması veya geçici olarak kullanılamaması durumunda isteğe bağlı olarak parola karma eşitlemesi ayarlayabilirsiniz. Bu eşitleme, kullanıcıların şirket içi Active Directory örneğinde oturum açmaları için kullandıkları parolayı kullanarak hizmette oturum açmalarını sağlar. Ayrıca, bir kullanıcı Azure AD'ye bağlı olmayan diğer hizmetlerde aynı e-posta adresini ve parolayı kullanmışsa, senkronize parola kalıplarını tehlikeye atıldığı bilinen parolalarla karşılaştırarak kimlik bilgilerini algılamasına da olanak tanır.

Daha fazla bilgi için bkz: [Azure AD Connect eşitlemesi ile parola karma senkronizasyonu uygula.](/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)

**En iyi uygulama**: Yeni uygulama geliştirme için kimlik doğrulama için Azure AD'yi kullanın.
**Ayrıntı**: Kimlik doğrulamayı desteklemek için doğru yetenekleri kullanın:

  - Çalışanlar için Azure REKLAM
  - Konuk kullanıcılar ve dış iş ortakları için [Azure AD B2B](../../active-directory/b2b/index.yml)
  - [Azure AD B2C,](../../active-directory-b2c/index.yml) müşterilerin uygulamalarınızı kullanırken profillerini nasıl kaydolduklarını, kaydolduklarını ve yönettiğini denetlemek için

Şirket içi kimliklerini bulut kimlikleriyle bütünleştirmeyen kuruluşlar, hesapları yönetmede daha fazla ek yükü olabilir. Bu genel merkez, hata ve güvenlik ihlalleri olasılığını artırır.

> [!Note]
> Kritik hesapların hangi dizinlerde bulunacağını ve kullanılan yönetici iş istasyonunun yeni bulut hizmetleri veya varolan işlemler tarafından yönetilip yönetilmeyeceğini seçmeniz gerekir. Varolan yönetim ve kimlik sağlama süreçlerini kullanmak bazı riskleri azaltabilir, ancak bir saldırganın şirket içi bir hesabı tehlikeye atması ve buluta yönelme riski oluşturabilir. Farklı roller için farklı bir strateji kullanmak isteyebilirsiniz (örneğin, BT yöneticileri ve iş birimi yöneticileri). İki seçenek sunulur. İlk seçenek, şirket içi Etkin Dizin örneğinizle eşitlenmemiş Azure AD Hesapları oluşturmaktır. Microsoft Intune'u kullanarak yönetebileceğiniz ve düzeltebileceğiniz Azure AD'ye yönetici iş istasyonunuzun katılın. İkinci seçenek, şirket içi Active Directory örneğinizle senkronize ederek varolan yönetici hesaplarını kullanmaktır. Yönetim ve güvenlik için Active Directory etki alanınızdaki varolan iş istasyonlarını kullanın.

## <a name="manage-connected-tenants"></a>Bağlı kiracıları yönetme
Güvenlik kuruluşunuz, riski değerlendirmek ve kuruluşunuzun ilkelerinin ve herhangi bir düzenleyici gereksinimin izlenip izlenmediğini belirlemek için görünürlüğe ihtiyaç duyar. Güvenlik kuruluşunuzun üretim ortamınıza ve ağınıza bağlı tüm aboneliklerde [(Azure ExpressRoute](../../expressroute/expressroute-introduction.md) veya [siteden siteye VPN](../../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)üzerinden) görünürlüğü olduğundan emin olmalısınız. Azure AD'deki [bir Global Administrator/Şirket Yöneticisi,](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator-permissions) [Kullanıcı Erişim Yöneticisi](../../role-based-access-control/built-in-roles.md#user-access-administrator) rolüne erişimlerini yükseltebilir ve ortamınıza bağlı tüm abonelikleri ve yönetilen grupları görebilir.

Sizin ve güvenlik grubunuzun ortamınıza bağlı tüm abonelikleri veya yönetim gruplarını görüntülediğinden emin olmak [için tüm Azure aboneliklerini ve yönetim gruplarını yönetmeye erişimi artırın.](../../role-based-access-control/elevate-access-global-admin.md) Riskleri değerlendirdikten sonra bu yükseltilmiş erişimi kaldırmalısınız.

## <a name="enable-single-sign-on"></a>Tek oturum açmayı etkinleştirme

Mobil ilk, bulut ilk dünyada, kullanıcılarınızın her yerde ve her zaman üretken olabilmesi için cihazlara, uygulamalara ve hizmetlere her yerden tek oturum açma (SSO) etkinleştirmek istersiniz. Yönetecek birden çok kimlik çözümlü olduğunda, bu yalnızca BT için değil, aynı zamanda birden çok parolayı hatırlamak zorunda olan kullanıcılar için de yönetimsorunu haline gelir.

Tüm uygulamalarınız ve kaynaklarınız için aynı kimlik çözümlerini kullanarak SSO'ya ulaşabilirsiniz. Ayrıca, kullanıcılarınız oturum açıp ihtiyaç duydukları kaynaklara erişmek için, ister şirket içinde ister bulutta bulunsa da aynı kimlik bilgilerini kullanabilir.

**En iyi uygulama**: SSO'ya olanak sağlar.  
**Ayrıntı**: Azure [AD, şirket içi Active Directory'yi](/azure/active-directory/connect/active-directory-aadconnect) buluta genişletir. Kullanıcılar, birincil çalışmalarını veya okul hesaplarını etki alanına katılan aygıtları, şirket kaynakları ve işlerini halletmek için ihtiyaç duydukları tüm web ve SaaS uygulamaları için kullanabilirler. Kullanıcıların birden çok kullanıcı adı ve parola kümesini hatırlamak zorunda kalmaması gerekir ve uygulama erişimleri, kuruluş grubu üyeliklerine ve çalışan olarak durumlarına göre otomatik olarak sağlanabilir (veya hükmün silinebilir). Galeri uygulamaları veya geliştirip [Azure AD Application Proxy](/azure/active-directory/active-directory-application-proxy-get-started) ile yayımladığınız şirket içi uygulamalar için bu erişimi siz denetleyebilirsiniz.

Kullanıcıların Azure AD'deki iş veya okul hesaplarını temel alarak [SaaS uygulamalarına](/azure/active-directory/active-directory-appssoaccess-whatis) erişmelerini sağlamak için SSO'yu kullanın. Bu, yalnızca Microsoft SaaS uygulamaları için değil, [Google Apps](/azure/active-directory/active-directory-saas-google-apps-tutorial) ve [Salesforce](/azure/active-directory/active-directory-saas-salesforce-tutorial)gibi diğer uygulamalar için de geçerlidir. Uygulamanızı Azure AD'yi [SAML tabanlı](/azure/active-directory/fundamentals-identity) kimlik sağlayıcısı olarak kullanacak şekilde yapılandırabilirsiniz. Güvenlik denetimi olarak Azure AD, kullanıcıların Azure AD üzerinden erişim izni verilmedikçe uygulamada oturum açmalarına izin veren bir belirteç düzenlemez. Doğrudan veya kullanıcıların üyesi olduğu bir grup aracılığıyla erişim izni verebilirsiniz.

Kullanıcıları ve uygulamaları için SSO oluşturmak için ortak bir kimlik oluşturmayan kuruluşlar, kullanıcıların birden çok parolaya sahip olduğu senaryolara daha fazla maruz kalır. Bu senaryolar, kullanıcıların parolaları yeniden kullanma veya zayıf parolalar kullanma olasılığını artırır.

## <a name="turn-on-conditional-access"></a>Koşullu Erişimi Aç

Kullanıcılar, her yerden çeşitli aygıtlar ve uygulamalar kullanarak kuruluşunuzun kaynaklarına erişebilir. BT yöneticisi olarak, bu aygıtların güvenlik ve uyumluluk standartlarınıza uyduğunu görmek istersiniz. Bir kaynağa kimlerin erişebileceğine odaklanmak artık yeterli değil.

Güvenliği ve üretkenliği dengelemek için, erişim denetimi hakkında karar vermeden önce kaynağa nasıl erişilebildiğini düşünmeniz gerekir. Azure AD Koşullu Erişim ile bu gereksinimi giderebilirsiniz. Koşullu Erişim ile, bulut uygulamalarınıza erişim koşullarına bağlı olarak otomatik erişim denetimi kararları alabilirsiniz.

**En iyi uygulama**: Kurumsal kaynaklara erişimi yönetmek ve kontrol etmek.  
**Ayrıntı**: SaaS uygulamaları ve Azure AD'ye bağlı uygulamalar için bir grup, konum ve uygulama hassasiyetine göre Azure AD [Koşullu Erişimi](/azure/active-directory/active-directory-conditional-access-azure-portal) yapılandırın.

**En iyi uygulama**: Eski kimlik doğrulama protokollerini engelleyin.
**Ayrıntı**: Saldırganlar, özellikle parola püskürtme saldırılarında eski protokollerde her gün zayıf lıklardan yararlanır. Koşullu Erişimi eski protokolleri engellemek için yapılandırın. Daha fazla bilgi için [Azure AD: Yap ve Yapma t'leri](https://www.youtube.com/watch?v=wGk0J4z90GI) videosuna bakın.

## <a name="plan-for-routine-security-improvements"></a>Rutin güvenlik iyileştirmeleri için plan

Güvenlik her zaman gelişmektedir ve bulut ve kimlik yönetimi çerçevenize düzenli olarak büyümeyi göstermenin ve çevrenizi güvence altına almanın yeni yollarını keşfetmenin bir yolunu oluşturmak önemlidir.

Identity Secure Score, Microsoft'un size güvenlik duruşunuzu objektif olarak ölçmek ve gelecekteki güvenlik geliştirmelerini planlamaya yardımcı olmak için sayısal bir puan sağlamak için çalışan yayımlayan önerilen güvenlik denetimleri kümesidir. Puanınızı diğer sektörlerdekilerle ve zaman içinde kendi eğilimlerinize göre de görüntüleyebilirsiniz.

**En iyi uygulama:** Rutin güvenlik incelemelerini ve endüstrinizdeki en iyi uygulamalara dayalı iyileştirmeler planlayın.
**Ayrıntı**: Zaman içinde geliştirmelerinizi sıralamak için Kimlik Güvenli Puan özelliğini kullanın.

## <a name="enable-password-management"></a>Parola yönetimini etkinleştirme

Birden çok kiracınız varsa veya kullanıcıların [kendi parolalarını sıfırlamalarını](../../active-directory/user-help/active-directory-passwords-update-your-own-password.md)sağlamak istiyorsanız, kötüye kullanımı önlemek için uygun güvenlik ilkeleri kullanmanız önemlidir.

**En iyi yöntem**: Kullanıcılarınız için self servis parola sıfırlama (SSPR) ayarlayın.  
**Ayrıntı**: Azure AD [self servis parola sıfırlama](/azure/active-directory-b2c/active-directory-b2c-reference-sspr) özelliğini kullanın.

**En iyi uygulama**: SSPR'nin gerçekten nasıl veya kullanıldığını izleyin.  
**Ayrıntı**: Azure AD Parola Sıfırlama Kayıt [İşlemi Faaliyet raporunu](/azure/active-directory/active-directory-passwords-get-insights)kullanarak kayıt yaptıran kullanıcıları izleyin. Azure AD'nin sağladığı raporlama özelliği, önceden oluşturulmuş raporları kullanarak soruları yanıtlamanıza yardımcı olur. Uygun şekilde lisansınız varsa, özel sorgular da oluşturabilirsiniz.

**En iyi uygulama**: Bulut tabanlı parola ilkelerini şirket içi altyapınıza genişletin.
**Ayrıntı**: Bulut tabanlı parola değişikliklerinde yaptığınız gibi şirket içi parola değişiklikleri için de aynı denetimleri gerçekleştirerek kuruluşunuzdaki parola ilkelerini geliştirin. Yasaklı parola listelerini mevcut altyapınıza genişletmek için Windows Server Active Directory aracıları için [Azure AD parola koruması](/azure/active-directory/authentication/concept-password-ban-bad) yükleyin. Şirket içinde parolaları değiştiren, ayarlayan veya sıfırlayan kullanıcıların ve yöneticilerin yalnızca bulut kullanıcılarıyla aynı parola ilkesine uymaları gerekir.

## <a name="enforce-multi-factor-verification-for-users"></a>Kullanıcılar için çok faktörlü doğrulamayı uygulayın

Tüm kullanıcılarınız için iki aşamalı doğrulama yapmanızı öneririz. Buna, hesapları tehlikeye girerse önemli bir etkiye sahip olabilecek yöneticileri ve kuruluşunuzdaki diğer yöneticiler (örneğin, finans memurları) dahildir.

İki aşamalı doğrulama gerektiren birden çok seçenek vardır. Sizin için en iyi seçenek hedeflerinize, çalıştırdığınız Azure REKLAM sürümüne ve lisans programınıza bağlıdır. Bir kullanıcının sizin için en iyi seçeneği belirlemesi [için iki aşamalı doğrulamayı nasıl istediğinize](/azure/active-directory/authentication/howto-mfa-userstates) bakın. Lisanslar ve fiyatlandırma hakkında daha fazla bilgi için [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) ve Azure [Çok Faktörlü Kimlik Doğrulama](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) fiyatlandırma sayfalarına bakın.

İki aşamalı doğrulamayı etkinleştirmek için seçenekler ve avantajlar şunlardır:

**Seçenek 1**: Azure AD Güvenliği Varsayılanları **Avantajı**ile tüm kullanıcılar için MFA'yı etkinleştirin ve oturum açma yöntemleri : Bu seçenek, ortamınızdaki tüm kullanıcılar için daha sıkı bir ilke ile MFA'yı kolayca ve hızlı bir şekilde uygulamanızı sağlar:

* İdari hesaplara ve yönetim oturum açma mekanizmalarına meydan okuma
* Tüm kullanıcılar için Microsoft Authenticator aracılığıyla MFA mücadelesi ne gerektirin
* Eski kimlik doğrulama protokollerini kısıtlayın.

Bu yöntem tüm lisans katmanları için kullanılabilir, ancak varolan Koşullu Erişim ilkeleriyle karıştırılamaz. Azure AD Güvenlik Varsayılanları'nda daha fazla bilgi bulabilirsiniz

**Seçenek 2**: [Kullanıcı durumunu değiştirerek Çok Faktörlü Kimlik Doğrulamayı etkinleştirin.](../../active-directory/authentication/howto-mfa-userstates.md)   
**Fayda**: İki aşamalı doğrulama gerektiren geleneksel yöntemdir. Hem [bulutta Azure Çok Faktörlü Kimlik Doğrulama ile hem](/azure/active-directory/authentication/concept-mfa-whichversion)de Azure Çok Faktörlü Kimlik Doğrulama Sunucusu ile çalışır. Bu yöntemi kullanmak, kullanıcıların her oturum açtıklarında iki aşamalı doğrulama gerçekleştirmelerini gerektirir ve Koşullu Erişim ilkelerini geçersiz kılar.

Çok Faktörlü Kimlik Doğrulama'nın nerede etkinleştirilmesi gerektiğini belirlemek [için, azure MFA'nın hangi sürümün kuruluşum için uygun olduğuna bakın.](/azure/active-directory/authentication/concept-mfa-whichversion)

**Seçenek 3**: [Koşullu Erişim ilkesiyle Çok Faktörlü Kimlik Doğrulamayı etkinleştirin.](/azure/active-directory/authentication/howto-mfa-getstarted)
**Yarar**: Bu [seçenek, Koşullu Erişim'i](/azure/active-directory/active-directory-conditional-access-azure-portal)kullanarak belirli koşullar altında iki aşamalı doğrulama için istekte beklemenizi sağlar. Belirli koşullar, farklı konumlardan, güvenilmeyen aygıtlardan veya riskli olduğunu düşündüğünüz uygulamalardan kullanıcı oturum açma olabilir. İki aşamalı doğrulama gerektiren belirli koşulları tanımlamak, kullanıcılarınız için sürekli istekten kaçınmanızı sağlar ve bu da hoş olmayan bir kullanıcı deneyimi olabilir.

Bu, kullanıcılarınız için iki aşamalı doğrulamayı etkinleştirmenin en esnek yoludur. Koşullu Erişim ilkesini etkinleştirmek yalnızca buluttaki Azure Çok Faktörlü Kimlik Doğrulaması için çalışır ve Azure AD'nin üstün bir özelliğidir. Bulut [tabanlı Azure Çok Faktörlü Kimlik Doğrulamayı Dağıt'ta](/azure/active-directory/authentication/howto-mfa-getstarted)bu yöntem hakkında daha fazla bilgi bulabilirsiniz.

**Seçenek 4**: Kullanıcıyı ve Oturum Açma Riskini [Azure AD Kimlik Koruması'nı](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa)değerlendirerek Koşullu Erişim ilkeleriyle Çok Faktörlü Kimlik Doğrulamayı etkinleştirin.   
**Yarar**: Bu seçenek şunları yapmanızı sağlar:

* Kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algıla.
* Kuruluşunuzun kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırın.
* Şüpheli olayları araştırın ve bunları çözmek için uygun eylemi gerçekleştirin.

Bu yöntem, tüm bulut uygulamaları için kullanıcı ve oturum açma riskine dayalı olarak iki aşamalı doğrulama gerekip gerekip gerekmeden gerek olmadığını belirlemek için Azure AD Kimlik Koruması risk değerlendirmesini kullanır. Bu yöntem, Azure Active Directory P2 lisanslama gerektirir. Bu yöntem hakkında daha fazla bilgiyi [Azure Active Directory Identity Protection'da](/azure/active-directory/identity-protection/overview)bulabilirsiniz.

> [!Note]
> Kullanıcı durumunu değiştirerek Çok Faktörlü Kimlik Doğrulamayı etkinleştiren Seçenek 1, Koşullu Erişim ilkelerini geçersiz kılar. 2 ve 3 seçenekleri Koşullu Erişim ilkelerini kullandığından, seçenek 1'i onlarla kullanamazsınız.

İki aşamalı doğrulama gibi fazladan kimlik koruma katmanları eklemeden kuruluşlar, kimlik bilgisi hırsızlığı saldırısına karşı daha duyarlıdır. Kimlik bilgisi hırsızlığı saldırısı veri uzlaşmasına yol açabilir.

## <a name="use-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanma

Bulut kaynakları için erişim yönetimi, bulutu kullanan tüm kuruluş için çok önemlidir. [Rol tabanlı erişim denetimi (RBAC),](/azure/role-based-access-control/overview)Azure kaynaklarına kimlerin erişebileceğini, bu kaynaklarla neler yapabileceğini ve hangi alanlara erişebileceklerini yönetmenize yardımcı olur.

Azure'daki belirli işlevlerden sorumlu gruplar veya bireysel roller inteleme, güvenlik riskleri yaratan insan ve otomasyon hatalarına yol açabilecek karışıklığı önlemeye yardımcı olur. Veri erişimi için güvenlik ilkelerini uygulamak isteyen kuruluşlar için, bilgi ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik [ilkelerine](https://en.wikipedia.org/wiki/Need_to_know) dayalı erişimi kısıtlamak zorunludur.

Güvenlik ekibinizin, riski değerlendirmek ve düzeltmek için Azure kaynaklarınızda görünürlüğe ihtiyacı vardır. Güvenlik ekibinin operasyonel sorumlulukları varsa, işlerini yapmak için ek izinlere ihtiyaç duyarlar.

[RBAC'ı,](/azure/role-based-access-control/overview) belirli bir kapsamdaki kullanıcılara, gruplara ve uygulamalara izin atamak için kullanabilirsiniz. Rol atamasının kapsamı abonelik, kaynak grubu veya tek bir kaynak olabilir.

**En iyi uygulama**: Ekibinizdeki görevleri ayırın ve yalnızca işlerini gerçekleştirmek için gereken kullanıcılara erişim miktarını tanıyın. Azure aboneliğinizde veya kaynaklarınızda herkese sınırsız izin vermek yerine, yalnızca belirli bir kapsamda belirli eylemlere izin verin.
**Ayrıntı**: Kullanıcılara ayrıcalık atamak için [Azure'da yerleşik RBAC rollerini](/azure/role-based-access-control/built-in-roles) kullanın.

> [!Note]
> Belirli izinler gereksiz karmaşıklık ve karışıklık yaratır, bir şeyi kırma korkusu olmadan düzeltilmesi zor bir "eski" yapılandırmada birikir. Kaynağa özgü izinlerden kaçının. Bunun yerine, şirket genelindeki izinler için yönetim gruplarını ve abonelikler içindeki izinler için kaynak grupları kullanın. Kullanıcıya özel izinlerden kaçının. Bunun yerine, Azure AD’deki gruplara erişim atayın.

**En iyi uygulama**: Azure sorumlulukları olan güvenlik ekiplerine Azure kaynaklarını görmek için erişim hakkı vererek riskleri değerlendirebilir ve düzeltebilirler.
**Ayrıntı**: Güvenlik ekiplerine RBAC [Security Reader](/azure/role-based-access-control/built-in-roles#security-reader) rolünü ver. Sorumlulukların kapsamına bağlı olarak kök yönetim grubunu veya segment yönetim grubunu kullanabilirsiniz:

* Tüm kurumsal kaynaklardan sorumlu ekipler için **kök yönetim grubu**
* Kapsamı sınırlı takımlar için **segment yönetim grubu** (genellikle düzenleyici veya diğer organizasyonel sınırlar nedeniyle)

**En iyi uygulama**: Doğrudan operasyonel sorumlulukları olan güvenlik ekiplerine uygun izinleri vermek.
**Ayrıntı**: Uygun rol ataması için RBAC yerleşik rollerini gözden geçirin. Yerleşik roller kuruluşunuzun özel gereksinimlerini karşılamazsa, Azure kaynakları [için özel roller](/azure/role-based-access-control/custom-roles)oluşturabilirsiniz. Yerleşik rollerde olduğu gibi, abonelik, kaynak grubu ve kaynak kapsamlarında kullanıcılara, gruplara ve hizmet ilkelerine özel roller atayabilirsiniz.

**En iyi uygulamalar**: Azure Güvenlik Merkezi'ne gereksinim duyduğu güvenlik rollerine erişim hakkı tanıyın. Güvenlik Merkezi, güvenlik ekiplerinin riskleri hızla belirlemesine ve düzeltmesine olanak tanır.
**Ayrıntı**: Güvenlik ilkelerini görüntüleyebilmeleri, güvenlik durumlarını görüntüleyebilmeleri, güvenlik ilkelerini görüntüleyebilmeleri, uyarıları ve önerileri görüntüleyebilmeleri ve uyarıları ve önerileri kapatabilmeleri için RBAC [Güvenlik Yöneticisi](/azure/role-based-access-control/built-in-roles#security-admin) rolüne bu gereksinimleri olan güvenlik ekipleri ekleyin. Bunu, sorumlulukların kapsamına bağlı olarak kök yönetim grubunu veya segment yönetim grubunu kullanarak yapabilirsiniz.

RBAC gibi yetenekleri kullanarak veri erişim denetimini zorlamayan kuruluşlar, kullanıcılarına gerekenden daha fazla ayrıcalık veriyor olabilir. Bu, kullanıcıların sahip olmamaları gereken veri türlerine (örneğin, yüksek iş etkisi) erişmelerine izin vererek veri uzlaşmasına yol açabilir.

## <a name="lower-exposure-of-privileged-accounts"></a>İmtiyazlı hesapların daha düşük pozlama

Ayrıcalıklı erişimi güvence altına almak, iş varlıklarını korumanın kritik ilk adımıdır. Güvenli bilgilere veya kaynaklara erişimi olan kişi sayısını en aza indirmek, kötü amaçlı bir kullanıcının erişim alma olasılığını azaltır veya yetkili bir kullanıcı nın farkında olmadan hassas bir kaynağı etkilemesini sağlar.

İmtiyazlı hesaplar, BT sistemlerini yöneten ve yöneten hesaplardır. Siber saldırganlar, bir kuruluşun verilerine ve sistemlerine erişmek için bu hesapları hedefler. Ayrıcalıklı erişimi güvence altına almak için, hesapları ve sistemleri kötü amaçlı bir kullanıcıya maruz kalma riskinden yalıtmalısınız.

Siber saldırganlara karşı ayrıcalıklı erişimi sağlamak için bir yol haritası geliştirmenizi ve izlemenizi öneririz. Azure AD, Microsoft Azure, Office 365 ve diğer bulut hizmetlerinde yönetilen veya bildirilen kimlikleri ve erişimi güvenli hale getirmek için ayrıntılı bir yol haritası oluşturma hakkında bilgi için, [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişimi sağlama'yı](/azure/active-directory/users-groups-roles/directory-admin-roles-secure)gözden geçirin.

Azure AD'de karma ve [bulut dağıtımları için ayrıcalıklı erişimi sağlamada](/azure/active-directory/users-groups-roles/directory-admin-roles-secure)bulunan en iyi uygulamaları aşağıda özetler:

**En iyi uygulama**: Ayrıcalıklı hesaplara erişimi yönetmek, denetlemek ve izlemek.   
**Ayrıntı**: [Azure AD Ayrıcalıklı Kimlik Yönetimi'ni](/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)açın. Ayrıcalıklı Kimlik Yönetimi'ni açtıktan sonra, ayrıcalıklı erişim rol değişiklikleri için bildirim e-posta iletileri alırsınız. Bu bildirimler, dizininizdeki ayrıcalıklı rollere ek kullanıcılar eklendiğinde erken uyarı sağlar.

**En iyi uygulama**: Tüm kritik yönetici hesaplarının Azure AD hesaplarının yönetildiğinden emin olun.
**Ayrıntı**: Tüm tüketici hesaplarını kritik yönetici rollerinden kaldırın (örneğin, hotmail.com, live.com ve outlook.com gibi Microsoft hesapları).

**En iyi uygulama**: Kimlik avı ve diğer saldırıların yönetim ayrıcalıklarını tehlikeye atmasını önlemek için tüm kritik yönetici rollerinin yönetim görevleri için ayrı bir hesaba sahip olmasını sağlayın.
**Ayrıntı**: Yönetim görevlerini gerçekleştirmek için gereken ayrıcalıkları atanan ayrı bir yönetici hesabı oluşturun. Microsoft Office 365 e-posta veya rasgele web tarama gibi günlük üretkenlik araçları için bu yönetim hesaplarının kullanımını engelleyin.

**En iyi uygulama**: İmtiyazlı rollerdeki hesapları belirleyin ve kategorilere ayırın.   
**Ayrıntı**: Azure AD Ayrıcalıklı Kimlik Yönetimi'ni adedini verdikten sonra, genel yönetici, ayrıcalıklı rol yöneticisi ve diğer yüksek ayrıcalıklı rollerdeki kullanıcıları görüntüleyin. Bu rollerde artık gerekmeyen hesapları kaldırın ve yönetici rollerine atanan kalan hesapları kategorilere ayırın:

* Yönetim kullanıcılarına ayrı ayrı atanmış ve yönetim dışı amaçlar için kullanılabilir (örneğin, kişisel e-posta)
* İdari kullanıcılara ayrı ayrı atanan ve yalnızca idari amaçlarla atanan
* Birden çok kullanıcı arasında paylaşılan
* Acil erişim senaryoları için
* Otomatik komut dosyaları için
* Harici kullanıcılar için

**En iyi uygulama**: Ayrıcalıkların maruz kalma süresini daha da azaltmak ve ayrıcalıklı hesapların kullanımına görünürlüğünüzü artırmak için "tam zamanında" (JIT) erişimi uygulayın.   
**Ayrıntı**: Azure AD Ayrıcalıklı Kimlik Yönetimi şunları yapmanızı sağlar:

* Kullanıcıları yalnızca ayrıcalıklarını JIT'den almalarıyla sınırlandırın.
* Ayrıcalıkların otomatik olarak iptal edildiğine güvenarak kısaltılmış bir süre için roller atayın.

**En iyi uygulama**: En az iki acil erişim hesabı tanımlayın.   
**Ayrıntı**: Acil erişim hesapları, kuruluşların varolan bir Azure Etkin Dizin ortamında ayrıcalıklı erişimi kısıtlamasına yardımcı olur. Bu hesaplar son derece ayrıcalıklıdır ve belirli kişilere atanmaz. Acil erişim hesapları, normal yönetim hesaplarının kullanılamayacağı senaryolarla sınırlıdır. Kuruluşlar, acil durum hesabının kullanımını yalnızca gerekli süreyle sınırlandırmalıdır.

Atanan veya genel yönetici rolü için uygun hesapları değerlendirin. `*.onmicrosoft.com` Etki alanını (acil erişim için tasarlanmıştır) kullanarak yalnızca bulutlara yönelik hesaplar görmüyorsanız, bunları oluşturun. Daha fazla bilgi için Azure [AD'deki acil erişim yönetim hesaplarını yönetme'ye](/azure/active-directory/users-groups-roles/directory-emergency-access)bakın.

**En iyi uygulama**: Acil bir durumda yerinde bir "kırık cam" işlemi var.
**Ayrıntı**: Azure [AD'de karma ve bulut dağıtımları için ayrıcalıklı erişimi sağlama](/azure/active-directory/users-groups-roles/directory-admin-roles-secure)adımlarını izleyin.

**En iyi yöntem**: Tüm kritik yönetici hesaplarının parolasız (tercih edilen) olmasını veya Çok Faktörlü Kimlik Doğrulama'yı gerektirmesini gerektirir.
**Ayrıntı**: Parola kullanmadan herhangi bir Azure REKLAM hesabında oturum açtırmak için [Microsoft Authenticator uygulamasını](/azure/active-directory/authentication/howto-authentication-phone-sign-in) kullanın. [Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-identity-verification)gibi Microsoft Authenticator da, bir aygıta bağlı olan ve biyometrik kimlik doğrulaması veya PIN kullanan bir kullanıcı kimlik bilgisini etkinleştirmek için anahtar tabanlı kimlik doğrulaması kullanır.

Azure AD yönetici rollerinden birine veya daha fazlasına kalıcı olarak atanan tüm bireysel kullanıcılar için oturum açma sırasında Azure Çok Faktörlü Kimlik Doğrulaması'nı gerektirme: Küresel Yönetici, Ayrıcalıklı Rol Yöneticisi, Exchange Online Yöneticisi ve SharePoint Çevrimiçi Yöneticisi. [Yönetici hesaplarınız için Çok Faktörlü Kimlik Doğrulaması'nı](/azure/active-directory/authentication/howto-mfa-userstates) etkinleştirin ve yönetici hesabı kullanıcılarının kaydolduğundan emin olun.

**En iyi uygulama**: Kritik yönetici hesapları için, üretim görevlerine izin verilmeyen bir yönetici iş istasyonuna (örneğin, tarama ve e-posta) sahip olun. Bu, yönetici hesaplarınızı tarama ve e-posta kullanan saldırı vektörlerinden koruyacak ve büyük bir olay riskini önemli ölçüde azaltır.
**Ayrıntı**: Yönetici iş istasyonu kullanın. İş istasyonu güvenliği düzeyi seçin:

- Son derece güvenli üretkenlik aygıtları, tarama ve diğer üretkenlik görevleri için gelişmiş güvenlik sağlar.
- [Ayrıcalıklı Erişim İş İstasyonları (PAWs),](/windows-server/identity/securing-privileged-access/privileged-access-workstations) hassas görevler için internet saldırılarına ve tehdit vektörlerine karşı korunan özel bir işletim sistemi sağlar.

**En iyi uygulama**: Çalışanlar kuruluşunuzdan ayrılırken yönetici hesaplarını deprovision.
**Ayrıntı**: Çalışanlar kuruluşunuzdan ayrılırken yönetici hesaplarını devre dışı bırakan veya silen bir işlem var.

**En iyi uygulama**: Yönetici hesaplarını güncel saldırı tekniklerini kullanarak düzenli olarak test edin.
**Ayrıntı**: Kuruluşunuzdaki gerçekçi saldırı senaryolarını çalıştırmak için Office 365 Saldırı Simülatörü veya üçüncü taraf teklifini kullanın. Bu, gerçek bir saldırı gerçekleşmeden önce savunmasız kullanıcıları bulmanıza yardımcı olabilir.

**En iyi uygulama**: En sık kullanılan saldırı tekniklerini azaltmak için adımlar atın.  
**Ayrıntı**: [Microsoft hesaplarını iş veya okul hesaplarına geçmesi gereken yönetim rollerinde tanımlayın](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts)  

[Genel yönetici hesapları için ayrı kullanıcı hesapları ve posta yönlendirmesi sağlama](/azure/active-directory/users-groups-roles/directory-admin-roles-secure)  

[İdari hesapların parolalarının son zamanlarda değiştiğinden emin olun](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#ensure-the-passwords-of-administrative-accounts-have-recently-changed)  

[Parola karma eşitlemesi açma](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#turn-on-password-hash-synchronization)  

[Tüm ayrıcalıklı rollerdeki ve maruz kalan kullanıcılar için Çok Faktörlü Kimlik Doğrulaması Gerektirme](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users)  

[Office 365 Güvenli Puanınızı edinin (Office 365 kullanıyorsanız)](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#obtain-your-office-365-secure-score-if-using-office-365)  

[Office 365 güvenlik ve uyumluluk kılavuzunu gözden geçirin (Office 365 kullanıyorsanız)](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#review-the-office-365-security-and-compliance-guidance-if-using-office-365)  

[Office 365 Etkinlik İzlemeyi Yapılandırma (Office 365 kullanıyorsanız)](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#configure-office-365-activity-monitoring-if-using-office-365)  

[Olay/acil durum müdahale planı sahipleri oluşturma](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#establish-incidentemergency-response-plan-owners)  

[Şirket içi ayrıcalıklı yönetim hesaplarının güvenli](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#turn-on-password-hash-synchronization)

Ayrıcalıklı erişimi güvence altına alamazsanız, çok fazla ayrıcalıklı rolde çok fazla kullanıcınız olduğunu ve saldırılara karşı daha savunmasız olduğunuzu görebilirsiniz. Siber saldırganlar da dahil olmak üzere kötü niyetli aktörler, genellikle yönetici hesaplarını ve ayrıcalıklı erişimin diğer öğelerini hedef alarak hassas verilere ve sistemlere kimlik bilgilerini kullanarak erişmeye çalışır.

## <a name="control-locations-where-resources-are-created"></a>Kaynakların oluşturulduğu konumları denetleme

Bulut işleçlerinin görevleri gerçekleştirmesini sağlarken, kuruluşunuzun kaynaklarını yönetmek için gereken kuralları bozmasını engellemek çok önemlidir. Kaynakların oluşturulduğu konumları denetlemek isteyen kuruluşlar bu konumları sabit kodlamalıdır.

Tanımları özellikle reddedilen eylemleri veya kaynakları açıklayan güvenlik ilkeleri oluşturmak için [Azure Kaynak Yöneticisi'ni](/azure/azure-resource-manager/resource-group-overview) kullanabilirsiniz. Bu ilke tanımlarını abonelik, kaynak grubu veya tek tek kaynak gibi istenilen kapsamda atarsınız.

> [!NOTE]
> Güvenlik ilkeleri RBAC ile aynı değildir. Kullanıcılara bu kaynakları oluşturma yetkisi vermek için RBAC'ı kullanıyorlar.
>
>

Kaynakların nasıl oluşturulduğunu denetlemeyen kuruluşlar, gereksinim duyduklarından daha fazla kaynak oluşturarak hizmeti kötüye kullanan kullanıcılara karşı daha duyarlıdır. Kaynak oluşturma işleminin sertleştirmesi, çok kiracılı bir senaryoyu güvence altına almak için önemli bir adımdır.

## <a name="actively-monitor-for-suspicious-activities"></a>Şüpheli etkinlikleri etkin bir şekilde izleme

Etkin bir kimlik izleme sistemi şüpheli davranışları hızla algılayabilir ve daha fazla araştırma için bir uyarı tetikleyebilir. Aşağıdaki tabloda, kuruluşların kimliklerini izlemelerine yardımcı olabilecek iki Azure REKLAM özelliği listelenir:

**En iyi uygulama**: Tanımlamak için bir yöntem var:

- [İzlenmeden](/azure/active-directory/active-directory-reporting-sign-ins-from-unknown-sources)oturum açma girişimleri.
- Kaba [kuvvet](/azure/active-directory/active-directory-reporting-sign-ins-after-multiple-failures) belirli bir hesaba saldırır.
- Birden çok konumdan oturum açma girişimleri.
- [Virüslü cihazlardan](/azure/active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices)oturum açma.
- Şüpheli IP adresleri.

**Ayrıntı**: Azure AD Premium [anomali raporlarını](/azure/active-directory/active-directory-view-access-usage-reports)kullanın. BT yöneticilerinin bu raporları günlük olarak veya isteğe bağlı olarak çalıştırması için (genellikle bir olay yanıt senaryosunda) işlem ve yordamlar uygulayın.

**En iyi uygulama**: Riskleri size ileden ve risk düzeyini (yüksek, orta veya düşük) iş gereksinimlerinize göre ayarlayabilen etkin bir izleme sistemine sahip olun.   
**Ayrıntı**: Geçerli riskleri kendi panosunda işaretleyen ve günlük özet bildirimleri e-posta yoluyla gönderen [Azure AD Kimlik Koruması'nı](/azure/active-directory/active-directory-identityprotection)kullanın. Kuruluşunuzun kimliklerinin korunmasına yardımcı olmak için, belirli bir risk düzeyine ulaşıldığında algılanan sorunlara otomatik olarak yanıt veren risk tabanlı ilkeleri yapılandırabilirsiniz.

Kimlik sistemlerini etkin olarak izlemeyen kuruluşlar, kullanıcı kimlik bilgilerinin tehlikeye girme riskiyle karşı lanır. Bu kimlik bilgileri aracılığıyla şüpheli etkinliklerin gerçekleştiği bilgisi olmadan, kuruluşlar bu tür bir tehdidi azaltamaz.

## <a name="use-azure-ad-for-storage-authentication"></a>Depolama kimlik doğrulaması için Azure AD'yi kullanma
[Azure Depolama,](/azure/storage/common/storage-auth-aad) Blob depolama ve Kuyruk depolama için Azure AD ile kimlik doğrulama ve yetkilendirmeyi destekler. Azure AD kimlik doğrulaması ile, kullanıcılara, gruplara ve uygulamalara tek bir blob kapsayıcısı veya sırası kapsamına kadar belirli izinler vermek için Azure rol tabanlı erişim denetimini kullanabilirsiniz.

[Depolama alanına erişimin kimliğini doğrulamak için Azure AD'yi](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)kullanmanızı öneririz.

## <a name="next-step"></a>Sonraki adım

Azure'u kullanarak bulut çözümlerinizi tasarlarken, dağıtırken ve yönetirken kullanabileceğiniz daha fazla güvenlik en iyi uygulamaları için Azure güvenlik en iyi uygulamaları ve [desenleri](best-practices-and-patterns.md) görün.
