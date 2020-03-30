---
title: Kimlik doğrulama oturumu yönetimini yapılandırma - Azure Etkin Dizini
description: Kullanıcı oturum sıklığı ve tarayıcı oturumu kalıcılığı dahil olmak üzere Azure AD kimlik doğrulama oturumu yapılandırmasını özelleştirin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jlu, calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e9c0c88064c00c97de7dc58a500910e81c04eef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263289"
---
# <a name="configure-authentication-session-management-with-conditional-access"></a>Koşullu Erişim ile kimlik doğrulama oturumu yönetimini yapılandırma

Karmaşık dağıtımlarda, kuruluşların kimlik doğrulama oturumlarını kısıtlaması gerekebilir. Bazı senaryolar şunları içerebilir:

* Yönetilmeyen veya paylaşılan bir aygıttan kaynak erişimi
* Harici bir ağdan hassas bilgilere erişim
* Yüksek etkili kullanıcılar
* Kritik iş uygulamaları

Koşullu Erişim denetimleri, tüm kullanıcıları etkilemeden kuruluşunuzdaki belirli kullanım durumlarını hedefleyen ilkeler oluşturmanıza olanak sağlar.

İlkenin nasıl yapılandırılabildiğini ayrıntılarına girmeden önce varsayılan yapılandırmayı inceleyelim.

## <a name="user-sign-in-frequency"></a>Kullanıcı oturum açma sıklığı

Oturum açma sıklığı, bir kaynağa erişmeye çalışırken kullanıcıdan yeniden oturum açmasının istenmesinden önceki süreyi tanımlar.

Kullanıcı oturum açma sıklığı için Azure Etkin Dizin (Azure AD) varsayılan yapılandırması 90 günlük bir yuvarlanma penceresidir. Kimlik bilgileri için kullanıcılarsoran genellikle mantıklı bir şey gibi görünüyor, ama geri tepebilir: düşünmeden kimlik bilgilerini girmek için eğitilmiş kullanıcılar istemeden kötü niyetli bir kimlik istemi onları sağlayabilir.

Bir kullanıcının oturum açmasını istememesi endişe verici gelebilir, gerçekte BT politikalarının herhangi bir ihlali oturumu iptal eder. Bazı örnekler arasında parola değişikliği, uyumsuz bir aygıt veya hesap devre dışı edilme (ancak bunlarla sınırlı değildir). [PowerShell'i kullanarak kullanıcıların oturumlarını](/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0)da açıkça iptal edebilirsiniz. Azure AD varsayılan yapılandırması ,"oturumlarının güvenlik duruşu değişmediyse kullanıcılardan kimlik bilgilerini vermelerini istemeyin" olarak aşağı gelir.

Oturum açma sıklığı ayarı, standartlara uygun OAUTH2 veya OIDC protokollerini uygulayan uygulamalarla çalışır. Aşağıdaki web uygulamaları da dahil olmak üzere Windows, Mac ve Mobile için Microsoft'un yerel uygulamalarının çoğu ayarı uygun.

- Word, Excel, PowerPoint Online
- OneNote Online
- Office.com
- O365 Yönetici portalı
- Exchange Online
- SharePoint ve OneDrive
- Takımlar web istemcisi
- Dynamics CRM Online
- Azure portalında

### <a name="user-sign-in-frequency-and-device-identities"></a>Kullanıcı oturum açma sıklığı ve cihaz kimlikleri

Azure AD'niz katıldıysa, karma Azure AD'niz katıldıysa veya Azure AD kayıtlı aygıtları, bir kullanıcı cihazının kilidini açtıysa veya etkileşimli olarak oturum açtıysa, bu olay sıklık ilkesinde oturum açma ilkesini de tatmin eder. Aşağıdaki 2 örnekte kullanıcı oturum açma sıklığı 1 saat olarak ayarlanır:

Örnek 1:

- Saat 00:00'da bir kullanıcı Windows 10 Azure AD aygıtına giriş yaptı ve SharePoint Online'da depolanan bir belge üzerinde çalışmaya başladı.
- Kullanıcı, aygıtında aynı belge üzerinde bir saat boyunca çalışmaya devam ediyor.
- Saat 01:00'de, kullanıcıdan, yöneticisi tarafından yapılandırılan Koşullu Erişim ilkesindeki oturum açma sıklığı gereksinimine bağlı olarak yeniden oturum açması istenir.

Örnek 2:

- Saat 00:00'da bir kullanıcı Windows 10 Azure AD aygıtına giriş yaptı ve SharePoint Online'da depolanan bir belge üzerinde çalışmaya başladı.
- Saat 00:30'da, kullanıcı kalkar ve cihazını kilitleme ye bir mola alır.
- Saat 00:45'te, kullanıcı moladan döner ve aygıtın kilidini açar.
- Saat 01:45'te, son oturum açma nın saat 00:45'te gerçekleştiğinden beri, kullanıcıdan, yönetici tarafından yapılandırılan Koşullu Erişim ilkesindeki oturum açma sıklığı gereksinimine bağlı olarak yeniden oturum açması istenir.

## <a name="persistence-of-browsing-sessions"></a>Tarama oturumlarının kalıcılığı

Kalıcı bir tarayıcı oturumu, kullanıcıların tarayıcı pencerelerini kapatıp yeniden açtıktan sonra oturumda kalmalarını sağlar.

Tarayıcı oturumu kalıcılığı için Azure AD varsayılanı, kişisel cihazlardaki kullanıcıların oturuma devam edip etmemeyi "Oturumda kal" seçeneğini göstererek seçmesine olanak tanır. başarılı kimlik doğrulamasonra istemi. Ad [FS Tek Oturum](/windows-server/identity/ad-fs/operations/ad-fs-single-sign-on-settings#enable-psso-for-office-365-users-to-access-sharepoint-online
)Açma Ayarları makaledeki kılavuz kullanılarak AD FS'de tarayıcı kalıcılığı yapılandırılırsa, bu ilkeyi kullanır ve Azure REKLAM oturumuna devam edeceğiz. Ayrıca, kiracınızdaki kullanıcıların "Oturum açmış olarak kalın" adlı yapıyı görüp görmediğini de yapılandırabilirsiniz. makaledeki kılavuzu kullanarak Azure portalındaki şirket marka bölmesinde uygun ayarı değiştirerek Azure [REKLAM oturum açma sayfanızı özelleştirin.](../fundamentals/customize-branding.md)

## <a name="configuring-authentication-session-controls"></a>Kimlik doğrulama oturumu denetimlerini yapılandırma

Koşullu Erişim bir Azure AD Premium özelliğidir ve premium lisans gerektirir. Koşullu Erişim hakkında daha fazla bilgi edinmek istiyorsanız, [Azure Etkin Dizinde Koşullu Erişim nedir?](overview.md#license-requirements)

> [!WARNING]
> Şu anda genel önizlemede [bulunan yapılandırılabilir belirteç ömrü](../develop/active-directory-configurable-token-lifetimes.md) özelliğini kullanıyorsanız, aynı kullanıcı veya uygulama birleşimi için iki farklı ilke oluşturulmasını desteklemediğimizi lütfen unutmayın: biri bu özelliklere sahip diğeri de yapılandırılabilir belirteç ömrü özelliğine sahip. Microsoft, yapılandırılabilir belirteç ömrü özelliğini 1 Mayıs 2020'de devre dışı bırakıp Koşullu Erişim kimlik doğrulama oturumu yönetimi özelliğiyle değiştirmeyi planlıyor.  

### <a name="policy-1-sign-in-frequency-control"></a>İlke 1: Oturum açma frekans kontrolü

1. Yeni ilke oluşturma
1. Hedef bulut uygulamaları da dahil olmak üzere müşterinin ortamı için gerekli tüm koşulları seçin.

   > [!NOTE]
   > En iyi kullanıcı deneyimi için Exchange Online ve SharePoint Online gibi önemli Microsoft Office uygulamaları için eşit kimlik doğrulama istemi sıklığı ayarlaman önerilir.

1. Erişim **Denetimleri** > **Oturumu'na** gidin ve **Oturum açma sıklığını** tıklatın
1. İlk metin kutusuna gün ve saatin gerekli değerini girin
1. Açılır düşüşten **Saat** veya **Gün** değeri seçin
1. İlkinizi kaydedin

![Oturum açma sıklığı için yapılandırılan Koşullu Erişim ilkesi](media/howto-conditional-access-session-lifetime/conditional-access-policy-session-sign-in-frequency.png)

Azure AD'de kayıtlı Windows aygıtlarında aygıtta oturum açma komut istemi olarak kabul edilir. Örneğin, Oturum Açma sıklığını Office uygulamaları için 24 saat olarak yapılandırıldıysanız, Azure AD kayıtlı Windows cihazlarındaki kullanıcılar aygıtta oturum açarak Oturum Açma ilkesini karşılar ve Office uygulamalarını açarken tekrar istenmez.

Aynı tarayıcı oturumunda çalışan farklı web uygulamaları için farklı Oturum açma sıklığını yapılandırmışsanız, aynı tarayıcı oturumunda çalışan tüm uygulamalar tek bir oturum belirteci paylaştığından, her iki uygulamaya da en katı ilke uygulanır.

### <a name="policy-2-persistent-browser-session"></a>İlke 2: Kalıcı tarayıcı oturumu

1. Yeni ilke oluşturma
1. Gerekli tüm koşulları seçin.

   > [!NOTE]
   > Bu denetimin koşul olarak "Tüm Bulut Uygulamaları"nı seçmeniz gerektiğini lütfen unutmayın. Tarayıcı oturumu kalıcılığı kimlik doğrulama oturumu belirteci tarafından denetlenir. Tarayıcı oturumundaki tüm sekmeler tek bir oturum belirteci paylaşır ve bu nedenle hepsinin kalıcılık durumunu paylaşması gerekir.

1. Access **Denetimleri** > **Oturumu'na** gidin ve **Kalıcı tarayıcı oturumuna** tıklayın
1. Açılır bırakma dan bir değer seçin
1. İlkenizi kaydedin

![Kalıcı tarayıcı için yapılandırılan Koşullu Erişim ilkesi](media/howto-conditional-access-session-lifetime/conditional-access-policy-session-persistent-browser.png)

> [!NOTE]
> Azure AD Koşullu Access'teki Kalıcı Tarayıcı Oturumu yapılandırması "Oturumda kalın" yazısının üzerine yazılır. her iki ilkeyi de yapılandırmışsanız, aynı kullanıcı için Azure portalındaki şirket marka bölmesini ayarlama.

## <a name="validation"></a>Doğrulama

Kullanıcıdan hedef uygulamaya giriş imasını ve ilkenizi nasıl yapılandırdığınıza bağlı olarak diğer koşulları simüle etmek için What-If aracını kullanın. Kimlik doğrulama oturumu yönetim denetimleri aracın sonucu olarak gösterilir.

![Koşullu Erişim Ne ise aracı sonuçları](media/howto-conditional-access-session-lifetime/conditional-access-what-if-tool-result.png)

## <a name="policy-deployment"></a>İlke dağıtımı

İlkenizin beklendiği gibi çalıştığından emin olmak için, önerilen en iyi uygulama, politikayı üretime dönüştürmeden önce test etmektir. İdeal olarak, yeni politikanızın beklendiği gibi çalışıp çalışmadığını doğrulamak için bir test kiracısı kullanın. Daha fazla bilgi için, [Azure Etkin Dizini'nde Koşullu Erişim için en iyi uygulamalar makalesine](best-practices.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Koşullu Erişim ilkesini nasıl yapılandırdığınızı bilmek istiyorsanız, [Azure Active Directory Koşullu Erişimi olan belirli uygulamalar için MFA'yı zorlar](app-based-mfa.md)makalesine bakın.
* Ortamınız için Koşullu Erişim ilkelerini yapılandırmaya hazırsanız, [Azure Etkin Dizininde Koşullu Erişim için En İyi Uygulamalar](best-practices.md)makalesine bakın.
