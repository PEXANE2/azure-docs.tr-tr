---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: 3d4c908e0caf1cf84159df49d98603fd13b75994
ms.sourcegitcommit: 28c93f364c51774e8fbde9afb5aa62f1299e649e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97821562"
---
Sıkıştırılmış ses işleme, [GStreamer](https://gstreamer.freedesktop.org)kullanılarak uygulanır. Lisanslama nedeniyle, GStreamer ikili dosyaları derlenmez ve konuşma SDK 'Sı ile bağlantılı değildir. Geliştiricilerin çeşitli bağımlılıklar ve eklentiler yüklemeleri, bkz. [Windows 'A yükleme](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c) veya [Linux 'a yükleme](https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c). Konuşma SDK 'sının çalışma zamanında ikilileri yükleyebilmesi için GStreamer ikililerinin sistem yolunda olması gerekir. Örneğin, Windows 'ta konuşma SDK 'Sı `libgstreamer-1.0-0.dll` çalışma zamanı sırasında bulabiliyor ise, GStreamer ikilileri sistem yolunda yer alır.

