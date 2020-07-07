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
ms.openlocfilehash: 8c5de43ed29856451ad67e02a426b07cc34a0d54
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80422425"
---
# <a name="linux-support"></a>Linux desteği

İş ortakları Windows sanal masaüstü için Linux SDK 'Yı kullanarak tek başına bir Windows sanal masaüstü istemcisi oluşturabilir. Ayrıca, istemci uygulamanızda Windows sanal masaüstü desteğini etkinleştirmek için de kullanabilirsiniz. Bu hızlı kılavuz, Linux SDK 'sının ne olduğunu ve nasıl kullanılacağını açıklar.

## <a name="connect-with-your-linux-device"></a>Linux aygıtınızla bağlantı kurmak

Aşağıdaki iş ortakları Linux cihazları için Windows sanal masaüstü istemcileri 'ni onayladı.

|İş Ortağı|İş ortağı belgeleri|İş ortağı desteği|
|:------|:--------------------|:--------------|
|![IGEL logosu](./media/partners/igel.png)|[IGEL istemci belgeleri](https://www.igel.com/igel-solution-family/windows-virtual-desktop/)|[ILEK desteği](https://www.igel.com/support/)|

## <a name="what-is-the-linux-sdk"></a>Linux SDK nedir?

Kaynak akışlarını almak, masaüstü veya uzak uygulama oturumlarına bağlanmak ve ilk taraf istemcilerimizin desteklediği birçok yeniden yönlendirmeleri kullanmak için SDK API 'Lerini kullanabilirsiniz.

> [!NOTE]
> SDK Şu anda geliştirme aşamasındadır. Bu belgeyi, genel kullanıma sunulduğunda SDK 'ya erişme yönergeleriyle birlikte güncelleştireceğiz.

### <a name="supported-linux-distributions"></a>Desteklenen Linux dağıtımları

SDK, Ubuntu 18,04 veya üzerini temel alan çoğu işletim sistemiyle uyumludur. Farklı bir Linux dağıtımına sahipseniz, gereksinimlerinizi en iyi şekilde desteklemeyi öğrenmek için sizinle birlikte çalışabiliriz.

### <a name="feature-support"></a>Özellik desteği

SDK, masaüstü ve uzak uygulama oturumlarına yönelik birden çok bağlantıyı destekler. Aşağıdaki yeniden yönlendirmeler desteklenir:

| Yönlendirme       | Destekleniyor |
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

## <a name="get-started-with-the-linux-sdk"></a>Linux SDK 'Yı kullanmaya başlama

Windows sanal masaüstü için bir Linux istemcisi geliştirebilmeniz için aşağıdaki işlemleri yapmanız gerekir:

1. Test veya üretim kullanımı için bir Windows sanal masaüstü ortamı oluşturun ve dağıtın.
2. Windows sanal masaüstü Kullanıcı deneyimini tanımak için kullanılabilir birinci taraf istemcileri test edin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki istemciler için belgelerimize göz atın:

- [Windows Masaüstü istemcisi](connect-windows-7-and-10.md)
- [Web istemcisi](connect-web.md)
- [Android istemcisi](connect-android.md)
- [macOS istemcisi](connect-macos.md)
- [iOS istemcisi](connect-ios.md)
