---
title: Android-Azure ' a Windows sanal masaüstüne (klasik) bağlanma
description: Android istemcisini kullanarak Windows sanal masaüstüne (klasik) bağlanma.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: c83d243a24fb0cacdf032dfa77e9bb17fdf5e824
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87383809"
---
# <a name="connect-to-windows-virtual-desktop-classic-with-the-android-client"></a>Android istemcisiyle Windows sanal masaüstüne (klasik) bağlanma

> İçin geçerlidir: Android 4,1 ve üzeri, Kmeos 53 ve üzeri ile Kmebooks.

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen Windows sanal masaüstü (klasik) için geçerlidir. Azure Resource Manager Windows sanal masaüstü nesnelerini yönetmeye çalışıyorsanız, [Bu makaleye](../connect-android.md)bakın.

Windows sanal masaüstü kaynaklarına, indirilebilir istemcimizi kullanarak Android cihazınızdan erişebilirsiniz. Google Play Store destekleyen Kmebook cihazlarında Android istemcisini de kullanabilirsiniz. Bu kılavuz, Android istemcisini nasıl ayarlayacağınızı bildirir.

## <a name="install-the-android-client"></a>Android istemcisini yükler

Kullanmaya başlamak için, istemcisini Android cihazınıza [indirin](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) ve yükleyin.

## <a name="subscribe-to-a-feed"></a>Bir akışa abone olma

Android cihazınıza erişebileceğiniz yönetilen kaynakların listesini almak için yöneticiniz tarafından sunulan akışa abone olun.

Bir akışa abone olmak için:

1. Bağlantı merkezi 'nde **+** , ve ardından **uzak kaynak akışı**' na dokunun.
2. Akış **URL 'si alanına AKıŞ** URL 'sini girin. Akış URL 'SI ya bir URL ya da bir e-posta adresi olabilir.
   - URL kullanıyorsanız, yöneticinizin size verdiği bir URL 'yi kullanın <https://rdweb.wvd.microsoft.com> .
   - E-posta kullanmak için e-posta adresinizi girin. Yöneticiniz sunucuyu bu şekilde yapılandırdıysa istemci, e-posta adresinizle ilişkili bir URL arar.
3. **İleri**' ye dokunun.
4. İstendiğinde kimlik bilgilerinizi sağlayın.
   - **Kullanıcı adı**için, Kullanıcı adına kaynaklara erişim izni verin.
   - **Parola**için Kullanıcı adıyla ilişkili parolayı verin.
   - Ayrıca, yöneticinizin kimlik doğrulamasını bu şekilde yapılandırdıysa ek etmenler sağlamanız istenebilir.

Abone olduktan sonra bağlantı merkezi, uzak kaynakları görüntülemelidir.

Akışa abone olduktan sonra akışın içeriği düzenli aralıklarla otomatik olarak güncelleştirilecek. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Android istemcisini kullanma hakkında daha fazla bilgi edinmek için [Android istemcisini kullanmaya başlama](/windows-server/remote/remote-desktop-services/clients/remote-desktop-android/)konusunu inceleyin.