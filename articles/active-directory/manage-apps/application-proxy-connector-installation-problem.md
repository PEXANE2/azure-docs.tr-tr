---
title: Uygulama Proxy Aracı Sıtkı 'nın yüklenmesinde sorun | Microsoft Dokümanlar
description: Uygulama Proxy AracıSı Bağlayıcısı'nı yüklerken karşılaşabileceğiniz sorunları giderme
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d773e6302edf0b799e6dfccc702750a9cc74f60
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406694"
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a>Uygulama Ara Sunucusu Aracı Bağlayıcısı’nı yüklerken sorun oluşuyor

Microsoft AAD Application Proxy Bağlayıcısı, bulut kullanılabilir uç noktadan dahili etki alanına bağlantı kurmak için giden bağlantıları kullanan bir dahili etki alanı bileşenidir.

## <a name="general-problem-areas-with-connector-installation"></a>Bağlayıcı kurulumu ile Genel Sorun Alanları

Bağlayıcının yüklenmesi başarısız olduğunda, kök neden genellikle aşağıdaki alanlardan biridir:

1.  **Bağlantı** – başarılı bir yüklemeyi tamamlamak için, yeni bağlayıcının gelecekteki güven özelliklerini kaydetmesi ve kurması gerekir. Bu, AAD Application Proxy bulut hizmetine bağlanarak yapılır.

2.  **Trust Establishment** – yeni bağlayıcı kendi imzalı bir sertifika oluşturur ve bulut hizmetine kaydolur.

3.  **Adminin kimlik doğrulaması** – yükleme sırasında, kullanıcı Bağlayıcı yüklemesini tamamlamak için yönetici kimlik bilgilerini sağlamalıdır.

> [!NOTE]
> Bağlayıcı yükleme günlükleri %TEMP% klasöründe bulunabilir ve yükleme hatasına neyin neden olduğu hakkında ek bilgi sağlamaya yardımcı olabilir.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>Bulut Uygulama Proxy hizmetine ve Microsoft Giriş sayfasına bağlantıyı doğrulama

**Amaç:** Bağlayıcı makinenin AAD Application Proxy kayıt bitiş noktasının yanı sıra Microsoft giriş sayfasına bağlanabildiğinizi doğrulayın.

1.  Bağlayıcı sunucusunda, 443 ve 80 bağlantı noktalarının açık olduğunu doğrulamak için [telnet](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) veya başka bir bağlantı noktası test aracı kullanarak bir bağlantı noktası testi çalıştırın.

2.  Bu bağlantı noktalarından herhangi biri başarılı olmazsa, Güvenlik Duvarı veya arka uç proxy'sinin gerekli etki alanları ve bağlantı noktalarına erişimi olduğunu doğrulayın, [şirket içi ortamınızı hazırlayın.](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment)

3.  Bir tarayıcı açın (ayrı sekme) ve `https://login.microsoftonline.com`aşağıdaki web sayfasına gidin: , bu sayfaya giriş yapabileceğinizden emin olun.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-certificate"></a>Uygulama Proxy güven sertifikası için Makine ve arka uç bileşenleri desteğini doğrulayın

**Amaç:** Konektör makinesinin, arka uç proxy'sinin ve güvenlik duvarının bağlayıcı tarafından gelecekteki güven için oluşturulan sertifikayı destekleyip sertifikanın geçerli olduğunu doğrulayın.

>[!NOTE]
>Konektör, TLS1.2 tarafından desteklenen bir SHA512 sertifikası oluşturmaya çalışır. Makine veya arka uç güvenlik duvarı ve proxy TLS1.2 desteklemiyorsa, yükleme başarısız olur.
>
>

**Gerekli ön koşulları gözden geçirin:**

1.  Makinenin TLS1.2'yi desteklediğini doğrulayın – 2012 R2'den sonraki tüm Windows sürümleri TLS 1.2'yi desteklemelidir. Konektör makineniz 2012 R2 veya önceki bir sürümüne aitse, makineye aşağıdaki KB'lerin takıldıklarından emin olun:<https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Ağ yöneticinize başvurun ve arka uç proxy'sinin ve güvenlik duvarının giden trafik için SHA512'yi engellemediğini doğrulamak isteyin.

**İstemci sertifikasını doğrulamak için:**

Geçerli istemci sertifikasının parmak izini doğrulayın. Sertifika deposu %ProgramData%\microsoft\Microsoft AAD Application Proxy Bağlayıcısı\Config\TrustSettings.xml adresinde bulunabilir

```
<?xml version="1.0" encoding="utf-8"?>
<ConnectorTrustSettingsFile xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <CloudProxyTrust>
    <Thumbprint>4905CC64B2D81BBED60962ECC5DCF63F643CCD55</Thumbprint>
    <IsInUserStore>false</IsInUserStore>
  </CloudProxyTrust>
</ConnectorTrustSettingsFile>
```

İşte olası **IsInUserStore** değerleri ve anlamları şunlardır:

- **false** - Müşteri sertifikası Register-AppProxyConnector komutu tarafından başlatılan yükleme veya kayıt sırasında oluşturuldu. Yerel makinenin sertifika deposundaki kişisel konteynerde saklanır. 

Sertifikayı doğrulamak için aşağıdaki adımları izleyin:

1. **çalıştır certlm.msc**
2. Yönetim konsolunda Kişisel konteyneri genişletin ve Sertifikalar'a tıklayın
3. **connectorregistrationca.msappproxy.net** tarafından verilen sertifikayı bulma

- **true** - Otomatik olarak yenilenen sertifika, Ağ Hizmetinin kullanıcı sertifikası deposundaki kişisel kapsayıcıda depolanır. 

Sertifikayı doğrulamak için aşağıdaki adımları izleyin:

1. Karşıdan yükleme [PsTools.zip](https://docs.microsoft.com/sysinternals/downloads/pstools)
2. [Paketten PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec) ayıklayın ve yüksek bir komut istemi **nden psexec -i -u "nt authority\network service" cmd.exe** çalıştırın.
3. Yeni ortaya çıkan komut istemi nde **certmgr.msc** çalıştırın
2. Yönetim konsolunda Kişisel konteyneri genişletin ve Sertifikalar'a tıklayın
3. **connectorregistrationca.msappproxy.ne tarafından verilen sertifikayı bulun

**İstemci sertifikasını yenilemek için:**

Bir bağlayıcı hizmete birkaç ay bağlı değilse, sertifikaları eski olabilir. Sertifika yenileme nin başarısız olması, süresi dolmuş bir sertifikaya yol açar. Bu, konektör hizmetinin çalışmayı durdurmasını sağlar. Olay 1000 bağlayıcının yönetici günlüğüne kaydedilir:

"Bağlayıcı yeniden kayıt başarısız oldu: Bağlayıcı güven sertifikasısüresi doldu. Connector'ın çalıştırdığı bilgisayarda PowerShell cmdlet Register-AppProxyConnector'u çalıştırın ve Konektörünüze yeniden kaydedin."

Bu durumda, kaydı tetiklemek için konektörü kaldırın ve yeniden yükleyin veya aşağıdaki PowerShell komutlarını çalıştırabilirsiniz:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

Register-AppProxyConnector komutu hakkında daha fazla bilgi edinmek için lütfen [Azure AD Application Proxy bağlayıcısı için katılımsız bir yükleme komut dosyası oluşturun](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-register-connector-powershell)

## <a name="verify-admin-is-used-to-install-the-connector"></a>Yöneticinin konektörü yüklemek için kullanıldığını doğrulayın

**Amaç:** Bağlayıcıyı yüklemeye çalışan kullanıcının doğru kimlik bilgilerine sahip bir yönetici olduğunu doğrulayın. Şu anda, yüklemenin başarılı olabilmesi için kullanıcının en az bir uygulama yöneticisi olması gerekir.

**Kimlik bilgilerinin doğru olduğunu doğrulamak için:**

`https://login.microsoftonline.com` Bağlanın ve aynı kimlik bilgilerini kullanın. Girişin başarılı olduğundan emin olun. **Azure Etkin Dizin**  - &gt; Kullanıcıları ve **Tüm Kullanıcıları** **Grupla'ya**  - &gt; giderek kullanıcı rolünü kontrol edebilirsiniz. 

Kullanıcı hesabınızı seçin, ardından ortaya çıkan menüde "Dizin Rolü"nü seçin. Seçili rolün "Uygulama Yöneticisi" olduğunu doğrulayın. Bu adımlar daki sayfaların hiçbirine erişemiyorsanız, gerekli role sahip olmazsınız.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Uygulama Proxy bağlayıcılarını anlama](application-proxy-connectors.md)
