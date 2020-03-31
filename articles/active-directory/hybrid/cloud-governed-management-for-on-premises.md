---
title: Şirket Içi İş Yükleri için Azure AD Bulut Yönetilen Yönetimi - Azure
description: Bu konu, şirket içi iş yükleri için bulut yönetim yönetimini açıklar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 510a5562740260eb2946ded074a5c37804c55375
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109512"
---
# <a name="how-azure-ad-delivers-cloud-governed-management-for-on-premises-workloads"></a>Azure AD, Şirket Içi İş Yükleri için Bulut Yönetimli Yönetimi Nasıl Sağlar?

Azure Active Directory (Azure AD), kimlik, erişim yönetimi ve güvenliğin tüm yönlerini kapsayan milyonlarca kuruluş tarafından kullanılan kapsamlı bir hizmet (IDaaS) çözümüdür. Azure AD bir milyardan fazla kullanıcı kimliğine sahiptir ve kullanıcıların oturum açmalarına ve her ikisine de güvenli bir şekilde erişmesine yardımcı olur:

* Microsoft Office 365, Azure portalı ve diğer binlerce Hizmet Olarak Yazılım (SaaS) uygulaması gibi dış kaynaklar.
* Bir kuruluşun kurumsal ağındaki ve intranetindeki uygulamalar gibi dahili kaynaklar ve bu kuruluş tarafından geliştirilen tüm bulut uygulamaları.

Kuruluşlar, 'saf bulut' ise Azure AD'yi veya şirket içi iş yükleri varsa 'karma' dağıtım olarak kullanabilir. Azure AD'nin karma dağıtımı, kuruluşun BT varlıklarını buluta geçirme veya mevcut şirket içi altyapıyı yeni bulut hizmetleriyle birlikte tümleştirmeye devam etme stratejisinin bir parçası olabilir.

Tarihsel olarak, 'karma' kuruluşlar Azure AD'yi mevcut şirket içi altyapılarının bir uzantısı olarak görmüştür. Bu dağıtımlarda, şirket içi kimlik yönetimi yönetimi, Windows Server Etkin Dizin veya diğer şirket içi dizin sistemleri denetim noktalarıdır ve kullanıcılar ve gruplar bu sistemlerden Azure AD gibi bir bulut diziniyle senkronize edilir. Bu kimlikler buluta yüklendikten sonra Office 365, Azure ve diğer uygulamalariçin kullanılabilir hale getirilebilir.

![Kimlik yaşam döngüsü](media/cloud-governed-management-for-on-premises//image1.png)

Kuruluşlar, uygulamalarıyla birlikte BT altyapılarını buluta taşıdıkça, pek çok kuruluş bir hizmet olarak kimlik yönetiminin geliştirilmiş güvenlik ve basitleştirilmiş yönetim yeteneklerini arıyor. Azure AD'deki bulut tarafından sunulan IDaaS özellikleri, kuruluşların kimlik yönetimlerini geleneksel şirket içi geleneksel şirket içi hızla benimsemelerine ve taşımalarına olanak tanıyan çözümler ve yetenekler sunarak bulut yönetim yönetimine geçişi hızlandırır sistemleri Azure AD'ye, mevcut ve yeni uygulamaları desteklemeye devam ederken.

Bu makalede, Microsoft'un karma IDaaS stratejisi özetlenebilir ve kuruluşların mevcut uygulamaları için Azure AD'yi nasıl kullanabilecekleri açıklanmaktadır.

## <a name="the-azure-ad-approach-to-cloud-governed-identity-management"></a>Bulut yönetimli kimlik yönetimine Azure AD yaklaşımı

Kuruluşlar buluta geçerken, tam ortamları üzerinde kontrollere sahip olacaklarına dair güvencelere ihtiyaç duyarlar - daha fazla güvenlik ve daha fazla etkinlik görünürlüğü, otomasyon tarafından desteklenen ve proaktif öngörüler. "**Bulut yönetim yönetimi**" kuruluşların kullanıcılarını, uygulamalarını, gruplarını ve aygıtlarını buluttan nasıl yönettiğini ve yönettiğini açıklar.

Bu modern dünyada, SaaS uygulamalarının yaygınlaşması ve işbirliği ve dış kimliklerin artan rolü nedeniyle, kuruluşların ölçekte etkin bir şekilde yönetebilmeleri gerekmektedir. Bulutun yeni risk ortamı, bir kuruluşun daha duyarlı olması gerektiği anlamına gelir - bulut kullanıcısından ödün veren kötü amaçlı bir aktör bulutu ve şirket içi uygulamaları etkileyebilir.

Özellikle, karma kuruluşların, bt'nin geçmişte el ile yaptığı görevleri temsilciolarak ve otomatikleştirebilmesi gerekir. Görevleri otomatikleştirmek için, kimlikle ilgili farklı kaynakların (kullanıcılar, gruplar, uygulamalar, aygıtlar) yaşam döngüsünü düzenleyen API'lere ve işlemlere ihtiyaç duyarlar, böylece bu kaynakların günlük yönetimini daha fazla kişi dışında başka kişilere devretebilirler. çekirdek BT personeli. Azure AD, şirket içi kimlik altyapısıgerektirmeden kullanıcılar için kullanıcı hesabı yönetimi ve yerel kimlik doğrulaması yoluyla bu gereksinimleri gidermektedir. Şirket içi altyapı oluşturmamak, şirket içi dizinde bulunmayan, ancak erişim yönetimi iş sonuçlarına ulaşmak için kritik öneme sahip olan iş ortakları gibi yeni kullanıcı topluluklarına sahip kuruluşlara fayda sağlayabilir.

Buna ek olarak, yönetim --- olmadan tam değildir ve bu yeni dünyada yönetim bir eklenti yerine kimlik sisteminin entegre bir parçasıdır. Kimlik yönetimi, kuruluşlara çalışanlar, iş ortakları ve satıcılar, hizmetler ve uygulamalar arasında kimlik ve erişim yaşam döngüsünü yönetme olanağı sağlar.

Kimlik yönetiminin bir araya getirilmesi, kuruluşun bulut yönetimi yönetimine geçişini kolaylaştırıyor, BT'nin ölçeklendirmesine olanak sağlıyor, konuklarla yeni zorlukları ele alıyor ve müşterilerin sahip olduklarından daha derin öngörüler ve otomasyon sağlıyor şirket içi altyapı. Bu yeni dünyada yönetim, bir kuruluşun kuruluş içindeki kaynaklara erişim üzerinde şeffaflık, görünürlük ve uygun denetimlere sahip olması anlamına gelir. Azure AD ile güvenlik işlemleri ve denetim ekipleri, kimlerin --- ve kimlere sahip olması gerektiği konusunda görünürlüğe sahiptir - kuruluştaki hangi kaynaklara (hangi cihazlarda) erişim, bu kullanıcıların bu erişimle ne yaptıkları ve kuruluşun uygun olup olmadığı ve kullanıp kullanmadığı şirket veya düzenleyici politikalara uygun olarak erişimi kaldırmak veya kısıtlamak için denetimler.

Yeni yönetim modeli, bu uygulamaları daha kolay yönetebildikleri ve bu uygulamalara erişimi sağlayabildikleri için, hem SaaS hem de iş yeri (LOB) uygulamalarıyla kuruluşlara fayda sağlar. Kuruluşlar, uygulamaları Azure AD ile tümleştirerek, hem bulut hem de şirket içi kaynaklı kimlikler üzerinden erişimi sürekli olarak kullanabilecek ve yönetebilecek. Uygulama yaşam döngüsü yönetimi daha otomatik hale gelir ve Azure AD, şirket içi kimlik yönetiminde kolayca ulaşılamayan uygulama kullanımına ilişkin zengin bilgiler sağlar. Kuruluşlar, Azure AD, Office 365 grupları ve Ekipler self servis özellikleri sayesinde, kolayca erişim yönetimi ve işbirliği için gruplar oluşturabilir ve işbirliği ve erişim yönetimi gereksinimlerini etkinleştirmek için buluttaki kullanıcıları ekleyebilir veya kaldırabilir.

Bulut yönetimi yönetimi için doğru Azure AD özelliklerini seçmek, kullanılacak uygulamalara ve bu uygulamaların Azure AD ile nasıl tümleştirilene bağlı. Aşağıdaki bölümlerde AD tümleşik uygulamalar için yapılacak yaklaşımlar ve federasyon protokollerini kullanan uygulamalar (örneğin, SAML, OAuth veya OpenID Connect) sıralanır.

## <a name="cloud-governed-management-for-ad-integrated-applications"></a>AD tümleşik uygulamalar için bulut yönetim yönetimi

Azure AD, güvenli uzaktan erişim ve bu uygulamalara Koşullu Erişim yoluyla kuruluşun şirket içi Active Directory tümleşik uygulamalarının yönetimini geliştirir. Buna ek olarak, Azure AD, kullanıcının aşağıdakiler de dahil olmak üzere mevcut AD hesapları için hesap yaşam döngüsü yönetimi ve kimlik bilgisi yönetimi de sağlar:

* **Şirket içi uygulamalar için güvenli uzaktan erişim ve Koşullu Erişim**

Birçok kuruluş için, şirket içi AD tümleşik web ve uzak masaüstü tabanlı uygulamalar için buluttan erişimi yönetmenin ilk adımı, güvenli uzaktan erişim sağlamak için [uygulama proxy'sini](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) bu uygulamaların önüne dağıtmaktır.

Azure AD'de tek bir oturum açtıktan sonra, kullanıcılar hem bulut hem de şirket içi uygulamalara harici bir URL veya dahili uygulama portalı üzerinden erişebilir. Örneğin, Application Proxy uzak erişim ve Uzak Masaüstü, SharePoint yanı sıra Tableau ve Qlik gibi uygulamalar ve iş hattı (LOB) uygulamaları için tek oturum açma sağlar. Ayrıca, Koşullu Erişim ilkeleri [kullanım koşullarını](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) görüntülemeyi ve [kullanıcının](https://docs.microsoft.com/azure/active-directory/conditional-access/require-tou) bir uygulamaya erişemeden önce bunları kabul etmesini sağlamayı içerebilir.

![App Proxy mimarisi](media/cloud-governed-management-for-on-premises/image2.png)

* **Active Directory hesapları için otomatik yaşam döngüsü yönetimi**

Kimlik yönetimi, kuruluşların *üretkenlik* --- bir kişinin kuruluşa katılması gibi ihtiyaç duyduğu kaynaklara ne kadar hızlı erişebildiğini --- arasında bir denge kurabilmesine yardımcı olur? --- ve *güvenlik* ---, bu kişinin istihdam durumunun ne zaman değiştiği gibi, erişimleri zaman içinde nasıl değişmelidir? Kimlik yaşam döngüsü yönetimi kimlik yönetiminin temelidir ve ölçekte etkili yönetişim, uygulamalar için kimlik yaşam döngüsü yönetim altyapısının modernleştirilmesini gerektirir.

Birçok kuruluş için, çalışanlar için kimlik yaşam döngüsü, bu kullanıcının bir insan sermayesi yönetimi (HCM) sisteminde temsiline bağlıdır. İş Günü'nü HCM sistemi olarak kullanan kuruluşlar için Azure AD, AD'deki kullanıcı hesaplarının [İş Günü'ndeki çalışanlar için otomatik olarak sağlanmasını ve bunların silinmesini](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial)sağlayabilir. Bunu yapmak, doğuştan gelen hesapların otomasyonu yoluyla kullanıcı üretkenliğinin artmasına yol açar ve kullanıcı rolleri değiştirdiğinde veya organizasyondan ayrıldığında uygulama erişiminin otomatik olarak güncelleştirilmesini sağlayarak riski yönetir. İş Günü odaklı kullanıcı sağlama [dağıtım planı,](https://aka.ms/WorkdayDeploymentPlan) beş adımlı bir süreçte İş Günü'nün En İyi Uygulama Uygulaması ile Active Directory User Provisioning çözümüne yönelik kuruluşlara yürüyen adım adım bir kılavuzdur.

Azure AD Premium, SAP, Oracle eBusiness ve Oracle PeopleSoft gibi diğer şirket içi HCM sistemlerinden kayıt içe aktarabilen Microsoft Identity Manager'ı da içerir.

İş-iş işbirliği, kuruluşunuz dışındaki kişilere erişim izni vermeyi giderek daha fazla gerektirir. [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/) işbirliği, kuruluşların uygulamalarını ve hizmetlerini konuk kullanıcılar ve dış ortaklarla güvenli bir şekilde paylaşmasına olanak sağlarken, kendi kurumsal verileri üzerinde denetim sağlar.

Azure AD, [gerektiğinde konuk kullanıcılar için AD'de otomatik olarak hesap oluşturabilir](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises) ve iş konuklarının başka bir parolaya ihtiyaç duymadan şirket içi AD ile entegre uygulamalara erişmesini sağlar. Kuruluşlar, uygulama proxy kimlik doğrulaması sırasında MFA denetimlerinin yapılması için konuk kullanıcı için [çok faktörlü kimlik doğrulama (MFA) ilkeleri](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)ayarlayabilir. Ayrıca, bulut B2B kullanıcıları üzerinde yapılan tüm [erişim incelemeleri](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews) şirket içi kullanıcılar için geçerlidir. Örneğin, bulut kullanıcısı yaşam döngüsü yönetimi ilkeleri yle silinirse, şirket içi kullanıcı da silinir.

**Active Directory hesapları için kimlik bilgileri yönetimi** Azure AD'nin self servis parola sıfırlaması, parolalarını unutan kullanıcıların yeniden kimlik doğrulaması yapılmasına ve değiştirilen parolalarla [şirket içi Active Directory'ye yazılmasıyla](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback)parolalarını sıfırlamalarına olanak tanır. Parola sıfırlama işlemi şirket içi Active Directory parola ilkelerini de kullanabilir: Bir kullanıcı parolasını sıfırladığında, bu dizine işlemeden önce şirket içi Active Directory ilkesini karşıladığından emin olmak için denetlenir. Self servis parola sıfırlama [dağıtım planı,](https://aka.ms/deploymentplans/sspr) web ve Windows'a entegre deneyimler aracılığıyla kullanıcılara self servis parola sıfırlama işlemi yapmak için en iyi uygulamaları özetler.

![Azure AD SSPR mimarisi](media/cloud-governed-management-for-on-premises/image3.png)

Son olarak, kullanıcıların AD'de parolalarını değiştirmelerine izin veren kuruluşlar için AD, şu anda genel önizlemede bulunan [Azure AD parola koruması özelliği](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises)aracılığıyla kuruluşun Azure AD'de kullandığı parola ilkesiyle aynı parola ilkesini kullanacak şekilde yapılandırılabilir.

Bir kuruluş, uygulamayı barındıran işletim sistemini Azure'a taşıyarak AD tümleşik bir uygulamayı buluta taşımaya hazır olduğunda, [Azure AD Etki Alanı Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) AD uyumlu etki alanı hizmetleri (etki alanı birleştirme, grup ilkesi, LDAP ve Kerberos/NTLM kimlik doğrulaması gibi) sağlar. Azure AD Etki Alanı Hizmetleri, kuruluşun mevcut Azure AD kiracısıyla tümleşerek kullanıcıların şirket kimlik bilgilerini kullanarak oturum açmalarını mümkün kılar. Ayrıca, varolan gruplar ve kullanıcı hesapları kaynaklara erişimi güvence altına almak için kullanılabilir ve şirket içi kaynakların Azure altyapı hizmetlerine daha sorunsuz bir 'kaldırma ve kaydırma' olmasını sağlar.

![Azure AD Domain Services](media/cloud-governed-management-for-on-premises/image4.png)

## <a name="cloud-governed-management-for-on-premises-federation-based-applications"></a>Şirket içi federasyon tabanlı uygulamalar için bulut yönetim yönetimi

Şirket içinde kimlik sağlayıcısı kullanan bir kuruluş için, uygulamaları Azure AD'ye taşımak daha güvenli erişim ve federasyon yönetimi için daha kolay bir yönetim deneyimi sağlar. Azure AD, Azure Çok Faktörlü Kimlik Doğrulaması da dahil olmak üzere uygulama başına parçalı erişim denetimlerini Azure AD Koşullu Erişim'i kullanarak yapılandırmayı sağlar. Azure AD, uygulamaya özgü belirteç imzalama sertifikaları ve yapılandırılabilir sertifika son kullanma tarihleri de dahil olmak üzere daha fazla özelliği destekler. Bu özellikler, araçlar ve kılavuzlar, kuruluşların şirket içi kimlik sağlayıcılarını emekliye ayırmalarını sağlar. Örneğin, Microsoft'un kendi BT'si, Microsoft'un dahili Active Directory Federation Services (AD FS) adresindeki 17.987 uygulamayı Azure AD'ye taşıdı.

![Azure AD evrimi](media/cloud-governed-management-for-on-premises/image5.png)

Federe uygulamaları kimlik sağlayıcısı olarak Azure AD'ye geçirmek https://aka.ms/migrateapps için aşağıdaki bağlantılara bakın:

* [Uygulamalarınızı Azure Etkin Dizini'ne Geçirme](https://aka.ms/migrateapps/whitepaper)(Geçiş) adlı teknik inceleme, geçişin faydalarını sunar ve geçiş için açıkça özetlenen dört aşamada nasıl planlanılır: keşif, sınıflandırma, geçiş ve sürekli yönetim. Süreci nasıl düşüneceğiniz ve projenizi tüketilmesi kolay parçalara ayırma konusunda size rehberlik edilecektir. Belge boyunca yol boyunca size yardımcı olacak önemli kaynaklara bağlantılar vardır.

* Çözüm kılavuzu [Etkin Dizin Federasyonu Hizmetlerinden Azure Etkin Dizini'ne Geçiş Uygulaması,](https://aka.ms/migrateapps/adfssolutionguide) bir uygulama geçiş projesiplanlama ve yürütmenin aynı dört aşamasını daha ayrıntılı olarak inceler. Bu kılavuzda, bu aşamaları, bir uygulamayı Active Directory Federation Services 'dan (AD FS) Azure AD'ye taşıma hedefine nasıl uygulayacağınızı öğreneceksiniz.

* [Etkin Dizin Federasyonu Hizmetleri Geçiş Eki Komut Dosyası,](https://aka.ms/migrateapps/adfstools) Azure AD'ye geçiş için başvuruların hazır olup olmadığını belirlemek için mevcut şirket içi Active Directory Federation Services (AD FS) sunucularında çalıştırılabilir.

## <a name="ongoing-access-management-across-cloud-and-on-premises-applications"></a>Bulut ve şirket içi uygulamalar da sürekli erişim yönetimi

Kuruluşların ölçeklenebilir erişimi yönetmek için bir işlem gerekir. Kullanıcılar erişim hakları biriktirmeye devam ediyor ve başlangıçta kendileri için sağlananın ötesine geçerek sona eriyor. Ayrıca, kurumsal kuruluşların erişim ilkesi ni ve denetimlerini sürekli olarak geliştirmek ve uygulamak için verimli bir şekilde ölçeklendirebilmeleri gerekir.

Genellikle, BT temsilcileri iş karar vericilerine onay kararlarına erişir. Ayrıca, BT kullanıcıların kendilerini içerebilir. Örneğin, bir şirketin Avrupa'daki pazarlama uygulamasındaki gizli müşteri verilerine erişen kullanıcıların şirketin ilkelerini bilmesi gerekir. Konuk kullanıcılar, davet edildikleri bir kuruluştaki verilerin işleme gereksinimlerinden de habersiz olabilir.

Kuruluşlar, [saas uygulamalarına](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)kullanıcı sağlama ile birleştiğinde [dinamik gruplar](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)gibi teknolojiler veya [Etki Alanı Arası Kimlik Yönetimi Sistemi (SCIM) standardı kullanılarak tümleşik uygulamalar](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)aracılığıyla erişim yaşam döngüsü işlemini otomatikleştirebilir. Kuruluşlar ayrıca, [konuk kullanıcıların şirket içi uygulamalara](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises)erişimi nin ne olduğunu da denetleyebilir. Bu erişim hakları daha sonra yinelenen [Azure AD erişim incelemeleri](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)kullanılarak düzenli olarak gözden geçirilebilir.

## <a name="future-directions"></a>Gelecek yönler

Karma ortamlarda, Microsoft'un stratejisi, **bulutun kimlik için denetim düzlemi olduğu**dağıtımları ve Etkin Dizin ve diğer şirket içi uygulamalar gibi şirket içi dizinlerin ve diğer kimlik sistemlerinin kullanıcılara erişim sağlama hedefi olduğu dağıtımları etkinleştirmektir. Bu strateji, bu uygulamalarda ve onlara dayanan iş yüklerinde hakların, kimliklerin ve erişimin sağlanmasını sağlamaya devam edecektir. Bu durumda, kuruluşlar son kullanıcı üretkenliğini tamamen buluttan yönlendirebilecektir.

![Azure AD mimarisi](media/cloud-governed-management-for-on-premises/image6.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu yolculuğa nasıl başalacağınız hakkında daha fazla bilgi için, 'de <https://aka.ms/deploymentplans> bulunan Azure AD dağıtım planlarına bakın. Azure Etkin Dizin (Azure AD) özelliklerinin nasıl dağıtılanöğretilenhakkında uçuça doğru kılavuzsağlarlar. Her plan, ortak Azure REKLAM özelliklerini başarıyla kullanıma çıkarmak için gereken iş değerini, planlama hususlarını, tasarımı ve işletim yordamlarını açıklar. Microsoft, Azure AD ile buluttan yönetmeye yeni özellikler eklediğimizde, dağıtım planlarını müşteri dağıtımlarından öğrenilen en iyi uygulamalar ve diğer geri bildirimlerle sürekli olarak güncelleştirir.
