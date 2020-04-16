---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: 64eab53d24a14bb51633be3f979db20963677d93
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422254"
---
Sıkıştırılmış ses işleme [GStreamer](https://gstreamer.freedesktop.org)kullanılarak uygulanır. Lisans lama nedenleriyle GStreamer ikilileri derlenmez ve Konuşma SDK ile bağlantılı değildir. Geliştiricilerin çeşitli bağımlılıklar ve eklentiler yüklemesi gerekir, [Windows'ta Yükleme](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c)bakın. Gstreamer ikililerinin sistem yolunda olması gerekir, böylece Konuşma SDK çalışma süresi sırasında gstreamer ikilileri yükleyebilirsiniz. Konuşma SDK çalışma sırasında libgstreamer-1.0-0.dll bulmak mümkün ise bu gstreamer ikilisistem yolunda olduğu anlamına gelir.

