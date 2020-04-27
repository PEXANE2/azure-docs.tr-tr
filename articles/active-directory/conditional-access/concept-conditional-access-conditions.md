---
title: Koşullu erişim ilkesindeki koşullar-Azure Active Directory
description: Azure AD koşullu erişim ilkesinde koşullar nelerdir?
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
ms.openlocfilehash: 622950c394d59d8ba504901f5bb0eea6bc04707f
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "82160724"
---
# <a name="conditional-access-conditions"></a>Koşullu erişim: koşullar

Bir yönetici, koşullu erişim ilkesinde, ilke kararlarını iyileştirmek için risk, cihaz platformu veya konum gibi koşullardan gelen sinyalleri kullanabilir. 

![Koşullu erişim ilkesi tanımlama ve koşulları belirtme](./media/concept-conditional-access-conditions/conditional-access-conditions.png)

Ayrıntılı ve belirli koşullu erişim ilkeleri oluşturmak için birden çok koşul birleştirilebilir.

Örneğin, hassas bir uygulamaya erişirken, yönetici, kimlik koruması ve konumdan çok faktörlü kimlik doğrulaması gibi diğer denetimlere ek olarak erişim kararlarına yönelik oturum açma risk bilgilerini çarpanlara katmayabilir.

## <a name="sign-in-risk"></a>Oturum açma riski

[Kimlik korumasına](../identity-protection/overview-identity-protection.md)erişimi olan müşteriler için, oturum açma riski koşullu erişim ilkesinin bir parçası olarak değerlendirilebilirler. Oturum açma riski, belirli bir kimlik doğrulama isteğinin kimlik sahibi tarafından yetkilendirilmemiş olma olasılığını temsil eder. Makalelerde oturum [açma riski hakkında](../identity-protection/concept-identity-protection-risks.md#sign-in-risk) daha fazla bilgi bulabilirsiniz, risk ve [nasıl yapılır: risk ilkelerini yapılandırma ve etkinleştirme](../identity-protection/howto-identity-protection-configure-risk-policies.md).

## <a name="device-platforms"></a>Cihaz platformları

Cihaz platformu, bir cihazda çalışan işletim sistemine göre belirlenir. Azure AD, cihaz tarafından sunulan Kullanıcı Aracısı dizeleri gibi bilgileri kullanarak platformu tanımlar. Kullanıcı Aracısı dizeleri değiştirilebilen için bu bilgiler doğrulanmamış olur. Cihaz platformu Microsoft Intune cihaz uyumluluk ilkeleriyle veya bir blok bildiriminin parçası olarak konser 'da kullanılmalıdır. Varsayılan değer tüm cihaz platformları için geçerlidir.

Azure AD koşullu erişim aşağıdaki cihaz platformlarını destekler:

- Android
- iOS
- Windows Phone
- Windows
- macOS

> [!WARNING]
> Microsoft, koşullu erişim ilkeleri ve macOS 10.15.4 tabanlı cihazlarla ilgili bir sorunu algılar. Blog gönderisine daha fazla bilgi bulunabilir, [bilinen sorun: koşullu erişim macOS 10.15.4 Native mail Client/diğer uygulamaları beklenmedik şekilde engelliyor](https://techcommunity.microsoft.com/t5/intune-customer-success/known-issue-conditional-access-unexpectedly-blocking-macos-10-15/ba-p/1322283).

**Diğer istemciler** koşulunu kullanarak eski kimlik doğrulamasını engellerseniz, cihaz platformu koşulunu da ayarlayabilirsiniz.

## <a name="locations"></a>Konumlar

Konum bir koşul olarak yapılandırılırken, kuruluşlar konumları dahil etmek veya hariç tutmak seçebilirler. Bu adlandırılmış konumlar, genel IPv4 ağ bilgilerini, ülkeyi veya bölgeyi, hatta belirli ülkelere veya bölgelere eşlenmeyen bilinmeyen alanları içerebilir. Yalnızca IP aralıkları güvenilir bir konum olarak işaretlenebilir.

**Herhangi bir konum**dahil edildiğinde, bu seçenek yalnızca adlandırılmış konumlar yapılandırılmamış Internet üzerindeki IP adreslerini içerir. **Herhangi bir konum**seçerken, Yöneticiler **tüm güvenilen** veya **Seçilen konumların**dışlanmasını seçebilirler.

Örneğin, bazı kuruluşlar, kullanıcıları ağa fiziksel Merkez gibi güvenilir bir konumda bağlıyken çok faktörlü kimlik doğrulaması gerektirmeyebilir. Yöneticiler herhangi bir konum içeren bir ilke oluşturabilir, ancak bu kişilerin Merkez ağları için seçili konumları dışlar.

Konumlar hakkında daha fazla bilgi için, [Azure Active Directory Koşullu erişim içindeki konum koşulunun ne olduğunu](location-condition.md)bulabilirsiniz.

## <a name="client-apps-preview"></a>İstemci uygulamaları (Önizleme)

Koşullu erişim ilkeleri, varsayılan olarak modern kimlik doğrulama protokollerini kullanan tarayıcı tabanlı uygulamalar ve uygulamalar için geçerlidir. Yöneticiler, bu uygulamaların yanı sıra Exchange ActiveSync istemcilerini ve eski protokolleri kullanan diğer istemcileri de dahil etmek için seçim yapabilir.

- Tarayıcı
   - Bunlar SAML, WS-Federation, OpenID Connect gibi protokolleri kullanan Web tabanlı uygulamaları veya OAuth gizli istemci olarak kaydedilmiş hizmetleri içerir.
- Mobil uygulamalar ve Masaüstü istemcileri
   - Modern kimlik doğrulama istemcileri
      - Bu seçenek, Office Masaüstü ve telefon uygulamaları gibi uygulamaları içerir.
   - Exchange ActiveSync istemcileri
      - Varsayılan olarak, Exchange ActiveSync (EAS) protokolünün tüm kullanımını içerir. **İlkeyi yalnızca desteklenen platformlara Uygula** seçeneğinin belirlenmesi IOS, Android ve Windows gibi desteklenen platformlarla sınırlı olacaktır.
      - İlke Exchange ActiveSync kullanımını engellediğinde, etkilenen Kullanıcı tek bir karantina e-postası alır. Bu e-posta, neden engellendikleri hakkında bilgi sağlar ve mümkünse düzeltme yönergelerini içerir.
   - Diğer istemciler
      - Bu seçenek, modern kimlik doğrulamasını desteklemeyen temel/eski kimlik doğrulama protokollerini kullanan istemcileri içerir.
         - Kimliği doğrulanmış SMTP-e-posta iletileri göndermek için POP ve IMAP istemci tarafından kullanılır.
         - Otomatik bulma-Exchange Online 'da posta kutularını bulmak ve bağlamak için Outlook ve EAS istemcileri tarafından kullanılır.
         - Exchange Online PowerShell-uzak PowerShell ile Exchange Online 'a bağlanmak için kullanılır. Exchange Online PowerShell için temel kimlik doğrulamasını engellerseniz, bağlanmak için Exchange Online PowerShell modülünü kullanmanız gerekir. Yönergeler için bkz. [Multi-Factor Authentication kullanarak Exchange Online PowerShell 'e bağlanma](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell).
         - Exchange Web Hizmetleri (EWS)-Outlook, Mac için Outlook ve üçüncü taraf uygulamalar tarafından kullanılan bir programlama arabirimidir.
         - IMAP4-IMAP e-posta istemcileri tarafından kullanılır.
         - HTTP üzerinden MAPI (MAPI/HTTP)-Outlook 2010 ve üzeri tarafından kullanılır.
         - Çevrimdışı adres defteri (OAB)-Outlook tarafından indirilen ve kullanılan adres listesi koleksiyonlarının bir kopyası.
         - Outlook her yerde (HTTP üzerinden RPC)-Outlook 2016 ve öncesi tarafından kullanılır.
         - Outlook hizmeti-Windows 10 için posta ve takvim uygulaması tarafından kullanılır.
         - POP3-POP e-posta istemcileri tarafından kullanılır.
         - Raporlama Web Hizmetleri-Exchange Online 'daki rapor verilerini almak için kullanılır.

Bu koşullar genellikle yönetilen bir cihaz gerektirdiğinde, eski kimlik doğrulamasını engelleyerek ve mobil veya masaüstü uygulamalarına izin veren bir şekilde kullanılır.

### <a name="supported-browsers"></a>Desteklenen tarayıcılar

Bu ayar tüm tarayıcılarla birlikte kullanılabilir. Bununla birlikte, uyumlu bir cihaz gereksinimi gibi bir cihaz ilkesini karşılamak için, aşağıdaki işletim sistemleri ve tarayıcılar desteklenir:

| İşletim Sistemi | Browsers (Tarayıcılar) |
| :-- | :-- |
| Windows 10 | Microsoft Edge, Internet Explorer, Chrome |
| Windows 8/8,1 | Internet Explorer, Chrome |
| Windows 7 | Internet Explorer, Chrome |
| iOS | Microsoft Edge, Intune Managed Browser, Safari |
| Android | Microsoft Edge, Intune Managed Browser, Chrome |
| Windows Phone | Microsoft Edge, Internet Explorer |
| Windows Server 2019 | Microsoft Edge, Internet Explorer, Chrome |
| Windows Server 2016 | Internet Explorer |
| Windows Server 2012 R2 | Internet Explorer |
| Windows Server 2008 R2 | Internet Explorer |
| macOS | Chrome, Safari |

#### <a name="why-do-i-see-a-certificate-prompt-in-the-browser"></a>Neden tarayıcıda bir sertifika istemi görüyorum?

Windows 7, iOS, Android ve macOS Azure AD, cihaz Azure AD 'ye kaydedildiğinde sağlanan bir istemci sertifikası kullanarak cihazı tanımlar.  Kullanıcı tarayıcıda ilk kez oturum açtığında, kullanıcıdan sertifikayı seçmesi istenir. Kullanıcının tarayıcıyı kullanmadan önce bu sertifikayı seçmeniz gerekir.

#### <a name="chrome-support"></a>Chrome desteği

**Windows 10 Creators Update (sürüm 1703)** veya sonraki sürümlerde Chrome desteği için [Windows 10 hesapları uzantısını](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji)yükler. Koşullu erişim ilkesi cihaza özgü ayrıntılar gerektirdiğinde bu uzantı gereklidir.

Bu uzantıyı Chrome tarayıcılarına otomatik olarak dağıtmak için aşağıdaki kayıt defteri anahtarını oluşturun:

|    |    |
| --- | --- |
| Yol | HKEY_LOCAL_MACHINE \Software\Policies\Google\Chrome\ExtensionInstallForcelist |
| Adı | 1 |
| Tür | REG_SZ (dize) |
| Veriler | ppnbnpeolgkicgegkbkbjmhlideopiji; https\://clients2.Google.com/service/update2/crx |

**Windows 8.1 ve 7**' de Chrome desteği için aşağıdaki kayıt defteri anahtarını oluşturun:

|    |    |
| --- | --- |
| Yol | HKEY_LOCAL_MACHINE \SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls |
| Adı | 1 |
| Tür | REG_SZ (dize) |
| Veriler | {"model": "https://device.login.microsoftonline.com", "filtre": {"veren": {"CN": "MS-Organization-Access"}}} |

Bu tarayıcılar cihaz kimlik doğrulamasını destekler, bu da cihazın bir ilkeye göre tanımlanmasına ve doğrulanmasını sağlar. Tarayıcı özel modda çalışıyorsa cihaz denetimi başarısız olur.

### <a name="supported-mobile-applications-and-desktop-clients"></a>Desteklenen mobil uygulamalar ve Masaüstü istemcileri

Kuruluşlar, **mobil uygulamaları ve Masaüstü istemcileri** istemci uygulaması olarak seçebilir.

Bu ayar, aşağıdaki mobil uygulamalardan ve Masaüstü istemcilerinden yapılan erişim girişimlerini etkiler:

| İstemci uygulamaları | Hedef hizmet | Platform |
| --- | --- | --- |
| Dynamics CRM uygulaması | Dynamics CRM | Windows 10, Windows 8.1, iOS ve Android |
| Posta/Takvim/Kişiler uygulaması, Outlook 2016, Outlook 2013 (modern kimlik doğrulaması ile)| Office 365 Exchange Online | Windows 10 |
| Uygulamalar için MFA ve konum ilkesi. Cihaz tabanlı ilkeler desteklenmez.| Tüm Apps App Service | Android ve iOS |
| Microsoft ekipleri Hizmetleri-bu, Microsoft ekiplerini ve tüm Istemci uygulamalarını (Windows Masaüstü, iOS, Android, WP ve Web istemcisi) destekleyen tüm hizmetleri denetler | Microsoft Teams | Windows 10, Windows 8.1, Windows 7, iOS, Android ve macOS |
| Office 2016 uygulamaları, Office 2013 (modern kimlik doğrulaması ile), [OneDrive eşitleme istemcisi](/onedrive/enable-conditional-access) | Office 365 SharePoint Online | Windows 8.1, Windows 7 |
| Office 2016 uygulamaları, evrensel Office uygulamaları, Office 2013 (modern kimlik doğrulaması ile), [OneDrive eşitleme istemcisi](/onedrive/enable-conditional-access) | Office 365 SharePoint Online | Windows 10 |
| Office 2016 (Word, Excel, PowerPoint, yalnızca OneNote). | Office 365 SharePoint Online | macOS |
| Office 2019| Office 365 SharePoint Online | Windows 10, macOS |
| Office mobil uygulamaları | Office 365 SharePoint Online | Android, iOS |
| Office Yammer uygulaması | Office 365 Yammer | Windows 10, iOS, Android |
| Outlook 2019 | Office 365 SharePoint Online | Windows 10, macOS |
| Outlook 2016 (macOS için Office) | Office 365 Exchange Online | macOS |
| Outlook 2016, Outlook 2013 (modern kimlik doğrulaması ile), Skype Kurumsal (modern kimlik doğrulaması ile) | Office 365 Exchange Online | Windows 8.1, Windows 7 |
| Outlook mobil uygulaması | Office 365 Exchange Online | Android, iOS |
| Power BI uygulaması | Power BI hizmeti | Windows 10, Windows 8.1, Windows 7, Android ve iOS |
| Skype Kurumsal | Office 365 Exchange Online| Android, iOS |
| Visual Studio Team Services uygulaması | Visual Studio Team Services | Windows 10, Windows 8.1, Windows 7, iOS ve Android |

### <a name="exchange-activesync-clients"></a>Exchange ActiveSync istemcileri

- Kuruluşlar kullanıcılara veya gruplara ilke atarken yalnızca Exchange ActiveSync istemcileri seçebilir. Tüm **Kullanıcılar**, tüm **Konuk ve dış kullanıcılar**ya da **Dizin rolleri** , tüm kullanıcıların engellenmesine neden olur.
- Exchange ActiveSync istemcilerine atanan bir ilke oluştururken, **Office 365 Exchange Online** , ilkeye atanmış tek bulut uygulaması olmalıdır. 
- Kuruluşlar, cihaz platformları koşulunu kullanarak bu ilkenin kapsamını belirli platformlara **daraltabilirler** .

İlkeye atanan erişim denetimi **onaylanan istemci uygulaması gerektir**' i kullanıyorsa, Kullanıcı Outlook Mobil istemcisini yüklemek ve kullanmak üzere yönlendirilir. **Multi-Factor Authentication** gerekli olduğunda, temel kimlik doğrulaması çok faktörlü kimlik doğrulamasını desteklemediğinden, etkilenen kullanıcılar engellenir.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Koşullu erişimle eski kimlik doğrulamasını engelleyin](block-legacy-authentication.md)
- [Koşullu erişim ile onaylanan istemci uygulamaları gerektirme](app-based-conditional-access.md)

### <a name="other-clients"></a>Diğer istemciler

**Diğer istemcileri**seçerek, modern kimlik doğrulaması kullanmayan IMAP, MAPI, pop, SMTP ve eski Office uygulamaları gibi posta protokolleriyle temel kimlik doğrulaması kullanan uygulamaları etkileyen bir koşul belirtebilirsiniz.

## <a name="device-state-preview"></a>Cihaz durumu (Önizleme)

Cihaz durumu koşulu, karma Azure AD 'ye katılmış ve/veya cihazların bir kuruluşun koşullu erişim ilkelerinden bir Microsoft Intune uyumluluk ilkesiyle uyumlu olarak işaretlenmiş cihazları dışlamak için kullanılabilir.

Örneğin, **cihazın karma Azure AD 'ye katılmış** ve **uyumlu olarak işaretlenmiş** cihaz **durumu** dahil olmak üzere *Microsoft Azure yönetimi* bulut uygulamasına erişen *tüm kullanıcılar* ve *erişim denetimleri*için **bloğu**. 
   - Bu örnek, karma Azure AD 'ye katılmış ve/veya uyumlu olarak işaretlenmiş cihazlardan Microsoft Azure yönetimine yalnızca erişim sağlayan bir ilke oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim: ver](concept-conditional-access-grant.md)

- [Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)
