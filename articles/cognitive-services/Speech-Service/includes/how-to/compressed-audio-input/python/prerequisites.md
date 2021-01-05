---
author: amitkumarshukla
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: amishu
ms.openlocfilehash: b8dda0347e5713ef35705425b54f29a110803488
ms.sourcegitcommit: 28c93f364c51774e8fbde9afb5aa62f1299e649e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97821528"
---
Sıkıştırılmış ses işleme, [GStreamer](https://gstreamer.freedesktop.org)kullanılarak uygulanır. Lisanslama nedeniyle, GStreamer ikili dosyaları derlenmez ve konuşma SDK 'Sı ile bağlantılı değildir. Geliştiricilerin çeşitli bağımlılıklar ve eklentiler yüklemeleri, bkz. [Windows 'A yükleme](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c) veya [Linux 'a yükleme](https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c). Konuşma SDK 'sının çalışma zamanında ikilileri yükleyebilmesi için GStreamer ikililerinin sistem yolunda olması gerekir. Örneğin, Windows 'ta konuşma SDK 'Sı `libgstreamer-1.0-0.dll` çalışma zamanı sırasında bulabiliyor ise, GStreamer ikilileri sistem yolunda yer alır.

