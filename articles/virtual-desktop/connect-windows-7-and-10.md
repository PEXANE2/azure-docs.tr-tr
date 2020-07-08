---
title: Windows sanal masaüstü Windows 10 veya 7-Azure 'a bağlanma
description: Windows masaüstü istemcisini kullanarak Windows sanal masaüstüne bağlanma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5d333b2f8bbb660e9fd71339159d942c0520f4f4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85201557"
---
# <a name="connect-with-the-windows-desktop-client"></a>Windows Masaüstü istemcisine bağlanma

> Uygulama hedefi: Windows 7, Windows 10 ve Windows 10 IoT Enterprise

Windows Masaüstü istemcisi 'ni kullanarak Windows 7, Windows 10 ve Windows 10 IoT Enterprise ile cihazlarda Windows sanal masaüstü kaynaklarına erişebilirsiniz.

>[!NOTE]
>Windows istemcisi varsayılan olarak Windows sanal masaüstü Fall 2019 sürümüne otomatik olarak girer. Ancak, istemci kullanıcının Azure Resource Manager kaynaklara sahip olduğunu algılarsa, kaynakları otomatik olarak ekler veya kullanıcılara kullanılabilir olduğunu bildirir.

> [!IMPORTANT]
> Windows sanal masaüstü, RemoteApp ve Masaüstü bağlantıları (ÇDC) istemcisini veya Uzak Masaüstü Bağlantısı (MSTSC) istemcisini desteklemez.

> [!IMPORTANT]
> Windows sanal masaüstü Şu anda Windows Mağazası 'ndan uzak masaüstü istemcisini desteklememektedir. Bu istemciye yönelik destek gelecekteki bir sürüme eklenecektir.

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

Windows Masaüstü istemcisinin nasıl kullanılacağı hakkında daha fazla bilgi edinmek için, [Windows Masaüstü istemcisi ile çalışmaya başlama](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/)konusunu inceleyin.
