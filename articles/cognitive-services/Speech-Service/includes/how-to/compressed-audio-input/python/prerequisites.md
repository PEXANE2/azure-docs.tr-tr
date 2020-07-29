---
author: amitkumarshukla
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: amishu
ms.openlocfilehash: a715d0ece88cf2caf1cb3d20d703f550353094c5
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87282762"
---
Sıkıştırılmış ses işleme kullanılarak uygulanır [`GStreamer`](https://gstreamer.freedesktop.org) . Lisanslama nedenleri için `GStreamer` ikili dosyalar derlenmez ve konuşma SDK 'sı ile bağlantılı değildir. Geliştiricilerin birkaç bağımlılık ve eklenti yüklemesi gerekir, bkz. [Windows 'A yükleme](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c). `GStreamer`konuşma SDK 'sının çalışma zamanında ikilileri yükleyebilmesi için ikililerin sistem yolunda olması gerekir. Konuşma SDK 'Sı `libgstreamer-1.0-0.dll` çalışma zamanı sırasında bulabiliyor ise, ikililerin sistem yolunda olduğu anlamına gelir.

