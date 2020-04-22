---
title: Yapılandırılabilir Azure AD belirteç ömürleri
titleSuffix: Microsoft identity platform
description: Azure AD tarafından verilen belirteçler için yaşam sürelerini nasıl ayarlayacağız öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/17/2020
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40
ms.reviewer: hirsin, jlu, annaba
ms.openlocfilehash: f4138c4ae24ae599d4058c9fd06c33b69657fe38
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680064"
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-preview"></a>Azure Etkin Dizin'de yapılandırılabilir belirteç yaşam ömürleri (Önizleme)

Azure Active Directory (Azure AD) tarafından verilen bir belirtecin ömrünü belirtebilirsiniz. Kuruluşunuzdaki tüm uygulamalar, çok kiracılı (çok kuruluşlu) bir uygulama veya belirli bir hizmet sorumlusu için belirteç ömrünü ayarlayabilirsiniz.

> [!IMPORTANT]
> Önizleme sırasında müşterilerden haber aldıktan sonra, Azure AD Koşullu Erişim'de [kimlik doğrulama oturumu yönetimi özelliklerini](https://go.microsoft.com/fwlink/?linkid=2083106) uyguladık. Bu yeni özelliği, işaret leme ömrünü sıklık olarak ayarlayarak yapılandırmak için kullanabilirsiniz. 1 Mayıs 2020'den sonra oturum yapılandırmak ve belirteçleri yenilemek için Yapılandırılabilir Belirteç Kullanım Alanı ilkesini kullanamazsınız. Amortismandan sonra erişim belirteç yaşam sürelerini yapılandırmaya devam edebilirsiniz.

Azure AD'de, bir ilke nesnesi tek tek uygulamalarda veya bir kuruluştaki tüm uygulamalarda uygulanan bir kural kümesini temsil eder. Her ilke türü, atandıkları nesnelere uygulanan bir dizi özellik içeren benzersiz bir yapıya sahiptir.

Bir ilkeyi kuruluşunuz için varsayılan ilke olarak atayabilirsiniz. İlke, daha yüksek önceliğe sahip bir ilke tarafından geçersiz kılındığı sürece, kuruluştaki herhangi bir uygulamaya uygulanır. Ayrıca belirli uygulamalara bir ilke atayabilirsiniz. Öncelik sırası ilke türüne göre değişir.

> [!NOTE]
> Yapılandırılabilir belirteç ömür boyu ilkesi yalnızca SharePoint Online ve OneDrive for Business kaynaklarına erişen mobil ve masaüstü istemcileri için geçerlidir ve web tarayıcısı oturumları için geçerli değildir.
> SharePoint Online ve OneDrive for Business için web tarayıcısı oturumlarının kullanım ömrünü yönetmek için [Koşullu Erişim oturumu yaşam süresi](../conditional-access/howto-conditional-access-session-lifetime.md) özelliğini kullanın. Boşta oturum zaman larını yapılandırma hakkında daha fazla bilgi edinmek için [SharePoint Online bloguna](https://techcommunity.microsoft.com/t5/SharePoint-Blog/Introducing-Idle-Session-Timeout-in-SharePoint-and-OneDrive/ba-p/119208) bakın.

## <a name="token-types"></a>Belirteç türleri

Belirteçleri, erişim belirteçleri, SAML belirteçleri, oturum belirteçleri ve kimlik belirteçleri için belirteç leri ömür boyu ilkeleri ayarlayabilirsiniz.

### <a name="access-tokens"></a>Erişim belirteçleri

İstemciler, korumalı bir kaynağa erişmek için erişim belirteçlerini kullanır. Erişim jetonu yalnızca belirli bir kullanıcı, istemci ve kaynak birleşimi için kullanılabilir. Erişim belirteçleri iptal edilemez ve süresi dolana kadar geçerlidir. Erişim jetonu elde eden kötü niyetli bir aktör, bu jetonu kullanım ömrü boyunca kullanabilir. Bir erişim belirteci nin kullanım ömrünü ayarlamak, sistem performansını iyileştirmek le istemcinin kullanıcının hesabı devre dışı bırakıldıktan sonra erişimi koruduğu süreyi artırmak arasında bir dengedir. Geliştirilmiş sistem performansı, bir istemcinin yeni bir erişim jetonunu elde etmek için kaç kez ihtiyaç duyduğuazaltılarak elde edilir.  Varsayılan değer 1 saattir - 1 saat sonra istemci yeni bir yenileme belirteci ve erişim belirteci elde etmek için (genellikle sessizce) yenileme belirteci kullanmalıdır. 

### <a name="saml-tokens"></a>SAML belirteçleri

SAML belirteçleri birçok web tabanlı SAAS uygulaması tarafından kullanılır ve Azure Active Directory'nin SAML2 protokol bitiş noktası kullanılarak elde edilir. Ayrıca WS-Federation kullanan uygulamalar tarafından da tüketilirler. Belirteç varsayılan ömrü 1 saattir. Uygulamanın bakış açısından, belirteç geçerlilik süresi belirteçteki `<conditions …>` öğenin NotOnOrAfter değeri tarafından belirtilir. Belirteç geçerlilik süresi sona erdikten sonra, istemci nin Tek Oturum Açma (SSO) Oturum belirteci sonucunda etkileşimli oturum açmadan genellikle tatmin olacak yeni bir kimlik doğrulama isteği başlatması gerekir.

NotOnOrAfter değeri bir `AccessTokenLifetime` `TokenLifetimePolicy`parametre kullanılarak değiştirilebilir. Varsa, poliçede yapılandırılan ömür boyu ve beş dakikalık bir saat çarpık faktörüne göre ayarlanır.

Özne onayı NotOnOrAfter `<SubjectConfirmationData>` öğesinde belirtilen Token Lifetime yapılandırması etkilenmez unutmayın. 

### <a name="refresh-tokens"></a>Belirteçleri yenileyin

İstemci, korumalı bir kaynağa erişmek için bir erişim jetonu aldığında, istemci de yenileyen bir belirteç alır. Yenileme belirteci, geçerli erişim belirteci süresi dolduğunda yeni erişim/yenileme belirteci çiftleri elde etmek için kullanılır. Yenileme belirteci, kullanıcı ve istemcinin birleşimine bağlıdır. Yenileme belirteci [herhangi bir zamanda iptal](access-tokens.md#token-revocation)edilebilir ve belirteç her kullanıldığında belirteç geçerliliği denetlenir.  Yeni erişim belirteçleri getirmek için kullanıldığında belirteçleri yenilemek iptal edilmez - ancak yeni bir belirteç alırken eski belirteci güvenli bir şekilde silmek en iyi yöntemdir. 

Bu, yenileme belirteçlerinin ne kadar süre kullanılabileceğini etkilediği için, gizli istemciler ve genel istemciler arasında bir ayrım yapmak önemlidir. Farklı istemci türleri hakkında daha fazla bilgi için [RFC 6749'a](https://tools.ietf.org/html/rfc6749#section-2.1)bakın.

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Gizli istemci yenileme belirteçleri ile belirteç ömürleri
Gizli istemciler, istemci parolalarını (gizli) güvenli bir şekilde depolayabilen uygulamalardır. İsteklerin kötü amaçlı bir aktörden değil, güvenli istemci uygulamasından geldiğini kanıtlayabilirler. Örneğin, bir web uygulaması gizli istemcidir, çünkü bir istemci sırrını web sunucusunda depolayabilir. Açığa çıkmadı. Bu akışlar daha güvenli olduğundan, bu akışlara verilen yenileme `until-revoked`belirteçlerinin varsayılan ömürleri, ilke kullanılarak değiştirilemez ve isteğe bağlı parola sıfırlamalarında iptal edilmez.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Kamu istemciyenileme belirteçleri ile belirteç ömürleri

Ortak istemciler istemci parolayı (gizli) güvenli bir şekilde depolayamaz. Örneğin, bir iOS/Android uygulaması kaynak sahibinden bir sırrı gizleyemez, bu nedenle ortak istemci olarak kabul edilir. Belirli bir dönemden daha eski bir dönemden daha eski kamu istemcilerinden yeni bir erişim/yenileme belirteç çifti edinmelerini önlemek için kaynaklarla ilgili ilkeler ayarlayabilirsiniz. (Bunu yapmak için Token Max Etkin Olmayan`MaxInactiveTime`Zamanı ()'yi yenile özelliğini kullanın. Ayrıca, yenileme belirteçlerinin artık kabul edilmeyen bir dönemi ayarlamak için ilkeleri de kullanabilirsiniz. (Bunu yapmak için Token Max Age'i yenile özelliğini kullanın.) Bir yenileme belirtecinin kullanım ömrünü, ortak istemci uygulaması kullanırken sessizce yeniden kimlik doğrulaması yapmak yerine, kullanıcının kimlik bilgilerini ne zaman ve ne sıklıkta yeniden girmek için gerekli olduğunu denetleyeceğiz.

> [!NOTE]
> Max Age özelliği, tek bir belirteç kullanılabilir zaman uzunluğudur. 

### <a name="id-tokens"></a>Kimlik belirteçleri
Kimlik belirteçleri web sitelerine ve yerel istemcilere aktarılır. Kimlik belirteçleri, kullanıcı hakkında profil bilgileri içerir. Kimlik belirteci, kullanıcı ve istemcinin belirli bir birleşimine bağlıdır. Kimlik belirteçleri, süresi dolana kadar geçerli kabul edilir. Genellikle, bir web uygulaması, kullanıcı için verilen kimlik belirteci nin kullanım ömrüyle uygulamadaki kullanıcının oturum ömrüyle eşleşir. Kimlik belirteci nin kullanım ömrünü, web uygulamasının uygulama oturumunun süresinin ne sıklıkta sona erdiğini ve kullanıcının Azure AD ile (sessizce veya etkileşimli olarak) yeniden kimlik doğrulamasını ne sıklıkta gerektirdiğini denetlemek için bir kimlik belirtecinin ömrünü ayarlayabilirsiniz.

### <a name="single-sign-on-session-tokens"></a>Tek oturum oturum belirteçleri
Bir kullanıcı Azure AD ile kimlik doğruladığında, kullanıcının tarayıcısı ve Azure AD ile tek bir oturum açma oturumu (SSO) oluşturulur. Çerez biçiminde ki SSO belirteci bu oturumu temsil eder. SSO oturum belirteci belirli bir kaynak/istemci uygulamasına bağlı değildir. SSO oturum belirteçleri iptal edilebilir ve geçerlilikleri her kullanıldığında denetlenir.

Azure AD iki tür SSO oturum belirteçleri kullanır: kalıcı ve kalıcı olmayan. Kalıcı oturum belirteçleri tarayıcı tarafından kalıcı tanımlama bilgileri olarak depolanır. Kalıcı olmayan oturum belirteçleri oturum çerezleri olarak depolanır. (Oturum çerezleri tarayıcı kapatıldığında yok edilir.) Genellikle, kalıcı olmayan bir oturum belirteci depolanır. Ancak, kullanıcı kimlik doğrulaması sırasında **beni imzalı tut** onay kutusunu seçtiğinde, kalıcı bir oturum belirteci depolanır.

Kalıcı olmayan oturum belirteçlerinin ömrü 24 saat. Kalıcı belirteçlerin ömrü 180 gündür. Geçerlilik süresi içinde bir SSO oturum belirteci kullanıldığında, geçerlilik süresi belirteç türüne bağlı olarak 24 saat veya 180 gün daha uzatılır. Bir SSO oturum belirteci geçerlilik süresi içinde kullanılmazsa, süresi dolmuş sayılır ve artık kabul edilmez.

İlk oturum belirteci yayımlandıktan sonraki zamanı ayarlamak için bir ilke kullanabilirsiniz ve oturum belirteci artık kabul edilmez. (Bunu yapmak için Oturum Belirteç Max Yaş özelliğini kullanın.) Bir oturum belirtecinin kullanım ömrünü, bir web uygulamasını kullanırken sessizce kimlik doğrulaması olmak yerine, kullanıcının kimlik bilgilerini ne zaman ve ne sıklıkta yeniden girmek için gerekli olduğunu denetlemek için ayarlayabilirsiniz.

### <a name="token-lifetime-policy-properties"></a>Belirteç ömür ilkesi özellikleri
Belirteç yaşam boyu ilkesi, belirteç yaşam alanı kuralları içeren bir ilke nesnesi türüdür. Belirtilen belirteç yaşam sürelerini denetlemek için ilkenin özelliklerini kullanın. İlke ayarlanmazsa, sistem varsayılan yaşam boyu değeri zorlar.

### <a name="configurable-token-lifetime-properties"></a>Yapılandırılabilir belirteç ömür boyu özellikleri
| Özellik | İlke özelliği dizesi | Etkiledi -ğini | Varsayılan | Minimum | Maksimum |
| --- | --- | --- | --- | --- | --- |
| Erişim Belirteç Ömür |AccessTokenÖmür<sup>2</sup> |Erişim belirteçleri, kimlik belirteçleri, SAML2 belirteçleri |1 saat |10 dakika |1 gün |
| Token Max Etkin Olmayan Süreyi Yenile |MaxInactiveTime |Belirteçleri yenileyin |90 gün |10 dakika |90 gün |
| Tek Faktörlü Yenileme Belirteç Max Yaş |MaxageSingleFactor |Belirteçleri yenileyin (tüm kullanıcılar için) |İptal edilene kadar |10 dakika |İptal edilene kadar<sup>1</sup> |
| Çok Faktörlü Yenileme Belirteç Max Yaş |MaxageMultiFactor |Belirteçleri yenileyin (tüm kullanıcılar için) |İptal edilene kadar |10 dakika |İptal edilene kadar<sup>1</sup> |
| Tek Faktörlü Oturum Belirteç Max Yaş |MaxageSessionSingleFactor |Oturum belirteçleri (kalıcı ve kalıcı olmayan) |İptal edilene kadar |10 dakika |İptal edilene kadar<sup>1</sup> |
| Çok Faktörlü Oturum Belirteç Max Yaş |MaxageSessionMultiFactor |Oturum belirteçleri (kalıcı ve kalıcı olmayan) |İptal edilene kadar |10 dakika |İptal edilene kadar<sup>1</sup> |

* <sup>1</sup>365 gün, bu öznitelikler için ayarlanabilecek en yüksek açık uzunluktur.
* <sup>2.000</sup> Microsoft Teams Web istemcisinin çalıştığından emin olmak için AccessTokenLifetime'ı Microsoft Teams için 15 dakikadan fazla tutman önerilir.

### <a name="exceptions"></a>Özel Durumlar
| Özellik | Etkiledi -ğini | Varsayılan |
| --- | --- | --- |
| Token Max Age'i yenile (iptal bilgileri yetersiz olan federe kullanıcılar için verilir<sup>1</sup>) |Belirteçleri yenileyin (iptal bilgileri yetersiz olan federe kullanıcılar için verilir<sup>1</sup>) |12 saat |
| Token Max Inactive Time'ı yenileyin (gizli istemciler için verilir) |Belirteçleri yenileme (gizli istemciler için verilir) |90 gün |
| Token Max Age'i yenile (gizli müşteriler için verilir) |Belirteçleri yenileme (gizli istemciler için verilir) |İptal edilene kadar |

* <sup>1.1.2</sup> İptal bilgileri yetersiz olan federe kullanıcılar arasında "LastPasswordChangeTimestamp" özniteliği eşitlenmemiş kullanıcılar yer almaktadır. AAD, eski bir kimlik bilgisine bağlı belirteçleri (değiştirilen parola gibi) ne zaman iptal edemeyeceğini doğrulayamadığından ve kullanıcının ve ilişkili belirteçlerin hala iyi durumda olduğundan emin olmak için daha sık iade etmeleri gerektiğinden, bu kullanıcılara bu kısa Max Yaş verilir. Bu deneyimi geliştirmek için, kiracı yöneticilerin "LastPasswordChangeTimestamp" özniteliğini eşitlediğinden emin olmaları gerekir (bu, Powershell kullanarak veya AADSync aracılığıyla kullanıcı nesnesi üzerinde ayarlanabilir).

### <a name="policy-evaluation-and-prioritization"></a>İlke değerlendirmesi ve önceliklendirme
Belirli bir uygulamaya, kuruluşunuza ve hizmet ilkelerine bir belirteç yaşam ilkesi oluşturabilir ve sonra atayabilirsiniz. Belirli bir uygulama için birden çok ilke uygulanabilir. Etkili olan belirteç yaşam ilkesi aşağıdaki kuralları izler:

* Bir ilke açıkça hizmet ilkesine atanmışsa, bu uygulanır.
* Hizmet sorumlusuna açıkça hiçbir ilke atanmamışsa, hizmet sorumlusunun ana kuruluşuna açıkça atanmış bir ilke uygulanır.
* Hizmet sorumlusuna veya kuruluşa açıkça hiçbir ilke atanmamışsa, uygulamaya atanan ilke uygulanır.
* Hizmet sorumlusuna, kuruluşa veya uygulama nesnesine ilke atanmamışsa, varsayılan değerler zorlanır. [(Yapılandırılan belirteç ömür boyu özellikleri](#configurable-token-lifetime-properties)tabloya bakın.)

Uygulama nesneleri ve hizmet temel nesneleri arasındaki ilişki hakkında daha fazla bilgi için [Azure Etkin Dizini'ndeki Uygulama ve hizmet temel nesneleri'ne](app-objects-and-service-principals.md)bakın.

Belirteç geçerliliği, belirteç kullanıldığı anda değerlendirilir. Erişilen uygulamada en yüksek önceliğe sahip ilke etkindir.

Burada kullanılan tüm zaman dilimleri C# [TimeSpan](/dotnet/api/system.timespan) nesnesine göre biçimlendirilir - D.HH:MM:SS.  Yani 80 gün ve 30 dakika. `80.00:30:00`  Önde gelen D sıfır ise bırakılabilir, bu `00:90:00`yüzden 90 dakika olacaktır.  

> [!NOTE]
> İşte bir örnek senaryo.
>
> Bir kullanıcı iki web uygulamasına erişmek ister: Web Application A ve Web Application B.
> 
> Faktörler:
> * Her iki web uygulaması da aynı ana kuruluştadır.
> * Sekiz saatlik Oturum Belirteç Max Yaş ile Token Ömür Boyu İlke 1 üst kuruluşun varsayılan olarak ayarlanır.
> * Web Application A düzenli kullanımlı bir web uygulamasıdır ve herhangi bir ilkeyle bağlantılı değildir.
> * Web Application B son derece hassas işlemler için kullanılır. Hizmet prensibi, 30 dakikalık Oturum Token Max Yaşı olan Token Lifetime Policy 2'ye bağlıdır.
>
> Saat 12:00'de, kullanıcı yeni bir tarayıcı oturumu başlatır ve Web Application A'ya erişmeye çalışır. Kullanıcı Azure AD'ye yönlendirilir ve oturum açması istenir. Bu, tarayıcıda oturum belirteci olan bir tanımlama bilgisi oluşturur. Kullanıcı, kullanıcının uygulamaya erişmesine izin veren bir kimlik belirteciyle Web Application A'ya geri yönlendirilir.
>
> Saat 12:15'te, kullanıcı Web Application B'ye erişmeye çalışır. Tarayıcı, oturum çerezini algılayan Azure AD'ye yönlendirir. Web Application B'nin hizmet ilkesi Token Yaşam Boyu İlkesi 2'ye bağlıdır, ancak aynı zamanda varsayılan Token Yaşam Boyu İlkesi 1 ile ana kuruluşun bir parçasıdır. Hizmet ilkelerine bağlı ilkeler kuruluş varsayılan ilkelerinden daha yüksek önceliğe sahip olduğundan, Belirteç Yaşam Boyu İlke 2 etkindir. Oturum belirteci ilk olarak son 30 dakika içinde yayımlandı, bu nedenle geçerli kabul edilir. Kullanıcı, kendilerine erişim sağlayan bir kimlik belirteciyle Web Application B'ye geri yönlendirilir.
>
> Saat 13:00'te, kullanıcı Web Application A'ya erişmeye çalışır. Kullanıcı Azure AD'ye yönlendirilir. Web Application A herhangi bir ilkeyle bağlantılı değildir, ancak varsayılan Token Yaşam Alanı İlkesi 1 olan bir kuruluşta olduğundan, bu ilke etkinolur. Başlangıçta son sekiz saat içinde verilen oturum çerezi algılanır. Kullanıcı sessizce yeni bir kimlik belirteci ile Web Application A'ya geri yönlendirilir. Kullanıcının kimliğini doğrulaması gerekmez.
>
> Hemen ardından, kullanıcı Web Application B'ye erişmeye çalışır. Kullanıcı Azure AD'ye yönlendirilir. Daha önce olduğu gibi, Token Ömür Boyu İlke 2 yürürlüğe girer. Belirteç 30 dakikadan daha önce verildiğinden, kullanıcıdan oturum açma kimlik bilgilerini yeniden girmesi istenir. Yepyeni bir oturum belirteci ve kimlik belirteci verilir. Kullanıcı daha sonra Web Application B'ye erişebilir.
>
>

## <a name="configurable-policy-property-details"></a>Yapılandırılabilir ilke özelliği ayrıntıları
### <a name="access-token-lifetime"></a>Erişim Belirteç Ömür
**Dize:** AccessTokenÖmür

**Etkiler:** Erişim belirteçleri, kimlik belirteçleri, SAML belirteçleri

**Özet:** Bu ilke, bu kaynağın ne kadar süreli erişim ve kimlik belirteçlerinin geçerli kabul edilebildiğini denetler. Access Token Lifetime özelliğinin azaltılması, kötü niyetli bir aktör tarafından uzun bir süre için erişim belirteci veya kimlik belirteci kullanılması riskini azaltır. (Bu belirteçler iptal edilemez.) Belirteçlerin daha sık değiştirilmesi gerektiği için, performansın olumsuz etkilenmesi dengelenir.

### <a name="refresh-token-max-inactive-time"></a>Token Max Etkin Olmayan Süreyi Yenile
**Dize:** MaxInactiveTime

**Etkiler:** Belirteçleri yenileyin

**Özet:** Bu ilke, istemci bu kaynağa erişmeye çalışırken yeni bir erişim/yenileme belirteci çiftini almak için artık kullanmadan önce yenileme belirtecinin ne kadar eski olabileceğini denetler. Yeni bir yenileme belirteci genellikle bir yenileme belirteci kullanıldığında döndürüldİğİnden, istemci belirtilen süre içinde geçerli yenileme belirteci kullanarak herhangi bir kaynağa erişmeye çalışırsa bu ilke erişimi engeller.

Bu ilke, istemcilerinde etkin olmayan kullanıcıları yeni bir yenileme belirteci almak için yeniden kimlik doğrulamaya zorlar.

Token Max Inactive Time özelliği, Tek Faktörlü Token Max Yaş ve Çok Faktörlü Yenileme Token Max Age özelliklerinden daha düşük bir değere ayarlanmalıdır.

### <a name="single-factor-refresh-token-max-age"></a>Tek Faktörlü Yenileme Belirteç Max Yaş
**Dize:** MaxageSingleFactor

**Etkiler:** Belirteçleri yenileyin

**Özet:** Bu ilke, yalnızca tek bir faktör kullanarak başarıyla doğrulandıktan sonra yeni bir erişim/yenileme belirteci çifti almak için kullanıcının yeni bir belirteç ne kadar süre kullanabileceğini denetler. Kullanıcı kimlik doğrulaması yaptıktan ve yeni bir yenileme belirteci aldıktan sonra, kullanıcı belirteç akışını belirtilen süre boyunca kullanabilir. (Geçerli yenileme belirteci iptal edilmedikçe ve etkin olmayan süreden daha uzun süre kullanılmadığı sürece bu durum geçerlidir.) Bu noktada, kullanıcı yeni bir yenileme belirteci almak için yeniden kimlik doğrulaması yapmak zorunda kalır.

Maksimum yaşın azaltılması, kullanıcıları daha sık kimlik doğrulamaya zorlar. Tek faktörlü kimlik doğrulama, çok faktörlü kimlik doğrulamadan daha az güvenli olarak kabul edilir, bu özelliği Çok Faktörlü Yenileme Token Max Age özelliğine eşit veya daha az değere ayarlamanızı öneririz.

### <a name="multi-factor-refresh-token-max-age"></a>Çok Faktörlü Yenileme Belirteç Max Yaş
**Dize:** MaxageMultiFactor

**Etkiler:** Belirteçleri yenileyin

**Özet:** Bu ilke, kullanıcının birden çok etken kullanarak en son kimlik doğrulaması yaptıktan sonra yeni bir erişim/yenileme belirteci çifti almak için yenileme belirteci ne kadar süre kullanabileceğini denetler. Kullanıcı kimlik doğrulaması yaptıktan ve yeni bir yenileme belirteci aldıktan sonra, kullanıcı belirteç akışını belirtilen süre boyunca kullanabilir. (Geçerli yenileme belirteci iptal edilmedikçe ve etkin olmayan süreden daha uzun süre kullanılmadığı sürece bu geçerlidir.) Bu noktada, kullanıcılar yeni bir yenileme belirteci almak için yeniden kimlik doğrulamayapmak zorunda kalırlar.

Maksimum yaşın azaltılması, kullanıcıları daha sık kimlik doğrulamaya zorlar. Tek faktörlü kimlik doğrulama, çok faktörlü kimlik doğrulamadan daha az güvenli olarak kabul edilir, bu özelliği Tek Faktörlü Yenileme Token Max Age özelliğine eşit veya daha büyük bir değere ayarlamanızı öneririz.

### <a name="single-factor-session-token-max-age"></a>Tek Faktörlü Oturum Belirteç Max Yaş
**Dize:** MaxageSessionSingleFactor

**Etkiler:** Oturum belirteçleri (kalıcı ve kalıcı olmayan)

**Özet:** Bu ilke, bir kullanıcının yalnızca tek bir faktör kullanarak başarıyla doğrulandıktan sonra yeni bir kimlik ve oturum belirteci almak için oturum belirteci ne kadar süre kullanabileceğini denetler. Kullanıcı kimlik doğrulaması yaptıktan ve yeni bir oturum belirteci aldıktan sonra, kullanıcı belirtilen süre boyunca oturum belirteç akışını kullanabilir. (Geçerli oturum belirteci iptal edilmedikçe ve süresi dolmamış sayılsa da bu durum geçerlidir.) Belirtilen süreden sonra, kullanıcı yeni bir oturum belirteci almak için yeniden kimlik doğrulaması yapmak zorunda kalır.

Maksimum yaşın azaltılması, kullanıcıları daha sık kimlik doğrulamaya zorlar. Tek faktörlü kimlik doğrulama, çok faktörlü kimlik doğrulamasından daha az güvenli olarak kabul edilir, bu özelliği Çok Faktörlü Oturum Token Max Age özelliğine eşit veya daha az bir değere ayarlamanızı öneririz.

### <a name="multi-factor-session-token-max-age"></a>Çok Faktörlü Oturum Belirteç Max Yaş
**Dize:** MaxageSessionMultiFactor

**Etkiler:** Oturum belirteçleri (kalıcı ve kalıcı olmayan)

**Özet:** Bu ilke, kullanıcının birden çok etken kullanarak en son kimlik doğrulaması yaptıktan sonra yeni bir kimlik ve oturum belirteci almak için oturum belirteci ne kadar süre kullanabileceğini denetler. Kullanıcı kimlik doğrulaması yaptıktan ve yeni bir oturum belirteci aldıktan sonra, kullanıcı belirtilen süre boyunca oturum belirteç akışını kullanabilir. (Geçerli oturum belirteci iptal edilmedikçe ve süresi dolmamış sayılsa da bu durum geçerlidir.) Belirtilen süreden sonra, kullanıcı yeni bir oturum belirteci almak için yeniden kimlik doğrulaması yapmak zorunda kalır.

Maksimum yaşın azaltılması, kullanıcıları daha sık kimlik doğrulamaya zorlar. Tek faktörlü kimlik doğrulama, çok faktörlü kimlik doğrulamasından daha az güvenli olarak kabul edilir, bu özelliği Tek Faktörlü Oturum Token Max Age özelliğine eşit veya daha büyük bir değere ayarlamanızı öneririz.

## <a name="example-token-lifetime-policies"></a>Örnek belirteç yaşam ilkeleri
Uygulamalar, hizmet ilkeleri ve genel kuruluşunuz için belirteç ömürleri oluşturup yönetebildiğinizde Azure AD'de birçok senaryo mümkündür. Bu bölümde, şu lar için yeni kurallar koymanıza yardımcı olabilecek birkaç yaygın ilke senaryosundan geçiyoruz:

* Belirteç Ömrü
* Token Max Inactive Time
* Token Max Yaş

Örneklerde, nasıl öğrenebileceğiniz:

* Kuruluşun varsayılan ilkesini yönetme
* Web oturum açma için bir ilke oluşturma
* Web API çağıran yerel bir uygulama için ilke oluşturma
* Gelişmiş bir ilkeyi yönetme

### <a name="prerequisites"></a>Ön koşullar
Aşağıdaki örneklerde, uygulamalar, hizmet ilkeleri ve genel kuruluşunuz için ilkeler oluşturur, günceller, bağlantı verir ve silebilirsiniz. Azure AD'de yeniyseniz, bu örneklere geçmeden önce [Azure AD kiracısını nasıl alacağınızı](quickstart-create-new-tenant.md) öğrenmenizi öneririz.  

Başlamak için aşağıdaki adımları yapın:

1. En son [Azure AD PowerShell Modülü Genel Önizleme yayınını indirin.](https://www.powershellgallery.com/packages/AzureADPreview)
2. Azure `Connect` AD yönetici hesabınızda oturum açma komutunu çalıştırın. Yeni bir oturum başlatmak her zaman bu komutu çalıştırın.

    ```powershell
    Connect-AzureAD -Confirm
    ```

3. Kuruluşunuzda oluşturulan tüm ilkeleri görmek için aşağıdaki komutu çalıştırın. Aşağıdaki senaryolarda çoğu işlemden sonra bu komutu çalıştırın. Komutu çalıştırmak, ilkelerinizin ** ** 'sini elde edebilirsiniz.

    ```powershell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a>Örnek: Kuruluşun varsayılan ilkesini yönetme
Bu örnekte, kullanıcılarınızın tüm kuruluşunuz genelinde daha az sıklıkta oturum açmalarını sağlayan bir ilke oluşturursunuz. Bunu yapmak için, kuruluşunuz genelinde uygulanan Tek Faktörlü Yenileme Belirteçleri için bir belirteç yaşam boyu ilkesi oluşturun. İlke, kuruluşunuzdaki her uygulamaya ve zaten bir ilke kümesi olmayan her hizmet ilkesine uygulanır.

1. Bir belirteç yaşam ilkesi oluşturun.

    1. Tek Faktörlü Yenileme Belirteci'ni "iptal edilene kadar" olarak ayarlayın. Giriş iptal edilene kadar belirteç süresi dolmaz. Aşağıdaki ilke tanımını oluşturun:

        ```powershell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    1. İlke oluşturmak için aşağıdaki komutu çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    1. Herhangi bir boşluk kaldırmak için aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -id | set-azureadpolicy -Definition @($((Get-AzureADPolicy -id ).Replace(" ","")))
        ```

    1. Yeni ilkenizi görmek ve politikanın **ObjectId'ini**almak için aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. İlkeyi güncelleştirin.

    Bu örnekte belirlediğiniz ilk ilkenin hizmetinizin gerektirdiği kadar katı olmadığına karar verebilirsiniz. Tek Faktörlü Yenileme Belirteci'nizi iki gün içinde sona erecek şekilde ayarlamak için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureADPolicy -Id $policy.Id -DisplayName $policy.DisplayName -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a>Örnek: Web oturum açma için bir ilke oluşturma

Bu örnekte, kullanıcıların web uygulamanızda daha sık kimlik doğrulamasını gerektiren bir ilke oluşturursunuz. Bu ilke, erişim/kimlik belirteçlerinin kullanım ömrünü ve çok faktörlü bir oturumun maksimum yaşını web uygulamanızın hizmet ilkesine göre ayarlar.

1. Bir belirteç yaşam ilkesi oluşturun.

    Bu ilke, web oturumunu açma için erişim/kimlik belirteci ömrünü ve maksimum tek faktörlü oturum belirteç yaşını iki saate ayarlar.

    1. İlkeoluşturmak için şu komutu çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    1. Yeni ilkenizi görmek ve **objectid**ilkesini almak için aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. İlkeyi hizmet müdürünüze atayın. Ayrıca hizmet müdürünün **ObjectId'ini** de almanız gerekir.

    1. Kuruluşunuzun tüm hizmet ilkelerini veya tek bir hizmet müdürünü görmek için [AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) cmdlet'ini kullanın.
        ```powershell
        # Get ID of the service principal
        $sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"
        ```

    1. Hizmet sorumlusunuz olduğunda, aşağıdaki komutu çalıştırın:
        ```powershell
        # Assign policy to a service principal
        Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
        ```

### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Örnek: Web API çağıran yerel bir uygulama için ilke oluşturma
Bu örnekte, kullanıcıların daha az sıklıkta kimlik doğrulaması gerektiren bir ilke oluşturursunuz. İlke ayrıca, kullanıcının yeniden kimlik doğrulaması yapmadan önce kullanıcının etkin olmayan süresini de uzatır. İlke web API'sine uygulanır. Yerel uygulama web API'sini kaynak olarak istediğinde, bu ilke uygulanır.

1. Bir belirteç yaşam ilkesi oluşturun.

    1. Bir web API'sı için katı bir ilke oluşturmak için aşağıdaki komutu çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    1. Yeni ilkenizi görmek için aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. İlkeyi web API'nize atayın. Ayrıca uygulamanızın **ObjectId'ini** de almanız gerekir. Uygulamanızın **ObjectId'ini**bulmak veya [Azure portalını](https://portal.azure.com/)kullanmak için [AzureADApplication](/powershell/module/azuread/get-azureadapplication) cmdlet'ini kullanın.

    Uygulamanızın **ObjectId'ini** alın ve ilkeyi atayın:

    ```powershell
    # Get the application
    $app = Get-AzureADApplication -Filter "DisplayName eq 'Fourth Coffee Web API'"

    # Assign the policy to your web API.
    Add-AzureADApplicationPolicy -Id $app.ObjectId -RefObjectId $policy.Id
    ```

### <a name="example-manage-an-advanced-policy"></a>Örnek: Gelişmiş bir ilkeyi yönetme
Bu örnekte, öncelik sisteminin nasıl çalıştığını öğrenmek için birkaç ilke oluşturursunuz. Ayrıca, birden çok nesneye uygulanan birden çok ilkeyi nasıl yönetacağınızı da öğrenirsiniz.

1. Bir belirteç yaşam ilkesi oluşturun.

    1. Tek Faktörlü Yenileme Belirteci ömrünü 30 güne ayarlayan bir kuruluş varsayılan ilkesi oluşturmak için aşağıdaki komutu çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    1. Yeni ilkenizi görmek için aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. İlkeyi bir hizmet müdürüne atayın.

    Şimdi, tüm kuruluş için geçerli olan bir politikanız var. Bu 30 günlük ilkeyi belirli bir hizmet ilkesi için korumak isteyebilirsiniz, ancak kuruluş varsayılan ilkesini "iptal edilene kadar" üst sınırıyla değiştirebilirsiniz.

    1. Kuruluşunuzun tüm hizmet ilkelerini görmek için [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) cmdlet'ini kullanırsınız.

    1. Hizmet sorumlusunuz olduğunda, aşağıdaki komutu çalıştırın:

        ```powershell
        # Get ID of the service principal
        $sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"

        # Assign policy to a service principal
        Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
        ```

1. `IsOrganizationDefault` Bayrağı false olarak ayarlayın:

    ```powershell
    Set-AzureADPolicy -Id $policy.Id -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

1. Yeni bir kuruluş varsayılan ilkesi oluşturun:

    ```powershell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    Artık hizmet anabilim inanına bağlı özgün ilkeniz vardır ve yeni ilke kuruluş varsayılan ilkesi olarak ayarlanır. Hizmet ilkelerine uygulanan ilkelerin kuruluş varsayılan ilkelerine göre önceliğe sahip olduğunu unutmamak gerekir.

## <a name="cmdlet-reference"></a>Cmdlet başvurusu

### <a name="manage-policies"></a>İlkeleri yönetme

İlkeleri yönetmek için aşağıdaki cmdlets kullanabilirsiniz.

#### <a name="new-azureadpolicy"></a>Yeni AzureAD Politikası

Yeni bir ilke oluşturur.

```powershell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Definition</code> |Tüm ilke kurallarını içeren dizilmiş JSON dizisi. | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |İlke adı dizesi. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |Doğruysa, ilkeyi kuruluşun varsayılan ilkesi olarak ayarlar. Eğer yanlışsa, hiçbir şey yapmaz. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |İlke türü. Belirteç ömürleri için her zaman "TokenLifetimePolicy"yi kullanın. | `-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[İsteğe bağlı] |İlke için alternatif bir kimlik ayarlar. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a>AzureAD İlkesi Al
Tüm Azure REKLAM ilkelerini veya belirli bir ilkeyi alır.

```powershell
Get-AzureADPolicy
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code>[İsteğe bağlı] |İstediğindiğiniz ilkenin **ObjectId (Kimliği).** |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>AzureADPolitikasıUygulamalı Nesne
Bir ilkeyle bağlantılı tüm uygulamaları ve hizmet ilkelerini alır.

```powershell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |İstediğindiğiniz ilkenin **ObjectId (Kimliği).** |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a>Ayar-AzureADPolitikası
Varolan bir ilkeyi güncelleştirir.

```powershell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |İstediğindiğiniz ilkenin **ObjectId (Kimliği).** |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |İlke adı dizesi. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;Definition</code>[İsteğe bağlı] |Tüm ilke kurallarını içeren dizilmiş JSON dizisi. |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;IsOrganizationDefault</code>[İsteğe bağlı] |Doğruysa, ilkeyi kuruluşun varsayılan ilkesi olarak ayarlar. Eğer yanlışsa, hiçbir şey yapmaz. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code>[İsteğe bağlı] |İlke türü. Belirteç ömürleri için her zaman "TokenLifetimePolicy"yi kullanın. |`-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[İsteğe bağlı] |İlke için alternatif bir kimlik ayarlar. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a>AzureAD İlkeyi Kaldır
Belirtilen ilkeyi siler.

```powershell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |İstediğindiğiniz ilkenin **ObjectId (Kimliği).** | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a>Uygulama ilkeleri
Uygulama ilkeleri için aşağıdaki cmdlet'leri kullanabilirsiniz.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Ekle-AzureADApplication Politikası
Belirtilen ilkeyi bir uygulamaya bağlar.

```powershell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** uygulaması. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |İlkenin **ObjectId'i.** | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a>AzureADApplication Politikası
Bir uygulamaya atanan ilkeyi alır.

```powershell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** uygulaması. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Remove-AzureADApplication İlke
Bir uygulamadan bir ilke kaldırır.

```powershell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** uygulaması. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |İlkenin **ObjectId'i.** | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a>Hizmet temel ilkeleri
Hizmet ana ilkeleri için aşağıdaki cmdlet'leri kullanabilirsiniz.

#### <a name="add-azureadserviceprincipalpolicy"></a>Ekle-AzureADServicePrincipalİlke
Belirtilen ilkeyi bir hizmet ilkesine bağlar.

```powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** uygulaması. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |İlkenin **ObjectId'i.** | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>AzureADServicePrincipalPolicy Al
Belirtilen hizmet ilkesine bağlı herhangi bir ilke alır.

```powershell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** uygulaması. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Remove-AzureADServicePrincipalPolicy
İlkeyi belirtilen hizmet ilkesinden kaldırır.

```powershell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectId (Id)** uygulaması. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |İlkenin **ObjectId'i.** | `-PolicyId <ObjectId of Policy>` |
