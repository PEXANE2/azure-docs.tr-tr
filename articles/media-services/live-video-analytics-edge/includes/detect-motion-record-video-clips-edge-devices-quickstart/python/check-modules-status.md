---
ms.openlocfilehash: 53052097fa6616f889b710c58488a9f7a616168d
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98956378"
---
[IoT Edge dağıtım bildirimi oluşturma ve dağıtma](../../../detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest) adımında, Visual Studio Code ' de **Azure IoT Hub** altında **LVA-örnek-cihaz** düğümünü genişletin (sol alt bölümde). Aşağıdaki modüllerin dağıtıldığını görmeniz gerekir:

* Adlı canlı video analizi modülü `lvaEdge`
* `rtspsim`Canlı video akışı kaynağı görevi gören BIR RTSP sunucusunun benzetimini yapan modül

  ![Modül](../../../media/quickstarts/lva-sample-device-node.png)

> [!NOTE]
> Yukarıdaki adımlar, kurulum betiği tarafından oluşturulan sanal makineyi kullandığınızı kabul ediyor. Bunun yerine kendi Edge cihazınızı kullanıyorsanız, bu hızlı başlangıç için kullanılan örnek video dosyasını çekmek ve depolamak için Edge cihazınıza gidin ve **yönetici haklarıyla** aşağıdaki komutları çalıştırın:  

```
mkdir /home/lvaadmin/samples
mkdir /home/lvaadmin/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
chown -R lvaadmin /home/lvaadmin/samples/  
```
