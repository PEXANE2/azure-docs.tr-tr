---
title: MacOS-Azure ' dan Windows sanal masaüstüne bağlanma
description: MacOS istemcisini kullanarak Windows sanal masaüstüne bağlanma.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 024a1ab1a7fef58bd5fd8f9e7e0fc743a4ecee71
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213168"
---
# <a name="connect-with-the-macos-client"></a>macOS istemcisiyle bağlanma

> Uygulama hedefi: macOS 10,12 veya üzeri

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen sonbahar 2019 sürümü için geçerlidir. Spring 2020 güncelleştirmesinde tanıtılan Azure Resource Manager Windows sanal masaüstü nesnelerini yönetmeye çalışıyorsanız, [Bu makaleye](../connect-macos.md)bakın.

Windows sanal masaüstü kaynaklarına, macOS cihazlarınızdan indirilebilir istemcimizi kullanarak erişebilirsiniz. Bu kılavuz, istemcisini nasıl ayarlayacağınızı bildirir.

## <a name="install-the-client"></a>İstemciyi yükleme

Başlamak için, istemciyi macOS cihazınıza [indirin](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) ve yükleyin.

## <a name="subscribe-to-a-feed"></a>Bir akışa abone olma

Size, macOS cihazınızda kullanabileceğiniz yönetilen kaynakların listesini almak için yöneticinize verdiği akışa abone olun.

Bir akışa abone olmak için:

1. Hizmete bağlanmak ve kaynaklarınızı almak için ana sayfada **çalışma alanı Ekle** ' yi seçin.
2. Akış URL 'sini girin. Bu bir URL veya e-posta adresi olabilir:
   - Bir URL kullanıyorsanız, yöneticinizin size verdiği bir URL 'yi kullanın. Normalde, URL olur <https://rdweb.wvd.microsoft.com> .
   - E-posta kullanmak için e-posta adresinizi girin. Bu, yönetici sunucuyu bu şekilde yapılandırdıysa, istemciye e-posta adresinizle ilişkili bir URL aramasını söyler.
3. **Ekle**'yi seçin.
4. İstendiğinde kullanıcı hesabınızla oturum açın.

Oturum açtıktan sonra, kullanılabilir kaynakların bir listesini görmeniz gerekir.

Bir akışa abone olduktan sonra, akışın içeriği düzenli aralıklarla otomatik olarak güncelleşmeyecektir. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

MacOS istemcisi hakkında daha fazla bilgi edinmek için [MacOS istemcisini kullanmaya başlama](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/) belgesine göz atın.