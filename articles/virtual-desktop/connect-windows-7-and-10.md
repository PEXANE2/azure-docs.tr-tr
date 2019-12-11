---
title: Windows sanal masaüstü Windows 10 veya 7-Azure 'a bağlanma
description: Windows masaüstü istemcisini kullanarak Windows sanal masaüstüne bağlanma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: helohr
ms.openlocfilehash: 745b33efe46c82e3b9358c9c5a2ed13292242db1
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74997352"
---
# <a name="connect-with-the-windows-desktop-client"></a>Windows Masaüstü istemcisine bağlanma

> Uygulama hedefi: Windows 7, Windows 10 ve Windows 10 IoT Enterprise

Windows Masaüstü istemcisi 'ni kullanarak Windows 7, Windows 10 ve Windows 10 IoT Enterprise ile cihazlarda Windows sanal masaüstü kaynaklarına erişebilirsiniz.

> [!IMPORTANT]
> Windows sanal masaüstü, RemoteApp ve Masaüstü bağlantıları (ÇDC) istemcisini veya Uzak Masaüstü Bağlantısı (MSTSC) istemcisini desteklemez.

## <a name="install-the-windows-desktop-client"></a>Windows masaüstü istemcisini yükler

Windows sürümünüzle eşleşen istemciyi seçin:

- [Windows 64-bit](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32-bit](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Yönetici hakları gerektirmeyen geçerli kullanıcı için istemcisini yükleyebilir veya yöneticiniz, cihazdaki tüm kullanıcıların erişebilmesi için istemciyi yükleyip yapılandırabilir.

Yüklendikten sonra istemci, **Uzak Masaüstü**araması yaparak başlangıç menüsünden başlatılabilir.

## <a name="subscribe-to-a-feed"></a>Bir akışa abone olma

Yöneticiniz tarafından sağlanan akışa abone olarak sizin için kullanabileceğiniz yönetilen kaynakların listesini alın. Abone olmak, kaynakları yerel bilgisayarınızda kullanılabilir hale getirir.

Bir akışa abone olmak için:

1. Windows masaüstü istemcisini açın.
2. Hizmete bağlanmak ve kaynaklarınızı almak için ana sayfada **abone ol** ' u seçin.
3. İstendiğinde kullanıcı hesabınızla oturum açın.

Başarıyla oturum açtıktan sonra, erişebileceğiniz kaynakların bir listesini görmeniz gerekir.

Kaynakları iki yöntemden biri ile başlatabilirsiniz.

- İstemcinin ana sayfasından bir kaynağa çift tıklayarak başlatın.
- Başlangıç menüsünden normalde diğer uygulamalar olacak şekilde bir kaynak başlatın.
  - Ayrıca, arama çubuğundaki uygulamaları da arayabilirsiniz.

Akışa abone olduktan sonra akışın içeriği düzenli aralıklarla otomatik olarak güncelleştirilir. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Windows Masaüstü istemcisinin nasıl kullanılacağı hakkında daha fazla bilgi edinmek için, [Windows Masaüstü istemcisi ile çalışmaya başlama](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/windowsdesktop)konusunu inceleyin.
