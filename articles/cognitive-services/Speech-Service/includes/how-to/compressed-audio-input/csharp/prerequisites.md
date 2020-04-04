---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 07405ad0da9c9ba280810402a638395a7feb8554
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637273"
---
Sıkıştırılmış ses işleme [GStreamer](https://gstreamer.freedesktop.org)kullanılarak uygulanır. Lisans lama nedenleriyle GStreamer ikilileri derlenmez ve Konuşma SDK ile bağlantılı değildir. Geliştiricilerin çeşitli bağımlılıklar ve eklentiler yüklemesi gerekir, [Windows'ta Yükleme](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c)bakın. Gstreamer ikililerinin sistem yolunda olması gerekir, böylece Konuşma SDK çalışma süresi sırasında gstreamer ikilileri yükleyebilirsiniz. Konuşma SDK çalışma sırasında libgstreamer-1.0-0.dll bulmak mümkün ise bu gstreamer ikilisistem yolunda olduğu anlamına gelir.

