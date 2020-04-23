---
title: Azure AD'de sürekli erişim değerlendirmesi
description: Azure AD'de sürekli erişim değerlendirmesiyle kullanıcı durumundaki değişikliklere daha hızlı yanıt verme
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
ms.openlocfilehash: e5b70c11cd6bc24f945b437decf22586cfb97557
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81873309"
---
# <a name="continuous-access-evaluation"></a>Sürekli erişim değerlendirmesi

Azure Etkin Dizin (Azure AD) ve Office 365 gibi Microsoft hizmetleri, birlikte çalışabilirliği en üst düzeye çıkarmak için açık standartlar ve protokoller kullanır. En kritik olanlardan biri Open ID Connect (OIDC) olduğunu. Outlook gibi bir istemci uygulaması Exchange Online gibi bir hizmete bağlandığında, API istekleri OAuth 2.0 erişim belirteçleri kullanılarak yetkilendirilir. Varsayılan olarak, bu erişim belirteçleri bir saat için geçerlidir. Süresi dolduğunda, istemci yenilemek için Azure AD'ye geri yönlendirilir. Bu da kullanıcı erişimi için ilkeleri yeniden değerlendirmek için bir fırsat sağlar – bir Koşullu Erişim ilkesi nedeniyle belirteç yenilemek için seçebilirsiniz, ya da kullanıcı dizinde devre dışı bırakıldı. 

Müşterilerimizden ezici geri bildirimler duyduk: Koşullu Erişim ilkelerini ve kullanıcı durumundaki değişiklikleri yeniden uygulamak için belirteç ömrüne erişim nedeniyle bir saatlik gecikme yeterince iyi değil.

Microsoft, OpenID Foundation'daki [Paylaşılan Sinyaller ve Olaylar](https://openid.net/wg/sse/) çalışma grubunun bir parçası olarak Sürekli Erişim Değerlendirme Protokolü (CAEP) girişiminin ilk katılımcılarından biri olmuştur. Kimlik sağlayıcılar ve güvenen taraflar, erişimi yeniden yetkilendirmek veya sonlandırmak için çalışma grubu tarafından tanımlanan güvenlik olaylarını ve sinyalleri kullanabilecektir. Bu heyecan verici bir iş ve birçok platform ve uygulamalar arasında güvenliği artıracaktır.

Güvenlik avantajları çok büyük olduğundan, standartlar gövdesi içinde devam eden çalışmamıza paralel olarak Microsoft'a özel bir başlangıç uygulaması kullanıma sunuz. Bu sürekli erişim değerlendirme (CAE) özelliklerini Microsoft hizmetlerinde dağıtmak için çalışırken, çok şey öğrendik ve bu bilgileri standartlar topluluğuyla paylaşıyoruz. Dağıtım deneyimimizin daha da iyi bir endüstri standardı hakkında bilgi verilmesine yardımcı olabileceğini ve onaylandıktan sonra bu standardı uygulamaya ve tüm katılımcı hizmetlerin yararlanmasına olanak sağlamaya kararlı olduğunu umuyoruz.

## <a name="how-does-cae-work-in-microsoft-services"></a>CAE Microsoft hizmetlerinde nasıl çalışır?

Exchange ve Teams için sürekli erişim değerlendirmesinin ilk uygulamasına odaklanıyoruz. Gelecekte diğer Microsoft hizmetlerine desteği genişletmeyi umuyoruz. Koşullu Erişim ilkeleri olmayan kiracılar için yalnızca sürekli erişim değerlendirmesini etkinleştirmeye başlayacağız. CAE'nin bu aşamasındaki öğrendiklerimizi, cae'nin devam eden lansmanını bilgilendirmek için kullanacağız.

## <a name="service-side-requirements"></a>Hizmet tarafı gereksinimleri

Hizmetlerin (kaynak sağlayıcılarının) Azure AD'deki kritik etkinliklere abone olmasını sağlayarak sürekli erişim değerlendirmesi yapılır, böylece bu etkinlikler gerçek zamanlı olarak değerlendirilebilir ve uygulanabilir. Aşağıdaki olaylar bu ilk CAE lansmanında uygulanacaktır:

- Kullanıcı Hesabı silinir veya devre dışı bırakılır
- Kullanıcının parolası değiştirilir veya sıfırlanır
- Yönetici, bir kullanıcı için tüm yenileme belirteçlerini açıkça iptal eder
- Azure AD Kimlik Koruması tarafından algılanan yüksek kullanıcı riski

Gelecekte, konum ve aygıt durumu değişiklikleri gibi olaylar da dahil olmak üzere daha fazla etkinlik eklemeyi umuyoruz. **Amacımız uygulamanın anlık olması iken, bazı durumlarda olay yayılma süresi nedeniyle 15 dakikaya kadar**gecikme süresi görülebilir. 

## <a name="client-side-claim-challenge"></a>İstemci tarafı talep sorunu

Sürekli erişim değerlendirmesi nden önce, istemciler süresi dolmadığı sürece erişim belirtecisini önbelleğinden yeniden oynatmaya çalışır. CAE ile, bir kaynak sağlayıcısının süresi dolmamış olsa bile bir belirteci reddedebileceği yeni bir servis talebi sıyoruz. Önbelleğe alınmış belirteçlerin süresi dolmamış olsa bile, istemcileri önbelleklerini atlamaları konusunda bilgilendirmek için, **talep itirazı**adı verilen bir mekanizma sunuyoruz. CAE, talep meydan okumasını anlamak için bir istemci güncelleştirmesi gerektirir. Aşağıdaki aşağıdaki uygulamaların en son sürümü talep meydan okumasını destekler:

- Windows için Outlook 
- Outlook iOS 
- Outlook Android 
- Outlook Mac 
- Windows için Takımlar
- Takımlar iOS 
- Takımlar Android 
- Takımlar Mac 

## <a name="token-lifetime"></a>Belirteç Ömrü

Risk ve politika gerçek zamanlı olarak değerlendirildiği için, sürekli erişim değerlendirmesi bilinçli oturumları müzakere istemcileri mevcut statik erişim belirteç ömür boyu ilkeleri yerine CAE güvenecektir, bu da yapılandırılabilir belirteç ömür boyu ilkesi artık CAE-bilinçli oturumları müzakere CAE yeteneğine sahip istemciler için onur anlamına gelir.

CAE oturumlarında jeton ömrünü 24 saate yükselteceğiz. İptal, rasgele bir zaman dilimi değil, kritik olaylar ve ilke değerlendirmesi tarafından yönlendirilir. Bu değişiklik, güvenlik duruşunuzu etkilemeden uygulamalarınızın kararlılığını artırır. 

## <a name="example-flows"></a>Örnek akışlar

### <a name="user-revocation-event-flow"></a>Kullanıcı iptal olay akışı:

![Kullanıcı iptal olay akışı](./media/concept-fundamentals-continuous-access-evaluation/user-revocation-event-flow.png)

1. CAE özellikli istemci, aad'ye kimlik bilgileri veya bazı kaynaklar için erişim belirteci isteyen bir yenilenme belirteci sunar.
1. Erişim belirteci, diğer yapılarla birlikte istemciye döndürülür.
1. Yönetici, [kullanıcı için tüm yenileme belirteçlerini](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0)açıkça iptal eder. Azure AD'den kaynak sağlayıcısına bir iptal olayı gönderilir.
1. Bir erişim belirteci kaynak sağlayıcısına sunulur. Kaynak sağlayıcısı belirteç geçerliliğini değerlendirir ve kullanıcı için herhangi bir iptal olayı olup olmadığını denetler. Kaynak sağlayıcısı, kaynağa erişim izni vermek için bu bilgileri kullanır.
1. Bu durumda, kaynak sağlayıcı erişimi reddeder ve istemciye 401+ talep itirazı gönderir
1. CAE özellikli istemci 401+ talep meydan okumasını anlar. Önbellekleri atlar ve 1. Azure AD daha sonra tüm koşulları yeniden değerlendirir ve kullanıcıdan bu durumda yeniden kimlik doğrulaması ister.
 
## <a name="faqs"></a>SSS

### <a name="what-is-the-lifetime-of-my-access-token"></a>Erişim Jetonumun ömrü nedir?

CAE özellikli istemciler kullanmıyorsanız, Access Token ömürünüzü [Yapılandırılabilir Token Ömür Boyu (CTL)](../develop/active-directory-configurable-token-lifetimes.md) önizleme özelliğiyle yapılandırmadığınız sürece varsayılan Erişim Belirteci ömrünüz 1 saat tir.

CAE'ye duyarlı oturumları müzakere eden CAE özellikli istemciler kullanıyorsanız, Access Token ömür boyu için CTL ayarlarınız üzerine yazılır ve Access Token yaşam süresi 24 saat olur.

### <a name="how-quick-is-enforcement"></a>Uygulama ne kadar hızlı?

Amacımız uygulamanın anlık olması iken, bazı durumlarda olay yayılma süresi nedeniyle 15 dakikaya kadar gecikme süresi görülebilir.

### <a name="how-will-cae-work-with-sign-in-frequency"></a>CAE Oturum Açma Sıklığı ile nasıl çalışacak?

Oturum Açma Sıklığı, CAE ile veya CAE olmadan onurlandırılacaktır.

## <a name="next-steps"></a>Sonraki adımlar

[Sürekli erişim değerlendirmesinin duyurulabilir](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933)
