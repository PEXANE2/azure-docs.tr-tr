---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: 64eab53d24a14bb51633be3f979db20963677d93
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81422254"
---
Sıkıştırılmış ses işleme, [GStreamer](https://gstreamer.freedesktop.org)kullanılarak uygulanır. Lisanslama nedeniyle, GStreamer ikili dosyaları derlenmez ve konuşma SDK 'Sı ile bağlantılı değildir. Geliştiricilerin birkaç bağımlılık ve eklenti yüklemesi gerekir, bkz. [Windows 'A yükleme](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c). Konuşma SDK 'sının çalışma zamanı sırasında GStreamer ikililerini yükleyebilmesi için GStreamer ikililerinin sistem yolunda olması gerekir. Konuşma SDK 'Sı çalışma zamanı sırasında libgstreamer-1.0 -0. dll ' i bulabiliyor ise, GStreamer ikili dosyalarının sistem yolunda olduğu anlamına gelir.

