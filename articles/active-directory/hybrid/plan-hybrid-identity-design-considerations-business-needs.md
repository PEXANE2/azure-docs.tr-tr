---
title: Karma bulut kimlik tasarımı azure için kimlik gereksinimleri | Microsoft Dokümanlar
description: Şirketin, karma kimlik tasarımı gereksinimlerini tanımlamanıza yol açacak iş gereksinimlerini belirleyin.
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
ms.openlocfilehash: 9ecc90e13f49c231d8d3ab0cff1de91443b80f21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65950905"
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için kimlik gereksinimlerini belirleme
Karma kimlik çözümü tasarlamanın ilk adımı, bu çözümden yararlanacak iş organizasyonu için gereksinimleri belirlemektir.  Karma kimlik destekleyici bir rol olarak başlar (kimlik doğrulaması sağlayarak diğer tüm bulut çözümlerini destekler) ve kullanıcılar için yeni iş yüklerinin kilidini açan yeni ve ilginç özellikler sağlamaya devam eder.  Kullanıcılarınız için benimsemek istediğiniz bu iş yükleri veya hizmetler, karma kimlik tasarımı gereksinimlerini belirler.  Bu hizmetlerin ve iş yüklerinin hem şirket içinde hem de bulutta karma kimlikten yararlanması gerekir.  

Şimdi bir gereklilik ve ne şirket gelecek için planlar anlamak için işin bu önemli yönlerini üzerinden gitmek gerekir. Eğer hibrid kimlik tasarımı için uzun vadeli strateji görünürlük yoksa, büyük olasılıkla iş ihtiyaçları büyümek ve değiştirmek gibi çözüm ölçeklenebilir olmayacaktır. Aşağıdaki diyagram, karma kimlik mimarisinin bir örneğini ve kullanıcılar için kilidi açılan iş yüklerini gösterir. Bu, kilidi açılabilir ve sağlam bir melez kimlik stratejisi ile teslim edilebilir tüm yeni olanaklar sadece bir örnektir. 

Karma kimlik mimarisinin ![bir parçası olan bazı bileşenler karma kimlik mimarisi](./media/plan-hybrid-identity-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>İş gereksinimlerini belirleme
Bu şirketler aynı endüstrinin bir parçası olsa bile, her şirketin farklı gereksinimleri olacaktır, gerçek iş gereksinimleri değişebilir. Hala sektörden en iyi uygulamaları kaldıraç, ama sonuçta hibrid kimlik tasarımı için gereksinimleri tanımlamak için yol açacaktır şirketin iş ihtiyaçları. 

İş ihtiyaçlarınızı belirlemek için aşağıdaki soruları yanıtlayanızdan emin olun:

* Şirketiniz BT işletim maliyetini düşürmek mi istiyor?
* Şirketiniz bulut varlıklarını (SaaS uygulamaları, altyapı) güvence altına almak istiyor mu?
* Şirketiniz BT'nizi modernize etmek istiyor mu?
  * Kullanıcılarınız daha mobil ve farklı türde trafiğin farklı kaynaklara erişmesine izin vermek için DMZ'nizde özel durumlar oluşturması için BT'den talep ediyor mu?
  * Şirketinizin bu modern kullanıcılara yayınlanması gereken ancak yeniden yazması kolay olmayan eski uygulamaları var mı?
  * Şirketinizin tüm bu görevleri yerine getirmesi ve aynı anda kontrol altına getirmesi gerekiyor mu?
* Şirketiniz, Microsoft'un Azure güvenlik uzmanlığının uzmanlığından şirket içinde yararlanan yeni araçlar getirerek kullanıcıların kimliklerini güvence altına almak ve riski azaltmak istiyor mu?
* Şirketiniz, şirket içindeki korkunç "harici" hesaplardan kurtulup bunları artık şirket içi ortamınızda hareketsiz bir tehdit oluşturmadıkları buluta taşımaya mı çalışıyor?

## <a name="analyze-on-premises-identity-infrastructure"></a>Şirket içi kimlik altyapılarını analiz edin
Artık şirketinizin iş gereksinimleriyle ilgili bir fikriniz olduğuna göre, şirket içi kimlik altyapınızı değerlendirmeniz gerekir. Bu değerlendirme, mevcut kimlik çözümünüzü bulut kimlik yönetim sistemine entegre etmek için teknik gereksinimleri tanımlamak için önemlidir. Aşağıdaki soruları yanıtladıklısa mutlaka:

* Şirketiniz şirket içinde hangi kimlik doğrulama ve yetkilendirme çözümlerini kullanır? 
* Şirketinizin şu anda şirket içi senkronizasyon hizmetleri var mı?
* Şirketiniz herhangi bir üçüncü taraf kimlik sağlayıcısı (IdP) kullanıyor mu?

Ayrıca şirketinizin sahip olabileceği bulut hizmetlerinden de haberdar olmanız gerekir. Ortamınızdaki SaaS, IaaS veya PaaS modelleri ile mevcut entegrasyonu anlamak için bir değerlendirme yapmak çok önemlidir. Bu değerlendirme sırasında aşağıdaki soruları yanıtladıklısa mutlaka:

* Şirketinizin bir bulut hizmeti sağlayıcısıyla herhangi bir entegrasyonu var mı?
* Evet ise, hangi hizmetler kullanılıyor?
* Bu entegrasyon şu anda üretimde mi yoksa pilot mu?

> [!NOTE]
> Cloud Discovery, microsoft Cloud App Security'nin 70'ten fazla risk faktörüne göre sıralanan ve puanlanan 16.000'den fazla bulut uygulamasından oluşan bulut uygulaması kataloğuna karşı trafik günlüklerinizi analiz eder ve bulut kullanımına, Gölge BT'ye ve Shadow IT'in kuruluşunuza oluşturduğu riskte sürekli görünürlük sağlar. Başlamak için [Cloud Discovery'yi ayarla'yı](/cloud-app-security/set-up-cloud-discovery)görün.
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Kimlik tümleştirme gereksinimlerini değerlendirme
Ardından, kimlik tümleştirme gereksinimlerini değerlendirmeniz gerekir. Bu değerlendirme, kullanıcıların kimlik doğrulamasını, kuruluşun varlığının bulutta nasıl görüneceğini, kuruluşun yetkilendirmeye nasıl izin vereceğini ve kullanıcı deneyiminin ne olacağını belirlemek için önemlidir. Aşağıdaki soruları yanıtladıklısa mutlaka:

* Kuruluşunuz federasyon, standart kimlik doğrulaması veya her ikisini birden kullanacak mı?
* Federasyon bir gereklilik midir?  Çünkü aşağıdaki:
  * Kerberos tabanlı SSO
  * Şirketinizin, SAML veya benzeri federasyon özelliklerini kullanan şirket içi uygulamaları (şirket içi veya üçüncü taraf dahil) vardır.
  * Akıllı Kartlar ile MFA. RSA SecurID, vb.
  * Aşağıdaki soruları ele alan istemci erişim kuralları:
    1. İstemcinin IP adresine bağlı olarak Office 365'e tüm harici erişimi engelleyebilir miyim?
    2. Exchange ActiveSync dışında Office 365'e tüm harici erişimi engelleyebilir miyim?
    3. Tarayıcı tabanlı uygulamalar (OWA, SPO) dışında Office 365'e tüm harici erişimi engelleyebilir miyim?
    4. Belirlenen AD gruplarının üyeleri için Office 365'e tüm harici erişimi engelleyebilir miyim?
* Güvenlik/denetim kaygıları
* Federe kimlik doğrulama zaten mevcut yatırım
* Kuruluşumuz buluttaki etki alanımız için hangi adı kullanacak?
* Kuruluşun özel bir etki alanı var mı?
  1. Bu alan alanı kamuya açık ve kolayca DNS üzerinden doğrulanabilir mi?
  2. Değilse, ad alternatif bir UPN kaydetmek için kullanılabilecek bir kamu malı var mı?
* Kullanıcı tanımlayıcıları bulut gösterimi için tutarlı mı? 
* Kuruluşun bulut hizmetleriyle tümleştirilmesi ni gerektiren uygulamaları var mı?
* Kuruluşun birden çok etki alanı var mı ve hepsi standart veya federe kimlik doğrulaması kullanacak mı?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Ortamınızda çalışan uygulamaları değerlendirme
Artık şirket içi ve bulut altyapınızla ilgili bir fikriniz olduğuna göre, bu ortamlarda çalışan uygulamaları değerlendirmeniz gerekir. Bu değerlendirme, bu uygulamaları bulut kimlik yönetim sistemine entegre etmek için teknik gereksinimleri tanımlamak için önemlidir. Aşağıdaki soruları yanıtladıklısa mutlaka:

* Başvurularımız nerede olacak?
* Kullanıcılar şirket içi uygulamalara erişecek mi?  Bulutun içinde mi? Yoksa ikisi birden mi?
* Varolan uygulama iş yüklerini alıp buluta taşıma planları var mı?
* Şirket içinde veya bulutta bulut kimlik doğrulaması kullanacak yeni uygulamalar geliştirme planları var mı?

## <a name="evaluate-user-requirements"></a>Kullanıcı gereksinimlerini değerlendirme
Ayrıca kullanıcı gereksinimlerini değerlendirmek zorunda. Bu değerlendirme, buluta geçiş sırasında biniş ve kullanıcılara yardımcı olmak için gereken adımları tanımlamak için önemlidir. Aşağıdaki soruları yanıtladıklısa mutlaka:

* Kullanıcılar uygulamalara şirket içinde erişecek mi?
* Kullanıcılar buluttaki uygulamalara erişecek mi?
* Kullanıcılar genellikle şirket içi ortamlarına nasıl giriş yapar?
* Kullanıcılar bulutta nasıl oturum açacak?

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Olay yanıt gereksinimlerini belirleyin,](plan-hybrid-identity-design-considerations-incident-response-requirements.md) her seçeneğin kullanılabilir seçenekleri ve artılarını/eksilerini gözden geçer.  Bu soruları yanıtlayarak, işletme ihtiyaçlarınıza en uygun seçeneği seçecektir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Dizin eşitleme gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

