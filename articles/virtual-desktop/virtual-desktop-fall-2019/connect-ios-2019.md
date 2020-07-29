---
title: İOS-Azure ' a Windows sanal masaüstüne (klasik) bağlanma
description: İOS istemcisini kullanarak Windows sanal masaüstüne (klasik) bağlanma.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 06de85e23b1d6350a48735506c8931922d40b799
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87270745"
---
# <a name="connect-to-windows-virtual-desktop-classic-with-the-ios-client"></a>İOS istemcisiyle Windows sanal masaüstüne (klasik) bağlanma

> Uygulama hedefi: iOS 13,0 veya üzeri. İPhone, iPad ve iPod Touch ile uyumludur.

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen Windows sanal masaüstü (klasik) için geçerlidir. Azure Resource Manager Windows sanal masaüstü nesnelerini yönetmeye çalışıyorsanız, [Bu makaleye](../connect-ios.md)bakın.

Windows sanal masaüstü kaynaklarına, indirilebilir istemcimiz ile iOS cihazınızdan erişebilirsiniz. Bu kılavuz, iOS istemcisini nasıl ayarlayacağınızı bildirir.

## <a name="install-the-ios-client"></a>İOS istemcisini yükler

Başlamak için, istemcisini iOS cihazınıza [indirin](https://aka.ms/rdios) ve yükleyin.

## <a name="subscribe-to-a-feed"></a>Bir akışa abone olma

İOS cihazınıza erişebileceğiniz yönetilen kaynakların listesini almak için yöneticiniz tarafından sunulan akışa abone olun.

Bir akışa abone olmak için:

1. Bağlantı merkezi 'nde **+** , ve ardından **çalışma alanı Ekle**' ye dokunun.
2. Akış **URL 'si alanına AKıŞ** URL 'sini girin. Akış URL 'SI ya bir URL ya da bir e-posta adresi olabilir.
   - Bir URL kullanıyorsanız, yöneticinizin size verdiği bir URL 'yi kullanın. Normalde, URL olur <https://rdweb.wvd.microsoft.com> .
   - E-posta kullanmak için e-posta adresinizi girin. Bu, yönetici sunucuyu bu şekilde yapılandırdıysa, istemciye e-posta adresinizle ilişkili bir URL aramasını söyler.
3. **İleri**' ye dokunun.
4. İstendiğinde kimlik bilgilerinizi sağlayın.
   - **Kullanıcı adı**için, Kullanıcı adına kaynaklara erişim izni verin.
   - **Parola**için Kullanıcı adıyla ilişkili parolayı verin.
   - Ayrıca, yöneticinizin kimlik doğrulamasını bu şekilde yapılandırdıysa ek etmenler sağlamanız istenebilir.
5. **Kaydet**'e dokunun.

Bundan sonra bağlantı merkezi, uzak kaynakları görüntülemelidir.

Akışa abone olduktan sonra akışın içeriği düzenli aralıklarla otomatik olarak güncelleştirilecek. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

İOS istemcisini kullanma hakkında daha fazla bilgi edinmek için [iOS istemcisi ile çalışmaya başlama](/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios/) belgelerini inceleyin.
