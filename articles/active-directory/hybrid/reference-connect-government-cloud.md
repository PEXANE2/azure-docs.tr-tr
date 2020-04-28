---
title: 'Azure AD Connect: Azure Kamu için karma kimlik konuları'
description: Kamu bulutuyla Azure AD Connect dağıtmaya yönelik özel konular.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/14/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ad28beb68afb5207e7b36c5d76c4dac808c5114
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81378931"
---
# <a name="hybrid-identity-considerations-for-azure-government"></a>Azure Kamu planlarında hibrit kimlik hakkında dikkate alınması gerekenler 
Aşağıdaki belgede, Azure Kamu bulutu ile hibrit bir ortam uygulamayla ilgili konular açıklanmaktadır.  Bu bilgiler, Azure Kamu bulutuyla çalışan yöneticiler ve Mimarlar için başvuru olarak sağlanır.  
> [!NOTE] 
> Şirket içi bir AD ortamını Azure governemnt bulutu ile tümleştirebilmek için [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594)en son sürümüne yükseltmeniz gerekir. 

> [!NOTE] 
> ABD kamu DoD uç noktalarının tam listesi için [belgelere](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-dod-endpoints) bakın 

## <a name="pass-through-authentication"></a>Geçişli kimlik doğrulaması 
Doğrudan kimlik doğrulama (PTA) ve Azure Kamu Bulutu 'nın uygulanması için aşağıdaki bilgiler verilmiştir.

### <a name="allow-access-to-urls"></a>URL 'lere erişime izin ver  
Doğrudan kimlik doğrulama aracısını dağıtılmadan önce sunucularınız ve Azure AD arasında bir güvenlik duvarı olup olmadığını doğrulayın. Güvenlik duvarınız veya ara sunucunuz DNS beyaz listeye izin veriyorsa aşağıdaki bağlantıları ekleyin: 
> [!NOTE]
> Aşağıdaki kılavuz, Azure Kamu ortamları için [uygulama proxy bağlayıcısını](https://aka.ms/whyappproxy) yüklemeye de yöneliktir.

|URL'si |Nasıl kullanılır?|
|-----|-----| 
|*. msappproxy.us *. servicebus.usgovcloudapi.net|Aracı ve Azure AD bulut hizmeti arasındaki iletişim |
|mscrl.microsoft.us:80 crl.microsoft.us:80 </br>ocsp.msocsp.us:80 www.microsoft.us:80| Aracı, sertifikaları doğrulamak için bu URL 'Leri kullanır.| 
|Login.Windows.us Secure.aadcdn.microsoftonline-p.com *. microsoftonline.us </br> *. microsoftonline-p.us </br>*. msauth.net </br> *. msauthimages.net </br>*. msecnd.net</br>*. msftauth.net </br>*. msftauthimages.net</br>*. phonefactor.net </br>enterpriseregistration.windows.net</br>management.azure.com </br>policykeyservice.dc.ad.msft.net</br>ctdl.windowsupdate.us:80| Aracı, kayıt işlemi sırasında bu URL 'Leri kullanır.| 

### <a name="install-the-agent-for-the-azure-government-cloud"></a>Azure Kamu Bulutu için aracıyı yükler 
Aracıyı Azure Kamu Bulutu için yüklemek üzere şu adımları izlemeniz gerekir: komut satırı terminalinde, aracıyı yüklemek için yürütülebilir dosyanın bulunduğu klasöre gidin. Yüklemesinin Azure Kamu için olduğunu belirten aşağıdaki komutu çalıştırın. 

Geçiş kimlik doğrulaması için: 
```
AADConnectAuthAgentSetup.exe ENVIRONMENTNAME="AzureUSGovernment"
```

Uygulama proxy 'Si için:
```
AADApplicationProxyConnectorInstaller.exe ENVIRONMENTNAME="AzureUSGovernment" 
```

## <a name="single-sign-on"></a>Çoklu oturum açma 
Azure AD Connect sunucunuzu ayarlama: geçiş kimlik doğrulamasını oturum açma yönteminiz olarak kullanırsanız, ek önkoşul denetimi gerekli değildir. Oturum açma yönteminiz olarak parola karması eşitlemesi kullanıyorsanız ve Azure AD Connect ile Azure AD arasında bir güvenlik duvarı varsa, aşağıdakileri doğrulayın:
- Azure AD Connect sürüm 1.1.644.0 veya üstünü kullanıyorsunuz. 
- Güvenlik duvarınız veya proxy DNS beyaz listeye izin veriyorsa, bağlantı noktası 443 üzerinden *. msapproxy.us URL 'Lerine bağlantı ekleyin. Aksi takdirde, haftalık olarak güncellenen Azure veri merkezi IP aralıklarına erişime izin verin. Bu önkoşul yalnızca özelliği etkinleştirdiğinizde geçerlidir. Bu, gerçek Kullanıcı oturum açma işlemleri için gerekli değildir. 

### <a name="rolling-out-seamless-sso"></a>Sorunsuz SSO kullanıma alınıyor 
Aşağıda belirtilen yönergeleri kullanarak kullanıcılarınıza sorunsuz SSO 'yu yavaş bir şekilde dağıtabilirsiniz. Aşağıdaki Azure AD URL 'sini, Active Directory grup ilkesi kullanarak, tüm kullanıcıların Intranet bölgesi ayarlarına ekleyerek başlayabilirsiniz:https://autologon.microsoft.us 

Ayrıca, grup ilkesi aracılığıyla betik aracılığıyla durum çubuğuna güncelleştirmelere Izin ver adlı bir Intranet bölgesi ilkesi ayarını etkinleştirmeniz gerekir. Tarayıcı konuları Mozilla Firefox (tüm platformlar) Mozilla Firefox, Kerberos kimlik doğrulamasını otomatik olarak kullanmaz. Her kullanıcının aşağıdaki adımları kullanarak, Azure AD URL 'sini Firefox ayarlarına el ile eklemesi gerekir: 
1. Firefox 'u çalıştırın ve adres çubuğuna about: config yazın. Gördüğünüz tüm bildirimleri kapatın. 
2. Network. Negotiate-Auth. Trusted-uris tercihini arayın. Bu tercih edilecek Kerberos kimlik doğrulaması için Firefox 'un güvenilen siteleri listelenir. 
3. Sağ tıklayın ve Değiştir ' i seçin. 
4. Alana https://autologon.microsoft.us girin.
5. Tamam ' ı seçin ve ardından tarayıcıyı yeniden açın. 

### <a name="microsoft-edge-based-on-chromium-all-platforms"></a>Kmıum temelinde Microsoft Edge (tüm platformlar) 
 `AuthNegotiateDelegateAllowlist`  `AuthServerAllowlist`Ortamınızdaki ilke ayarlarını veya bu ayarları geçersiz kıldıysanız, Azure AD 'nin URL 'sini (https://autologon.microsoft.us) bunlara de) eklemediğinizden emin olun. 

### <a name="google-chrome-all-platforms"></a>Google Chrome (tüm platformlar) 
 `AuthNegotiateDelegateWhitelist`  `AuthServerWhitelist`Ortamınızdaki ilke ayarlarını veya bu ayarları geçersiz kıldıysanız, Azure AD 'nin URL 'sini (https://autologon.microsoft.us) bunlara de) eklemediğinizden emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
[Geçişli kimlik doğrulaması](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites)
[Çoklu oturum açma](how-to-connect-sso-quick-start.md#step-1-check-the-prerequisites) 
