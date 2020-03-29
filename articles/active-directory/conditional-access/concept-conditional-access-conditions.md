---
title: Koşullu Erişim ilkesindeki koşullar - Azure Etkin Dizini
description: Azure AD Koşullu Erişim ilkesindeki koşullar nelerdir
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb14369275a111476867f2263766e1bb87b7c87d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295344"
---
# <a name="conditional-access-conditions"></a>Koşullu Erişim: Koşullar

Koşullu Erişim ilkesi nde, yönetici ilke kararlarını geliştirmek için risk, aygıt platformu veya konum gibi koşullardan gelen sinyalleri kullanabilir. 

![Koşullu Erişim ilkesini tanımlayın ve koşulları belirtin](./media/concept-conditional-access-conditions/conditional-access-conditions.png)

İnce taneli ve özel Koşullu Erişim ilkeleri oluşturmak için birden çok koşul birleştirilebilir.

Örneğin, hassas bir uygulamaya erişirken bir yönetici, çok faktörlü kimlik doğrulama gibi diğer denetimlere ek olarak kimlik koruması ve konumdaki oturum açma risk bilgilerini erişim kararına dahil edebilir.

## <a name="sign-in-risk"></a>Oturum açma riski

[Kimlik Koruması](../identity-protection/overview-identity-protection.md)erişimi olan müşteriler için oturum açma riski Koşullu Erişim ilkesinin bir parçası olarak değerlendirilebilir. Oturum açma riski, belirli bir kimlik doğrulama isteğinin kimlik sahibi tarafından yetkilendirilmeyen olasılığını gösterir. Oturum açma riski hakkında daha fazla bilgiyi makalelerde bulabilirsiniz, [risk nedir](../identity-protection/concept-identity-protection-risks.md#sign-in-risk) ve [nasıl yapılır: Risk politikalarını yapılandırın ve etkinleştirin.](../identity-protection/howto-identity-protection-configure-risk-policies.md)

## <a name="device-platforms"></a>Cihaz platformları

Aygıt platformu, aygıtta çalışan işletim sistemi ile karakterize edilir. Azure AD, kullanıcı aracı dizeleri gibi aygıt tarafından sağlanan bilgileri kullanarak platformu tanımlar. Kullanıcı aracı dizeleri değiştirilebildiği için bu bilgiler doğrulanmaz. Aygıt platformu, Microsoft Intune aygıt uyumluluk ilkeleriyle birlikte veya bir blok deyiminin parçası olarak kullanılmalıdır. Varsayılan değer, tüm aygıt platformlarına uygulanacaktır.

Azure AD Koşullu Erişim aşağıdaki aygıt platformlarını destekler:

- Android
- iOS
- Windows Phone
- Windows
- macOS

**Diğer istemciler** koşulunu kullanarak eski kimlik doğrulamasını engellerseniz, aygıt platformu koşulunu da ayarlayabilirsiniz.

## <a name="locations"></a>Konumlar

Konum bir koşul olarak yapılandırırken, kuruluşlar konumları eklemeyi veya hariç tutmayı seçebilir. Bu adlandırılmış konumlar, genel IPv4 ağ bilgilerini, ülke veya bölgeyi ve hatta belirli ülkelere veya bölgelerle eşlenemeyen bilinmeyen alanları içerebilir. Yalnızca IP aralıkları güvenilir bir konum olarak işaretlenebilir.

**Herhangi bir konum**dahil edildiğinde, bu seçenek sadece adlandırılmış konumlar yapılandırılmış değil, internet üzerinde herhangi bir IP adresi içerir. **Herhangi bir konum**seçerken, yöneticiler tüm **güvenilen** veya **seçili konumları**hariç tutabilirsiniz.

Örneğin, bazı kuruluşlar, kullanıcıları ağa fiziksel karargahları gibi güvenilir bir konumda bağlandığında çok faktörlü kimlik doğrulama gerektirmemeyi seçebilir. Yöneticiler herhangi bir konumu içeren ancak karargah ağları için seçilen konumları hariç tutan bir ilke oluşturabilir.

Konumlar hakkında daha fazla bilgi makalede bulunabilir, [Azure Active Directory Koşullu Erişim konum durumu nedir.](location-condition.md)

## <a name="client-apps-preview"></a>İstemci uygulamaları (önizleme)

Koşullu Erişim ilkeleri varsayılan olarak, modern kimlik doğrulama protokollerini kullanan tarayıcı tabanlı uygulamalar ve uygulamalar için geçerlidir. Bu uygulamalara ek olarak, yöneticiler Exchange ActiveSync istemcilerini ve eski protokolleri kullanan diğer istemcileri dahil etmeyi seçebilirler.

- Tarayıcı
   - Bunlar, SAML, WS-Federation, OpenID Connect gibi protokolleri kullanan web tabanlı uygulamaları veya OAuth gizli istemcisi olarak kaydedilmiş hizmetleri içerir.
- Mobil uygulamalar ve masaüstü istemcileri
   - Modern kimlik doğrulama istemcileri
      - Bu seçenek, Office masaüstü ve telefon uygulamaları gibi uygulamaları içerir.
   - Exchange ActiveSync istemcileri
      - Varsayılan olarak bu Exchange ActiveSync (EAS) protokolünün tüm kullanımını içerir. Yalnızca **desteklenen platformlara Uygula ilkesini** seçmek, iOS, Android ve Windows gibi desteklenen platformlarla sınırlanır.
      - İlke Exchange ActiveSync kullanımını engellediğinde etkilenen kullanıcıya tek bir karantina e-postası gönderilecektir. Bu e-posta, neden engellendikleri hakkında bilgi sağlar ve mümkünse düzeltme yönergeleri içerir.
   - Diğer istemciler
      - Bu seçenek, modern kimlik doğrulamayı desteklemeyen temel/eski kimlik doğrulama protokollerini kullanan istemcileri içerir.
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

Bu koşullar genellikle yönetilen bir aygıt gerektiren, eski kimlik doğrulaması engelleme ve web uygulamalarını engelleme, ancak mobil veya masaüstü uygulamalarına izin vermek için kullanılır.

### <a name="supported-browsers"></a>Desteklenen tarayıcılar

Bu ayar tüm tarayıcılarda çalışır. Ancak, uyumlu bir aygıt gereksinimi gibi bir aygıt ilkesini karşılamak için aşağıdaki işletim sistemleri ve tarayıcılar desteklenir:

| İşletim Sistemi | Browsers (Tarayıcılar) |
| :-- | :-- |
| Windows 10 | Microsoft Edge, Internet Explorer, Chrome |
| Windows 8 / 8.1 | Internet Explorer, Chrome |
| Windows 7 | Internet Explorer, Chrome |
| iOS | Microsoft Edge, Intune Yönetilen Tarayıcı, Safari |
| Android | Microsoft Edge, Intune Yönetilen Tarayıcı, Chrome |
| Windows Phone | Microsoft Edge, Internet Explorer |
| Windows Server 2019 | Microsoft Edge, Internet Explorer, Chrome |
| Windows Server 2016 | Internet Explorer |
| Windows Server 2012 R2 | Internet Explorer |
| Windows Server 2008 R2 | Internet Explorer |
| macOS | Chrome, Safari |

#### <a name="why-do-i-see-a-certificate-prompt-in-the-browser"></a>Tarayıcıda neden bir sertifika istemi görüyorum?

Windows 7, iOS, Android ve macOS Azure AD'da, aygıt Azure AD'ye kaydedildiğinde sağlanan bir istemci sertifikası kullanarak aygıtı tanımlar.  Bir kullanıcı tarayıcı üzerinden ilk giriş yaptığında, kullanıcıdan sertifikayı seçmesi istenir. Kullanıcı tarayıcıyı kullanmadan önce bu sertifikayı seçmelidir.

#### <a name="chrome-support"></a>Chrome desteği

**Windows 10 Creators Update (sürüm 1703)** veya daha sonra Chrome desteği için [Windows 10 Hesapları uzantısını](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji)yükleyin. Koşullu Erişim ilkesi aygıta özel ayrıntılar gerektiriyorsa, bu uzantı gereklidir.

Bu uzantıyı Chrome tarayıcılarına otomatik olarak dağıtmak için aşağıdaki kayıt defteri anahtarını oluşturun:

|    |    |
| --- | --- |
| Yol | HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome\ExtensionInstallForcelist |
| Adı | 1 |
| Tür | REG_SZ (String) |
| Veri | ppnbnpeolgkicgegkbjbjlideopiji;https\://clients2.google.com/service/update2/crx |

**Windows 8.1 ve 7'deki**Chrome desteği için aşağıdaki kayıt defteri anahtarını oluşturun:

|    |    |
| --- | --- |
| Yol | HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls |
| Adı | 1 |
| Tür | REG_SZ (String) |
| Veri | {"pattern":"https://device.login.microsoftonline.com","filter":{"ISSUER":{"CN":"MS-Organization-Access"}}} |

Bu tarayıcılar aygıt kimlik doğrulamasını destekleyerek aygıtın bir ilke yle tanımlanmasını ve doğrulanmasına olanak tanır. Tarayıcı özel modda çalışıyorsa aygıt denetimi başarısız olur.

### <a name="supported-mobile-applications-and-desktop-clients"></a>Desteklenen mobil uygulamalar ve masaüstü istemcileri

Kuruluşlar **Mobil uygulamaları ve masaüstü istemcilerini** istemci uygulaması olarak seçebilir.

Bu ayar, aşağıdaki mobil uygulamalardan ve masaüstü istemcilerinden yapılan erişim denemelerini etkiler:

| İstemci uygulamaları | Hedef Servis | Platform |
| --- | --- | --- |
| Dynamics CRM uygulaması | Dynamics CRM | Windows 10, Windows 8.1, iOS ve Android |
| Posta/Takvim/Kişiler uygulaması, Outlook 2016, Outlook 2013 (modern kimlik doğrulama ile)| Office 365 Exchange Online | Windows 10 |
| Uygulamalar için MFA ve konum ilkesi. Aygıt tabanlı ilkeler desteklenmez.| Tüm Uygulamalarım uygulama hizmeti | Android ve iOS |
| Microsoft Teams Services - Bu, Microsoft Teams'i ve tüm İstemci Uygulamalarını destekleyen tüm hizmetleri denetler - Windows Desktop, iOS, Android, WP ve web istemcisi | Microsoft Teams | Windows 10, Windows 8.1, Windows 7, iOS, Android ve macOS |
| Office 2016 uygulamaları, Office 2013 (modern kimlik doğrulama ile), [OneDrive eşitleme istemcisi](/onedrive/enable-conditional-access) | Office 365 SharePoint Online | Windows 8.1, Windows 7 |
| Office 2016 uygulamaları, Universal Office uygulamaları, Office 2013 (modern kimlik doğrulama ile), [OneDrive sync istemcisi](/onedrive/enable-conditional-access) | Office 365 SharePoint Online | Windows 10 |
| Office 2016 (Word, Excel, PowerPoint, yalnızca OneNote). | Office 365 SharePoint Online | macOS |
| Ofis 2019| Office 365 SharePoint Online | Windows 10, macOS |
| Office mobil uygulamaları | Office 365 SharePoint Online | Android, iOS |
| Office Yammer uygulaması | Office 365 Yammer | Windows 10, iOS, Android |
| Outlook 2019 | Office 365 SharePoint Online | Windows 10, macOS |
| Outlook 2016 (macOS için Office) | Office 365 Exchange Online | macOS |
| Outlook 2016, Outlook 2013 (modern kimlik doğrulamaile), Skype for Business (modern kimlik doğrulamaile) | Office 365 Exchange Online | Windows 8.1, Windows 7 |
| Outlook mobil uygulaması | Office 365 Exchange Online | Android, iOS |
| Power BI uygulaması | Power BI hizmeti | Windows 10, Windows 8.1, Windows 7, Android ve iOS |
| Skype Kurumsal | Office 365 Exchange Online| Android, iOS |
| Visual Studio Takım Hizmetleri uygulaması | Visual Studio Team Services | Windows 10, Windows 8.1, Windows 7, iOS ve Android |

### <a name="exchange-activesync-clients"></a>Exchange ActiveSync istemcileri

- Kuruluşlar yalnızca kullanıcılara veya gruplara ilke atarken Exchange ActiveSync istemcilerini seçebilir. Tüm **kullanıcıları,** **tüm konuk ve dış kullanıcıları**veya **Dizin rollerini** seçmek tüm kullanıcıların engellenmesine neden olur.
- Exchange ActiveSync istemcilerine atanmış bir ilke oluştururken, **Office 365 Exchange Online** ilke için atanan tek bulut uygulaması olmalıdır. 
- Kuruluşlar, **Aygıt platformları** koşulunu kullanarak bu iipolitikasın kapsamını belirli platformlara daraltabilir.

İlke için atanan erişim denetimi **onaylı istemci uygulamasını gerektirir**kullanıyorsa, kullanıcı Outlook mobil istemcisini yüklemeye ve kullanmaya yönlendirilir. **Çok faktörlü kimlik doğrulamanın** gerekli olması durumunda, temel kimlik doğrulama çok faktörlü kimlik doğrulamasını desteklemediği için etkilenen kullanıcılar engellenir.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Koşullu Erişim ile eski kimlik doğrulamasını engelleme](block-legacy-authentication.md)
- [Koşullu Erişim ile onaylanmış istemci uygulamaları gerektirme](app-based-conditional-access.md)

### <a name="other-clients"></a>Diğer istemciler

**Diğer istemcileri**seçerek, IMAP, MAPI, POP, SMTP ve modern kimlik doğrulaması kullanmayan eski Office uygulamaları gibi posta protokolleriyle temel kimlik doğrulamasını kullanan uygulamaları etkileyen bir koşul belirtebilirsiniz.

## <a name="device-state-preview"></a>Aygıt durumu (önizleme)

Aygıt durumu durumu, karma Azure AD'sı birleştirilmiş ve/veya kuruluşun Koşullu Erişim ilkelerinden Microsoft Uyum ilkesiyle uyumlu olarak işaretlenmiş aygıtları hariç tutmak için kullanılabilir.

Örneğin, **Device Hybrid Azure AD** hariç tüm **aygıt durumu** dahil olmak üzere Microsoft Azure *Yönetimi* bulut uygulamasına erişen tüm *kullanıcılar* katıldı ve Aygıt uyumlu **olarak işaretlendi** ve *Access denetimleri*için , **Engelle**. 
   - Bu örnek, yalnızca karma Azure AD'sı bağlanan ve/veya uyumlu olarak işaretlenmiş aygıtlardan Microsoft Azure Yönetimi'ne erişime izin veren bir ilke oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu Erişim: Hibe](concept-conditional-access-grant.md)

- [Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)
