---
title: Yapılandırılabilir belirteç yaşam süreleri
titleSuffix: Microsoft identity platform
description: Microsoft Identity platform tarafından verilen erişim, SAML ve KIMLIK belirteçleri için yaşam sürelerini ayarlamayı öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 02/01/2021
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40, content-perf, FY21Q1, contperf-fy21q1
ms.reviewer: hirsin, jlu, annaba
ms.openlocfilehash: 1bd60a60aa5f6fffcc459f0e14d550740e48496d
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99428158"
---
# <a name="configurable-token-lifetimes-in-the-microsoft-identity-platform-preview"></a>Microsoft Identity platformunda yapılandırılabilir belirteç yaşam süreleri (Önizleme)

Microsoft Identity platform tarafından verilen erişim, KIMLIK veya SAML belirtecinin ömrünü belirtebilirsiniz. Kuruluşunuzdaki tüm uygulamalar, çok kiracılı (çok kuruluşlu) bir uygulama veya belirli bir hizmet sorumlusu için belirteç ömrünü ayarlayabilirsiniz. Ancak, şu anda [yönetilen kimlik hizmeti sorumluları](../managed-identities-azure-resources/overview.md)için belirteç yaşam sürelerini yapılandırmayı desteklemiyoruz.

Azure AD 'de bir ilke nesnesi, tek tek uygulamalarda veya bir kuruluştaki tüm uygulamalarda zorlanan bir kurallar kümesini temsil eder. Her ilke türünün, atandıkları nesnelere uygulanan bir özellikler kümesi ile benzersiz bir yapısı vardır.

Bir ilkeyi kuruluşunuz için varsayılan ilke olarak belirleyebilirsiniz. İlke, daha yüksek önceliğe sahip bir ilke tarafından geçersiz kılınmadığı sürece, kuruluştaki herhangi bir uygulamaya uygulanır. Ayrıca, belirli uygulamalara bir ilke atayabilirsiniz. Öncelik sırası, ilke türüne göre farklılık gösterir.

Örnekler için, [belirteç yaşam sürelerinin nasıl yapılandırılacağı hakkında örnekleri](configure-token-lifetimes.md)okuyun.

> [!NOTE]
> Yapılandırılabilir belirteç ömür ilkesi yalnızca SharePoint Online ve OneDrive Iş kaynaklarına erişen mobil ve Masaüstü istemcilerine uygulanır ve Web tarayıcı oturumları için geçerli değildir.
> SharePoint Online ve OneDrive Iş için Web tarayıcısı oturumlarının ömrünü yönetmek üzere [koşullu erişim oturumu ömrü](../conditional-access/howto-conditional-access-session-lifetime.md) özelliğini kullanın. Boş oturum zaman aşımlarını yapılandırma hakkında daha fazla bilgi edinmek için [SharePoint Online bloguna](https://techcommunity.microsoft.com/t5/SharePoint-Blog/Introducing-Idle-Session-Timeout-in-SharePoint-and-OneDrive/ba-p/119208) bakın.

## <a name="license-requirements"></a>Lisans gereksinimleri

Bu özelliğin kullanılması için Azure AD Premium P1 lisansı gerekir. Gereksinimleriniz için doğru lisans bulmak için bkz. [ücretsiz ve Premium sürümlerin genel kullanıma sunulan özelliklerini karşılaştırma](https://azure.microsoft.com/pricing/details/active-directory/).

[Microsoft 365 iş lisanslarına](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) sahip müşterilerin koşullu erişim özelliklerine de erişimi vardır.

## <a name="token-lifetime-policies-for-access-saml-and-id-tokens"></a>Erişim, SAML ve KIMLIK belirteçleri için belirteç ömür ilkeleri

Erişim belirteçleri, SAML belirteçleri ve KIMLIK belirteçleri için belirteç ömür ilkeleri ayarlayabilirsiniz. 

### <a name="access-tokens"></a>Erişim belirteçleri

İstemciler korumalı bir kaynağa erişmek için erişim belirteçlerini kullanır. Erişim belirteci, yalnızca belirli bir Kullanıcı, istemci ve kaynak birleşimi için kullanılabilir. Erişim belirteçleri iptal edilemez ve süresi sona ermeden geçerli olur. Erişim belirteci elde eden kötü niyetli bir aktör bunu yaşam süresinin kapsamı için kullanabilir. Erişim belirtecinin kullanım süresini ayarlamak, sistem performansını artırma ve kullanıcının hesabı devre dışı bırakıldıktan sonra istemcinin erişimi koruduğunu geçen süreyi artırma arasında bir denge. Gelişmiş sistem performansı, bir istemcinin yeni bir erişim belirteci almak için ihtiyaç duymasının kaç kez daha fazla azaltılmasıyla elde edilir.  Varsayılan değer 1 saattir-1 saat sonra, istemcinin yenileme belirtecini kullanması (genellikle sessizce) yeni bir yenileme belirteci ve erişim belirteci almaları gerekir.

### <a name="saml-tokens"></a>SAML belirteçleri

SAML belirteçleri, Web tabanlı birçok SaaS uygulaması tarafından kullanılır ve Azure Active Directory SAML2 protokol uç noktası kullanılarak elde edilir. Bunlar, WS-Federation kullanan uygulamalar tarafından da kullanılır. Belirtecin varsayılan yaşam süresi 1 saattir. Bir uygulamanın perspektifinden, belirtecin geçerlilik süresi belirteçteki öğenin NotOnOrAfter değeri ile belirtilir `<conditions …>` . Belirtecin geçerlilik süresi sona erdikten sonra, istemcinin yeni bir kimlik doğrulama isteği başlatması gerekir. Bu, genellikle çoklu oturum açma (SSO) oturum belirtecinin bir sonucu olarak etkileşimli oturum açmadan memnun olur.

NotOnOrAfter değeri, `AccessTokenLifetime` içindeki parametresi kullanılarak değiştirilebilir `TokenLifetimePolicy` . Varsa, ilkede yapılandırılan yaşam süresine ayarlanır ve beş dakikalık bir saat eğriltme faktörü olur.

Öğesinde belirtilen konu onayı NotOnOrAfter, `<SubjectConfirmationData>` belirteç ömür yapılandırmasından etkilenmez. 

### <a name="id-tokens"></a>Kimlik belirteçleri

KIMLIK belirteçleri Web sitelerine ve yerel istemcilere geçirilir. KIMLIK belirteçleri bir kullanıcıyla ilgili profil bilgilerini içerir. KIMLIK belirteci, belirli bir Kullanıcı ve istemci birleşimine bağlanır. KIMLIK belirteçleri, süresi doluncaya kadar geçerli kabul edilir. Genellikle, bir Web uygulaması, kullanıcının uygulamadaki oturum ömrünü Kullanıcı için verilen KIMLIK belirtecinin ömrü boyunca eşleştirir. Web uygulamasının uygulama oturumunun ne sıklıkta dolacağını ve kullanıcının Microsoft Identity platformu ile yeniden kimlik doğrulaması gerektirdiğini (sessizce veya etkileşimli olarak) denetlemek için bir KIMLIK belirtecinin ömrünü ayarlayabilirsiniz.

### <a name="token-lifetime-policy-properties"></a>Belirteç ömür ilkesi özellikleri

Belirteç ömür ilkesi, belirteç ömrü kurallarını içeren bir ilke nesnesi türüdür. Bu ilke, bu kaynak için ne kadar erişim, SAML ve KIMLIK belirteçleri geçerli kabul edileceğini denetler. İlke ayarlanmamışsa, sistem varsayılan yaşam süresi değerini uygular. 

Erişim belirteci yaşam süresi özelliğinin azaltılması, bir erişim belirtecinin veya kötü amaçlı aktör tarafından uzun bir süre için kullanılan KIMLIK belirtecinin riskini azaltır. (Bu belirteçler iptal edilemez.) Bu konuda, belirteçlerin daha sık değiştirilmeleri gerektiğinden, performansın olumsuz bir şekilde etkilenmesi önemlidir.

Bir örnek için bkz. [Web oturumu için Ilke oluşturma](configure-token-lifetimes.md#create-a-policy-for-web-sign-in).

| Özellik | İlke Özellik dizesi | Ekranlarını | Varsayılan | Minimum | Maksimum |
| --- | --- | --- | --- | --- | --- |
| Erişim belirteci ömrü |AccessTokenLifetime |Erişim belirteçleri, KIMLIK belirteçleri, SAML2 belirteçleri |1 saat |10 dakika |1 gün |

> [!NOTE]
> Microsoft ekipleri web istemcisinin çalıştığından emin olmak için, Microsoft ekipleri için AccessTokenLifetime 'ın 15 dakikadan fazla tutulması önerilir.

## <a name="token-lifetime-policies-for-refresh-tokens-and-session-tokens"></a>Belirteçleri ve oturum belirteçlerini yenileme için belirteç ömür ilkeleri

Yenileme belirteçleri ve oturum belirteçleri için belirteç ömür ilkeleri ayarlayamazsınız.

> [!IMPORTANT]
> 30 Ocak 2021 itibariyle yenileme ve oturum belirteci yaşam sürelerini yapılandıramazsınız. Azure Active Directory var olan ilkelerde artık yenileme ve oturum belirteci yapılandırması yok.  Mevcut belirteçlerin süre dolduktan sonra verilen yeni belirteçler [varsayılan yapılandırma](#configurable-token-lifetime-properties-after-the-retirement)olarak ayarlanır. Yenileme ve oturum belirteci yapılandırması kullanımdan kaldırıldıktan sonra erişimi, SAML ve KIMLIK belirteci yaşam sürelerini yapılandırmaya devam edebilirsiniz.
>
> Mevcut belirtecin ömrü değiştirilmeyecektir. Süreleri dolduktan sonra, varsayılan değere göre yeni bir belirteç verilir.
>
> Bir kullanıcının yeniden oturum açması istenmeden önce geçen süreyi tanımlamaya devam etmeniz gerekiyorsa, koşullu erişimde oturum açma sıklığını yapılandırın. Koşullu erişim hakkında daha fazla bilgi edinmek için [koşullu erişimle kimlik doğrulama oturumu yönetimini yapılandırma](../conditional-access/howto-conditional-access-session-lifetime.md)makalesini okuyun.

:::image type="content" source="./media/active-directory-configurable-token-lifetimes/roadmap.svg" alt-text="Kullanımdan kaldırma bilgileri":::

### <a name="refresh-tokens"></a>Belirteçleri Yenile

İstemci korunan bir kaynağa erişmek için bir erişim belirteci aldığında, istemci Ayrıca bir yenileme belirteci alır. Yenileme belirteci, geçerli erişim belirtecinin süresi dolmuşsa yeni erişim/yenileme belirteci çiftleri almak için kullanılır. Yenileme belirteci, Kullanıcı ve istemci birleşimine bağlanır. Yenileme belirteci [herhangi bir zamanda iptal](access-tokens.md#token-revocation)edilebilir ve belirteç her kullanıldığında belirtecin geçerliliği denetlenir.  Yeni erişim belirteçleri getirmek için kullanıldığında yenileme belirteçleri iptal edilmez; bununla birlikte, yeni bir belirteç alırken eski belirteci güvenli bir şekilde silmek için en iyi uygulamadır.

Gizli istemcilerle ortak istemciler arasında ayrım yapmak önemlidir, çünkü yenileme belirteçlerinin ne kadar süreyle kullanılabileceğini etkiler. Farklı istemci türleri hakkında daha fazla bilgi için bkz. [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Gizli istemci yenileme belirteçleriyle belirteç yaşam süreleri
Gizli istemciler, bir istemci parolasını (gizli) güvenli bir şekilde depolayabilen uygulamalardır. İsteklerin kötü niyetli bir aktörden değil, güvenli istemci uygulamasından geldiğini kanıtlayabilirler. Örneğin, bir Web uygulaması Web sunucusunda bir istemci gizli dizisi depolayabildiğinden gizli bir istemcdir. Gösterilmez. Bu akışlar daha güvenli olduğundan, bu akışlara verilen yenileme belirteçlerinin varsayılan yaşam süreleri, `until-revoked` ilke kullanılarak değiştirilemez ve gönüllü parola sıfırlamaları üzerinde iptal edilmez.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Ortak istemci yenileme belirteçleri ile belirteç yaşam süreleri

Ortak istemciler, bir istemci parolasını (gizli) güvenli bir şekilde depolayaamaz. Örneğin, bir iOS/Android uygulaması, kaynak sahibinden bir parolayı gizli hale getirebilir, bu yüzden ortak istemci olarak kabul edilir. Belirli bir süreden daha eski olan ortak istemcilerden gelen yenileme belirteçlerinin, yeni bir erişim/yenileme belirteci çifti almasını engellemek için kaynaklar üzerinde ilkeler ayarlayabilirsiniz. Bunu yapmak için, [yenileme belirteci en fazla etkin olmayan süre özelliğini](#refresh-token-max-inactive-time) ( `MaxInactiveTime` ) kullanın. Ayrıca, yenileme belirteçlerinin daha fazla kabul edilmeden önce bir süre ayarlamak için ilkeleri kullanabilirsiniz. Bunu yapmak için, [Tek Etmenli yenileme belirteci maksimum yaşı](#single-factor-session-token-max-age) veya [Multi-Factor Session Token Max Age](#multi-factor-refresh-token-max-age) özelliğini kullanın. Bir yenileme belirtecinin kullanım süresini, bir genel istemci uygulaması kullanılırken sessizce yeniden kimlik doğrulaması yapmak yerine, kullanıcının kimlik bilgilerini ne zaman ve ne sıklıkta yeniden girmesi gerektiğini denetlemek için ayarlayabilirsiniz.

Maksimum yaş özelliği, tek bir belirtecin kullanılabileceği sürenin uzunluğudur. 

### <a name="single-sign-on-session-tokens"></a>Çoklu oturum açma oturum belirteçleri
Bir Kullanıcı Microsoft Identity platformu ile kimlik doğruladığında, kullanıcının tarayıcısı ve Microsoft Identity platformu ile çoklu oturum açma oturumu (SSO) oluşturulur. Bir tanımlama bilgisi biçimindeki SSO belirteci bu oturumu temsil eder. SSO oturum belirteci belirli bir kaynak/istemci uygulamasına bağlanmamış. SSO oturum belirteçleri iptal edilebilir ve bunların geçerlilik süresi her kullanıldığında denetlenir.

Microsoft Identity platformu iki tür SSO oturum belirteci kullanır: kalıcı ve kalıcı olmayan. Kalıcı oturum belirteçleri, tarayıcı tarafından kalıcı tanımlama bilgileri olarak depolanır. Kalıcı olmayan oturum belirteçleri oturum tanımlama bilgileri olarak depolanır. (Tarayıcı kapalıyken oturum tanımlama bilgileri yok edilir.) Genellikle kalıcı olmayan bir oturum belirteci depolanır. Ancak Kullanıcı kimlik doğrulaması sırasında Oturumumu **açık tut** onay kutusunu seçtiğinde kalıcı bir oturum belirteci depolanır.

Kalıcı olmayan oturum belirteçlerinin 24 saat ömrü vardır. Kalıcı belirteçlerde 90 gün ömrü vardır. Geçerlilik süresi içinde SSO oturumu belirteci kullanıldığında, belirteç türüne bağlı olarak, geçerlilik süresi bir 24 saat veya 90 gün daha genişletilir. Bir SSO oturum belirteci geçerlilik süresi içinde kullanılmıyorsa, süresi dolmuş olarak kabul edilir ve artık kabul edilmez.

Oturum belirtecinin artık kabul edilmeden önce ilk oturum belirtecinin verilme zamanından sonraki süreyi ayarlamak için bir ilke kullanabilirsiniz. (Bunu yapmak için, oturum belirteci en yüksek yaş özelliğini kullanın.) Bir Web uygulaması kullanılırken sessizce kimlik doğrulamasından geçmek yerine, bir kullanıcının kimlik bilgilerini ne zaman ve ne sıklıkta yeniden girmesi gerektiğini denetlemek için bir oturum belirtecinin ömrünü ayarlayabilirsiniz.

### <a name="refresh-and-session-token-lifetime-policy-properties"></a>Yenileme ve oturum belirteci ömür ilkesi özellikleri
Belirteç ömür ilkesi, belirteç ömrü kurallarını içeren bir ilke nesnesi türüdür. Belirtilen belirteç ömrünü denetlemek için ilkenin özelliklerini kullanın. İlke ayarlanmamışsa, sistem varsayılan yaşam süresi değerini uygular.

#### <a name="configurable-token-lifetime-properties"></a>Yapılandırılabilir belirteç ömrü özellikleri
| Özellik | İlke Özellik dizesi | Ekranlarını | Varsayılan | Minimum | Maksimum |
| --- | --- | --- | --- | --- | --- |
| Yenileme belirteci en fazla etkin olmayan süre |Maxınactivetime |Belirteçleri Yenile |90 gün |10 dakika |90 gün |
| Single-Factor yenileme belirtecinin en fazla yaşı |Maxagesinglefaktör |Belirteçleri Yenile (tüm kullanıcılar için) |İptal edilene kadar |10 dakika |Until-iptal edildi<sup>1</sup> |
| Multi-Factor Refresh belirtecinin en fazla yaşı |MaxAgeMultiFactor |Belirteçleri Yenile (tüm kullanıcılar için) | İptal edilene kadar |10 dakika |180 gün<sup>1</sup> |
| Single-Factor oturum belirtecinin maksimum yaşı |Maxagesessionsinglefaktör |Oturum belirteçleri (kalıcı ve kalıcı olmayan) |İptal edilene kadar |10 dakika |Until-iptal edildi<sup>1</sup> |
| Multi-Factor Session belirtecinin en fazla yaşı |MaxAgeSessionMultiFactor |Oturum belirteçleri (kalıcı ve kalıcı olmayan) | İptal edilene kadar |10 dakika | 180 gün<sup>1</sup> |

* <sup>1</sup>365 gün, bu öznitelikler için ayarlanoluşturulabilecek maksimum açık uzunluktadır.

#### <a name="exceptions"></a>Özel durumlar
| Özellik | Ekranlarını | Varsayılan |
| --- | --- | --- |
| Yenileme belirteci maksimum yaşı (yetersiz iptal bilgileri<sup>1</sup>olan Federasyon kullanıcıları için verilir) |Belirteçleri Yenile (yetersiz iptal bilgileri<sup>1</sup>olan Federasyon kullanıcıları için verilir) |12 saat |
| Yenileme belirteci en fazla etkin olmayan süre (gizli istemciler için verilir) |Belirteçleri Yenile (gizli istemciler için verilen) |90 gün |
| Yenileme belirteci en fazla yaş (gizli istemciler için verilen) |Belirteçleri Yenile (gizli istemciler için verilen) |İptal edilene kadar |

* <sup>1</sup> yetersiz iptal bilgisine sahip olan Federasyon kullanıcıları, "LastPasswordChangeTimestamp" özniteliği eşitlenmemiş tüm kullanıcıları içerir. Azure Active Directory eski bir kimlik bilgisine bağlı olan belirteçlerin iptal edileceği zamanı doğrulayamadığından ve Kullanıcı ve ilişkili belirteçlerin hala iyi durumda olduğundan emin olmak için daha sık kontrol etmeleri gereken bu kullanıcılara bu kısa maksimum yaş verilir. Bu deneyimi geliştirmek için, kiracı yöneticilerinin "LastPasswordChangeTimestamp" özniteliğini eşitlediklerinden emin olması gerekir (Bu, PowerShell kullanılarak veya AADSync aracılığıyla Kullanıcı nesnesinde ayarlanabilir).

### <a name="configurable-policy-property-details"></a>Yapılandırılabilir ilke özelliği ayrıntıları

#### <a name="refresh-token-max-inactive-time"></a>Yenileme belirteci en fazla etkin olmayan süre
**Dize:** Maxınactivetime

**Şunları etkiler:** Belirteçleri Yenile

**Özet:** Bu ilke, bir istemcinin bu kaynağa erişmeye çalışırken yeni bir erişim/yenileme belirteci çifti almak için daha önce bir yenileme belirtecinin ne kadar eski olduğunu denetler. Yenileme belirteci kullanılırken genellikle yeni bir yenileme belirteci döndürüldüğünden, istemci belirli bir süre boyunca geçerli yenileme belirtecini kullanarak herhangi bir kaynağa erişmeyi denediğinde Bu ilke erişimi engeller.

Bu ilke, istemcileri üzerinde etkin olmayan kullanıcıları yeni bir yenileme belirteci almak için yeniden kimlik doğrulaması yapmak üzere zorlar.

Yenileme belirteci en fazla etkin olmayan süre özelliği, Single-Factor belirteç en fazla geçerlilik süresi ve çok faktörlü yenileme belirteci en fazla yaş özelliklerinden daha düşük bir değere ayarlanmalıdır.

Bir örnek için bkz. [bir Web API 'si çağıran yerel uygulama için Ilke oluşturma](configure-token-lifetimes.md#create-a-policy-for-a-native-app-that-calls-a-web-api).

#### <a name="single-factor-refresh-token-max-age"></a>Single-Factor yenileme belirtecinin en fazla yaşı
**Dize:** Maxagesinglefaktör

**Şunları etkiler:** Belirteçleri Yenile

**Özet:** Bu ilke, bir kullanıcının en son kimlik doğrulamasından sonra yalnızca tek bir faktör kullanarak başarıyla bir erişim/yenileme belirteci kullanması için yenileme belirtecini ne kadar süreyle kullandığını denetler. Kullanıcı kimlik doğrulamasından geçtikten ve yeni bir yenileme belirteci aldıktan sonra, Kullanıcı belirli bir süre için yenileme belirteci akışını kullanabilir. (Geçerli yenileme belirteci iptal edilmediği ve etkin olmayan süreden uzun süredir kullanılmamış olmadığı sürece bu doğrudur.) Bu noktada, Kullanıcı yeni yenileme belirteci almak için yeniden kimlik doğrulaması yapmak üzere zorlanır.

Maksimum yaşı azaltmak, kullanıcıların daha sık kimlik doğrulamasından geçmesini zorlar. Tek faktörlü kimlik doğrulamasının Multi-Factor Authentication 'dan daha az güvenli olduğu kabul edildiğinden, bu özelliği Multi-Factor Refresh belirtecinin maksimum yaşı özelliğinden eşit veya ondan daha küçük bir değere ayarlamanızı öneririz.

Bir örnek için bkz. [bir Web API 'si çağıran yerel uygulama için Ilke oluşturma](configure-token-lifetimes.md#create-a-policy-for-a-native-app-that-calls-a-web-api).

#### <a name="multi-factor-refresh-token-max-age"></a>Multi-Factor Refresh belirtecinin en fazla yaşı
**Dize:** MaxAgeMultiFactor

**Şunları etkiler:** Belirteçleri Yenile

**Özet:** Bu ilke, bir kullanıcının birden çok faktör kullanarak başarıyla kimlik doğrulamasından sonra yeni erişim/yenileme belirteci eşleştirmesi için yenileme belirtecini ne kadar süreyle kullandığını denetler. Kullanıcı kimlik doğrulamasından geçtikten ve yeni bir yenileme belirteci aldıktan sonra, Kullanıcı belirli bir süre için yenileme belirteci akışını kullanabilir. (Geçerli yenileme belirteci iptal edilmediği ve etkin olmayan süreden uzun süre kullanılmamış olduğu sürece bu doğru olur.) Bu noktada, kullanıcılar yeni yenileme belirteci almak için yeniden kimlik doğrulaması yapmak üzere zorlanır.

Maksimum yaşı azaltmak, kullanıcıların daha sık kimlik doğrulamasından geçmesini zorlar. Tek faktörlü kimlik doğrulamasının Multi-Factor Authentication 'dan daha az güvenli olduğu kabul edildiğinden, bu özelliği Single-Factor yenileme belirtecinin en yüksek yaş özelliğinden eşit veya ondan büyük bir değere ayarlamanızı öneririz.

Bir örnek için bkz. [bir Web API 'si çağıran yerel uygulama için Ilke oluşturma](configure-token-lifetimes.md#create-a-policy-for-a-native-app-that-calls-a-web-api).

#### <a name="single-factor-session-token-max-age"></a>Single-Factor oturum belirtecinin maksimum yaşı
**Dize:** Maxagesessionsinglefaktör

**Şunları etkiler:** Oturum belirteçleri (kalıcı ve kalıcı olmayan)

**Özet:** Bu ilke, bir kullanıcının en son kimlik doğrulamasından geçtikten sonra yalnızca tek bir faktör kullanarak yeni bir KIMLIK ve oturum belirteci almak için oturum belirtecini ne kadar süreyle kullanabileceğinizi denetler. Kullanıcı kimlik doğrulamasından ve yeni bir oturum belirtecini aldıktan sonra, Kullanıcı, belirtilen süre boyunca oturum belirteci akışını kullanabilir. (Geçerli oturum belirteci iptal edilmediği ve süresi dolmamış olduğu sürece bu doğrudur.) Belirtilen süre geçtikten sonra, Kullanıcı yeni bir oturum belirteci almak için yeniden kimlik doğrulaması yapmak üzere zorlanır.

Maksimum yaşı azaltmak, kullanıcıların daha sık kimlik doğrulamasından geçmesini zorlar. Tek faktörlü kimlik doğrulamasının Multi-Factor Authentication 'dan daha az güvenli olduğu kabul edildiğinden, bu özelliği Multi-Factor Session Token Max Age özelliğinden eşit veya ondan küçük bir değere ayarlamanızı öneririz.

Bir örnek için bkz. [Web oturumu için Ilke oluşturma](configure-token-lifetimes.md#create-a-policy-for-web-sign-in).

#### <a name="multi-factor-session-token-max-age"></a>Multi-Factor Session belirtecinin en fazla yaşı
**Dize:** MaxAgeSessionMultiFactor

**Şunları etkiler:** Oturum belirteçleri (kalıcı ve kalıcı olmayan)

**Özet:** Bu ilke, bir kullanıcının birden çok faktör kullanarak başarıyla kimlik doğrulamasından geçtikten sonra yeni bir KIMLIK ve oturum belirteci almak için oturum belirtecini ne kadar süreyle kullanabileceğinizi denetler. Kullanıcı kimlik doğrulamasından ve yeni bir oturum belirtecini aldıktan sonra, Kullanıcı, belirtilen süre boyunca oturum belirteci akışını kullanabilir. (Geçerli oturum belirteci iptal edilmediği ve süresi dolmamış olduğu sürece bu doğrudur.) Belirtilen süre geçtikten sonra, Kullanıcı yeni bir oturum belirteci almak için yeniden kimlik doğrulaması yapmak üzere zorlanır.

Maksimum yaşı azaltmak, kullanıcıların daha sık kimlik doğrulamasından geçmesini zorlar. Tek faktörlü kimlik doğrulaması Multi-Factor Authentication 'dan daha az güvenli kabul edildiğinden, bu özelliği Single-Factor oturum belirteci en yüksek yaş özelliğinden eşit veya ondan daha büyük bir değere ayarlamanızı öneririz.

## <a name="configurable-token-lifetime-properties-after-the-retirement"></a>Kullanımdan kaldırıldıktan sonra yapılandırılabilir belirteç ömrü özellikleri
Yenileme ve oturum belirteci yapılandırması aşağıdaki özelliklerden ve sırasıyla ayarlanan değerleriyle etkilenir. 30 Ocak 2021 tarihinde yenileme ve oturum belirteci yapılandırması kullanımdan kaldırıldıktan sonra, Azure AD yalnızca aşağıda açıklanan varsayılan değerleri kabul eder. Oturum açma sıklığını yönetmek için koşullu erişimi kullanmamaya karar verirseniz yenileme ve oturum belirteçleriniz bu tarih için varsayılan yapılandırmaya ayarlanır ve artık yaşam sürelerini değiştiremeyeceksiniz.  

|Özellik   |İlke Özellik dizesi    |Ekranlarını |Varsayılan |
|----------|-----------|------------|------------|
|Erişim belirteci ömrü |AccessTokenLifetime |Erişim belirteçleri, KIMLIK belirteçleri, SAML2 belirteçleri |1 saat |
|Yenileme belirteci en fazla etkin olmayan süre |Maxınactivetime  |Belirteçleri Yenile |90 gün  |
|Single-Factor yenileme belirtecinin en fazla yaşı  |Maxagesinglefaktör  |Belirteçleri Yenile (tüm kullanıcılar için)  |İptal edilene kadar  |
|Multi-Factor Refresh belirtecinin en fazla yaşı  |MaxAgeMultiFactor  |Belirteçleri Yenile (tüm kullanıcılar için) |İptal edilene kadar  |
|Single-Factor oturum belirtecinin maksimum yaşı  |Maxagesessionsinglefaktör |Oturum belirteçleri (kalıcı ve kalıcı olmayan)  |İptal edilene kadar |
|Multi-Factor Session belirtecinin en fazla yaşı  |MaxAgeSessionMultiFactor  |Oturum belirteçleri (kalıcı ve kalıcı olmayan)  |İptal edilene kadar |

Kullanımdan kaldırılacak ilkeleri bulmak için PowerShell kullanabilirsiniz.  Kuruluşunuzda oluşturulan tüm ilkeleri görmek veya belirli bir ilkeye bağlı olan uygulamaları ve hizmet sorumlularını bulmak için [PowerShell cmdlet 'lerini](configure-token-lifetimes.md#get-started) kullanın.

## <a name="policy-evaluation-and-prioritization"></a>İlke değerlendirmesi ve önceliği belirleme
Belirli bir uygulamaya, kuruluşunuza ve hizmet sorumlularına bir belirteç ömür İlkesi oluşturup bu ilke atayabilirsiniz. Belirli bir uygulama için birden çok ilke uygulanabilir. Geçerli olan belirteç ömrü ilkesi şu kurallara uyar:

* Bir ilke hizmet sorumlusuna açıkça atanmışsa, zorlanır.
* Hizmet sorumlusuna açıkça bir ilke atanmamışsa, hizmet sorumlusunun ana kuruluşa açıkça atanan bir ilke zorlanır.
* Hizmet sorumlusuna veya kuruluşa açıkça bir ilke atanmamışsa, uygulamaya atanan ilke zorlanır.
* Hizmet sorumlusuna, kuruluşa veya uygulama nesnesine bir ilke atanmamışsa, varsayılan değerler zorlanır. ( [Yapılandırılabilir belirteç ömrü özelliklerindeki](#configurable-token-lifetime-properties-after-the-retirement)tabloya bakın.)

Uygulama nesneleri ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için, bkz. [Azure Active Directory uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

Belirtecin geçerliliği, belirtecin kullanıldığı sırada değerlendirilir. Erişildiği uygulama üzerinde en yüksek önceliğe sahip ilke devreye girer.

Burada kullanılan tüm zaman dilimlerini C# [TimeSpan](/dotnet/api/system.timespan) nesnesine göre biçimlendirilir-D. hh: mm: ss.  Bu nedenle 80 gün ve 30 dakika olabilir `80.00:30:00` .  0 olduğunda önde gelen D bırakılabilir, bu nedenle 90 dakika olur `00:90:00` .  

### <a name="example-scenario"></a>Örnek senaryo

Kullanıcı iki Web uygulamasına erişmek istiyor: Web uygulaması A ve Web uygulaması B.

Etmen
* Her iki Web uygulaması da aynı ana kuruluştur.
* Bir oturum belirteci en fazla sekiz saat olan belirteç ömür Ilkesi 1, ana kuruluşun varsayılan değeri olarak ayarlanır.
* Web uygulaması, düzenli olarak kullanılan bir Web uygulamasıdır ve hiçbir ilkeye bağlanmaz.
* Web uygulaması B, son derece duyarlı süreçler için kullanılır. Hizmet sorumlusu, en fazla 30 dakikalık bir oturum belirteci olan belirteç ömür Ilkesi 2 ' ye bağlıdır.

12:00 PM 'de Kullanıcı yeni bir tarayıcı oturumu başlatır ve Web uygulamasına erişmeye çalışır. Kullanıcı Microsoft Identity platformuna yönlendirilir ve oturum açması istenir. Bu, tarayıcıda oturum belirtecine sahip bir tanımlama bilgisi oluşturur. Kullanıcı, kullanıcının uygulamaya erişmesine izin veren bir KIMLIK belirteciyle bir Web uygulamasına yeniden yönlendirilir.

12:15 ' de, Kullanıcı B Web uygulamasına erişmeye çalışır. Tarayıcı, oturum tanımlama bilgisini algılayan Microsoft Identity platformu 'na yeniden yönlendirir. Web uygulaması B 'nin hizmet sorumlusu, belirteç ömür Ilkesi 2 ' ye bağlıdır, ancak aynı zamanda üst kuruluşun bir parçasıdır ve varsayılan belirteç yaşam süresi Ilke 1 ' dir. Belirteç yaşam süresi Ilkesi 2, hizmet sorumlularıyla bağlantılı ilkelerin kuruluş varsayılan ilkelerine kıyasla daha yüksek bir önceliğe sahip olduğu için devreye girer. Oturum belirteci ilk olarak son 30 dakika içinde verildi, bu nedenle geçerli kabul edilir. Kullanıcı, erişim izni veren bir KIMLIK belirteciyle B Web uygulamasına yeniden yönlendirilir.

1:00 ' de, Kullanıcı A Web uygulamasına erişmeyi dener. Kullanıcı Microsoft Identity platformuna yönlendirilir. A Web uygulaması hiçbir ilkeyle bağlantılı değildir, ancak varsayılan belirteç ömür Ilkesi 1 olan bir kuruluşta olduğundan, bu ilke geçerli olur. Son sekiz saat içinde ilk olarak verilen oturum tanımlama bilgisi algılanır. Kullanıcı yeni bir KIMLIK belirteci ile sessizce Web uygulamasına geri yönlendirilir. Kullanıcının kimlik doğrulaması için gerekli değildir.

Daha sonra Kullanıcı, B Web uygulamasına erişmeye çalışır. Kullanıcı Microsoft Identity platformuna yönlendirilir. Daha önce olduğu gibi, belirteç ömür Ilkesi 2 etkili olur. Belirteç 30 dakikadan önce verildiği için kullanıcıdan oturum açma kimlik bilgilerini yeniden girmesi istenir. Yepyeni bir oturum belirteci ve KIMLIK belirteci verilir. Kullanıcı daha sonra B Web uygulamasına erişebilir.

## <a name="cmdlet-reference"></a>Cmdlet başvurusu

[Grafik önizleme modülüne yönelik Azure Active Directory PowerShell](/powershell/module/azuread/?view=azureadps-2.0-preview#service-principals&preserve-view=true&preserve-view=true)içindeki cmdlet 'ler şunlardır.

### <a name="manage-policies"></a>İlkeleri yönetme

İlkeleri yönetmek için aşağıdaki cmdlet 'leri kullanabilirsiniz.

| Cmdlet | Açıklama | 
| --- | --- |
| [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Yeni bir ilke oluşturur. |
| [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Tüm Azure AD ilkelerini veya belirli bir ilkeyi alır. |
| [Get-AzureADPolicyAppliedObject](/powershell/module/azuread/get-azureadpolicyappliedobject?view=azureadps-2.0-preview&preserve-view=true) | Bir ilkeyle bağlantılı tüm uygulamaları ve hizmet sorumlularını alır. |
| [Set-AzureADPolicy](/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Var olan bir ilkeyi güncelleştirir. |
| [Remove-AzureADPolicy](/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Belirtilen ilkeyi siler. |

### <a name="application-policies"></a>Uygulama ilkeleri
Uygulama ilkeleri için aşağıdaki cmdlet 'leri kullanabilirsiniz.</br></br>

| Cmdlet | Açıklama | 
| --- | --- |
| [Add-AzureADApplicationPolicy](/powershell/module/azuread/add-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Belirtilen ilkeyi bir uygulamaya bağlar. |
| [Get-Azureadakural Ilkesi](/powershell/module/azuread/get-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Bir uygulamaya atanan ilkeyi alır. |
| [Remove-AzureADApplicationPolicy](/powershell/module/azuread/remove-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Bir uygulamadan bir ilkeyi kaldırır. |

### <a name="service-principal-policies"></a>Hizmet sorumlusu ilkeleri
Hizmet sorumlusu ilkeleri için aşağıdaki cmdlet 'leri kullanabilirsiniz.

| Cmdlet | Açıklama | 
| --- | --- |
| [Add-Azureadservicesprincipalpolicy](/powershell/module/azuread/add-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Belirtilen ilkeyi bir hizmet sorumlusu ile bağlantılandırır. |
| [Get-Azureadservicesprincipalpolicy](/powershell/module/azuread/get-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Belirtilen hizmet sorumlusuna bağlı herhangi bir ilkeyi alır.|
| [Remove-AzureADServicePrincipalPolicy](/powershell/module/azuread/remove-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | İlkeyi belirtilen hizmet sorumlusu 'ndan kaldırır.|

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için, [belirteç yaşam sürelerinin nasıl yapılandırılacağı hakkında örnekleri](configure-token-lifetimes.md)okuyun.
