---
title: Windows Sanal Masaüstü Linux Desteği - Azure
description: Windows Sanal Masaüstü için kısa bir genel bakış Linux desteği.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 967fd1fa182b7c8e581fd74cc287c5a6ba0e4038
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127832"
---
# <a name="linux-support"></a>Linux desteği

Bağımsız bir Windows Sanal Masaüstü istemcisi oluşturmak için Windows Sanal Masaüstü için Linux SDK'yı kullanabilirsiniz. İstemci uygulamanızda Windows Sanal Masaüstü desteğini etkinleştirmek için de kullanabilirsiniz. Bu hızlı kılavuz, Linux SDK'nın ne olduğunu ve nasıl kullanmaya başlayacağımı açıklayacaktır.

## <a name="what-is-the-linux-sdk"></a>Linux SDK nedir?

Kaynak akışlarını almak, masaüstü veya uzak uygulama oturumlarına bağlanmak ve birinci taraf istemcilerimizin desteklediği yönlendirmelerin çoğunu kullanmak için SDK API'lerini kullanabilirsiniz.

> [!NOTE]
> SDK şu anda geliştirilmekte. Bu belgeyi kullanılabilir olduğunda SDK'ya erişim yönergeleriyle güncelleştireceğiz.

### <a name="supported-linux-distributions"></a>Desteklenen Linux dağıtımları

SDK, Ubuntu 18.04 veya sonraki işletim sistemlerinin çoğu ile uyumludur. Farklı bir Linux dağıtımınız varsa, ihtiyaçlarınızı en iyi nasıl destekleyeceğinizi bulmak için sizinle birlikte çalışabiliriz.

### <a name="feature-support"></a>Özellik desteği

SDK, masaüstü ve uzak uygulama oturumlarına birden çok bağlantıyı destekler. Aşağıdaki yönlendirmeler desteklenir:

| Yönlendirme       | Destekleniyor |
| :---------------- | :-------: |
| Klavye          | &#10004;  |
| Fare             | &#10004;  |
| Ses içinde          | &#10004;  |
| Ses çıkış         | &#10004;  |
| Pano (metin)  | &#10004;  |
| Pano (resim) | &#10004;  |
| Pano (dosya)  | &#10004;  |
| Smartcard         | &#10004;  |
| Sürücü/klasör      | &#10004;  |

SDK, oturumunuz için seçtiğiniz monitörler bitişik olduğu sürece birden çok monitör ekran yapılandırmasını da destekler.

Yeni özellikler ve yönlendirmeler için destek ekledikçe bu belgeyi güncelleştireceğiz. Yeni özellikler ve diğer geliştirmeler önermek istiyorsanız, [UserVoice sayfamızı](https://go.microsoft.com/fwlink/?linkid=2116523)ziyaret edin.

## <a name="get-started-with-the-linux-sdk"></a>Linux SDK ile başlayın

Windows Sanal Masaüstü için bir Linux istemcisi geliştirebilmeniz için önce aşağıdaki leri yapmanız gerekir:

1. Sınama veya üretim kullanımı için bir Windows Sanal Masaüstü ortamı oluşturun ve dağıtın.
2. Windows Sanal Masaüstü kullanıcı deneyimini tanımak için kullanılabilir birinci taraf istemcileri test edin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki müşteriler için belgelerimize göz atın:

- [Windows Masaüstü istemcisi](connect-windows-7-and-10.md)
- [Web istemcisi](connect-web.md)
- [Android istemcisi](connect-android.md)
- [macOS istemcisi](connect-macos.md)
- [iOS istemcisi](connect-ios.md)
