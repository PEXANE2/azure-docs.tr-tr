---
title: Windows sanal masaüstü kimlik doğrulaması-Azure
description: Windows sanal masaüstü için kimlik doğrulama yöntemleri.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/27/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 8328fa9c9e32a22e8e5093aa343f333cd1914ddc
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038695"
---
# <a name="supported-authentication-methods"></a>Desteklenen kimlik doğrulama yöntemleri

Bu makalede, Windows sanal masaüstü 'nde kullanabileceğiniz hangi kimlik doğrulama türleri hakkında kısa bir genel bakış sunuyoruz.

## <a name="session-host-authentication"></a>Oturum Ana bilgisayar kimlik doğrulaması

Windows sanal masaüstü, oturum ana bilgisayar kimlik doğrulaması için hem NT LAN Manager (NTLM) hem de Kerberos destekler. Ancak, Kerberos kullanmak için istemcinin, etki alanı denetleyicisinde çalışan bir Anahtar Dağıtım Merkezi (KDC) hizmetinden Kerberos güvenlik biletleri alması gerekir. Bilet almak için, istemcinin etki alanı denetleyicisine doğrudan bir görüş satırı olması gerekir. Şirket ağınızı kullanarak doğrudan bir görüş satırı edinebilirsiniz. Ayrıca kurumsal ağınıza bir VPN bağlantısı da kullanabilirsiniz.

Bunlar şu anda desteklenen oturum açma yöntemleridir:

- Windows Masaüstü istemcisi
    - Kullanıcı adı ve parola
    - Kart
    - Windows Hello
- Windows Mağazası istemcisi
    - Kullanıcı adı ve parola
- Web istemcisi
    - Kullanıcı adı ve parola
- Android
    - Kullanıcı adı ve parola
- iOS
    - Kullanıcı adı ve parola
- Mac OS
    - Kullanıcı adı ve parola

>[!NOTE]
>Akıllı kart ve Windows Hello, oturum açmak için yalnızca Kerberos kullanabilir. Kerberos ile oturum açmak için etki alanı denetleyicisine bir görüş satırı gerekir.

## <a name="single-sign-on-sso"></a>Çoklu oturum açma (SSO)

Windows sanal masaüstü Şu anda kimlik doğrulama veya SSO için Active Directory Federasyon Hizmetleri (AD FS) (ADFS) desteklememektedir.

Oturum Ana bilgisayarı için kimlik bilgileriniz istenmeden kaçınmanın tek yolu, bunları istemciye kaydetmaktır. Diğer kullanıcıların kaynaklarınıza erişmesini engellemek için bunu yalnızca güvenli cihazlarla yapmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtımınızı güvenli tutmanın diğer yollarını merak ediyor musunuz? [En Iyi güvenlik uygulamalarına](security-guide.md)göz atın.
