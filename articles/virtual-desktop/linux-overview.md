---
title: Windows sanal masaüstü Linux desteği-Azure
description: Windows sanal masaüstü için kısa bir genel bakış Linux desteği.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f91e130bfa83c6a9b116c05d7293aa70945e2dc2
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87903553"
---
# <a name="linux-support"></a>Linux desteği

Linux cihazlarımızda, Linux basit istemci iş ortaklarımız tarafından sağlanmış olan aşağıdaki desteklenen istemcilerle Windows sanal masaüstü kaynaklarına erişebilirsiniz. Desteklenen Windows sanal masaüstü istemcilerini daha Linux tabanlı işletim sistemleri ve cihazlarda etkinleştirmek için bir dizi iş ortaklarıyla çalışıyoruz. Burada listelenmeyen bir Linux platformunda Windows sanal masaüstü desteği ' ni istiyorsanız, lütfen [UserVoice sayfamızda](https://remotedesktop.uservoice.com/forums/923035-remote-desktop-support-on-linux)bize bildirin.

## <a name="connect-with-your-linux-device"></a>Linux aygıtınızla bağlantı kurmak

Aşağıdaki iş ortakları Linux cihazları için Windows sanal masaüstü istemcileri 'ni onayladı.

|İş Ortağı|İş ortağı belgeleri|İş ortağı desteği|
|:------|:--------------------|:--------------|
|![IGEL logosu](./media/partners/igel.png)|[IGEL istemci belgeleri](https://www.igel.com/igel-solution-family/windows-virtual-desktop/)|[ILEK desteği](https://www.igel.com/support/)|

## <a name="what-is-the-linux-sdk"></a>Linux SDK nedir?

Linux basit istemci ortakları, kaynak akışlarını almak, masaüstü veya uzak uygulama oturumlarına bağlanmak ve ilk taraf istemcilerimizin desteklediği birçok yeniden yönlendirmeleri kullanmak için Windows sanal masaüstü Linux SDK API 'Lerini kullanabilir. SDK, Ubuntu 18,04 veya üzerini temel alan çoğu işletim sistemiyle uyumludur.

### <a name="feature-support"></a>Özellik desteği

SDK, masaüstü ve uzak uygulama oturumlarına yönelik birden çok bağlantıyı destekler. Aşağıdaki yeniden yönlendirmeler desteklenir:

| Yönlendirme       | Desteklenir |
| :---------------- | :-------: |
| Klavye          | &#10004;  |
| Fare             | &#10004;  |
| İçindeki ses          | &#10004;  |
| Ses çıkışı         | &#10004;  |
| Pano (metin)  | &#10004;  |
| Pano (görüntü) | &#10004;  |
| Pano (dosya)  | &#10004;  |
| Kart         | &#10004;  |
| Sürücü/klasör      | &#10004;  |

Ayrıca, oturumunuz için seçtiğiniz izleyiciler bitişik olduğu sürece SDK birden fazla izleyici görüntüleme yapılandırmasını da destekler.

Yeni özellikler ve yeniden yönlendirmeler için destek eklediğimiz için bu belgeyi güncelleştireceğiz. Yeni özellikler ve diğer iyileştirmeler önermek istiyorsanız [UserVoice](https://go.microsoft.com/fwlink/?linkid=2116523)sayfamızı ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki istemciler için belgelerimize göz atın:

- [Windows Masaüstü istemcisi](connect-windows-7-10.md)
- [Web istemcisi](connect-web.md)
- [Android istemcisi](connect-android.md)
- [macOS istemcisi](connect-macos.md)
- [iOS istemcisi](connect-ios.md)
