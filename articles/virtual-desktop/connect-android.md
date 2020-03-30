---
title: Android'den Windows Sanal Masaüstüne bağlanın - Azure
description: Android istemcisini kullanarak Windows Sanal Masaüstüne nasıl bağlanabilirsiniz?
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: d2990c82efbcdac7d453f920301787b8c83db1e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295370"
---
# <a name="connect-with-the-android-client"></a>Android istemcisiyle bağlanma

> Geçerli olan: Android 4.1 ve sonrası, ChromeOS 53 ve sonraki Chromebook'lar.

>[!NOTE]
> Android istemcisinden Windows Sanal Masaüstü kaynaklarına erişebilme özelliği şu anda önizlemede mevcuttur.

İndirilebilir istemcimiz ile Android cihazınızdan Windows Sanal Masaüstü kaynaklarına erişebilirsiniz. Android istemcisini Google Play Store'u destekleyen Chromebook cihazlarında da kullanabilirsiniz. Bu kılavuz, Android istemcisini nasıl kuracağınızı size söyleyecektir.

## <a name="install-the-android-client"></a>Android istemcisini yükleme

Başlamak için istemciyi Android cihazınıza [indirin](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) ve yükleyin.

## <a name="subscribe-to-a-feed"></a>Özet akışına abone ol

Android cihazınızda erişebileceğiniz yönetilen kaynakların listesini almak için yöneticiniz tarafından sağlanan özet akışına abone olun.

Özet akışına abone olmak için:

1. Bağlantı Merkezi'nde, **+** **Uzak Kaynak Akışı'na**dokunun.
2. Özet akışı URL'sini **Feed URL** alanına girin. Özet akışı URL'si bir URL veya e-posta adresi olabilir.
   - Bir URL kullanıyorsanız, yöneticinizin size verdiği <https://rdweb.wvd.microsoft.com>URL'yi kullanın.
   - E-postakullanmak için e-posta adresinizi girin. Yöneticiniz sunucuyu bu şekilde yapılandırırsa, istemci e-posta adresinizle ilişkili bir URL arar.
3. **NEXT'e**dokunun.
4. İstendiğinde kimlik bilgilerinizi sağlayın.
   - **Kullanıcı adı**için, kaynaklara erişmek için izin veren kullanıcı adı verin.
   - **Parola**için, kullanıcı adı ile ilişkili parolayı verin.
   - Yöneticiniz kimlik doğrulamasını bu şekilde yapılandırırsa ek etkenler sağlamanız da isdenedilebilir.

Abone olduktan sonra Bağlantı Merkezi uzak kaynakları görüntülemelidir.

Bir özet akışına abone olduktan sonra, özet akışının içeriği düzenli olarak otomatik olarak güncellenir. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Android istemcisini nasıl kullanacağınız hakkında daha fazla bilgi edinmek için Android [istemcisiyle başlayın'a](/windows-server/remote/remote-desktop-services/clients/remote-desktop-android/)göz atın.
