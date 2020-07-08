---
title: Eski kimlik doğrulamasını engelle-Azure Active Directory
description: Azure AD koşullu erişim kullanarak eski kimlik doğrulamasını engelleyerek güvenlik durunuzu geliştirmeyi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/13/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd66bc742d0832cba5d6f302bfe30c85e2d82716
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85253350"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Nasıl yapılır: koşullu erişimle Azure AD 'de eski kimlik doğrulamasını engelleme   

Kullanıcılarınıza bulut uygulamalarınıza kolay erişim sağlamak için Azure Active Directory (Azure AD) eski kimlik doğrulaması dahil olmak üzere çok çeşitli kimlik doğrulama protokollerini destekler. Ancak, eski protokoller Multi-Factor Authentication 'ı (MFA) desteklemez. MFA birçok ortamda, kimlik hırsızlığına yönelik ortak bir gereksinimdir. 

Alex Weinert, Microsoft 'taki kimlik Güvenliği Direktörü, 12 Mart 2020 ' de, [kuruluşunuzda eski kimlik doğrulamasını engellemek Için yeni araçlar](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/new-tools-to-block-legacy-authentication-in-your-organization/ba-p/1225302#) , kuruluşların eski kimlik doğrulamasını nasıl engelleyebileceklerini ve Microsoft 'un bu görevi gerçekleştirmek için sağladığı ek araçları vurgular:

> MFA 'nın etkili olabilmesi için eski kimlik doğrulamasını da engellemeniz gerekir. Bunun nedeni, POP, SMTP, IMAP ve MAPI gibi eski kimlik doğrulama protokollerinin MFA 'ya zorlayamadığı için, bu nedenle bunları, duyuru işlemleri için tercih edilen giriş noktaları kuruluşunuza saldırıyor...
> 
>... Azure Active Directory (Azure AD) trafiği analizinden gelen eski kimlik doğrulama numaraları Stark:
> 
> - Parola spreyi saldırılarının yüzde 99 ' sinden fazlası eski kimlik doğrulama protokollerini kullanır
> - Credential stuffing saldırılarının yüzde 97 ' sinden fazlası eski kimlik doğrulamasını kullanır
> - Eski kimlik doğrulama 67 deneyimini devre dışı bırakmış olan kuruluşların Azure AD hesapları, eski kimlik doğrulamasının etkin olduğu kadardan daha az sayıda kombinasyon
>

Ortamınız, kiracınızın korumasını geliştirmek için eski kimlik doğrulamasını engellemeye hazırsanız, bu hedefi koşullu erişimle gerçekleştirebilirsiniz. Bu makalede, kiracınız için eski kimlik doğrulamasını engelleyen koşullu erişim ilkelerini nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, hakkında bilgi sahibi olduğunuz varsayılmaktadır: 

- Azure AD koşullu erişim 'in [temel kavramları](overview.md) 
- Azure portal koşullu erişim ilkelerini yapılandırmaya yönelik [en iyi yöntemler](best-practices.md)

## <a name="scenario-description"></a>Senaryo açıklaması

Azure AD, eski kimlik doğrulama dahil olmak üzere en yaygın olarak kullanılan kimlik doğrulama ve yetkilendirme protokollerini destekler. Eski kimlik doğrulaması, temel kimlik doğrulaması kullanan protokollerin anlamına gelir. Genellikle, bu protokoller herhangi bir tür ikinci faktör kimlik doğrulamasını zorunlu kılamaz. Eski kimlik doğrulamasına dayalı uygulamalar için örnekler şunlardır:

- Daha eski Microsoft Office uygulamalar
- POP, IMAP ve SMTP gibi posta protokollerini kullanan uygulamalar

Tek faktörlü kimlik doğrulaması (örneğin, Kullanıcı adı ve parola) bu günlerde yeterli değil. Parolaların tahmin edilmesi kolay olduğu ve bizim (insanların) iyi parola seçerken kötü olduğu için parolalar hatalı. Parolalar ayrıca kimlik avı ve parola spreyi gibi çeşitli saldırılara karşı savunmasız kalır. Parola tehditlerine karşı korumak için yapabileceğiniz en kolay şeylerden biri MFA uygulamasıdır. MFA ile, bir saldırgan kullanıcının parolasını elinde bıraksa bile, parola kimlik doğrulaması ve verilere erişmek için tek başına yeterli değildir.

Eski kimlik doğrulaması kullanan uygulamaların kiracının kaynaklarına erişmesini nasıl önleyebilirim? Öneri yalnızca bir koşullu erişim ilkesiyle engellenmeniz önerilir. Gerekirse, yalnızca belirli kullanıcıların ve belirli ağ konumlarının eski kimlik doğrulamasına dayalı uygulamalar kullanmasına izin verebilirsiniz.

Koşullu erişim ilkeleri, ilk faktör kimlik doğrulaması tamamlandıktan sonra zorlanır. Bu nedenle, koşullu erişim hizmet reddi (DoS) saldırıları gibi senaryolar için birinci hat savunma olarak tasarlanmamıştır, ancak erişimi anlamak için bu olaylardaki sinyalleri (örneğin, oturum açma risk düzeyi, isteğin konumu vb.) kullanabilir.

## <a name="implementation"></a>Uygulama

Bu bölümde, eski kimlik doğrulamasını engellemek için bir koşullu erişim ilkesinin nasıl yapılandırılacağı açıklanmaktadır. 

### <a name="legacy-authentication-protocols"></a>Eski kimlik doğrulama protokolleri

Aşağıdaki seçenekler eski kimlik doğrulama protokolleri olarak kabul edilir

- Kimliği doğrulanmış SMTP-e-posta iletileri göndermek için POP ve IMAP istemci tarafından kullanılır.
- Otomatik bulma-Exchange Online 'da posta kutularını bulmak ve bağlamak için Outlook ve EAS istemcileri tarafından kullanılır.
- Exchange ActiveSync (EAS)-Exchange Online 'daki posta kutularına bağlanmak için kullanılır.
- Exchange Online PowerShell-uzak PowerShell ile Exchange Online 'a bağlanmak için kullanılır. Exchange Online PowerShell için temel kimlik doğrulamasını engellerseniz, bağlanmak için Exchange Online PowerShell modülünü kullanmanız gerekir. Yönergeler için bkz. [Multi-Factor Authentication kullanarak Exchange Online PowerShell 'e bağlanma](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell).
- Exchange Web Hizmetleri (EWS)-Outlook, Mac için Outlook ve üçüncü taraf uygulamalar tarafından kullanılan bir programlama arabirimidir.
- IMAP4-IMAP e-posta istemcileri tarafından kullanılır.
- HTTP üzerinden MAPI (MAPI/HTTP)-Outlook 2010 ve üzeri tarafından kullanılır.
- Çevrimdışı adres defteri (OAB)-Outlook tarafından indirilen ve kullanılan adres listesi koleksiyonlarının bir kopyası.
- Outlook her yerde (HTTP üzerinden RPC)-Outlook 2016 ve öncesi tarafından kullanılır.
- Outlook hizmeti-Windows 10 için posta ve takvim uygulaması tarafından kullanılır.
- POP3-POP e-posta istemcileri tarafından kullanılır.
- Raporlama Web Hizmetleri-Exchange Online 'daki rapor verilerini almak için kullanılır.
- Diğer istemciler-eski kimlik doğrulamasından yararlanarak tanımlanan diğer protokoller.

Bu kimlik doğrulama protokolleri ve hizmetleri hakkında daha fazla bilgi için bkz. [Azure Active Directory portalındaki oturum açma etkinlik raporları](../reports-monitoring/concept-sign-ins.md#filter-sign-in-activities).

### <a name="identify-legacy-authentication-use"></a>Eski kimlik doğrulama kullanımını tanımla

Dizininizde eski kimlik doğrulamasını engelleyebilmeniz için önce, kullanıcılarınızın eski kimlik doğrulaması kullanan uygulamalar olup olmadığını ve bunun genel dizininizi nasıl etkileyeceğini anlamanız gerekir. Azure AD oturum açma günlükleri, eski kimlik doğrulaması kullanıp kullandığınızı anlamak için kullanılabilir.

1. **Azure Portal**  >  **Azure Active Directory**  >  **oturum açma**işlemleri ' ne gidin.
1. **Sütunlar**  >  **istemci uygulaması**' na tıklanarak gösterilmezse, istemci uygulaması sütununu ekleyin.
1. **Filtre Ekle**  >  **Istemci uygulaması** > eski tüm kimlik doğrulama protokollerini seçip **Uygula**' ya tıklayın.

Filtreleme yalnızca eski kimlik doğrulama protokolleri tarafından yapılan oturum açma girişimlerini gösterir. Her bir bireysel oturum açma girişimine tıkladığınızda ek ayrıntılar gösterilecektir. **Temel bilgi** sekmesindeki **istemci uygulaması** alanı, hangi eski kimlik doğrulama protokolünün kullanıldığını gösterir.

Bu Günlükler, hangi kullanıcıların eski kimlik doğrulamasına bağlı olduğunu ve hangi uygulamaların kimlik doğrulama isteklerini yapmak için eski protokolleri kullandığını gösterir. Bu günlüklerde görünmeyen ve eski kimlik doğrulaması kullanmayan kullanıcılar için, yalnızca bu kullanıcılar için bir koşullu erişim ilkesi uygulayın.

### <a name="block-legacy-authentication"></a>Eski kimlik doğrulamasını engelleme 

Koşullu erişim ilkesinde, kaynaklarınıza erişmek için kullanılan istemci uygulamalarına bağlı bir koşul belirleyebilirsiniz. İstemci uygulamaları koşulu, **mobil uygulamalar ve Masaüstü istemcileri**altında **Exchange ActiveSync istemcileri** ve **diğer istemciler** ' i seçerek eski kimlik doğrulamasını kullanan uygulamalar için kapsamı daraltmanızı sağlar.

![Diğer istemciler](./media/block-legacy-authentication/01.png)

Bu uygulamalara erişimi engellemek için **erişimi engelle**' yi seçmeniz gerekir.

![Erişimi engelleme](./media/block-legacy-authentication/02.png)

### <a name="select-users-and-cloud-apps"></a>Kullanıcıları ve bulut uygulamalarını seçin

Kuruluşunuz için eski kimlik doğrulamasını engellemek istiyorsanız, şunu seçerek bunu deneyebilirsiniz:

- Tüm kullanıcılar
- Tüm bulut uygulamaları
- Erişimi engelleme

![Atamalar](./media/block-legacy-authentication/03.png)

Bu yapılandırma koşullu erişim ilkelerine yönelik [en iyi uygulamaları](best-practices.md) ihlal ettiğinden, Azure bu şekilde bir ilke oluşturmanızı önleyen bir güvenlik özelliğine sahiptir.
 
![İlke yapılandırması desteklenmiyor](./media/block-legacy-authentication/04.png)

*Tüm kullanıcıların ve tüm bulut uygulamalarının* kiracınızda oturum açmasını engellemek mümkün olduğundan, güvenlik özelliği gereklidir. En düşük uygulama gereksinimini karşılamak için en az bir kullanıcıyı dışarıda bırakmanız gerekir. Ayrıca bir dizin rolü de dışlayabilirsiniz.

![İlke yapılandırması desteklenmiyor](./media/block-legacy-authentication/05.png)

İlkenize bir Kullanıcı dışlayarak bu güvenlik özelliğini karşılamanız gerekir. İdeal olarak, [Azure AD 'de birkaç acil erişim yönetim hesabı](../users-groups-roles/directory-emergency-access.md) tanımlamanız ve bunları ilkenize dışlayamazsınız.

İlkenizin eski kimlik doğrulamasını engellemek için etkinleştirildiğinde [yalnızca rapor modunu](concept-conditional-access-report-only.md) kullanmak, kuruluşunuzun ilkenin etkisinin ne olduğunu izlemeye yönelik bir fırsat sağlar.

## <a name="policy-deployment"></a>İlke dağıtımı

İlkenizi üretime eklemeden önce şunları yapın:
 
- **Hizmet hesapları** -konferans odası telefonları gibi hizmet hesapları veya cihazlar tarafından kullanılan Kullanıcı hesaplarını belirler. Bu hesapların güçlü parolalara sahip olduğundan emin olun ve bunları dışlanan bir gruba ekleyin.
- **Oturum açma raporları** -oturum açma raporunu gözden geçirin ve **diğer istemci** trafiğini arayın. En iyi kullanımı belirler ve neden kullanımda olduğunu araştırın. Genellikle trafik, modern kimlik doğrulaması kullanmayan eski Ofis istemcileri veya bazı üçüncü taraf posta uygulamaları tarafından oluşturulur. Kullanım kullanımını Bu uygulamalardan uzağa taşımaya yönelik bir plan yapın veya etki düşükse, kullanıcılarınıza bu uygulamaları artık kullanamazlar.
 
Daha fazla bilgi için bkz. [Yeni bir Ilkeyi nasıl dağıtmanız gerekir?](best-practices.md#how-should-you-deploy-a-new-policy).

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

**Diğer istemcileri** kullanarak erişimin engellenmesi, Exchange Online PowerShell ve Dynamics 365 ' i temel kimlik doğrulaması kullanarak engeller.

**Diğer istemciler** için bir ilkeyi yapılandırmak, kuruluşun tamamını spconnect gibi belirli istemcilerden engeller. Eski istemciler beklenmeyen yollarla kimlik doğrulaması yaptığından bu blok oluşur. Sorun, eski Office istemcileri gibi başlıca Office uygulamalarına uygulanmaz.

İlkenin etkili olması 24 saate kadar sürebilir.

**Diğer istemciler** koşulu için kullanılabilir tüm izin denetimlerini seçebilirsiniz; Ancak, son kullanıcı deneyimi her zaman aynı engellenen erişimdir.

**Diğer istemciler** koşulunu kullanarak eski kimlik doğrulamasını engellerseniz, cihaz platformunu ve konum koşulunu da ayarlayabilirsiniz. Örneğin, mobil cihazlar için yalnızca eski kimlik doğrulamasını engellemek istiyorsanız, şu seçeneği belirleyerek **cihaz platformları** koşulunu ayarlayın:

- Android
- iOS
- Windows Phone

![İlke yapılandırması desteklenmiyor](./media/block-legacy-authentication/06.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)
- Koşullu erişim ilkelerini henüz yapılandırmaya alışmıyorsanız, bir örnek için [Azure Active Directory Koşullu erişimi olan belirli uygulamalar IÇIN MFA isteme](app-based-mfa.md) konusuna bakın.
- Modern kimlik doğrulama desteği hakkında daha fazla bilgi için bkz. [modern kimlik doğrulama office 2013 ve office 2016 istemci uygulamaları Için nasıl kullanılır](/office365/enterprise/modern-auth-for-office-2013-and-2016) 
- [Office 365 ve Microsoft 365 kullanarak e-posta göndermek üzere çok işlevli bir cihaz veya uygulamayı ayarlama](/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3)
