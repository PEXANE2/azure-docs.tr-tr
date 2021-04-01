---
ms.openlocfilehash: 3f92bae608284c8b619be34a0e08f15e831bf88e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101749965"
---
[IoT Edge dağıtım bildirimi oluşturma ve dağıtma](../../../detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest) adımında, Visual Studio Code ' de **Azure IoT Hub** altında **LVA-örnek-cihaz** düğümünü genişletin (sol alt bölümde). Aşağıdaki modüllerin dağıtıldığını görmeniz gerekir:

* Adlı canlı video analizi modülü `lvaEdge`
* `rtspsim`Canlı video akışı kaynağı görevi gören BIR RTSP sunucusunun benzetimini yapan modül

  ![Modül](../../../media/quickstarts/lva-sample-device-node.png)

> [!NOTE]
> Yukarıdaki adımlar, kurulum betiği tarafından oluşturulan sanal makineyi kullandığınızı kabul ediyor. Bunun yerine kendi Edge cihazınızı kullanıyorsanız, bu hızlı başlangıç için kullanılan örnek video dosyasını çekmek ve depolamak için Edge cihazınıza gidin ve **yönetici haklarıyla** aşağıdaki komutları çalıştırın:  

```
mkdir /home/lvaedgeuser/samples
mkdir /home/lvaedgeuser/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaedgeuser/samples/input/camera-300s.mkv  
chown -R lvalvaedgeuser:localusergroup /home/lvaedgeuser/samples/  
```
