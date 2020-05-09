---
title: MacOS-Azure ' dan Windows sanal masaüstüne bağlanma
description: MacOS istemcisini kullanarak Windows sanal masaüstüne bağlanma.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: ca54c60d424722d31d195d10eb15751a2ec6c0eb
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612920"
---
# <a name="connect-with-the-macos-client"></a>macOS istemcisiyle bağlanma

> Uygulama hedefi: macOS 10,12 veya üzeri

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/connect-macos-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. 
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows sanal masaüstü kaynaklarına, macOS cihazlarınızdan indirilebilir istemcimizi kullanarak erişebilirsiniz. Bu kılavuz, istemcisini nasıl ayarlayacağınızı bildirir.

## <a name="install-the-client"></a>İstemciyi yükleme

Başlamak için, istemciyi MacOS cihazınıza [indirin](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) ve yükleyin.

## <a name="subscribe-to-a-feed"></a>Bir akışa abone olma

Size, macOS cihazınızda kullanabileceğiniz yönetilen kaynakların listesini almak için yöneticinize verdiği akışa abone olun.

Bir akışa abone olmak için:

1. Hizmete bağlanmak ve kaynaklarınızı almak için ana sayfada **çalışma alanı Ekle** ' yi seçin.
2. Akış URL 'sini girin. Bu bir URL veya e-posta adresi olabilir:
   - Bir URL kullanıyorsanız, yöneticinizin size verdiği bir URL 'yi kullanın. Normalde, URL olur <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>.
   - E-posta kullanmak için e-posta adresinizi girin. Bu, yönetici sunucuyu bu şekilde yapılandırdıysa, istemciye e-posta adresinizle ilişkili bir URL aramasını söyler.
3. **Add (Ekle)** seçeneğini belirleyin.
4. İstendiğinde kullanıcı hesabınızla oturum açın.

Oturum açtıktan sonra, kullanılabilir kaynakların bir listesini görmeniz gerekir.

Bir akışa abone olduktan sonra, akışın içeriği düzenli aralıklarla otomatik olarak güncelleşmeyecektir. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

MacOS istemcisi hakkında daha fazla bilgi edinmek için [MacOS istemcisini kullanmaya başlama](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/) belgesine göz atın.
