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
ms.openlocfilehash: 466e1ce0efbdec3f5475634f3857d02554d93d98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049140"
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

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Uygulama Proxy güven sertifikası için Makine ve arka uç bileşenleri desteği doğrulayın

**Amaç:** Konektör makinesi, arka uç proxy ve güvenlik duvarının bağlayıcı tarafından gelecekteki güven için oluşturulan sertifikayı destekleyebilir doğrulayın.

>[!NOTE]
>Konektör, TLS1.2 tarafından desteklenen bir SHA512 sertifikası oluşturmaya çalışır. Makine veya arka uç güvenlik duvarı ve proxy TLS1.2 desteklemiyorsa, yükleme başarısız olur.
>
>

**Sorunu gidermek için:**

1.  Makinenin TLS1.2'yi desteklediğini doğrulayın – 2012 R2'den sonraki tüm Windows sürümleri TLS 1.2'yi desteklemelidir. Konektör makineniz 2012 R2 veya önceki bir sürümüne aitse, makineye aşağıdaki KB'lerin takıldıklarından emin olun:<https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Ağ yöneticinize başvurun ve arka uç proxy'sinin ve güvenlik duvarının giden trafik için SHA512'yi engellemediğini doğrulamak isteyin.

## <a name="verify-admin-is-used-to-install-the-connector"></a>Yöneticinin konektörü yüklemek için kullanıldığını doğrulayın

**Amaç:** Bağlayıcıyı yüklemeye çalışan kullanıcının doğru kimlik bilgilerine sahip bir yönetici olduğunu doğrulayın. Şu anda, yüklemenin başarılı olabilmesi için kullanıcının en az bir uygulama yöneticisi olması gerekir.

**Kimlik bilgilerinin doğru olduğunu doğrulamak için:**

`https://login.microsoftonline.com` Bağlanın ve aynı kimlik bilgilerini kullanın. Girişin başarılı olduğundan emin olun. **Azure Etkin Dizin**  - &gt; Kullanıcıları ve **Tüm Kullanıcıları** **Grupla'ya**  - &gt; giderek kullanıcı rolünü kontrol edebilirsiniz. 

Kullanıcı hesabınızı seçin, ardından ortaya çıkan menüde "Dizin Rolü"nü seçin. Seçili rolün "Uygulama Yöneticisi" olduğunu doğrulayın. Bu adımlar daki sayfaların hiçbirine erişemiyorsanız, gerekli role sahip olmazsınız.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Uygulama Proxy bağlayıcılarını anlama](application-proxy-connectors.md)
