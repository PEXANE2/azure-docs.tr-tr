---
title: Eski kimlik doğrulamasını engelleme - Azure Etkin Dizin
description: Azure AD Koşullu Erişimi kullanarak eski kimlik doğrulamasını engelleyerek güvenlik duruşunuzu nasıl geliştireceğinizi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76dd07a59a9fa7c0d6231a766ff4090c11f9f5bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331919"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Nasıl yapilir: Koşullu Erişim ile Azure AD'ye eski kimlik doğrulamasını engelleme   

Kullanıcılarınıza bulut uygulamalarınıza kolay erişim sağlamak için Azure Active Directory (Azure AD), eski kimlik doğrulama dahil olmak üzere çok çeşitli kimlik doğrulama protokollerini destekler. Ancak, eski protokoller çok faktörlü kimlik doğrulamasını (MFA) desteklemez. MFA birçok ortamda kimlik hırsızlığı nı ele almak için ortak bir gerekliliktir. 

Microsoft Kimlik Güvenliği Direktörü Alex Weinert, 12 Mart 2020'deki blog gönderisinde [kuruluşunuzdaki eski kimlik doğrulamasını engellemek için yeni araçlar,](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/new-tools-to-block-legacy-authentication-in-your-organization/ba-p/1225302#) kuruluşların neden eski kimlik doğrulamasını engellemesi gerektiğini ve Microsoft'un bu görevi gerçekleştirmek için hangi ek araçları sağladığını vurgular:

> MFA'nın etkili olabilmek için eski kimlik doğrulamasını da engellemeniz gerekir. Bunun nedeni, POP, SMTP, IMAP ve MAPI gibi eski kimlik doğrulama protokollerinin MFA'yı uygulayaması ve bunları kuruluşunuza saldıran düşmanlar için tercih edilen giriş noktaları haline getiremememeğidir...
> 
>... Azure Active Directory (Azure AD) trafiğinin analizinden eski kimlik doğrulaması ile ilgili sayılar yalın:
> 
> - Parola püskürtme saldırılarının yüzde 99'dan fazlası eski kimlik doğrulama protokollerini kullanır
> - Kimlik bilgileri doldurma saldırılarının yüzde 97'den fazlası eski kimlik doğrulaması kullanır
> - Eski kimlik doğrulama deneyimini devre dışı eden kuruluşlardaki Azure AD hesapları, eski kimlik doğrulamanın etkinleştirildiği kuruluşlara göre yüzde 67 daha az uzlaşma
>

Ortamınız kiracınızın korumasını iyileştirmek için eski kimlik doğrulamasını engellemeye hazırsa, bu hedefi Koşullu Erişim ile gerçekleştirebilirsiniz. Bu makalede, kiracınız için eski kimlik doğrulamasını engelleyen Koşullu Erişim ilkelerini nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, aşina olduğunuzu varsayar: 

- Azure AD Koşullu Erişim'in [temel kavramları](overview.md) 
- Azure portalında Koşullu Erişim ilkelerini yapılandırmak için [en iyi uygulamalar](best-practices.md)

## <a name="scenario-description"></a>Senaryo açıklaması

Azure AD, eski kimlik doğrulama dahil olmak üzere en yaygın olarak kullanılan kimlik doğrulama ve yetkilendirme protokollerinden birkaçını destekler. Eski kimlik doğrulaması, temel kimlik doğrulaması kullanan protokolleri ifade eder. Genellikle, bu protokoller ikinci faktör kimlik doğrulaması herhangi bir tür uygulayamaz. Eski kimlik doğrulamaya dayalı uygulamalara örnek olarak şunlar verilebilir:

- Eski Microsoft Office uygulamaları
- POP, IMAP ve SMTP gibi posta protokollerini kullanan uygulamalar

Tek faktörlü kimlik doğrulama (örneğin, kullanıcı adı ve parola) bu gün yeterli değildir. Onlar tahmin etmek kolay ve biz (insanlar) iyi şifreleri seçiminde kötü olduğu gibi parolalar kötüdür. Parolalar, kimlik avı ve parola püskürtme gibi çeşitli saldırılara karşı da savunmasızdır. Parola tehditlerine karşı korumak için yapabileceğiniz en kolay şeylerden biri MFA uygulamaktır. MFA ile, bir saldırgan bir kullanıcının parolasını ele geçirse bile, parola tek başına verileri başarıyla doğrulamak ve erişmek için yeterli değildir.

Eski kimlik doğrulamasını kullanan uygulamaların kiracınızın kaynaklarına erişmesini nasıl önleyebilirsiniz? Öneri, koşullu erişim ilkesiyle bunları engellemektir. Gerekirse, yalnızca belirli kullanıcıların ve belirli ağ konumlarının eski kimlik doğrulamasına dayalı uygulamaları kullanmasına izin verirsiniz.

Koşullu Erişim ilkeleri, ilk faktör kimlik doğrulaması tamamlandıktan sonra uygulanır. Bu nedenle, Koşullu Erişim, hizmet reddi (DoS) saldırıları gibi senaryolar için ilk satır savunması olarak tasarlanmamıştır, ancak erişimi belirlemek için bu olaylardan gelen sinyalleri (örneğin, oturum açma risk düzeyi, isteğin konumu vb.) kullanabilir.

## <a name="implementation"></a>Uygulama

Bu bölümde, eski kimlik doğrulamasını engellemek için Koşullu Erişim ilkesinin nasıl yapılandırılabildiğini açıklanmaktadır. 

### <a name="legacy-authentication-protocols"></a>Eski kimlik doğrulama protokolleri

Aşağıdaki seçenekler eski kimlik doğrulama protokolleri olarak kabul edilir

- Kimlik doğrulaması SMTP - POP ve IMAP istemcileri tarafından e-posta iletileri göndermek için kullanılır.
- Autodiscover - Exchange Online'daki posta kutularını bulmak ve bağlanmak için Outlook ve EAS istemcileri tarafından kullanılır.
- Exchange Online PowerShell - Uzaktan PowerShell ile Exchange Online'a bağlanmak için kullanılır. Exchange Online PowerShell için Temel kimlik doğrulamasını engellerseniz, bağlanmak için Exchange Online PowerShell Modülünü kullanmanız gerekir. Talimatlar için, [çok faktörlü kimlik doğrulaması kullanarak Exchange Online PowerShell'e Bağlan'a](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell)bakın.
- Exchange Web Services (EWS) - Outlook, Outlook for Mac ve üçüncü taraf uygulamalar tarafından kullanılan bir programlama arabirimi.
- IMAP4 - IMAP e-posta istemcileri tarafından kullanılır.
- MAPI üzerinde HTTP (MAPI/HTTP) - Outlook 2010 ve sonrası tarafından kullanılır.
- Çevrimdışı Adres Defteri (OAB) - Outlook tarafından indirilen ve kullanılan adres listesi koleksiyonlarının bir kopyası.
- Outlook Anywhere (Http üzerinden RPC) - Outlook 2016 ve daha önce tarafından kullanılır.
- Outlook Hizmeti - Windows 10 için Posta ve Takvim uygulaması tarafından kullanılır.
- POP3 - POP e-posta istemcileri tarafından kullanılır.
- Web Hizmetlerini Raporlama - Exchange Online'da rapor verilerini almak için kullanılır.
- Diğer istemciler - Eski kimlik doğrulaması kullandığı belirlenen diğer protokoller.

Bu kimlik doğrulama protokolleri ve hizmetleri hakkında daha fazla bilgi için [Azure Etkin Dizin portalında oturum açma etkinlik raporlarına](../reports-monitoring/concept-sign-ins.md#filter-sign-in-activities)bakın.

### <a name="identify-legacy-authentication-use"></a>Eski kimlik doğrulama kullanımını belirleme

Dizininizde eski kimlik doğrulamasını engelleyebilmeniz için önce, kullanıcılarınızın eski kimlik doğrulaması kullanan uygulamaları olup olmadığını ve bunun genel dizininizi nasıl etkilediğini anlamanız gerekir. Azure AD oturum açma günlükleri, eski kimlik doğrulaması kullanıp kullanmadığınızı anlamak için kullanılabilir.

1. **Azure portalı** > **Azure Active Directory** > **Oturum Açma'ya**gidin.
1. **Sütunlar** > **İstemci Uygulaması'na**tıklayarak gösterilmezse İstemci Uygulaması sütununa ekleyin.
1. **Filtreler** > **Ekle Istemci Uygulaması** > tüm eski kimlik doğrulama protokollerini seçin ve **Uygula'yı**tıklatın.

Filtreleme yalnızca eski kimlik doğrulama protokolleri tarafından yapılan oturum açma girişimlerini gösterir. Her bir oturum açma girişimine tıkladığınızda ek ayrıntılar gösterecektir. **Temel Bilgiler** sekmesi altındaki **İstemci Uygulaması** alanı, hangi eski kimlik doğrulama protokolünün kullanıldığını gösterir.

Bu günlükler, hangi kullanıcıların hala eski kimlik doğrulamasına bağlı olduğunu ve kimlik doğrulama isteklerini gerçekleştirmek için hangi uygulamaların eski protokolleri kullandığını gösterir. Bu günlüklerde görünmeyen ve eski kimlik doğrulaması kullanmadığı doğrulanan kullanıcılar için yalnızca bu kullanıcılar için koşullu erişim ilkesi uygulayın.

### <a name="block-legacy-authentication"></a>Eski kimlik doğrulamasını engelleme 

Koşullu Erişim ilkesinde, kaynaklarınıza erişmek için kullanılan istemci uygulamalarına bağlı bir koşul ayarlayabilirsiniz. İstemci uygulamaları koşulu, **Mobil uygulamalar ve masaüstü istemcileri**için Diğer **istemcileri** seçerek eski kimlik doğrulaması kullanarak kapsamı uygulamalara daraltmanızı sağlar.

![Diğer istemciler](./media/block-legacy-authentication/01.png)

Bu uygulamalara erişimi engellemek için **Erişimi Engelle'yi**seçmeniz gerekir.

![Hizmete erişimi](./media/block-legacy-authentication/02.png)

### <a name="select-users-and-cloud-apps"></a>Belirli kullanıcılar ve bulut uygulamaları

Kuruluşunuz için eski kimlik doğrulamasını engellemek istiyorsanız, büyük olasılıkla bunu şu seçerek başarabileceğinizi düşünüyorsunuzdur:

- Tüm kullanıcılar
- Tüm bulut uygulamaları
- Hizmete erişimi

![Atamalar](./media/block-legacy-authentication/03.png)

Azure, bu yapılandırma Koşullu Erişim ilkeleri için [en iyi uygulamaları](best-practices.md) ihlal ettiği için böyle bir ilke oluşturmanızı engelleyen bir güvenlik özelliğine sahiptir.
 
![İlke yapılandırması desteklenmiyor](./media/block-legacy-authentication/04.png)

Güvenlik özelliği gereklidir, çünkü *tüm kullanıcıları engelleyin ve tüm bulut uygulamaları* tüm kuruluşunuzun kiracınıza oturum etmesini engelleme potansiyeline sahiptir. En az en iyi uygulama gereksinimini karşılamak için en az bir kullanıcıyı hariç tutmanız gerekir. Dizin rolünü de dışlayabilirsin.

![İlke yapılandırması desteklenmiyor](./media/block-legacy-authentication/05.png)

Bu güvenlik özelliğini, bir kullanıcıyı politikanızdan hariç tutarak karşılayabilirsiniz. İdeal olarak, [Azure AD'de](../users-groups-roles/directory-emergency-access.md) birkaç acil erişimli yönetim hesabı tanımlamalı ve bunları ilkenizden hariç tutmalısınız.

Eski kimlik doğrulamasını engellemek için ilkenizin etkinleştirilmesini etkinleştirirken [yalnızca rapor modunu](concept-conditional-access-report-only.md) kullanmak, kuruluşunuz için iipolitikasının etkisinin ne olacağını izleme fırsatı sağlar.

## <a name="policy-deployment"></a>İlke dağıtımı

Politikanızı üretime koymadan önce şunları göz altına alın:
 
- **Hizmet hesapları** - Servis hesabı olarak veya konferans odası telefonları gibi aygıtlar tarafından kullanılan kullanıcı hesaplarını belirleyin. Bu hesapların güçlü parolalara sahip olduğundan emin olun ve bunları dışlanmış bir gruba ekleyin.
- **Oturum açma raporları** - Oturum açma raporunu gözden geçirin ve **diğer istemci** trafiğini arayın. En iyi kullanımı belirleyin ve neden kullanıldığını araştırın. Trafik genellikle, modern kimlik doğrulaması kullanmayan eski Office istemcileri veya bazı üçüncü taraf posta uygulamaları tarafından oluşturulur. Kullanımı bu uygulamalardan uzaklaştırmak için bir plan yapın veya etkisi düşükse, kullanıcılarınıza bu uygulamaları artık kullanamayacaklarını bildirin.
 
Daha fazla bilgi için [bkz.](best-practices.md#how-should-you-deploy-a-new-policy)

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

**Diğer istemcileri** kullanarak erişimi engelleme de temel auth kullanarak Exchange Online PowerShell ve Dynamics 365 engeller.

**Diğer istemciler** için bir ilke yapılandırma SPConnect gibi belirli istemcilerden tüm organizasyonu engeller. Eski istemcilerin beklenmeyen şekillerde kimlik doğrulaması olduğundan bu engelleme olur. Sorun, eski Office istemcileri gibi büyük Office uygulamaları için geçerli değildir.

Poliçenin yürürlüğe girmesi 24 saat kadar sürebilir.

**Diğer istemciler** durumu için kullanılabilir tüm hibe denetimlerini seçebilirsiniz; ancak, son kullanıcı deneyimi her zaman aynıdır - engellenen erişim.

**Diğer istemciler** koşulunu kullanarak eski kimlik doğrulamasını engellerseniz, aygıt platformunu ve konum koşulunu da ayarlayabilirsiniz. Örneğin, yalnızca mobil aygıtlar için eski kimlik doğrulamasını engellemek istiyorsanız, aşağıdakileri seçerek **aygıt platformları** koşulunu ayarlayın:

- Android
- iOS
- Windows Phone

![İlke yapılandırması desteklenmiyor](./media/block-legacy-authentication/06.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu Erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)
- Koşullu Erişim ilkelerini yapılandırmayı henüz bilmiyorsanız, örneğin [Azure Active Directory Koşullu Erişim'e sahip belirli uygulamalar için MFA gereksinimi bkz.](app-based-mfa.md)
- Modern kimlik doğrulama desteği hakkında daha fazla bilgi için Bkz. [Office 2013 ve Office 2016 istemci uygulamaları için modern kimlik doğrulama nasıl çalışır?](/office365/enterprise/modern-auth-for-office-2013-and-2016) 
