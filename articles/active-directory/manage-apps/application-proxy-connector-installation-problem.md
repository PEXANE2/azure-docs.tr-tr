---
title: Uygulama proxy 'Si aracı bağlayıcısını yükleme sorunu | Microsoft Docs
description: Uygulama proxy 'Si aracı bağlayıcısını yüklerken karşılaşabileceğiniz sorunları giderme
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/21/2018
ms.author: kenwith
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 602ca070bcaefd20585681e409ab85e9d455160a
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84764698"
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a>Uygulama Ara Sunucusu Aracı Bağlayıcısı’nı yüklerken sorun oluşuyor

Microsoft AAD uygulama proxy Bağlayıcısı, bulut kullanılabilir uç noktadan iç etki alanına bağlantı kurmak için giden bağlantıları kullanan bir iç etki alanı bileşenidir.

## <a name="general-problem-areas-with-connector-installation"></a>Bağlayıcı yüklemesiyle ilgili genel sorun alanı

Bağlayıcının yüklemesi başarısız olduğunda, kök neden genellikle aşağıdaki alanlardan biridir:

1.  **Bağlantı** – başarılı bir yükleme işleminin tamamlanabilmesi için yeni bağlayıcının sonraki güven özelliklerini kaydetmesi ve kurması gerekir. Bu, AAD uygulama proxy 'Si bulut hizmetine bağlanarak yapılır.

2.  **Güven kurulumu** – yeni bağlayıcı, otomatik olarak imzalanan bir sertifika oluşturur ve bulut hizmetine kaydeder.

3.  **Yöneticinin kimlik doğrulaması** – yükleme sırasında, kullanıcının bağlayıcı yüklemesini tamamlaması için yönetici kimlik bilgilerini sağlaması gerekir.

> [!NOTE]
> Bağlayıcı yükleme günlükleri,% TEMP% klasöründe bulunabilir ve yükleme hatasına neden olan sorun hakkında ek bilgi sağlanmasına yardımcı olabilir.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>Cloud Application Proxy hizmeti ve Microsoft oturum açma sayfasıyla bağlantıyı doğrulama

**Amaç:** Bağlayıcı makinenin AAD uygulama proxy 'Si kayıt uç noktasına ve Microsoft oturum açma sayfasına bağlanabildiğini doğrulayın.

1.  Bağlayıcı sunucusunda, 443 ve 80 bağlantı noktalarının açık olduğunu doğrulamak için [Telnet](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) veya diğer bağlantı noktası test aracı kullanarak bir bağlantı noktası testi çalıştırın.

2.  Bu bağlantı noktalarından herhangi biri başarılı olmazsa, güvenlik duvarının veya arka uç proxy 'sinin gerekli etki alanlarına ve bağlantı noktalarına erişimi olduğunu doğrulayın, Şirket [içi ortamınızı hazırlayın](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment).

3.  Bir tarayıcı (ayrı bir sekme) açın ve aşağıdaki Web sayfasına gidin: `https://login.microsoftonline.com` Bu sayfada oturum açabildiğinizden emin olun.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-certificate"></a>Uygulama proxy 'Si güven sertifikası için makine ve arka uç bileşenleri desteğini doğrulama

**Amaç:** Bağlayıcı makinesinin, arka uç proxy 'sinin ve güvenlik duvarının, daha sonra güven için bağlayıcı tarafından oluşturulan sertifikayı destekleyebildiğini ve sertifikanın geçerli olduğunu doğrulayın.

>[!NOTE]
>Bağlayıcı, TLS 1.2 tarafından desteklenen bir SHA512 olur sertifikası oluşturmaya çalışır. Makine veya arka uç güvenlik duvarı ve proxy TLS 1.2 'yi desteklemiyorsa, yükleme başarısız olur.
>
>

**Gerekli önkoşulları gözden geçirin:**

1.  Makinenin TLS 1.2 'yi desteklediğini doğrulayın – 2012 R2 'den sonraki tüm Windows sürümleri TLS 1,2 ' i desteklemelidir. Bağlayıcı makineniz 2012 R2 veya daha önceki bir sürümden yüklüyse, şu KBs 'lerin makinede yüklü olduğundan emin olun:<https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Ağ yöneticinize başvurun ve arka uç proxy 'sinin ve güvenlik duvarının giden trafik için SHA512 olur engellemediğinden emin olun.

**İstemci sertifikasını doğrulamak için:**

Geçerli istemci sertifikasının parmak izini doğrulayın. Sertifika depolama alanı%ProgramData%\microsoft\Microsoft AAD uygulama proxy 'Si Connector\Config\TrustSettings.xml bulunabilir

```
<?xml version="1.0" encoding="utf-8"?>
<ConnectorTrustSettingsFile xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <CloudProxyTrust>
    <Thumbprint>4905CC64B2D81BBED60962ECC5DCF63F643CCD55</Thumbprint>
    <IsInUserStore>false</IsInUserStore>
  </CloudProxyTrust>
</ConnectorTrustSettingsFile>
```

Olası **ısınuserstore** değerleri ve anlamları aşağıda verilmiştir:

- **false** -istemci sertifikası, yükleme sırasında veya Register-AppProxyConnector komutuyla başlatılan kayıt sırasında oluşturulmuştur. Bu, yerel makinenin sertifika deposundaki kişisel kapsayıcıda saklanır. 

Sertifikayı doğrulamak için aşağıdaki adımları izleyin:

1. **Certlm. msc** çalıştırma
2. Yönetim konsolunda kişisel kapsayıcıyı genişletin ve Sertifikalar ' a tıklayın.
3. **Connectorregistrationca.msappproxy.net** tarafından verilen sertifikayı bulma

- **doğru** -otomatik olarak yenilenen sertifika, ağ hizmetinin Kullanıcı sertifikası deposundaki kişisel kapsayıcıda depolanır. 

Sertifikayı doğrulamak için aşağıdaki adımları izleyin:

1. [PsTools.zip](https://docs.microsoft.com/sysinternals/downloads/pstools) indir
2. Paketten [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec) 'yi ayıklayın ve yükseltilmiş bir komut isteminden **PsExec-i-u "NT authorıty\servıce" cmd.exe** çalıştırın.
3. Yeni görünen komut isteminde **certmgr. msc** dosyasını çalıştır
2. Yönetim konsolunda kişisel kapsayıcıyı genişletin ve Sertifikalar ' a tıklayın.
3. **Connectorregistrationca.msappproxy.net** tarafından verilen sertifikayı bulma

**İstemci sertifikasını yenilemek için:**

Bir bağlayıcı birkaç ay için hizmete bağlı değilse, sertifikaları güncel olmayabilir. Sertifika yenileme hatası, zaman aşımına uğradı bir sertifikaya yol açar. Bu, bağlayıcı hizmetinin çalışmayı durdurmasına neden olur. Olay 1000, bağlayıcının yönetim günlüğüne kaydedilir:

"Bağlayıcı yeniden kaydı başarısız oldu: bağlayıcı güven sertifikasının geçerliliği bitti. Bağlayıcınızı yeniden kaydettirmek için bağlayıcının çalıştığı bilgisayarda PowerShell cmdlet 'i Register-AppProxyConnector komutunu çalıştırın. "

Bu durumda, kayıt tetiklemek için bağlayıcıyı kaldırın ve yeniden yükleyin veya aşağıdaki PowerShell komutlarını çalıştırın:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

Register-AppProxyConnector komutu hakkında daha fazla bilgi edinmek için lütfen bkz [. Azure AD uygulama ara sunucusu Bağlayıcısı için katılımsız yükleme betiği oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-register-connector-powershell)

## <a name="verify-admin-is-used-to-install-the-connector"></a>Bağlayıcıyı yüklemek için yöneticinin kullanıldığını doğrulayın

**Amaç:** Bağlayıcıyı yüklemeye çalışan kullanıcının doğru kimlik bilgilerine sahip bir yönetici olduğunu doğrulayın. Şu anda, yüklemenin başarılı olması için kullanıcının en az bir uygulama Yöneticisi olması gerekir.

**Kimlik bilgilerinin doğru olduğunu doğrulamak için:**

' A bağlanın `https://login.microsoftonline.com` ve aynı kimlik bilgilerini kullanın. Oturum açmanın başarılı olduğundan emin olun. **Azure Active Directory**  - &gt; **Kullanıcılar ve gruplar**  - &gt; **tüm kullanıcılar**Azure Active Directory giderek Kullanıcı rolünü kontrol edebilirsiniz. 

Kullanıcı hesabınızı ve ardından ortaya çıkan menüdeki "Dizin rolü" nü seçin. Seçili rolün "uygulama Yöneticisi" olduğunu doğrulayın. Bu adımlarla ilgili sayfaların hiçbirine erişemiyorsanız, gerekli rolünüzün olması gerekmez.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Uygulama Ara Sunucusu bağlayıcılarını anlama](application-proxy-connectors.md)
