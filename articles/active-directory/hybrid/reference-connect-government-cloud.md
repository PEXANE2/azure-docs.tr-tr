---
title: 'Azure AD Connect: Azure Kamu için karma kimlik konuları'
description: Azure AD Connect'i devlet bulutuyla dağıtmak için özel hususlar.
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
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81378931"
---
# <a name="hybrid-identity-considerations-for-azure-government"></a>Azure Kamu için karma kimlik konuları 
Aşağıdaki belgede, Azure Genel Durumu bulutuyla karma bir ortam uygulanmasına yönelik hususlar açıklanmaktadır.  Bu bilgiler, Azure Genel Bulutu ile çalışan yöneticiler ve mimarlar için başvuru olarak sağlanır.  
> [!NOTE] 
> Şirket içi bir REKLAM ortamını Azure Governemnt bulutuyla entegre etmek için [Azure AD Connect'in](https://www.microsoft.com/download/details.aspx?id=47594)en son sürümüne yükseltmeniz gerekir. 

> [!NOTE] 
> ABD Hükümeti DoD Bitiş Noktalarının tam listesi için [belgelere](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-dod-endpoints) bakın 

## <a name="pass-through-authentication"></a>Geçiş Kimlik Doğrulaması 
Geçiş kimlik doğrulaması (PTA) ve Azure Kamu bulutunun uygulanması için aşağıdaki bilgiler sağlanır.

### <a name="allow-access-to-urls"></a>URL'lere erişime izin verme  
Geçiş kimlik doğrulaması aracısını dağıtmadan önce, sunucularınız ile Azure AD arasında bir güvenlik duvarı olup olmadığını doğrulayın. Güvenlik duvarınız veya proxy'niz DNS'nin beyaz listeye alınmasına izin veriyorsa, aşağıdaki bağlantıları ekleyin: 
> [!NOTE]
> Aşağıdaki kılavuz, Azure Kamu ortamları için [Uygulama Proxy bağlayıcısını](https://aka.ms/whyappproxy) yüklemek için de geçerlidir.

|URL'si |Nasıl kullanılır?|
|-----|-----| 
|*.msappproxy.us *.servicebus.usgovcloudapi.net|Aracı ve Azure AD bulut hizmeti arasındaki iletişim |
|mscrl.microsoft.us:80 crl.microsoft.us:80 </br>ocsp.msocsp.us:80 www.microsoft.us:80| Aracı, sertifikaları doğrulamak için bu URL'leri kullanır.| 
|login.windows.us secure.aadcdn.microsoftonline-p.com *.microsoftonline.us </br> *.microsoftonline-p.us </br>*.msauth.net </br> *.msauthimages.net </br>*.msecnd.net</br>*.msftauth.net </br>*.msftauthimages.net</br>*.phonefactor.net </br>enterpriseregistration.windows.net</br>management.azure.com </br>policykeyservice.dc.ad.msft.net</br>ctdl.windowsupdate.us:80| Aracı, kayıt işlemi sırasında bu URL'leri kullanır.| 

### <a name="install-the-agent-for-the-azure-government-cloud"></a>Azure Devlet bulutu için aracıyı yükleme 
Azure Devlet bulutu aracısını yüklemek için şu özel adımları izlemeniz gerekir: Komut satırı terminalinde aracıyı yüklemek için çalıştırılabilen klasöre gidin. Yüklemenin Azure Kamu için olduğunu belirten aşağıdaki komutu çalıştırın. 

Passthrough Kimlik Doğrulama için: 
```
AADConnectAuthAgentSetup.exe ENVIRONMENTNAME="AzureUSGovernment"
```

Uygulama Proxy için:
```
AADApplicationProxyConnectorInstaller.exe ENVIRONMENTNAME="AzureUSGovernment" 
```

## <a name="single-sign-on"></a>Tek işaret 
Azure AD Connect sunucunuzu kurun: Oturum açma yönteminiz olarak Geçiş Kimlik Doğrulaması kullanıyorsanız, ek ön koşul denetimi gerekmez. Oturum açma yönteminiz olarak parola karma senkronizasyonunu kullanıyorsanız ve Azure AD Connect ile Azure AD arasında bir güvenlik duvarı varsa, aşağıdakileri sağlayın:
- Sürüm 1.1.644.0 veya daha sonra Azure AD Connect'i kullanırsınız. 
- Güvenlik duvarınız veya proxy'niz DNS'nin beyaz listeye alınmasına izin veriyorsa, bağlantıları 443 portundaki *.msapproxy.us URL'lerine ekleyin. Değilse, haftalık olarak güncelleştirilen Azure veri merkezi IP aralıklarına erişin. Bu ön koşul yalnızca özelliği etkinleştirdiğinizde geçerlidir. Gerçek kullanıcı oturum açmaları için gerekli değildir. 

### <a name="rolling-out-seamless-sso"></a>Sorunsuz SSO'yu kullanıma sayıl 
Aşağıda verilen talimatları kullanarak Kesintisiz SSO'yu kullanıcılarınıza kademeli olarak dağıtabilirsiniz. Active Directory'de Grup İlkesi'ni kullanarak tüm veya seçili kullanıcıların Intranet bölge ayarlarına aşağıdaki Azure REKLAM URL'sini ekleyerek işe başlarsınız:https://autologon.microsoft.us 

Buna ek olarak, Grup İlkesi aracılığıyla komut dosyası aracılığıyla durum çubuğuna güncelleştirmelere izin ver adlı bir Intranet bölgesi ilkesi ayarını etkinleştirmeniz gerekir. Tarayıcı hususlar Mozilla Firefox (tüm platformlar) Mozilla Firefox otomatik olarak Kerberos kimlik doğrulaması kullanmaz. Her kullanıcı aşağıdaki adımları kullanarak Azure REKLAM URL'sini Firefox ayarlarına el ile eklemelidir: 
1. Firefox'u çalıştırın ve adres çubuğuna config adresini girin. Gördüğünüz bildirimleri kapatın. 
2. Network.negotiate-auth.trusted-uris tercihini arayın. Bu tercih, Firefox'un Kerberos kimlik doğrulaması için güvendiği siteleri listeler. 
3. Sağ tıklatın ve Değiştir'i seçin. 
4. Sahaya girin. https://autologon.microsoft.us
5. Tamam'ı seçin ve ardından tarayıcıyı yeniden açın. 

### <a name="microsoft-edge-based-on-chromium-all-platforms"></a>Microsoft Edge Krom (tüm platformlar) dayalı 
Ortamınızdaki `AuthNegotiateDelegateAllowlist`  `AuthServerAllowlist` ilke ayarlarını geçersiz kardıysanız, Azure AD'nin URL'sinihttps://autologon.microsoft.us) de (onlara) eklediğinizden emin olun. 

### <a name="google-chrome-all-platforms"></a>Google Chrome (tüm platformlar) 
Ortamınızdaki `AuthNegotiateDelegateWhitelist`  `AuthServerWhitelist` ilke ayarlarını geçersiz kardıysanız, Azure AD'nin URL'sinihttps://autologon.microsoft.us) de (onlara) eklediğinizden emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
[Geçiş Kimlik Doğrulama](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites)
[Tek Oturum Açma](how-to-connect-sso-quick-start.md#step-1-check-the-prerequisites) 
