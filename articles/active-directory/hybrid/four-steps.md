---
title: Güçlü bir kimlik temeline dört adım - Azure AD
description: Bu konu, karma kimlik müşterilerinin güçlü bir kimlik temeli oluşturmak için atabileceği dört adımı açıklar.
services: active-directory
author: martincoetzer
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2019
ms.subservice: hybrid
ms.author: martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3eb98f543e17981be0d5b9ab08fa4e146659b47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74206788"
---
# <a name="four-steps-to-a-strong-identity-foundation-with-azure-active-directory"></a>Azure Active Directory ile güçlü bir kimlik temeline dört adım

Uygulamalara ve verilere erişimi yönetmek, uygulamaların buluta hızlı bir şekilde taşınması nedeniyle çevre ağları ve güvenlik duvarları gibi geleneksel ağ güvenliği sınır stratejilerine artık güvenemez. Artık kuruluşlar, kuruluşun uygulamalarına ve verilerine kimlerin ve neyin erişenekadar erişebilenleri denetlemek için kimlik çözümlerine güvenmelidir. Daha fazla kuruluş, çalışanların internete bağlanabilecekleri her yerden kendi cihazlarını işlerine ve aygıtlarını kullanmalarına olanak sağlıyor. Bu aygıtların uyumlu ve güvenli olmasını sağlamak, bir kuruluşun uygulamayı seçtiği kimlik çözümünde önemli bir husustur. Günümüzün dijital işyerinde kimlik, buluta taşınan tüm kuruluşların [birincil kontrol düzlemidir.](https://www.microsoft.com/security/technology/identity-access-management?rtc=1)

Kuruluşlar, Azure Active Directory (Azure AD) karma kimlik çözümlerini benimseyerek, otomasyon, temsilcilik, self servis ve tek oturum açma özellikleri yle üretkenliğin kilidini açan birinci sınıf özelliklere erişmeye kazanç sağlar. Çalışanlarınuzun işlerini yapmaları gereken her yerden şirket kaynaklarına erişmesine olanak sağlarken, BT ekibinizin de doğru kişilerin güvenli üretkenlik sağlamak için doğru kaynaklara erişimini sağlayarak bu erişimi yönetmesine olanak tanır.

Öğrenmelerimize dayanarak, en iyi uygulamaların bu denetim listesi, kuruluşunuzda *güçlü* bir kimlik temeli oluşturmak için önerilen eylemleri hızla dağıtmanıza yardımcı olacaktır:

* Uygulamalara kolayca bağlanma
* Her kullanıcı için otomatik olarak bir kimlik oluşturma
* Kullanıcılarınızı güvenli bir şekilde güçlendirin
* Öngörülerinizi operasyonel hale

## <a name="step-1---connect-to-apps-easily"></a>Adım 1 - Uygulamalara kolayca bağlanın

Uygulamalarınızı Azure AD'ye bağlayarak, tek oturum açma (SSO) sağlayarak son kullanıcı üretkenliğini ve güvenliğini artırabilir ve kullanıcı sağlama yapabilirsiniz. Uygulamalarınızı tek bir yerde, Azure AD'de yöneterek, yönetim ek lerini en aza indirebilir ve güvenlik ve uyumluluk ilkeleriniz için tek bir denetim noktası elde edebilirsiniz.

Bu bölüm, uygulamalara kullanıcı erişimini yönetme, dahili uygulamalara güvenli uzaktan erişim sağlama ve uygulamalarınızı Azure AD'ye geçirmenin avantajlarını kapsar.

### <a name="make-apps-available-to-your-users-seamlessly"></a>Uygulamaları kullanıcılarınızın kullanımına sorunsuz bir şekilde sun

Azure AD, yöneticilerin [Azure portalındaki](https://portal.azure.com/)Kurumsal uygulamalar galerisine [uygulama eklemesine](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal) olanak tanır. Kurumsal uygulamalar galerisine uygulama eklemek, uygulamaları kimlik sağlayıcınız olarak Azure AD'yi kullanacak şekilde yapılandırmanızı kolaylaştırır. Ayrıca, Koşullu Erişim ilkeleriyle uygulamaya kullanıcı erişimini yönetmenize ve kullanıcıların parolalarını tekrar tekrar girmek zorunda kalmaması ve hem şirket içinde hem de şirket içinde otomatik olarak oturum açmaları için uygulamalara tek oturum açma (SSO) yapılandırmanızı sağlar bulut tabanlı uygulamalar.

Uygulamalar Azure AD galerisine eklendikten sonra, kullanıcılar kendilerine atanan uygulamaları görebilir ve gerektiğinde diğer uygulamaları arayabilir ve isteyebilir. Azure AD, kullanıcıların uygulamalarına erişmeleri için [çeşitli yöntemler](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences) sunar:

* Erişim paneli/Uygulamalarım
* Office 365 uygulama başlatıcısı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Doğrudan oturum açma bağlantıları

Uygulamalara kullanıcı erişimi hakkında daha fazla bilgi edinmek için **bkz.**

### <a name="migrate-apps-from-active-directory-federation-services-to-azure-ad"></a>Uygulamaları Active Directory Federation Services'den Azure AD'ye geçirin

Active Directory Federation Services'dan (ADFS) Azure AD'ye tek oturum açma yapılandırması geçirmek, güvenlik, daha tutarlı yönetilebilirlik ve işbirliği konusunda ek özellikler sağlar. En iyi sonuçlar için, uygulamalarınızı AD FS'den Azure AD'ye geçirmenizi öneririz. Uygulama kimlik doğrulamanızı ve yetkilendirmenizi Azure AD'ye getirmek size aşağıdaki avantajları sağlar:

* Maliyeti yönetme
* Riski yönetme
* Üretkenliği artırma
* Uyumluluk ve yönetişimin ele alınması

Daha fazla bilgi edinmek [için Uygulamalarınızı Azure Etkin Dizin teknik incelemesine geçirin.](https://aka.ms/migrateapps/whitepaper)

### <a name="enable-secure-remote-access-to-apps"></a>Uygulamalara güvenli uzaktan erişim sağlama

[Azure AD Application Proxy,](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-application-proxy) kuruluşların dahili uygulamalara güvenli bir şekilde erişmeye ihtiyaç duyan uzak kullanıcılar için şirket içi uygulamaları bulutta yayımlamaları için basit bir çözüm sağlar. Azure AD'de tek bir oturum açtıktan sonra, kullanıcılar hem bulut hem de şirket içi uygulamalara harici URL'ler veya dahili uygulama portalı aracılığıyla erişebilir.

Azure AD Application Proxy aşağıdaki avantajları sunar:

* Azure AD'yi şirket içi kaynaklara genişletme
  * Bulut ölçekli güvenlik ve koruma
  * Koşullu Erişim ve Çok Faktörlü Kimlik Doğrulama gibi kolay etkinleştirilebilen özellikler
* VPN ve geleneksel ters proxy çözümleri gibi çevre ağında hiçbir bileşen yok
* Gelen bağlantı gerekmez
* Bulutta ve şirket içinde cihazlar, kaynaklar ve uygulamalar arasında tek oturum açma (SSO)
* Son kullanıcıların her zaman ve her yerde üretken olmalarını sağlar

### <a name="discover-shadow-it-with-microsoft-cloud-app-security"></a>Microsoft Cloud App Security ile Gölge BT'yi Keşfedin

Modern işletmelerde, BT departmanları genellikle kullanıcıların işlerini yapmak için kullandıkları tüm bulut uygulamalarından haberdar değildir. BT yöneticilerine çalışanlarının kaç bulut uygulaması kullandığını düşündükleri sorulduğunda, ortalama 30 veya 40 diyorlar. Gerçekte, ortalama 1.000'den fazla ayrı uygulama kuruluşunuzdaki çalışanlar tarafından kullanılıyor. Çalışanların %80'i, kimsenin incelemediği ve güvenlik ve uyumluluk ilkelerinizle uyumlu olmadığı yaptırımuygulamaları kullanır.

[Microsoft Bulut Uygulama Güvenliği](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) (MCAS), kullanıcıların SSO ve Koşullu Erişim gibi özelliklerden yararlanabilmesi için BT'nin onaylayıp Kurumsal uygulamalar galerisine ekleyebileceği kullanıcılar arasında popüler olan yararlı uygulamaları belirlemenize yardımcı olabilir.

<em>"**Cloud App Security,** kullanıcılarımızın Accenture'un korunmasına yardımcı olan temel güvenlik ilkelerini destekleyecek şekilde bulut ve SaaS uygulamalarımızı doğru şekilde kullanmalarını sağlamamıza yardımcı olur."</em> --- [John Blasi, Genel Müdür, Bilgi Güvenliği, Accenture](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security)

McAS, gölge BT'yi algılamanın yanı sıra uygulamaların risk düzeyini de belirleyebilir, kurumsal verilere yetkisiz erişimi, olası veri sızıntısını ve uygulamaların doğasında bulunan diğer güvenlik risklerini engelleyebilir.

## <a name="step-2---establish-one-identity-for-every-user-automatically"></a>Adım 2 - Her kullanıcı için otomatik olarak bir kimlik oluşturma

Azure AD karma kimlik çözümünde şirket içi ve bulut tabanlı dizinleri bir araya getirmek, mevcut kimliklerinizi bulutta sağlayarak mevcut şirket içi Active Directory yatırımınızı yeniden kullanmanıza olanak tanır. Çözüm, şirket içi kimlikleri Azure AD ile eşitlerken, BT, kimlikler için birincil doğruluk kaynağı olarak şirket içi Active Directory'yi mevcut yönetim çözümleriyle çalışır durumda tutar. Microsoft'un Azure AD karma kimlik çözümü şirket içi ve bulut tabanlı özellikleri kapsar ve konumlarından bağımsız olarak tüm kaynaklara kimlik doğrulama ve yetkilendirme için ortak bir kullanıcı kimliği oluşturur.

Şirket içi dizinlerinizi Azure AD ile tümleştirmek, kullanıcılarınızı daha üretken hale getirir ve hem bulut hem de şirket içi kaynaklara erişmek için ortak bir kimlik sağlayarak kullanıcıların uygulamalar ve hizmetler arasında birden çok hesap kullanmasını engeller. Birden çok hesap kullanmak, hem son kullanıcılar hem de BT için bir acı noktasıdır. Son kullanıcı açısından bakıldığında, birden çok hesaba sahip olmak, birden çok parolayı hatırlamak zorunda kalmak anlamına gelir. Bunu önlemek için, birçok kullanıcı güvenlik açısından kötü olan her hesap için aynı parolayı yeniden kullanır. BT açısından bakıldığında, yeniden kullanım genellikle son kullanıcı şikayetleriyle birlikte daha fazla parola sıfırlama ve yardım masası maliyetine yol açar.

Azure AD Connect, şirket içi kimliklerinizi Azure AD ile senkronize etmek için kullanılan ve bulut uygulamalarına erişmek için kullanılabilen araçtır. Kimlikler Azure AD'ye yüklendikten sonra Salesforce veya Concur gibi SaaS uygulamalarına sağlayabilir.

Bu bölümde, yüksek kullanılabilirlik sağlamak, bulut için modern kimlik doğrulaması sağlamak ve şirket içi ayak izinizi azaltmak için önerileri listeliyoruz.

> [!NOTE]
> Azure AD Connect hakkında daha fazla bilgi edinmek istiyorsanız, Azure [AD Bağlantı Eşitleme nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis)

### <a name="set-up-a-staging-server-for-azure-ad-connect-and-keep-it-up-to-date"></a>Azure AD Connect için bir hazırlama sunucusu ayarlayın ve güncel tutun

Azure AD Connect, sağlama işleminde önemli bir rol oynar. Eşitleme Sunucusu herhangi bir nedenle çevrimdışı olursa, şirket içi değişiklikler bulutta güncelleştirilmez ve kullanıcılara erişim sorunlarına neden olur. Eşitleme sunucusu çevrimdışı olduktan sonra yöneticilerin eşitleme yi hızlı bir şekilde sürdürmelerine olanak tanıyan bir hata stratejisi tanımlamak önemlidir.

Birincil Azure AD Connect sunucunuzun çevrimdışı olması durumunda yüksek kullanılabilirlik sağlamak için Azure AD Connect için ayrı bir [hazırlama sunucusu](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-staging-server) dağıtmanız önerilir. Sunucu dağıtmak, yöneticinin evreleme sunucusunu basit bir yapılandırma anahtarıyla üretime "tanıtmasına" olanak tanır. Hazırlama modunda yapılandırılan bir bekleme sunucusuna sahip olmak, yeni yapılandırma değişikliklerini sınamanızı ve dağıtmanızı ve eskisini devre dışı bırakmak durumunda yeni bir sunucu tanıtmanızı da sağlar.

> [!TIP]
> Azure AD Connect düzenli olarak güncelleştirilir. Bu nedenle, her yeni sürümün sağladığı performans geliştirmelerinden, hata düzeltmelerinden ve yeni özelliklerden yararlanmak için evreleme sunucusunu güncel tutmanız önerilir.

### <a name="enable-cloud-authentication"></a>Bulut kimlik doğrulamasını etkinleştirme

Şirket içi Active Directory'ye sahip kuruluşlar, Azure AD Connect'i kullanarak dizinlerini Azure AD Bağlantısı'na genişletmeli ve uygun kimlik doğrulama yöntemini yapılandırmalıdır. Kuruluşunuz için [doğru kimlik doğrulama yöntemini seçmek,](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn) uygulamaları buluta taşıma yolculuğunuzdaki ilk adımdır. Tüm bulut verilerine ve kaynaklarına erişimi kontrol ettiği için kritik bir bileşendir.

Azure AD'deki şirket içi dizin nesneleri için bulut kimlik doğrulamasını etkinleştirmek için en basit ve önerilen yöntem [Parola Karma Eşitleme(PHS)](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) etkinleştirmektir. Alternatif olarak, bazı kuruluşlar [Geçiş Kimlik Doğrulaması'nı](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) (PTA) etkinleştirmeyi düşünebilir.

PHS veya PTA'yı seçin, kullanıcıların kurumsal ağınızdaki Windows 7 ve 8 cihazlarını kullanırken kullanıcı adlarını ve parolalarını uygulamada sürekli girmeden bulut uygulamalarına erişebilmeleri için [Sorunsuz Tek Oturum](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso) Açma'yı etkinleştirmeyi unutmayın. Tek oturum açmadan, kullanıcıların uygulamaya özgü parolaları hatırlaması ve her uygulamada oturum açmaları gerekir. Aynı şekilde, BT personelinin Office 365, Box ve Salesforce gibi her uygulama için kullanıcı hesapları oluşturması ve güncelleştirmesi gerekir. Kullanıcıların parolalarını hatırlamaları ve her uygulamada oturum açmaları için zaman harcamaları gerekir. Tüm kuruluşa standartlaştırılmış tek oturum açma mekanizması sağlamak, en iyi kullanıcı deneyimi, riskin azaltılması, rapor verme yeteneği ve yönetişim için çok önemlidir.

AD FS'yi veya başka bir şirket içi kimlik doğrulama sağlayıcısını kullanan kuruluşlar için, kimlik sağlayıcınız olarak Azure AD'ye geçmek karmaşıklığı azaltabilir ve kullanılabilirliği artırabilir. Federasyonu kullanmak için özel kullanım durumlarınız yoksa, azaltılmış şirket içi ayak izinin avantajlarından ve bulutun sunduğu esneklikten yararlanmak için federe kimlik doğrulamadan PHS ve Dikişsiz SSO veya PTA ve Dikişsiz SSO'ya geçiş yapmanızı öneririz. geliştirilmiş kullanıcı deneyimleri. Daha fazla bilgi için bkz: [Azure Etkin Dizini için federasyondan parola karma eşitlemesine geçir.](https://docs.microsoft.com/azure/active-directory/hybrid/plan-migrate-adfs-password-hash-sync)

### <a name="enable-automatic-deprovisioning-of-accounts"></a>Hesapların otomatik olarak yok edilmesini etkinleştirme

Uygulamalarınız için otomatik sağlama ve deprovisioning etkinleştirme, birden çok sistem arasında kimliklerin yaşam döngüsünü yönetmek için en iyi stratejidir. Azure AD, serviceNow ve Salesforce gibi çeşitli popüler SaaS uygulamalarına ve [SCIM 2.0 protokolünü](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)uygulayan diğer uygulamalara kullanıcı hesaplarının [otomatik, ilke tabanlı sağlanmasını](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-automatic-user-provisioning-portal) ve bunların yok olmasını destekler. Özel kod veya CSV dosyalarının el ile yüklenmesini gerektiren geleneksel sağlama çözümlerinin aksine, sağlama hizmeti bulutta barındırılır ve Azure portalı kullanılarak kurulabilen ve yönetilebilen önceden tümleşik bağlayıcılara sahiptir. Otomatik deprovisioning'in önemli bir avantajı, kullanıcıların kimliklerini kuruluştan ayrıldıklarında anahtar SaaS uygulamalarından anında kaldırarak kuruluşunuzun güvenliğini sağlamaya yardımcı olmasıdır.

Otomatik kullanıcı hesabı sağlama ve nasıl çalıştığı hakkında daha fazla bilgi edinmek için Azure [Etkin Dizini ile SaaS Uygulamalarına OtomatikLeştirme ve Kullanım Danamasını](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)Bölümü'ne bakın.

## <a name="step-3---empower-your-users-securely"></a>Adım 3 - Kullanıcılarınızı güvenli bir şekilde güçlendirin

Günümüzün dijital işyerinde, güvenliği verimlilikle dengelemek önemlidir. Ancak, son kullanıcılar genellikle üretkenliklerini ve bulut uygulamalarına erişimlerini yavaşlatan güvenlik önlemlerini geri püskürter. Azure AD, bu sorunu gidermek için, kullanıcıların yönetim ek yükü en aza indirirken üretken kalmalarını sağlayan self servis özellikleri sağlar.

Bu bölümde, tetikte kalırken kullanıcılarınızı güçlendirerek cihazınızdaki sürtünmeyi ortadan kaldırmak için öneriler listeleneb.)'de.

### <a name="enable-self-service-password-reset-for-all-users"></a>Tüm kullanıcılar için Self Servis Parola Sıfırlama'yı etkinleştirme

Azure'un [self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) (SSPR), BT yöneticilerinin kullanıcıların parolalarını veya hesaplarını yönetici müdahalesi olmadan sıfırlamalarına ve kilidini açmalarına olanak sağlaması için basit bir araç sunar. Sistem, kullanıcıların sisteme erişimini izleyen ayrıntılı raporlama içerir, ayrıca kötüye kullanım veya uygunsuz kullanım konusunda uyaran bildirimler sağlar.

Varsayılan olarak, Azure AD parola sıfırlama gerçekleştirirken hesapların kilidini açar. Ancak, Azure AD Connect [tümleştirmesini şirket içinde](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#on-premises-integration)etkinleştirdiğinizde, kullanıcıların parolayı sıfırlamak zorunda kalmadan hesaplarının kilidini açmalarını sağlayan bu iki işlemi ayırma seçeneğiniz de vardır.

### <a name="ensure-all-users-are-registered-for-mfa-and-sspr"></a>Tüm kullanıcıların MFA ve SSPR'ye kayıtlı olduğundan emin olun

Azure, kullanıcıların MFA ve SSPR'ye kayıtlı olduğundan emin olmak için sizin ve kuruluşunuz tarafından kullanılabilecek raporlar sağlar. Kayıt yaptırmayan kullanıcıların bu süreçte eğitilmesi gerekebilir.

MFA [oturum açma raporu,](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-reporting) MFA kullanımı hakkında bilgi içerir ve MFA'nın kuruluşunuzdaki çalışma şeklihakkında size bilgi verir. Azure AD için oturum açma etkinliğine (ve denetimlere ve risk algılamalarına) erişim sağlamak, sorun giderme, kullanım analitiği ve adli tıp araştırmaları için çok önemlidir.

Aynı şekilde, [Self servis Şifre Yönetimi raporu,](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-reporting) SSPR'ye kimin kaydolduğunu (veya kaydetmediğini) belirlemek için kullanılabilir.

### <a name="self-service-app-management"></a>Self servis uygulama yönetimi

Kullanıcılarınız uygulamaları erişim panelinden kendi kendine keşfedebilmeniz için, kullanıcıların kendi kendine keşfetmesine ve erişimini istemesine izin vermek istediğiniz uygulamalara [self servis uygulama erişimini](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-manage-self-service-access) etkinleştirmeniz gerekir. Self servis uygulama erişimi, kullanıcıların uygulamaları kendi kendine keşfetmelerine ve isteğe bağlı olarak iş grubunun bu uygulamalara erişimi onaylamasına izin vermek için harika bir yoldur. İş grubunun, bu kullanıcılara [Parola Tek Oturum](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery#configure-password-sso-for-an-azure-ad-gallery-app) Açma Uygulamaları için atanan kimlik bilgilerini erişim panellerinden yönetmesine izin verebilirsiniz.

### <a name="self-service-group-management"></a>Self servis grup yönetimi

Kullanıcıları uygulamalara atamak, grupları kullanırken en iyi şekilde eşlenir, çünkü büyük esneklik ve ölçekte yönetme olanağı sağlar:

* Dinamik grup üyeliğini kullanarak öznitelik tabanlı
* Uygulama sahiplerine temsilci

Azure AD, güvenlik grupları ve Office 365 gruplarını kullanarak kaynaklara erişimi yönetme olanağı sağlar. Bu gruplar, üyelik isteklerini onaylayan veya reddedebilen ve grup üyeliğinin denetimini devreden bir grup sahibi tarafından yönetilebilir. Self [servis grup yönetimi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management)olarak bilinen bu özellik, yönetime atanamayan grup sahiplerinin isteklerini işlemek için yöneticilere güvenmek zorunda kalmadan gruplar oluşturmasına ve yönetmesine izin vererek zamandan tasarruf sağlar.

## <a name="step-4---operationalize-your-insights"></a>Adım 4 - Öngörülerinizi operasyonel hale

Güvenlikle ilgili olayların ve ilgili uyarıların denetlemesi ve günlüğe kaydetmesi, kullanıcıların üretken kalmasını ve kuruluşunuzun güvende olmasını sağlamak için etkin bir stratejinin temel bileşenleridir. Güvenlik günlükleri ve raporları aşağıdaki gibi soruları yanıtlamaya yardımcı olabilir:

* Parasını ödediğin şeyi mi kullanıyorsun?
* Kiracımda şüpheli ya da kötü niyetli bir şey oluyor mu?
* Bir güvenlik olayı sırasında kim etkilendi?

Güvenlik günlükleri ve raporları, şüpheli etkinliklerin elektronik bir kaydını sağlar ve ağın ve dahili saldırıların girişimini veya başarılı bir şekilde dış ağını gösteren desenleri algılamanıza yardımcı olur. Denetimi, kullanıcı etkinliğini izlemek, mevzuata uygunluğu belgelemek, adli analiz yapmak ve daha fazlasını yapmak için kullanabilirsiniz. Uyarılar güvenlik olayları bildirimleri sağlar.

### <a name="assign-least-privileged-admin-roles-for-operations"></a>İşlemler için en az ayrıcalıklı yönetici rolleri atama

Operasyonlara yaklaşımınızı düşündüğünüzde, göz önünde bulundurulması gereken birkaç yönetim düzeyi vardır. Birinci düzey, yönetim yükünü küresel yöneticinizin (ler) üzerine yerleştirir. Her zaman küresel yönetici rolünü kullanarak, küçük şirketler için uygun olabilir. Ancak, yardım masası personeli ve belirli görevlerden sorumlu yöneticilere sahip daha büyük kuruluşlar için, bu kişilere yukarıda ve ötesindeki görevleri yönetme olanağı sağladığından, küresel yönetici rolünü atamak bir güvenlik riski olabilir yapabilecekkapasitelerini.

Bu durumda, bir sonraki yönetim düzeyini göz önünde bulundurmalısınız. Azure AD'yi kullanarak, son kullanıcıları görevleri daha az ayrıcalıklı rollerde yönetebilecek "sınırlı yöneticiler" olarak atayabilirsiniz. Örneğin, yalnızca okunur erişimle güvenlikle ilgili özellikleri yönetme olanağı sağlamak için yardım masası personelinizi [güvenlik okuyucu](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#security-reader) rolüolarak atayabilirsiniz. Ya da parola dışı kimlik bilgilerini sıfırlama veya Azure Hizmet Durumu'nu okuma ve yapılandırma olanağı sağlamak için kimlik [doğrulama yöneticisi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#authentication-administrator) rolünü bireylere atamak mantıklı olabilir.

Daha fazla bilgi edinmek için [Azure Etkin Dizin'inde Yönetici rol izinlerine](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)bakın.

### <a name="monitor-hybrid-components-azure-ad-connect-sync-ad-fs-using-azure-ad-connect-health"></a>Azure AD Connect Health'i kullanarak karma bileşenleri (Azure AD Connect eşitleme, AD FS) izleyin

Azure AD Connect ve AD FS, yaşam döngüsü yönetimini ve kimlik doğrulamasını kırabilecek ve sonuçta kesintilere yol açabilecek kritik bileşenlerdir. Bu nedenle, bu bileşenlerin izlenmesi ve raporlanması için Azure AD Connect Health'i dağıtmanız gerekir.

Daha fazla bilgi için [Azure AD Connect Health'i kullanarak SPS'yi izleyin'](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs)i okuyun.

### <a name="use-azure-monitor-to-collect-data-logs-for-analytics"></a>Analitik için veri günlükleri toplamak için Azure Monitörünü kullanın

[Azure Monitor,](https://docs.microsoft.com/azure/azure-monitor/overview) derin öngörüler, gelişmiş analitik ve akıllı makine öğrenimi sağlayan tüm Azure REKLAM günlükleri için birleşik bir izleme portalıdır. Azure Monitor ile, kaynaklarınızın durumu ve performansı hakkında daha fazla görünürlük elde etmek için portal içinde ve API'ler aracılığıyla ölçümleri ve günlükleri tüketebilirsiniz. Geleneksel üçüncü taraf SIEM sistemlerini destekleyen API'ler ve veri dışaaklama seçenekleri aracılığıyla geniş bir ürün entegrasyonu yelpazesi sağlarken portal içinde tek bir cam deneyimi bölmesi sağlar. Azure Monitor ayrıca, uyarı kurallarını uyarı kurallarını yapılandırma veya kaynaklarınızı etkileyen konularda otomatik eylemlerde bulunmak için de izin verir.

![Azure İzleyici](./media/four-steps/image1.png)

### <a name="create-custom-dashboards-for-your-leadership-and-your-day-to-day"></a>Liderliğiniz ve günden güne özel panolar oluşturun

SIEM çözümü olmayan kuruluşlar Azure AD için [Power BI İçerik Paketini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-power-bi-content-pack) indirebilir. Power BI içerik paketi, kullanıcılarınızın Azure AD özelliklerini nasıl benimsediğini ve kullandığını anlamanıza yardımcı olmak için önceden oluşturulmuş raporlar içerir ve bu da dizininizdeki tüm etkinlikler hakkında bilgi edinmenize olanak tanır. Ayrıca kendi özel [panonuzu](https://docs.microsoft.com/power-bi/service-dashboards) oluşturabilir ve günlük etkinlikleri raporlamak için liderlik ekibinizle paylaşabilirsiniz. Panolar, işletmenizi izlemek ve en önemli ölçümlerinizi bir bakışta görmek için harika bir yoldur. Panolardaki görselleştirmeler, bir veya daha fazla temel alınan veri kümesinden ve bağlantılı rapordan oluşabilir. Bir panoda şirket içi ve bulut verileri bir arada kullanılabilir ve verilerin konumundan bağımsız olarak tümleşik bir görünüm oluşturulabilir.

![Güç BI özel pano](./media/four-steps/image2.png)

### <a name="understand-your-support-call-drivers"></a>Destek arama sürücülerinizi anlayın

Bu makalede özetlenen bir karma kimlik çözümü uyguladığınızda, sonuçta destek çağrılarınızda bir azalma fark etmelisiniz. Unutulan parolalar ve hesap kilitlenmeleri gibi yaygın sorunlar Azure'un self servis parola sıfırlama sını uygulayarak azaltılırken, self servis uygulama erişimini etkinleştirmek kullanıcıların uygulamalara güvenmeden kendi kendine keşfetmesini ve erişimini istemesine olanak tanır. BT personelinizde.

Destek aramalarında bir azalma gözlemlemezseniz, SSPR veya self servis uygulama erişiminin doğru yapılandırıldığını veya sistematik olarak yapIlebilen başka yeni sorunlar olup olmadığını doğrulamak için destek çağrı sürücülerinizi analiz etmenizi öneririz Ele.

*"Dijital dönüşüm yolculuğumuzda, etkin bir ekosistem için bizimle, iş ortaklarımıza ve bulut hizmeti sağlayıcılarına sorunsuz ve güvenli entegrasyonu kolaylaştırmak için güvenilir bir kimlik ve erişim yönetimi sağlayıcısına ihtiyacımız vardı; Azure AD, riskleri algılamamızı ve yanıt vermemizi sağlayan gerekli yetenekleri ve görünürlüğü sunan en iyi seçenekti."* --- [Yazan Almasri, Küresel Bilgi Güvenliği Direktörü, Aramex](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en)

### <a name="monitor-your-usage-of-apps-to-drive-insights"></a>Öngörüler yönlendirmek için uygulamaları kullanımınızı izleyin

Shadow IT'i keşfetmenin yanı sıra, Microsoft [Cloud App Security'yi](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) kullanarak kuruluşunuz genelinde uygulama kullanımını izlemek, bulut uygulamaları vaadinden tam olarak yararlanmak için hareket ederken kuruluşunuzda yardımcı olabilir. Daha iyi bir etkinlik görünürlüğü sayesinde varlıklarınızın denetimini korumanıza yardımcı olabilir ve bulut uygulamaları ndaki kritik verilerin korunmasını artırabilir. MCAS kullanarak kuruluşunuzdaki uygulama kullanımını izlemek aşağıdaki soruları yanıtlamanıza yardımcı olabilir:

* Çalışanlar verileri depolamak için hangi onaylanmamış uygulamaları kullanıyor?
* Hassas veriler bulutta nerede ve ne zaman depolanıyor?
* Buluttaki hassas verilere kim erişiyor?

*"Cloud App Security ile, anormallikleri hızlı bir şekilde tespit edebilir ve harekete geçebiliriz."* --- [Eric LePenske, Kıdemli Müdür, Bilgi Güvenliği, Accenture](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security)

## <a name="summary"></a>Özet

Karma Kimlik çözümünü uygulamanın birçok yönü vardır, ancak bu dört adımlı denetim listesi, kullanıcıların daha üretken ve güvenli olmasını sağlayacak bir kimlik altyapısını hızla gerçekleştirmenize yardımcı olur.

* Uygulamalara kolayca bağlanma
* Her kullanıcı için otomatik olarak bir kimlik oluşturma
* Kullanıcılarınızı güvenli bir şekilde güçlendirin
* Öngörülerinizi operasyonel hale

Bu belgenin kuruluşunuz için güçlü bir kimlik temeli oluşturmak için yararlı bir yol haritası olmasını umuyoruz.

## <a name="identity-checklist"></a>Kimlik kontrol listesi

Kuruluşunuzdaki daha sağlam bir kimlik temeline yolculuğunuza başlarken başvuru için aşağıdaki denetim listesini yazdırmanızı öneririz.

### <a name="today"></a>Bugün

|Açtınız mı?|Öğe|
|:-|:-|
||Bir grup için Pilot Self Servis Şifre Sıfırlama (SSPR)|
||Azure AD Connect Health'i kullanarak karma bileşenleri izleme|
||Operasyon için en az ayrıcalıklı yönetici rolleri atama|
||Microsoft Cloud App Security ile Gölge BT'yi Keşfedin|
||Analiz için veri günlükleri toplamak için Azure Monitörünü kullanın|

### <a name="next-two-weeks"></a>Önümüzdeki iki hafta

|Açtınız mı?|Öğe|
|:-|:-|
||Bir uygulamayı kullanıcılarınız için kullanılabilir hale getirin|
||Tercih için bir SaaS uygulaması için Pilot Azure AD sağlık|
||Azure AD Connect için bir hazırlama sunucusu kurun ve güncel tutun|
||Uygulamaları ADFS'den Azure AD'ye geçirmebaşlatma|
||Liderliğiniz ve günden güne özel panolar oluşturun|

### <a name="next-month"></a>Gelecek ay

|Açtınız mı?|Öğe|
|:-|:-|
||Öngörüler yönlendirmek için uygulamaları kullanımınızı izleyin|
||Pilot, uygulamalara uzaktan erişimi güvenli hale getirmek|
||Tüm kullanıcıların MFA ve SSPR'ye kayıtlı olduğundan emin olun|
||Bulut kimlik doğrulamasını etkinleştirme|

### <a name="next-three-months"></a>Önümüzdeki üç ay

|Açtınız mı?|Öğe|
|:-|:-|
||Self servis uygulama yönetimini etkinleştirme|
||Self servis grup yönetimini etkinleştirme|
||Öngörüler yönlendirmek için uygulamaları kullanımınızı izleyin|
||Destek arama sürücülerinizi anlayın|

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory'nin özelliklerini ve bu beş adımlı denetim listesini kullanarak güvenli duruşunuzu nasıl artırabileceğinizi öğrenin - [Kimlik altyapınızı güvence altına almak için beş adım.](https://aka.ms/securitysteps)

Azure AD'deki kimlik özelliklerinin, kuruluşların kimlik yönetimlerini geleneksel şirket içi sistemlerden Azure AD'ye hızlı bir şekilde benimsemelerine ve taşımalarına olanak tanıyan çözümler ve özellikler sağlayarak bulut yönetim yönetimine geçişinizi hızlandırmanıza nasıl yardımcı olabileceğini öğrenin - [Azure AD Şirket İçi İş Yükleri için Bulut Yönetim Yönetimini Nasıl Sunar.](https://aka.ms/cloudgoverned)
