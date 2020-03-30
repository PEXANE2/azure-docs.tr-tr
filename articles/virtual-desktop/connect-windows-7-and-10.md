---
title: Windows Sanal Masaüstü Windows 10 veya 7'ye bağlanın - Azure
description: Windows Desktop istemcisini kullanarak Windows Sanal Masaüstüne nasıl bağlanabilirsiniz?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b6436059cc8b3637edfc2a146d0aab3e2beae6a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154347"
---
# <a name="connect-with-the-windows-desktop-client"></a>Windows Masaüstü istemcisine bağlanma

> Şu şekilde uygulanır: Windows 7, Windows 10 ve Windows 10 IoT Enterprise

Windows Desktop istemcisini kullanarak Windows 7, Windows 10 ve Windows 10 IoT Enterprise'a sahip cihazlarda Windows Sanal Masaüstü kaynaklarına erişebilirsiniz.

> [!IMPORTANT]
> Windows Sanal Masaüstü, RemoteApp ve Masaüstü Bağlantıları (RADC) istemcisini veya Uzak Masaüstü Bağlantısı (MSTSC) istemcisini desteklemez.

> [!IMPORTANT]
> Windows Sanal Masaüstü şu anda Windows Mağazası'ndan Uzak Masaüstü istemcisini desteklemiyor. Bu istemci için destek gelecekteki bir sürümde eklenecektir.

## <a name="install-the-windows-desktop-client"></a>Windows Desktop istemcisini yükleme

Windows sürümünüzle eşleşen istemciyi seçin:

- [Windows 64 bit](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32 bit](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Yönetici hakları gerektirmeyen geçerli kullanıcı için istemciyi yükleyebilirsiniz veya yöneticiniz istemciyi aygıttaki tüm kullanıcıların erişebileceği şekilde yükleyebilir ve yapılandırabilir.

Kurulduktan sonra, istemci **Uzak Masaüstü**arayarak Başlat menüsünden başlatılabilir.

## <a name="subscribe-to-a-feed"></a>Özet akışına abone ol

Yöneticiniz tarafından sağlanan özet akışına abone olarak kullanabileceğiniz yönetilen kaynakların listesini alın. Abone olmak, kaynakları yerel bilgisayarınızda kullanılabilir hale getirir.

Özet akışına abone olmak için:

1. Windows Desktop istemcisini açın.
2. Hizmete bağlanmak ve kaynaklarınızı almak için ana sayfadaki **Abone Ol'u** seçin.
3. İstendiğinde kullanıcı hesabınızla oturum açın.

Başarılı bir şekilde oturum açmadan sonra, erişebileceğiniz kaynakların bir listesini görmeniz gerekir.

Kaynakları iki yöntemden biriyle başlatabilirsiniz.

- İstemcinin ana sayfasından, başlatmak için bir kaynağa çift tıklayın.
- Başlat Menüsünden diğer uygulamalarda normalde yaptığınız gibi bir kaynak başlatın.
  - Ayrıca arama çubuğundaki uygulamaları da arayabilirsiniz.

Bir özet akışına abone olduktan sonra, özet akışının içeriği düzenli olarak otomatik olarak güncellenir. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Windows Desktop istemcisini nasıl kullanacağınız hakkında daha fazla bilgi edinmek için [Windows Desktop istemcisiyle başlayın'a](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/)göz atın.
