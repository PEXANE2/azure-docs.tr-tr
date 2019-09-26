---
title: MacOS-Azure ' dan Windows sanal masaüstüne bağlanma
description: MacOS 'tan Windows sanal masaüstüne bağlanma.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: helohr
ms.openlocfilehash: bf6bfb52bd763eabd88f23701c3d1f240cdf4844
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71310197"
---
# <a name="connect-from-macos"></a>MacOS 'tan Bağlan

> Uygulama hedefi: macOS 10,12 veya üzeri

>[!NOTE]
> MacOS istemcisinden Windows sanal masaüstü kaynaklarına erişme özelliği şu anda önizlemede kullanılabilir.

Windows sanal masaüstü kaynaklarına, macOS cihazlarınızdan indirilebilir istemcimizi kullanarak erişebilirsiniz. Bu kılavuz, istemcisini nasıl ayarlayacağınızı bildirir.

## <a name="install-the-client"></a>İstemciyi yükler

Başlamak için, istemciyi MacOS cihazınıza [indirin](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) ve yükleyin.

## <a name="subscribe-to-a-feed"></a>Bir akışa abone olma

Size, macOS cihazınızda kullanabileceğiniz yönetilen kaynakların listesini almak için yöneticinize verdiği akışa abone olun.

Bir akışa abone olmak için:

1. Hizmete bağlanmak ve kaynaklarınızı almak için ana sayfada **akış Ekle** ' yi seçin.
2. Akış URL 'sini girin. Bu bir URL veya e-posta adresi olabilir:
   - Bir URL kullanıyorsanız, yöneticinizin size verdiği bir URL 'yi kullanın. Normalde, URL olur <https://rdweb.wvd.microsoft.com>.
   - E-posta kullanmak için e-posta adresinizi girin. Bu, yönetici sunucuyu bu şekilde yapılandırdıysa, istemciye e-posta adresinizle ilişkili bir URL aramasını söyler.
3. **Abone ol**' u seçin.
4. İstendiğinde kullanıcı hesabınızla oturum açın.

Oturum açtıktan sonra, kullanılabilir kaynakların bir listesini görmeniz gerekir.

Bir akışa abone olduktan sonra, akışın içeriği düzenli aralıklarla otomatik olarak güncelleşmeyecektir. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="client-documentation"></a>İstemci belgeleri

MacOS istemcisi hakkında daha fazla bilgi edinmek için [MacOS istemcisini kullanmaya başlama](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac) belgesine göz atın.
