---
title: iOS'tan Windows Sanal Masaüstüne bağlanma - Azure
description: iOS istemcisini kullanarak Windows Sanal Masaüstüne nasıl bağlanabilirsiniz.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/08/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3bf9ea6c68e4cbbe721705639e6c6416c0e93c9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128260"
---
# <a name="connect-with-the-ios-client"></a>iOS istemcisiyle bağlanma

> Şu şekilde geçerlidir: iOS 13.0 veya sonraki. iPhone, iPad ve iPod touch ile uyumludur.

Indirilebilir istemcimiz ile windows sanal masaüstü kaynaklarına iOS aygıtınızdan erişebilirsiniz. Bu kılavuz, iOS istemcisini nasıl ayarladığınızı size söyleyecektir.

## <a name="install-the-ios-client"></a>iOS istemcisini yükleme

Başlamak için istemciyi iOS aygıtınıza [indirin](https://aka.ms/rdios) ve yükleyin.

## <a name="subscribe-to-a-feed"></a>Özet akışına abone ol

iOS aygıtınızda erişebileceğiniz yönetilen kaynakların listesini almak için yöneticiniz tarafından sağlanan özet akışına abone olun.

Özet akışına abone olmak için:

1. Bağlantı Merkezi'nde, **+** **Çalışma Alanı Ekle'ye**dokunun ve sonra dokunun.
2. Özet akışı URL'sini **Feed URL** alanına girin. Özet akışı URL'si bir URL veya e-posta adresi olabilir.
   - BIR URL kullanıyorsanız, yöneticinizin size verdiği URL'yi kullanın. Normalde, URL' <https://rdweb.wvd.microsoft.com>dir.
   - E-postakullanmak için e-posta adresinizi girin. Bu, yöneticiniz sunucuyu bu şekilde yapılandırırsa istemciye e-posta adresinizle ilişkili bir URL aramasını söyler.
3. **İleri'ye**dokunun.
4. İstendiğinde kimlik bilgilerinizi sağlayın.
   - **Kullanıcı adı**için, kaynaklara erişmek için izin veren kullanıcı adı verin.
   - **Parola**için, kullanıcı adı ile ilişkili parolayı verin.
   - Yöneticiniz kimlik doğrulamasını bu şekilde yapılandırırsa ek etkenler sağlamanız da isdenedilebilir.
5. **Kaydet**'e dokunun.

Bundan sonra, Bağlantı Merkezi uzak kaynakları görüntülemelidir.

Bir özet akışına abone olduktan sonra, özet akışının içeriği düzenli olarak otomatik olarak güncellenir. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

iOS istemcisini nasıl kullanacağınız hakkında daha fazla bilgi edinmek için [iOS istemci belgeleriyle başlayın'a](/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios/) göz atın.
