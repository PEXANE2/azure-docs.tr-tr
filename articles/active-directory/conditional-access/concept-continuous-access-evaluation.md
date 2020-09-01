---
title: Azure AD 'de sürekli erişim değerlendirmesi
description: Azure AD 'de sürekli erişim değerlendirmesiyle Kullanıcı durumundaki değişikliklere daha hızlı yanıt verme
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jlu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39736f0a369064e1a825ba3f6975a01c5e9ecc40
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89147642"
---
# <a name="continuous-access-evaluation"></a>Sürekli erişim değerlendirmesi

Belirteç süre sonu ve yenileme, sektördeki standart bir mekanizmadır. Outlook gibi bir istemci uygulaması Exchange Online gibi bir hizmete bağlanırsa, API istekleri OAuth 2,0 erişim belirteçleri kullanılarak yetkilendirilir. Varsayılan olarak, bu erişim belirteçleri bir saat için geçerlidir, süreleri dolduğunda istemci Azure AD 'ye yeniden yönlendirilir ve bunları yeniler. Bu yenileme süresi, Kullanıcı erişimi için ilkeleri yeniden değerlendirmeye yönelik bir fırsat sağlar. Örneğin: koşullu erişim ilkesi nedeniyle belirteci yenilememe veya Kullanıcı dizinde devre dışı bırakıldığı için bu işlemi seçemeyebilirsiniz. 

Müşteriler, ağ konumu veya kimlik bilgisi hırsızlığı gibi ilke ve bu değişiklik ile ilgili zorlanmaları gibi durumlar arasındaki gecikmekle ilgili sorunları ifade edebilir. Azaltılmış belirteç yaşam sürelerinin "bleko nesnesi" yaklaşımına sahip denedik, ancak riskleri ortadan kaldırmadan kullanıcı deneyimlerini ve güvenilirliğini düşürebilecekleri tespit ettik.

İlke ihlallerine veya güvenlik sorunlarına yönelik zamanında yanıt, Azure AD gibi belirteç verenler ve Exchange Online gibi bağlı olan taraf arasında "konuşma" gerektirir. Bu iki yönlü konuşma bize iki önemli özellik sunar. Bağlı olan taraf, bir istemci gibi yeni bir konumdan geldiği ve belirteç verenine söyleme işlemleri fark edebilir. Ayrıca, izin verilen tarafa, hesap uzlaşması, disablement veya diğer endişeler nedeniyle belirli bir kullanıcı için belirteçleri kullanmayı durdurmasına söylemek için bir yol sağlar. Bu konuşmaya yönelik mekanizma, sürekli erişim değerlendirmesi (CAE). Amaç gerçek zamanlı olacak şekilde Yanıtlandı, ancak bazı durumlarda, olay yayma süresi nedeniyle 15 dakikaya varan gecikme süresi gözlemlenebilir.

Sürekli erişim değerlendirmesinin ilk uygulamasını Exchange, takımlar ve SharePoint Online 'a odaklanır. 

### <a name="key-benefits"></a>Önemli avantajlar

- Kullanıcı sonlandırma veya parola değiştirme/sıfırlama: Kullanıcı oturumu iptali, neredeyse gerçek zamanlı olarak zorlanır.
- Ağ konumu değişikliği: koşullu erişim konumu ilkeleri neredeyse gerçek zamanlı olarak uygulanacak.
- Güvenilen ağ dışında bir makineye belirteç verme, koşullu erişim konumu ilkeleriyle engellenebilir.

## <a name="scenarios"></a>Senaryolar 

Sürekli erişim değerlendirmesi, kritik olay değerlendirmesi ve koşullu erişim ilkesi değerlendirmesi oluşturan iki senaryo vardır.

### <a name="critical-event-evaluation"></a>Kritik olay değerlendirmesi

Sürekli erişim değerlendirmesi, Exchange Online, SharePoint Online ve takımlar gibi hizmetleri etkinleştirerek, bu olayların gerçek zamanlı olarak değerlendirilebilecek ve zorlanabilmesi için Azure AD 'deki kritik olaylara abone olmayı sağlar. Kritik olay değerlendirmesi koşullu erişim ilkelerine bağlı değildir, bu nedenle herhangi bir kiracıda kullanılabilir. Aşağıdaki olaylar şu anda değerlendirilir:

- Kullanıcı hesabı silinmiş veya devre dışı
- Kullanıcı parolası değiştirildi veya sıfırlandı
- Multi-Factor Authentication Kullanıcı için etkinleştirildi
- Yönetici, bir kullanıcı için tüm yenileme belirteçlerini açıkça iptal eder
- Azure AD Kimlik Koruması tarafından algılanan yükseltilmiş kullanıcı riski

Bu işlem, kullanıcıların kurumsal SharePoint Online dosyalarına, e-postaya, takvime veya görevlere erişimi kaybettiği senaryoya ve bu kritik olaylardan birini kullandıktan sonra dakika içinde O365 istemci uygulamalarından gelen takımlara izin vermez. 

### <a name="conditional-access-policy-evaluation-preview"></a>Koşullu erişim ilkesi değerlendirmesi (Önizleme)

Exchange ve SharePoint, hizmet içinde değerlendirilebilecek şekilde anahtar koşullu erişim ilkelerini eşitlenebilir.

Bu işlem, kullanıcıların ağ konumu değişikliklerinden hemen sonra O365 istemci uygulamalarından veya SharePoint Online 'dan kurumsal dosyalara, e-postaya, takvime veya görevlere erişimi kaybetmediği senaryoyu sağlar.

> [!NOTE]
> Tüm uygulama ve kaynak sağlayıcısı birleşimi desteklenmez. Aşağıdaki tabloya bakın. Office, Word, Excel ve PowerPoint 'e başvurur

| | Outlook Web | Outlook Win32 | Outlook iOS | Outlook Android | Outlook Mac |
| :--- | :---: | :---: | :---: | :---: | :---: |
| SharePoint Online | Desteklenir | Desteklenir | Desteklenmiyor | Desteklenmiyor | Desteklenir |
| Exchange Online | Desteklenir | Desteklenir | Desteklenir | Desteklenir | Desteklenir |

| | Office Web uygulamaları | Office Win32 uygulamaları | İOS için Office | Android için Office | Mac için Office |
| :--- | :---: | :---: | :---: | :---: | :---: |
| SharePoint Online | Desteklenir | Desteklenir | Desteklenmiyor | Desteklenir | Desteklenir |
| Exchange Online | Desteklenir | Desteklenir | Desteklenmiyor | Desteklenir | Desteklenir |

### <a name="client-side-claim-challenge"></a>İstemci tarafı talebi sınaması

Sürekli erişim değerlendirmesinden önce istemciler, süresi dolmayan sürece erişim belirtecini her zaman kendi önbelleğinden yeniden yürütmeye çalışır. CAE sayesinde, bir kaynak sağlayıcının, zaman aşımına ermese bile bir belirteci reddedebileceği yeni bir durum sunuyoruz. Önbelleğe alınmış belirteçlerin dolmasa bile, istemcilerin önbelleğini atlamasına bildirmek için, belirtecin reddedildiğini ve Azure AD tarafından yeni bir erişim belirtecinin verilmesi gerektiğini göstermek için **talep sınama** adlı bir mekanizma tanıtıldık. CAE, talep sınamasını anlamak için bir istemci güncelleştirmesi gerektirir. Aşağıdaki uygulamaların en son sürümü talep sınamasını destekler:

- Outlook pencereleri
- Outlook iOS
- Outlook Android
- Outlook Mac
- Outlook Web App
- Windows için takımlar (yalnızca takımlar kaynağı için)
- Takımlar iOS (yalnızca takımlar kaynağı için)
- Takımlar Android (yalnızca takımlar kaynağı için)
- Takımlar Mac (yalnızca takımlar kaynağı için)
- Windows için Word/Excel/PowerPoint
- İOS için Word/Excel/PowerPoint
- Android için Word/Excel/PowerPoint
- Mac için Word/Excel/PowerPoint

### <a name="token-lifetime"></a>Belirteç ömrü

Risk ve ilke gerçek zamanlı olarak değerlendirildiğinden, sürekli erişim değerlendirmesi kullanan oturumlara yönelik olan istemciler, var olan statik erişim belirteci ömür ilkeleri yerine CAE 'yi kullanır, bu da yapılandırılabilir belirteç yaşam süresi ilkesi, CAE duyarlı oturumlara yönelik olan CAE özellikli istemciler için artık kabul edilmez.

Belirteç ömrü, CAE oturumlarında 28 saate kadar uzun süreli olacak şekilde artmıştır. İptal etme işlemi, yalnızca rastgele bir süre değil, kritik olaylar ve ilke değerlendirmesi tarafından çalıştırılır. Bu değişiklik, güvenlik duruşunu etkilemeden uygulamaların kararlılığını artırır. 

CAE özellikli istemciler kullanmıyorsanız, erişim belirteci yaşam süresini [yapılandırılabilir belirteç ömrü (CTL)](../develop/active-directory-configurable-token-lifetimes.md) önizleme özelliği ile yapılandırmadığınız takdirde varsayılan erişim belirteci yaşam süresi 1 saat olarak kalır.

## <a name="example-flows"></a>Örnek akışlar

### <a name="user-revocation-event-flow"></a>Kullanıcı iptali olay akışı:

![Kullanıcı iptali olay akışı](./media/concept-continuous-access-evaluation/user-revocation-event-flow.png)

1. CAE özellikli bir istemci, bir kaynak için erişim belirteci isteyen Azure AD 'ye kimlik bilgileri veya yenileme belirteci sunar.
1. İstemciye diğer yapıtlar ile birlikte bir erişim belirteci döndürülür.
1. Yönetici [, Kullanıcı için tüm yenileme belirteçlerini açıkça iptal eder](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0). Azure AD 'den kaynak sağlayıcısına bir iptal olayı gönderilir.
1. Kaynak sağlayıcısına bir erişim belirteci sunulur. Kaynak sağlayıcısı belirtecin geçerliliğini değerlendirir ve Kullanıcı için herhangi bir iptal olayı olup olmadığını denetler. Kaynak sağlayıcı, kaynağa erişim izni vermeye karar vermek için bu bilgileri kullanır.
1. Bu durumda, kaynak sağlayıcısı erişimi reddeder ve istemciye bir 401 + talep sınaması gönderir.
1. CAE özellikli istemci, 401 + talep sınamasını anlamıştır. Önbellekler atlar ve adım 1 ' e döner ve yenileme belirtecini Azure AD 'ye geri talebi ile birlikte gönderir. Daha sonra Azure AD tüm koşulları yeniden değerlendirmenize ve kullanıcıdan bu durumda yeniden kimlik doğrulaması sorması istenir.

### <a name="user-condition-change-flow-public-preview"></a>Kullanıcı koşulu değişiklik akışı (Genel Önizleme):

Aşağıdaki örnekte, koşullu erişim Yöneticisi bir konum tabanlı koşullu erişim ilkesini yalnızca belirli IP aralıklarından erişime izin verecek şekilde yapılandırmıştır:

![Kullanıcı koşulu olay akışı](./media/concept-continuous-access-evaluation/user-condition-change-flow.png)

1. CAE özellikli bir istemci, bir kaynak için erişim belirteci isteyen Azure AD 'ye kimlik bilgileri veya yenileme belirteci sunar.
1. Azure AD, Kullanıcı ve istemcinin koşulları karşılayıp karşılamadığını görmek için tüm koşullu erişim ilkelerini değerlendirir.
1. İstemciye diğer yapıtlar ile birlikte bir erişim belirteci döndürülür.
1. Kullanıcı izin verilen bir IP aralığı dışına geçiyor
1. İstemci, kaynak sağlayıcısına izin verilen bir IP aralığı dışında bir erişim belirteci sunar.
1. Kaynak sağlayıcısı belirtecin geçerliliğini değerlendirir ve Azure AD 'den eşitlenen konum ilkesini denetler.
1. Bu durumda, kaynak sağlayıcısı erişimi reddeder ve izin verilen IP aralığından gelmediği için istemciye bir 401 + talep sınaması gönderir.
1. CAE özellikli istemci, 401 + talep sınamasını anlamıştır. Önbellekler atlar ve adım 1 ' e döner ve yenileme belirtecini Azure AD 'ye geri talebi ile birlikte gönderir. Azure AD, tüm koşulları yeniden değerlendirmeye ayırır ve bu durumda erişimi reddeder.

## <a name="enable-or-disable-cae-preview"></a>CAE 'yi etkinleştirme veya devre dışı bırakma (Önizleme)

1. **Azure Portal** koşullu erişim Yöneticisi, güvenlik yöneticisi veya genel yönetici olarak oturum açın
1. **Azure Active Directory**  >  **güvenliği**  >  **sürekli erişim değerlendirmesi**' ne gidin.
1. **Önizlemeyi etkinleştir**' i seçin.

Bu sayfadan, önizlemeye tabi olacak kullanıcıları ve grupları isteğe bağlı olarak sınırlayabilirsiniz.

![Azure portal CAE önizlemesini etkinleştirme](./media/concept-continuous-access-evaluation/enable-cae-preview.png)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="ip-address-configuration"></a>IP adresi yapılandırması

Kimlik sağlayıcınız ve kaynak sağlayıcılarınız farklı IP adresleri görebilir. Bu uyumsuzluk, kuruluşunuzdaki ağ proxy uygulamaları veya kimlik sağlayıcınız ile kaynak sağlayıcınız arasında yanlış IPv4/IPv6 yapılandırması nedeniyle oluşabilir. Örneğin:

- Kimlik sağlayıcınız istemciden bir IP adresi görür.
- Kaynak sağlayıcınız bir proxy üzerinden geçtikten sonra istemciden farklı bir IP adresi görür.
- Kimlik sağlayıcınızın gördüğü IP adresi ilkede izin verilen bir IP aralığının parçası, ancak kaynak sağlayıcıdan gelen IP adresi değil.

Bu senaryo, sınırlı döngüden kaçınmak için ortamınızda varsa, Azure AD bir saatlik CAE belirteci oluşturacak ve istemci konumu değişikliğini zorlamayacaktır. Bu durumda bile, istemci konum değişikliği olaylarının yanı sıra [diğer olayları](#critical-event-evaluation) değerlendirdiğimiz için, güvenlik geleneksel bir saat belirteçleriyle karşılaştırıldığında geliştirilmiştir.

> [!IMPORTANT]
> Sürekli erişim değerlendirmesi için konumları yapılandırırken yalnızca [IP tabanlı koşullu erişim konumu koşulunu](../conditional-access/location-condition.md)kullanın. Ülke konum koşullarını veya Azure Multi-Factor Authentication hizmet ayarları sayfasında bulunan güvenilir IP 'ler özelliğini kullanmayın.

### <a name="office-and-web-account-manager-settings"></a>Office ve web hesabı Yöneticisi ayarları

| Office güncelleştirme kanalı | Disableadadlatopwamoverride | DisableAADWAM |
| --- | --- | --- |
| Yarı yıllık kurumsal kanal | Etkin veya 1 olarak ayarlanırsa, CAE desteklenmez. | Etkin veya 1 olarak ayarlanırsa, CAE desteklenmez. |
| Geçerli kanal <br> veya <br> Aylık kurumsal kanal | CAE, ayarından bağımsız olarak desteklenir | CAE, ayarından bağımsız olarak desteklenir |

Office Update kanallarının bir açıklaması için bkz. [Microsoft 365 uygulamaları için güncelleştirme kanallarına genel bakış](https://docs.microsoft.com/deployoffice/overview-update-channels). Kuruluşların web hesabı Yöneticisi 'Ni (WAM) devre dışı bırakmadığından önerilir.

### <a name="policy-change-timing"></a>İlke değiştirme zamanlaması

Azure AD ve kaynak sağlayıcıları arasındaki çoğaltma gecikmesi nedeniyle, yöneticiler tarafından yapılan ilke değişikliklerinin Exchange Online için geçerli olması 2 saate kadar sürebilir.

Örnek: yönetici, bir IP adresi aralığının 11:00 ' de e-postaya erişmesini engellemek için bir ilke ekler. daha önce bu IP aralığından gelen bir Kullanıcı, 1:00 PM 'e kadar e-postaya erişmeye devam edebilir.

### <a name="coauthoring-in-office-apps"></a>Office uygulamalarında birlikte yazma

Aynı belge üzerinde aynı anda birden çok kullanıcı işbirliği yaptığı zaman, kullanıcının belgeye erişimi, Kullanıcı iptali veya ilke değişiklik olayları temelinde CAE tarafından hemen iptal edilmeyebilir. Bu durumda, Kullanıcı, belgeyi kapatıp, Word, Excel veya PowerPoint veya 10 saatlik bir süre sonra erişimi kaybeder.

Bu süreyi azaltmak için bir SharePoint Yöneticisi isteğe bağlı olarak SharePoint Online ve OneDrive Iş 'te depolanan belgeler için birlikte bulunan en yüksek yaşam süresini, [SharePoint Online 'da bir ağ konumu ilkesini yapılandırarak](/sharepoint/control-access-based-on-network-location)değiştirebilir. Bu yapılandırma değiştirildikten sonra, birlikte yazma oturumlarının maksimum ömrü 15 dakikaya düşürülür ve "set-SPOTenant – ıpaddresswactokenlifetime" SharePoint Online PowerShell komutu kullanılarak daha fazla ayarlanabilir

### <a name="enable-after-a-user-is-disabled"></a>Kullanıcı devre dışı bırakıldıktan sonra etkinleştir

Devre dışı bırakıldıktan sonra bir kullanıcıyı etkinleştirir. Hesap etkinleştirilmeden önce bir gecikme süresi olacaktır. SPO ve takımlar 15 dakikalık gecikmeye sahip olacaktır. Gecikme, EXO için 35-40 dakikadır.

## <a name="faqs"></a>SSS

### <a name="how-will-cae-work-with-sign-in-frequency"></a>Oturum açma sıklığıyla nasıl çalışacaksınız?

Oturum açma sıklığı CAE ile veya olmadan kabul edilecek.

## <a name="next-steps"></a>Sonraki adımlar

[Sürekli erişim değerlendirmesi duyurusu](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933)
