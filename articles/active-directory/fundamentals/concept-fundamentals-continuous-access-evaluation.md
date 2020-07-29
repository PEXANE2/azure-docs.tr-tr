---
title: Azure AD 'de sürekli erişim değerlendirmesi
description: Azure AD 'de sürekli erişim değerlendirmesiyle Kullanıcı durumundaki değişikliklere daha hızlı yanıt verme
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jlu
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0cb402741163c657b3e7961eb5a4f9c8e18dafd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84673029"
---
# <a name="continuous-access-evaluation"></a>Sürekli erişim değerlendirmesi

Azure Active Directory (Azure AD) ve Office 365 gibi Microsoft Hizmetleri, birlikte çalışabilirliği en üst düzeye çıkarmak için açık standartları ve protokolleri kullanır. En kritik olanlarından biri açık KIMLIK Connect (OıDC). Outlook gibi bir istemci uygulaması Exchange Online gibi bir hizmete bağlanırsa, API istekleri OAuth 2,0 erişim belirteçleri kullanılarak yetkilendirilir. Varsayılan olarak, bu erişim belirteçleri bir saat için geçerlidir. Bu süre dolduğunda, istemci Azure AD 'ye yeniden yönlendirilir ve bunları yeniler. Bu Ayrıca, Kullanıcı erişimi için ilkeleri yeniden değerlendirmeye yönelik bir fırsat sağlar. koşullu erişim ilkesi veya Kullanıcı dizinde devre dışı bırakıldığı için belirteci yenilememe seçebiliriz. 

Belirteç süre sonu ve yenileme, sektördeki standart bir mekanizmadır. Bu şekilde, müşteriler Kullanıcı için risk koşullarından ne zaman değişiklik olduğu (örneğin, şirket ofisinin yerel Kafeterye geçme ya da siyah Pazar üzerinde bulunan Kullanıcı kimlik bilgileri) ve bu değişiklik ile ilgili ilkelerin zorlanmasında olduğu gibi geciktikleri hakkında kaygıları ifade etti. Azaltılmış belirteç yaşam sürelerinin "bleko nesnesi" yaklaşımına sahip denedik, ancak riskleri ortadan kaldırmadan kullanıcı deneyimlerini ve güvenilirliğini düşürebilecekleri tespit ettik.

İlke ihlallerine veya güvenlik sorunlarına yönelik zamanında yanıt, Azure AD gibi belirteç verenler ve Exchange Online gibi bağlı olan taraf arasında "konuşma" gerektirir. Bu iki yönlü konuşma bize iki önemli özellik sunar. Bağlı olan taraf, bir istemci gibi yeni bir konumdan geldiği ve belirteç verenine söyleme işlemleri fark edebilir. Ayrıca, izin verilen tarafa, hesap uzlaşması, disablement veya diğer endişeler nedeniyle belirli bir kullanıcı için belirteçleri kullanmayı durdurmasına söylemek için bir yol sağlar. Bu konuşmaya yönelik mekanizma, sürekli erişim değerlendirmesi (CAE).

Microsoft, OpenID Foundation 'daki [paylaşılan sinyaller ve olaylar](https://openid.net/wg/sse/) çalışma grubunun parçası olarak sürekli erişim değerlendirme Protokolü (caep) girişiminden erken bir katılımcı oldu. Kimlik sağlayıcıları ve bağlı olan taraflar, erişimi yeniden Yetkilendir veya sonlandırmak için çalışma grubu tarafından tanımlanan güvenlik olayları ve sinyallerden yararlanabilir. Bu heyecan verici bir çalışmadır ve birçok platformda ve uygulamada güvenliği iyileştirir.

Güvenlik avantajları çok harika olduğundan, Microsoft 'a özgü bir başlangıç uygulamasını, standart gövdeler içinde devam eden çalışmalarımıza paralel olarak kullanıma sunuyoruz. Bu sürekli erişim değerlendirmesi (CAE) yeteneklerini Microsoft hizmetleri genelinde dağıtmaya çalışırken, çok fazla öğrendik ve bu bilgileri standartlar topluluğuyla paylaştık. Dağıtım konusundaki deneyimimizin daha iyi bir sektör standardı bilgilendirilmesine yardımcı olabilir ve bu standart bir kez, tüm katılım hizmetlerinin avantajına olanak tanımak için bu standardı uygulamaya kararlıdır.

## <a name="how-does-cae-work-in-microsoft-services"></a>Microsoft hizmetlerinde CAE nasıl çalışır?

Exchange ve ekiplere sürekli erişim değerlendirmesi için ilk uygulamamıza odaklanıyoruz. Daha sonra diğer Microsoft hizmetlerine yönelik desteği genişletmemiz dileğiyle. Yalnızca koşullu erişim ilkeleri olmayan kiracılar için sürekli erişim değerlendirmesini etkinleştirmeye başlayacağız. CAE 'nin bu aşamasından dersleri, sürekli KASEETIMIZI bilgilendirmek için kullanacağız.

## <a name="service-side-requirements"></a>Hizmet tarafı gereksinimleri

Sürekli erişim değerlendirmesi, hizmetler (kaynak sağlayıcıları) tarafından Azure AD 'deki kritik olaylara abone olunarak, bu olayların gerçek zamanlı olarak değerlendirilebilecek ve zorlanabilmesi için uygulanır. Aşağıdaki olaylar bu ilk Kae piyasaya sürülmeden zorlanır:

- Kullanıcı hesabı silinmiş veya devre dışı
- Kullanıcı parolası değiştirildi veya sıfırlandı
- MFA Kullanıcı için etkin
- Yönetici, bir kullanıcı için tüm yenileme belirteçlerini açıkça iptal eder
- Azure AD Kimlik Koruması tarafından algılanan yükseltilmiş kullanıcı riski

Gelecekte konum ve cihaz durumu değişiklikleri gibi olaylar da dahil olmak üzere daha fazla olay eklemeyi umuyoruz. **Hedefimiz zorlamasının anında olması, bazı durumlarda olay yayma süresi nedeniyle 15 dakikalık bir gecikme süresine neden olabilir**. 

## <a name="client-side-claim-challenge"></a>İstemci tarafı talebi sınaması

Sürekli erişim değerlendirmesinden önce istemciler, süresi dolmayan sürece erişim belirtecini her zaman kendi önbelleğinden yeniden yürütmeye çalışır. CAE sayesinde, bir kaynak sağlayıcının, zaman aşımına ermese bile bir belirteci reddedebileceği yeni bir durum sunuyoruz. Önbelleğe alınmış belirteçlerin dolmasa bile, istemcilerin önbelleklerini atlamasına bildirmek için **talep sınaması**adlı bir mekanizma tanıtılmaktadır. CAE, talep sınamasını anlamak için bir istemci güncelleştirmesi gerektirir. Aşağıdaki uygulamaların en son sürümü talep sınamasını destekler:

- Windows için Outlook 
- İOS için Outlook 
- Android için Outlook 
- Mac için Outlook 
- Windows için takımlar
- İOS için takımlar 
- Android için takımlar 
- Mac için takımlar 

## <a name="token-lifetime"></a>Belirteç Ömrü

Risk ve ilke gerçek zamanlı olarak değerlendirildiğinden, sürekli erişim değerlendirmesi kullanan oturumlara yönelik olan istemciler, var olan statik erişim belirteci ömür ilkeleri yerine CAE 'yi kullanır, bu da yapılandırılabilir belirteç yaşam süresi ilkesi, CAE duyarlı oturumlara yönelik olan CAE özellikli istemciler için artık kabul edilmez.

Erişim belirteci ömrünü, CAE oturumlarında 24 saate artıracağız. İptal etme işlemi, önemli olaylar ve ilke değerlendirmesi tarafından, rastgele bir zaman diliminde çalıştırılır. Bu değişiklik güvenlik durunuzu etkilemeden uygulamalarınızın kararlılığını artırır. 

## <a name="example-flows"></a>Örnek akışlar

### <a name="user-revocation-event-flow"></a>Kullanıcı iptali olay akışı:

![Kullanıcı iptali olay akışı](./media/concept-fundamentals-continuous-access-evaluation/user-revocation-event-flow.png)

1. CAE özellikli bir istemci, bazı kaynaklar için erişim belirteci isteyen AAD 'ye kimlik bilgileri veya yenileme belirteci sunar.
1. İstemciye diğer yapıtlar ile birlikte bir erişim belirteci döndürülür.
1. Yönetici [, Kullanıcı için tüm yenileme belirteçlerini açıkça iptal eder](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0). Azure AD 'den kaynak sağlayıcısına bir iptal olayı gönderilir.
1. Kaynak sağlayıcısına bir erişim belirteci sunulur. Kaynak sağlayıcısı belirtecin geçerliliğini değerlendirir ve Kullanıcı için herhangi bir iptal olayı olup olmadığını denetler. Kaynak sağlayıcı, kaynağa erişim izni vermeye karar vermek için bu bilgileri kullanır.
1. Bu durumda, kaynak sağlayıcısı erişimi reddeder ve istemciye bir 401 + talep sınaması gönderir
1. CAE özellikli istemci, 401 + talep sınamasını anlamıştır. Önbellekler atlar ve adım 1 ' e döner ve yenileme belirtecini Azure AD 'ye geri talebi ile birlikte gönderir. Daha sonra Azure AD tüm koşulları yeniden değerlendirmenize ve kullanıcıdan bu durumda yeniden kimlik doğrulaması sorması istenir.

## <a name="faqs"></a>SSS

### <a name="what-is-the-lifetime-of-my-access-token"></a>Erişim Belirteçlerimin yaşam süresi nedir?

CAE özellikli istemciler kullanmıyorsanız, erişim belirteci yaşam süresini [yapılandırılabilir belirteç ömrü (CTL)](../develop/active-directory-configurable-token-lifetimes.md) önizleme özelliği ile yapılandırmadığınız takdirde varsayılan erişim belirteci yaşam süresi 1 saat olur.

CAE uyumlu oturumlar için anlaşacak CAE özellikli istemciler kullanıyorsanız, erişim belirteci ömrü için CTL ayarlarınızın üzerine yazılır ve erişim belirteci ömrü 24 saat olur.

### <a name="how-quick-is-enforcement"></a>Ne kadar hızlı zorlama?

Hedefimiz zorlamasının anında olması, bazı durumlarda olay yayma süresi nedeniyle 15 dakikalık bir gecikme süresine neden olabilir.

### <a name="how-will-cae-work-with-sign-in-frequency"></a>Oturum açma sıklığıyla nasıl çalışacaksınız?

Oturum açma sıklığı CAE ile veya olmadan kabul edilecek.

## <a name="next-steps"></a>Sonraki adımlar

[Sürekli erişim değerlendirmesi duyurusu](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933)
