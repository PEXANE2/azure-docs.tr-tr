---
title: Azure Active Directory B2C'de başvuru - güven çerçeveleri | Microsoft Dokümanlar
description: Azure Active Directory B2C özel ilkeleri ve Kimlik Deneyimi Çerçevesi hakkında bir konu.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ffa25eec9c4b668f428d8e8b5a780a5fe4625a2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188894"
---
# <a name="define-trust-frameworks-with-azure-ad-b2c-identity-experience-framework"></a>Azure AD B2C Kimlik Deneyimi Çerçevesi ile Güven Çerçevelerini Tanımla

Kimlik Deneyimi Çerçevesi'ni kullanan Azure Active Directory B2C (Azure AD B2C) özel ilkeleri kuruluşunuza merkezi bir hizmet sağlar. Bu hizmet, büyük bir ilgi topluluğunda kimlik federasyonunun karmaşıklığını azaltır. Karmaşıklık tek bir güven ilişkisine ve tek bir meta veri alışverişine indirgenir.

Azure AD B2C özel ilkeleri, aşağıdaki soruları yanıtlamanızı sağlamak için Kimlik Deneyimi Çerçevesini kullanır:

- Uyulması gereken yasal, güvenlik, gizlilik ve veri koruma politikaları nelerdir?
- İlgili kişiler ve akredite katılımcı olma süreçleri nelerdir?
- Akredite kimlik bilgi sağlayıcıları ("talep sağlayıcılar" olarak da bilinir) kimlerdir ve ne sunarlar?
- Akredite güvenen taraflar kimlerdir (ve isteğe bağlı olarak, ne gerektirirler)?
- Katılımcılar için teknik "tel üzerinde" birlikte çalışabilirlik gereksinimleri nelerdir?
- Dijital kimlik bilgilerinin alışverişi için uygulanması gereken operasyonel "çalışma zamanı" kuralları nelerdir?

Tüm bu soruları yanıtlamak için, Kimlik Deneyimi Çerçevesi'ni kullanan Azure AD B2C özel ilkeleri Güven Çerçevesi (TF) yapısını kullanır. Bu yapıyı ve ne sağladığını ele alalım.

## <a name="understand-the-trust-framework-and-federation-management-foundation"></a>Güven Çerçevesi ve federasyon yönetimi temelini anlama

Güven Çerçevesi, bir ilgi topluluğundaki katılımcıların uyması gereken kimlik, güvenlik, gizlilik ve veri koruma politikalarının yazılı bir belirtimidir.

Federe kimlik, Internet ölçeğinde son kullanıcı kimlik güvencesi elde etmek için bir temel sağlar. Kimlik yönetimini üçüncü taraflara devreterek, son kullanıcı için tek bir dijital kimlik birden çok güvenilen tarafla yeniden kullanılabilir.

Kimlik güvencesi, kimlik sağlayıcılarının (IDP'ler) ve öznitelik sağlayıcılarının (ATP'ler) belirli güvenlik, gizlilik ve operasyonel politika ve uygulamalara uymasını gerektirir.  Doğrudan denetim gerçekleştiremiyorlarsa, güvenen tarafların (RP'ler) birlikte çalışmayı seçtikleri IDP'ler ve ATP'lerle güven ilişkileri geliştirmeleri gerekir.

Dijital kimlik bilgilerinin tüketici ve sağlayıcılarının sayısı arttıkça, bu güven ilişkilerinin çift akıllıca yönetimine, hatta ağ bağlantısı için gerekli olan teknik meta verilerin çift el ile değişimine devam etmek zordur.  Federasyon merkezleri bu sorunların çözümünde sadece sınırlı bir başarı elde etti.

### <a name="what-a-trust-framework-specification-defines"></a>Güven Çerçevesi belirtimi nin tanımladığı
TF'ler, her bir ilgi alanı topluluğunun belirli bir TF belirtimine tabi olduğu Açık Kimlik Değişimi (OIX) Güven Çerçevesi modelinin temel leridir. Böyle bir TF belirtimi tanımlar:

- **Aşağıdakilerin tanımıyla ilgi çeken toplum için güvenlik ve gizlilik ölçümleri:**
    - Katılımcılar tarafından sunulan/gerekli olan güvence düzeyleri (LOA); örneğin, dijital kimlik bilgilerinin orijinalliği için sıralı bir güven derecelendirme kümesi.
    - Katılımcılar tarafından sunulan/gerekli olan koruma düzeyleri (LOP); örneğin, ilgi çekici topluluktaki katılımcılar tarafından işlenen dijital kimlik bilgilerinin korunması için sıralı bir güven derecelendirmesi kümesi.

- **Katılımcılar tarafından sunulan/gerekli olan dijital kimlik bilgilerinin açıklaması.**

- **Dijital kimlik bilgilerinin üretimi ve tüketimi ve böylece LOA ve LOP'un ölçülmesi için teknik politikalar. Bu yazılı ilkeler genellikle aşağıdaki ilke kategorilerini içerir:**
    - Kimlik kanıtlama ilkeleri, örneğin: *Bir kişinin kimlik bilgileri ne kadar güçlü bir şekilde incelenir?*
    - Örneğin: Güvenlik *ilkeleri, bilgi bütünlüğü ve gizliliği ne kadar güçlü bir şekilde korunmaktadır?*
    - Örneğin: Bir *kullanıcının kişisel tanımlayıcı bilgiler (KIŞISEL Bilgiler) üzerinde ne gibi bir denetimi vardır?*
    - Örneğin: Bir sağlayıcı işlemleri *durdurursa, kişisel bilgilersürekliliği ve korunması nasıl çalışır?*

- **Dijital kimlik bilgilerinin üretimi ve tüketimi için teknik profiller. Bu profiller şunlardır:**
    - Dijital kimlik bilgilerinin belirli bir LOA'da kullanılabildiği kapsam arabirimleri.
    - On-the-wire birlikte çalışabilirlik için teknik gereksinimler.

- **Toplulukkatılımcılarının gerçekleştirebileceği çeşitli rollerin açıklamaları ve bu rolleri yerine getirmek için gerekli nitelikler.**

Bu nedenle, bir TF belirtimi, kimlik bilgilerinin ilgili topluluğun katılımcıları arasında nasıl değiş tokuş ediliş ini yönetir: taraflara, kimlik ve öznitelik sağlayıcılarına güvenmek ve öznitelik doğrulayıcıları.

TF belirtimi, topluluk içinde dijital kimlik bilgilerinin iddiasını ve tüketimini düzenleyen ilgi topluluğunun yönetimi için bir veya birden fazla belgedir. İlgi çekici bir topluluğun üyeleri arasında çevrimiçi işlemler için kullanılan dijital kimliklere güven sağlamak için tasarlanmış belgelenmiş bir politika ve yordamlar kümesidir.

Başka bir deyişle, Bir TF belirtimi bir topluluk için uygun bir federe kimlik ekosistemi oluşturmak için kuralları tanımlar.

Şu anda böyle bir yaklaşımın yararı üzerinde yaygın bir anlaşma var. Güven çerçevesi özelliklerinin doğrulanabilir güvenlik, güvence ve gizlilik özellikleriyle dijital kimlik ekosistemlerinin geliştirilmesini kolaylaştırdığı, yani birden fazla ilgi alanı topluluğunda yeniden kullanılabileceğini hiç şüphe yoktur.

Bu nedenle, Kimlik Deneyimi Çerçevesi'ni kullanan Azure AD B2C özel ilkeleri, birlikte çalışabilirliği kolaylaştırmak için bir TF için veri gösteriminin temeli olarak belirtimi kullanır.

Kimlik Deneyimi Çerçevesi'nden yararlanan Azure AD B2C Özel ilkeler, insan ve makine tarafından okunabilir verilerin bir karışımı olarak Bir TF belirtimini temsil eder. Bu modelin bazı bölümleri (genellikle yönetişime yönelik bölümler) ilgili yordamlarla birlikte yayımlanmış güvenlik ve gizlilik politikası belgelerine referans olarak (varsa) temsil edilir. Diğer bölümler, operasyonel otomasyonu kolaylaştıran yapılandırma meta verilerini ve çalışma zamanı kurallarını ayrıntılı olarak açıklar.

## <a name="understand-trust-framework-policies"></a>Güven Çerçevesi ilkelerini anlayın

Uygulama açısından, TF belirtimi kimlik davranışları ve deneyimleri üzerinde tam denetim sağlayan bir dizi ilkeden oluşur.  Kimlik Deneyimi Çerçevesi'nden yararlanan Azure AD B2C özel ilkeleri, tanımlayabilen ve yapılandırabilen bu tür bildirimsel ilkeler aracılığıyla kendi TF'nizi yazmanızı ve oluşturmanızı sağlar:

- Belge referansı veya TF ile ilgili toplumun federe kimlik ekosistemini tanımlayan referanslar. Bunlar TF belgelerine bağlantılardır. (Önceden tanımlanmış) operasyonel "çalışma zamanı" kuralları veya taleplerin değişimini ve kullanımını otomatikleştiren ve/veya kontrol eden kullanıcı yolculukları. Bu kullanıcı yolculukları bir LOA (ve lop) ile ilişkilidir. Bu nedenle bir ilke, çeşitli LOA'lar (ve LOP'lar) ile kullanıcı yolculuklarına sahip olabilir.

- İlgi alanı ndaki kimlik ve öznitelik sağlayıcıları ve destekledikleri teknik profiller ve bunlarla ilgili LOA/LOP akreditasyonu.

- Öznitelik doğrulayıcıları veya talep sağlayıcıları ile tümleştirme.

- Toplumdaki güvenen partiler (çıkarımla).

- Katılımcılar arasında ağ iletişimi kurmak için meta veriler. Bu meta veriler, teknik profillerle birlikte, bir işlem sırasında, güvenen taraf ve diğer topluluk katılımcıları arasında "kabloüzerinde" birlikte çalışabilirliği sağlamak için kullanılır.

- Varsa protokol dönüştürmesi (örneğin, SAML 2.0, OAuth2, WS-Federation ve OpenID Connect).

- Kimlik doğrulama gereksinimleri.

- Varsa çok faktörlü orkestrasyon.

- Mevcut tüm iddialar için paylaşılan bir şema ve ilgi bir topluluğun katılımcıları için eşlemeler.

- Tüm talep dönüşümleri, bu bağlamda olası veri en aza itimizasyonu ile birlikte, iddiaların değişimi ve kullanımını sürdürmek için.

- Bağlama ve şifreleme.

- İddiaların depolanması.

### <a name="understand-claims"></a>Talepleri anlama

> [!NOTE]
> "Talep" olarak değiştirilebilen tüm olası kimlik bilgileri türlerine topluca atıfta bulunuruz: son kullanıcının kimlik doğrulama kimlik bilgisi, kimlik inceleme, iletişim cihazı, fiziksel konum, kişisel olarak tanımlayıcı öznitelikleri hakkındaki iddialar, ve saire.
>
> "Öznitelikler" yerine "talepler" terimini kullanırız, çünkü çevrimiçi işlemlerde bu veri yapıları, güvenen taraf tarafından doğrudan doğrulanabilecek gerçekler değildir. Bunun yerine, güvenen tarafın son kullanıcının istenen işlemini sağlamak için yeterli güveni geliştirmesi gereken gerçekler hakkındaki iddialar veya iddialardır.
>
> Kimlik Deneyimi Çerçevesi'ni kullanan Azure AD B2C özel ilkeleri, temel protokolün olup olmadığına bakılmaksızın her türlü dijital kimlik bilgisinin tutarlı bir şekilde alışverişini kolaylaştırmak için tasarlandıklarından, "talepler" terimini de kullanıyoruz. kullanıcı kimlik doğrulaması veya öznitelik alma için tanımlanır.  Aynı şekilde, kimlik sağlayıcılarını, öznitelik sağlayıcılarını topluca ifade etmek ve belirli işlevlerini birbirinden ayırmak istemediğimizde doğrulayıcıları atfetmek için "talep sağlayıcılar" terimini kullanırız.

Böylece, kimlik bilgilerinin güvenilen bir taraf, kimlik ve öznitelik sağlayıcıları arasında nasıl değiş tokuş edilir ve öznitelik doğrulayıcıları arasında nasıl değiş tokuş edilir. Güvenen bir tarafın kimlik doğrulaması için hangi kimlik ve öznitelik sağlayıcılarının gerekli olduğunu denetlerler. Bunlar etki alanına özgü bir dil (DSL), yani, devralma ile belirli bir uygulama etki alanı için uzmanlaşmış bir bilgisayar dili olarak kabul edilmelidir, *eğer* ifadeler, çok biçimlilik.

Bu ilkeler, Kimlik Deneyimi Çerçevesi'nden yararlanan Azure AD B2C Özel ilkelerinde TF yapısının makinetarafından okunabilir kısmını oluşturur. Bunlar, talep sağlayıcılarının meta verileri ve teknik profilleri, iddia şema tanımları, talep dönüşüm işlevleri ve operasyonel orkestrasyon ve otomasyonu kolaylaştırmak için doldurulan kullanıcı yolculukları dahil olmak üzere tüm operasyonel ayrıntıları içerir.

Bu belgelerin *yaşayan belgeler* olduğu varsayılır, çünkü içeriklerinin zaman içinde politikalarda beyan edilen aktif katılımcılarla ilgili olarak değişme olasılığı yüksektir. Katılımcı olmanın şart ve koşullarının da değişme potansiyeli vardır.

Federasyon kurulumu ve bakımı, farklı talep sağlayıcılar/doğrulayıcılar (temsil edilen topluluk) politikalar kümesine katıldıkça veya ayrıldıkça, güvenen tarafları sürekli güven ve bağlantı yeniden yapılandırmalarından koruyarak büyük ölçüde basitleştirilir.

Birlikte çalışabilirlik de önemli bir sorundur. Ek talep sağlayıcılar/doğrulayıcılar tümleşik olmalıdır, çünkü güvenen tarafların gerekli tüm protokolleri destekleme olasılığı düşüktür. Azure AD B2C özel ilkeleri, endüstri standardı protokolleri destekleyerek ve tarafların ve öznitelik sağlayıcılarının aynı protokolü desteklemediği zaman istekleri aktarmak için belirli kullanıcı yolculukları uygulayarak bu sorunu çözer.

Kullanıcı yolculukları, protokol profillerini ve güvenen taraf ve diğer katılımcılar arasında "kabloya" birlikte çalışabilirliği sağlamak için kullanılan meta verileri içerir. TF belirtiminin bir parçası olarak yayımlanmış ilkelere uyumu zorlamak için kimlik bilgileri değişimi istek/yanıt iletilerine uygulanan operasyonel çalışma zamanı kuralları da vardır. Kullanıcı yolculukları fikri, müşteri deneyiminin özelleştirilmesinin anahtarıdır. Ayrıca, sistemin protokol düzeyinde nasıl çalıştığına da ışık tutuyor.

Bu temelde, parti uygulamalarına ve portallarına güvenerek, bağlamlarına bağlı olarak, belirli bir ilkenin adını geçen Kimlik Deneyimi Çerçevesi'nden yararlanan ve tam olarak davranış ve bilgi alışverişini sağlayan Azure AD B2C özel ilkelerini uygulayabilir onlar herhangi bir muss, yaygara, ya da risk olmadan istiyorum.
