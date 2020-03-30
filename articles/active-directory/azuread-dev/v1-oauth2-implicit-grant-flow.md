---
title: Azure AD'deki OAuth2 örtülü hibe akışını anlama | Microsoft Dokümanlar
description: Azure Active Directory'nin OAuth2 örtülü hibe akışını uygulaması ve uygulamanız için doğru olup olmadığı hakkında daha fazla bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: conceptual
ms.workload: identity
ms.date: 08/15/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: eaa3844bfbbef8cb71dbe8691cab894c921ce00a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154517"
---
# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Azure Etkin Dizin (AD)'deki OAuth2 örtülü hibe akışını anlama

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

OAuth2 örtülü hibe OAuth2 belirtimigüvenlik endişeleri en uzun listesi ile hibe olduğu için kötü üne sahiptir. Ve yine de, bu yaklaşım ADAL JS tarafından uygulanan ve biz SPA uygulamaları yazarken tavsiye biridir. Ne verir? Her şey bir denge meselesidir: ve görünüşe göre, örtük hibe bir tarayıcıdan JavaScript üzerinden bir Web API tüketen uygulamalar için takip edebilirsiniz en iyi yaklaşımdır.

## <a name="what-is-the-oauth2-implicit-grant"></a>OAuth2 örtülü hibe nedir?

Quintessential [OAuth2 yetkilendirme kodu hibe](https://tools.ietf.org/html/rfc6749#section-1.3.1) iki ayrı uç noktaları kullanan yetkilendirme hibe. Yetkilendirme bitiş noktası, yetkilendirme koduyla sonuçlanan kullanıcı etkileşimi aşaması için kullanılır. Belirteç bitiş noktası daha sonra istemci tarafından bir erişim belirteci için kod alışverişi için kullanılır ve genellikle de bir yenileme belirteci. Web uygulamalarının, yetkilendirme sunucusunun istemcinin kimliğini doğrulayabilmesi için kendi uygulama kimlik bilgilerini belirteç bitiş noktasına sunması gerekir.

[OAuth2 örtülü hibe](https://tools.ietf.org/html/rfc6749#section-1.3.2) diğer yetki hibe bir çeşididir. Bir istemcinin, belirteç bitiş noktasına başvurmadan veya istemcinin kimliğini doğrulamadan doğrudan yetkilendirme bitiş noktasından [OpenId Connect'i](https://openid.net/specs/openid-connect-core-1_0.html)kullanırken bir erişim jetonu (ve id_token) almasına olanak tanır. Bu varyant, Bir Web tarayıcısında çalışan JavaScript tabanlı uygulamalar için tasarlanmıştır: orijinal OAuth2 belirtiminde, belirteçleri bir URI parçası döndürülür. Bu, belirteç bitlerini istemcideki JavaScript koduiçin kullanılabilir hale getirir, ancak bunların sunucuya yönlendirmelere dahil edilmeyeceğini garanti eder. OAuth2 örtülü hibesinde, yetkilendirme bitiş noktası, daha önce sağlanan bir yeniden yönlendirme URI'si kullanarak belirteçleri doğrudan istemciye verir. Ayrıca, JavaScript uygulamasının belirteç bitiş noktasına başvurması gerekiyorsa gerekli olan çapraz orijinli aramalar için gereksinimleri ortadan kaldırma avantajına sahiptir.

OAuth2 örtük hibe önemli bir özelliği bu tür akışları istemciye yenileme belirteçleri dönmek asla gerçektir. Bir sonraki bölümde, bunun nasıl gerekli olmadığı ve aslında bir güvenlik sorunu olacağı gösterilmektedir.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>OAuth2 örtülü hibe için uygun senaryolar

OAuth2 belirtimi, örtük hibenin kullanıcı aracısı uygulamalarını etkinleştirmek için tasarlandığını beyan eder – yani, JavaScript uygulamaları bir tarayıcı içinde yürütülür. Bu tür uygulamaların belirleyici özelliği, JavaScript kodunun sunucu kaynaklarına (genellikle bir Web API'si) erişmek ve uygulama kullanıcı deneyimini buna göre güncelleştirmek için kullanılmasıdır. Gmail veya Outlook Web Erişimi gibi uygulamaları düşünün: Gelen kutunuzdan bir ileti seçtiğinizde, yeni seçimi görüntülemek için yalnızca ileti görselleştirme paneli değişirken, sayfanın geri kalanı değiştirilmemiş olarak kalır. Bu özellik, her kullanıcı etkileşiminin tam sayfa postback ve yeni sunucu yanıtının tam sayfa olarak işlenmesiyle sonuçlandığı geleneksel yönlendirme tabanlı Web uygulamalarıyla zıttır.

JavaScript tabanlı yaklaşımı aşırıya katan uygulamalara tek sayfalı uygulamalar veya SPA'lar denir. Fikir, bu uygulamaların yalnızca bir başlangıç HTML sayfası ve ilişkili JavaScript hizmet, sonraki tüm etkileşimleri Web API aramaları JavaScript üzerinden gerçekleştirilen tarafından tahrik ediliyor olmasıdır. Ancak, uygulama çoğunlukla postback odaklı ama zaman zaman JS çağrıları gerçekleştirir hibrid yaklaşımlar, nadir değildir - örtük akış kullanımı hakkında tartışma da bu için ilgilidir.

Yönlendirme tabanlı uygulamalar genellikle tanımlama bilgileri aracılığıyla isteklerini güvence altına alır, ancak bu yaklaşım JavaScript uygulamaları için de çalışmaz. Çerezler yalnızca oluşturuldukları etki alanına göre çalışır, Ancak JavaScript çağrıları diğer etki adlarına yönlendirilebilir. Aslında, bu sık sık olacaktır: Microsoft Graph API, Office API, Azure API çağıran uygulamaları düşünün - tüm uygulama nın sunulduğu yerden etki alanı dışında ikamet eden. JavaScript uygulamaları için büyüyen bir eğilim, üçüncü taraf Web API'lerinin iş işlevlerini uygulamak için %100'e güvenerek hiçbir geri uça sahip olmakdeğildir.

Şu anda, Bir Web API'ye yapılan çağrıları korumanın tercih edilen yöntemi, her çağrıya Bir OAuth2 erişim belirtecinin eşlik ettiği OAuth2 taşıyıcı belirteci yaklaşımını kullanmaktır. Web API gelen erişim belirteci inceler ve gerekli kapsamları bulursa, istenen işlem erişim verir. Örtük akış, JavaScript uygulamalarının bir Web API'si için erişim belirteçleri elde etmesi için kullanışlı bir mekanizma sağlayarak çerezlerle ilgili olarak çok sayıda avantaj sunar:

* Jetonlar, çapraz menşeli aramalara gerek kalmadan güvenilir bir şekilde elde edilebilir – jetonların yerlerinin yerinden edilmeyeceğinin iade garantisi olduğu yeniden yönlendirme URI'nin zorunlu kaydı
* JavaScript uygulamaları, hedefledikleri web AP'leri için, etki alanlarında herhangi bir kısıtlama olmadan, gereksinim duydukları kadar erişim belirteçleri elde edebilirsiniz
* Oturum veya yerel depolama gibi HTML5 özellikleri belirteç önbelleğe alma ve ömür boyu yönetimi üzerinde tam kontrol sağlarken, tanımlama bilgileri yönetimi uygulamaya opaktır
* Erişim belirteçleri, site ler arası istek sahteciliği (CSRF) saldırılarına açık değildir

Örtük hibe akışı, çoğunlukla güvenlik nedenleriyle yenileme belirteçleri vermez. Bir yenileme belirteci erişim belirteçleri kadar dar kapsamlı değildir, dolayısıyla dışarı sızdırılmış durumda çok daha fazla hasar veren çok daha fazla güç veren. Örtük akışta, belirteçler URL'de teslim edilir, bu nedenle engelleme riski yetkilendirme kodu hibesinden daha yüksektir.

Ancak, bir JavaScript uygulaması, kullanıcıdan sürekli olarak kimlik bilgilerini sormadan erişim belirteçlerini yenilemek için emrinde başka bir mekanizmaya sahiptir. Uygulama, Azure AD'nin yetkilendirme bitiş noktasına karşı yeni belirteç istekleri gerçekleştirmek için gizli bir iframe kullanabilir: tarayıcının Azure AD etki alanına karşı etkin bir oturumu (okuma: oturum çerezi vardır) olduğu sürece, kimlik doğrulama isteği kullanıcı etkileşimine gerek kalmadan başarılı bir şekilde gerçekleşir.

Bu model, JavaScript uygulamasına erişim belirteçlerini bağımsız olarak yenileme ve hatta yeni bir API için yeni lerini edinme olanağı verir (kullanıcının daha önce onlar için onay vermiş olması koşuluyla). Bu, yenileme belirteci gibi yüksek değerli bir yapıyı edinme, koruma ve koruma yükünü önler. Sessiz yenilemeyi mümkün kılan yapı, Azure AD oturumu çerezi, uygulama dışında yönetilir. Bu yaklaşımın bir diğer avantajı da, kullanıcının Azure AD'de oturum açtığı uygulamaları kullanarak azure AD'den oturum açabiliyor ve tarayıcı sekmelerinden herhangi birinde çalışıyor. Bu, Azure AD oturumu çerezinin silinmesine neden olur ve JavaScript uygulaması oturumu imzalayan kullanıcının belirteçlerini otomatik olarak yenileme yeteneğini kaybeder.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Örtülü hibe uygulamam için uygun mu?

Örtülü hibe diğer hibelere göre daha fazla risk sunar ve dikkat etmeniz gereken alanlar iyi belgelenmiştir (örneğin, [Dolaylı Akış'ta Kaynak Sahibinin Kimliğine Bürünmek için Erişim Belirteci'nin Kötüye Kullanımı][OAuth2-Spec-Implicit-Misuse] ve [OAuth 2.0 Tehdit Modeli ve Güvenlik Hususları).][OAuth2-Threat-Model-And-Security-Implications] Ancak, daha yüksek risk profili büyük ölçüde etkin kodu çalıştıran, uzak bir kaynak tarafından tarayıcıya sunulan uygulamaları etkinleştirmek için olması nedeniyle. Bir SPA mimarisi planlıyorsanız, arka uç bileşenleriniz yoksa veya JavaScript üzerinden Bir Web API'sini çağırmak istiyorsanız, belirteç edinimi için örtülü akışın kullanılması önerilir.

Uygulamanız yerel bir istemciise, örtük akış pek uygun değildir. Azure AD oturum çerezinin yerel bir istemci bağlamında olmaması, uygulamanızı uzun ömürlü bir oturumu sürdürme şeklinden mahrum eder. Bu da, uygulamanızın yeni kaynaklar için erişim belirteçleri alırken kullanıcıdan sürekli olarak istekte bulunduğu anlamına gelir.

Arka uç içeren bir Web uygulaması geliştiriyorsanız ve arka uç kodundan bir API tüketiyorsanız, örtük akış da uygun değildir. Diğer bağışlar size çok daha fazla güç verir. Örneğin, OAuth2 istemci kimlik bilgileri hibesi, kullanıcı delegasyonlarının aksine, uygulamanın kendisine atanan izinleri yansıtan belirteçleri alma olanağı sağlar. Bu, istemcinin, kullanıcı bir oturumda etkin bir şekilde meşgul olmasa bile kaynaklara programlı erişimi koruma yeteneğine sahip olduğu anlamına gelir. Sadece bu değil, ama bu tür hibeler daha yüksek güvenlik garantileri verir. Örneğin, erişim belirteçleri kullanıcı tarayıcısı üzerinden hiçbir zaman geçiş yapmaz, tarayıcı geçmişinde kaydedilme riskini göze alamazlar ve böyle devam eder. İstemci uygulaması, belirteç isteğinde de güçlü kimlik doğrulaması gerçekleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulama tümleştirme işleminde ek derinlik sağlamak için [bir uygulamayı Azure AD ile nasıl tümleştirileştirene][ACOM-How-To-Integrate] bakın.

<!--Image references-->

<!--Reference style links in use-->
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: ../develop/active-directory-how-to-integrate.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
