---
title: Karma bulut kimlik tasarımı Azure için kimlik gereksinimleri | Microsoft Docs
description: Hibrit kimlik tasarımı için gereksinimleri tanımlamanızı sağlayacak şirketin iş ihtiyaçlarını belirleyin.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
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
ms.openlocfilehash: 074c203a0a5688855d4f7607a877b25ce6ee6ad7
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89660575"
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için kimlik gereksinimlerini belirleme
Karma kimlik çözümü tasarlamanın ilk adımı, bu çözümü kullanacak iş kuruluşunun gereksinimlerini belirlemektir.  Karma kimlik, bir destekleme rolü olarak başlar (kimlik doğrulaması sağlayarak diğer tüm bulut çözümlerini destekler) ve kullanıcılar için yeni iş yüklerinin kilidini açmak üzere yeni ve ilginç yetenekler sağlamak için üzerine gider.  Kullanıcılarınız için benimsemek istediğiniz bu iş yükleri veya hizmetler karma kimlik tasarımı için gereksinimleri dikte eder.  Bu hizmetlerin ve iş yüklerinin hem şirket içinde hem de bulutta karma kimlikle yararlanması gerekir.  

Artık gereksinimin ne olduğunu ve şirketin geleceğe yönelik planlama yaptığını anlamak için işletmeden bu ana yönlere gitmeniz gerekir. Karma kimlik tasarımı için uzun süreli stratejinin görünebilirliği yoksa, işin büyümesi ve değişmesi nedeniyle çözümünüzün ölçeklendirilememesi gerekir. Aşağıdaki diyagramda, karma kimlik mimarisinin ve kullanıcılar için kilitsiz olan iş yüklerinin bir örneği gösterilmektedir. Bu yalnızca, kesintisiz bir karma kimlik stratejisiyle birlikte açılabilir ve teslim edilebilir tüm yeni olasılıklarla ilgili bir örnektir. 

Karma kimlik mimarisi ![ karma kimlik mimarisinin parçası olan bazı bileşenler](./media/plan-hybrid-identity-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>İş ihtiyaçlarını belirleme
Her şirket farklı gereksinimlere sahip olacaktır, bu şirketler aynı sektörde yer alsa bile, gerçek iş gereksinimleri farklılık gösterebilir. Sektördeki en iyi uygulamalardan yararlanmaya devam edebilirsiniz, ancak bu, son olarak şirketin iş gereksinimleridir ve bu da karma kimlik tasarımı için gereksinimleri tanımlamanızı sağlar. 

İş ihtiyaçlarınızı belirlemek için aşağıdaki soruları cevaplayın:

* Şirketiniz BT operasyonel maliyetini kesmeyi mı arıyor?
* Şirketiniz, bulut varlıklarını (SaaS uygulamaları, altyapı) güvenli hale getirmeye çalışıyor mu?
* Şirketiniz modernleştirin mi?
  * Kullanıcılarınızın farklı türdeki trafiğe farklı kaynaklara erişmesine izin vermek için kullanıcılarınız daha mobil mi ve DMZ için özel durumlar oluşturmasını mi planlıyor?
  * Şirketinizde bu modern kullanıcılara yayımlanması gereken eski uygulamalar var, ancak yeniden yazma kolay değil mi?
  * Şirketinizin tüm bu görevleri gerçekleştirmesi ve aynı zamanda denetimi altına getirmek ister misiniz?
* Şirketiniz, Microsoft 'un Azure Güvenlik uzmanlığı 'nın tesisinden yararlanan yeni araçlar sunarak kullanıcıların kimliklerini güvenli hale getirmek ve riski azaltmak ister misiniz?
* Şirketiniz şirket içinde yer alan "harici" hesapların kurtulacağı ve bunları şirket içi ortamınızda artık devre dışı bir tehdit olmayan buluta taşıyadeniyor mu?

## <a name="analyze-on-premises-identity-infrastructure"></a>Şirket içi kimlik altyapısını çözümle
Artık şirket iş gereksinimleriyle ilgili bir fikriniz olduğuna göre, şirket içi kimlik altyapınızı değerlendirmeniz gerekir. Bu değerlendirme, geçerli kimlik çözümünüzü bulut kimlik yönetimi sistemine bütünleştirmek için teknik gereksinimlerin tanımlanması açısından önemlidir. Aşağıdaki soruları cevaplamadığınızdan emin olun:

* Şirketiniz şirket içinde hangi kimlik doğrulama ve yetkilendirme çözümünü kullanıyor? 
* Şirketiniz Şu anda şirket içi eşitleme hizmetlerine sahip mi?
* Şirketiniz herhangi bir üçüncü taraf kimlik sağlayıcısı (IDP) kullanıyor mu?

Ayrıca, şirketinizin sahip olabileceği bulut hizmetlerinden de haberdar olmanız gerekir. Ortamınızda geçerli tümleştirmeyi, IaaS veya PaaS modellerini anlamak için bir değerlendirme gerçekleştirme çok önemlidir. Bu değerlendirme sırasında aşağıdaki soruları yanıtlamadığınızdan emin olun:

* Şirketiniz bir bulut hizmeti sağlayıcısıyla tümleştirilmesine sahip mi?
* Yanıt Evet ise, hangi hizmetler kullanılıyor?
* Bu tümleştirme Şu anda üretimde mi yoksa bir pilot mı?

> [!NOTE]
> Cloud Discovery, trafik günlüklerinizi, en fazla 70 risk etmenine göre sıralanmış ve puanlanmış 16.000 bulut uygulaması kataloğuna Microsoft Cloud App Security göre analiz ederek bulut kullanımı, gölge BT ve kuruluşunuzda yer alan risk gölgesi hakkında size sürekli görünürlük sağlar. Başlamak için bkz. [set up Cloud Discovery](/cloud-app-security/set-up-cloud-discovery).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Kimlik Tümleştirme gereksinimlerini değerlendir
Ardından, kimlik Tümleştirme gereksinimlerini değerlendirmeniz gerekir. Bu değerlendirme, kullanıcıların nasıl kimlik doğrulaması yapacağının, kuruluşun sahibinin bulutta nasıl görüneceğine, kuruluşun yetkilendirilmesine nasıl izin vertığınızın ve kullanıcı deneyiminin ne olacağı hakkında teknik gereksinimlerin tanımlanması açısından önemlidir. Aşağıdaki soruları cevaplamadığınızdan emin olun:

* Kuruluşunuz federasyon, standart kimlik doğrulama veya her ikisini de kullanıyor mu?
* Federasyona gereksinimi var mı?  Aşağıdakiler nedeniyle:
  * Kerberos tabanlı SSO
  * Şirketiniz, SAML veya benzer Federasyon özellikleri kullanan şirket içi uygulamalara (yerleşik veya üçüncü taraf) sahiptir.
  * Akıllı kartlar aracılığıyla MFA. RSA SecurID, vb.
  * Aşağıdaki soruları ele alan istemci erişim kuralları:
    1. Microsoft 365 için tüm dış erişimi, istemcinin IP adresine göre engelleyebilir miyim?
    2. Exchange ActiveSync hariç Microsoft 365 tüm dış erişimi engelleyebilir miyim?
    3. Tarayıcı tabanlı uygulamalar haricinde Microsoft 365 tüm dış erişimi engelleyebilir (OWA, SPO)
    4. Belirtilen AD gruplarının üyeleri için Microsoft 365 tüm dış erişimi engelleyebilir miyim
* Güvenlik/Denetim sorunları
* Federal Kimlik doğrulamasında zaten mevcut yatırım
* Kuruluşumuz, buluttaki etki alanı için hangi adı kullanır?
* Kuruluşun özel bir etki alanı mı var?
  1. Bu etki alanı genel ve DNS aracılığıyla kolayca doğrulanabilir mi?
  2. Değilse, AD 'de alternatif bir UPN kaydetmek için kullanılabilecek bir ortak etki alanınız var mı?
* Kullanıcı tanımlayıcıları bulut temsili için tutarlı mi? 
* Kuruluşun bulut hizmetleriyle tümleştirme gerektiren uygulamaları var mı?
* Kuruluşun birden çok etki alanı var ve hepsi standart veya federal kimlik doğrulaması kullanacak mı?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Ortamınızda çalışan uygulamaları değerlendirin
Şirket içi ve Bulut altyapınız hakkında bir fikriniz olduğuna göre, bu ortamlarda çalışan uygulamaları değerlendirmeniz gerekir. Bu değerlendirme, bu uygulamaları bulut kimlik yönetimi sistemiyle bütünleştirmek için teknik gereksinimleri tanımlamak açısından önemlidir. Aşağıdaki soruları cevaplamadığınızdan emin olun:

* Uygulamalarımız nerede canlı?
* Kullanıcıların şirket içi uygulamalara erişimi olacak mı?  Mi? Ya da her ikisi?
* Mevcut uygulama iş yüklerini alıp buluta taşımak için plan var mı?
* Bulut kimlik doğrulamasını kullanacak şirket içinde veya bulutta yer alacak yeni uygulamalar geliştirmeye yönelik planlar var mı?

## <a name="evaluate-user-requirements"></a>Kullanıcı gereksinimlerini değerlendir
Ayrıca, Kullanıcı gereksinimlerini değerlendirmeniz gerekir. Bu değerlendirme, şirket içinde buluta geçiş yaparken ve kullanıcılara yardımcı olmak için gerekli olan adımları tanımlamak açısından önemlidir. Aşağıdaki soruları cevaplamadığınızdan emin olun:

* Kullanıcılar, şirket içi uygulamalara erişsin mi?
* Kullanıcıların buluttaki uygulamalara erişimi olacak mı?
* Kullanıcılar şirket içi ortamlarında genellikle nasıl oturum açsın?
* Kullanıcılar bulutta nasıl oturum açacaktır?

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md) seçeneği, her bir seçeneğin kullanılabilir ve uzmanları/dezavantajları üzerinden alınacaktır.  Bu sorulara yanıt vererek, iş gereksinimlerinize en uygun seçeneği seçersiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Dizin eşitleme gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

