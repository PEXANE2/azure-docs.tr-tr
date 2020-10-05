---
ms.openlocfilehash: e46a56742ab8b98c53c1cd05e840e8ad4b8a73da
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "88682237"
---
[IoT Edge dağıtım bildirimi oluşturma ve dağıtma](../../../detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest) adımında, Visual Studio Code ' de **Azure IoT Hub** altında **LVA-örnek-cihaz** düğümünü genişletin (sol alt bölümde). Aşağıdaki modüllerin dağıtıldığını görmeniz gerekir:

* Adlı canlı video analizi modülü `lvaEdge`
* `rtspsim`Canlı video akışı kaynağı görevi gören BIR RTSP sunucusunun benzetimini yapan modül

  ![Modül](../../../media/quickstarts/lva-sample-device-node.png)

> [!NOTE]
> Kurulum betiğimizden temin yerine kendi Edge cihazınızı kullanıyorsanız, bu hızlı başlangıç için kullanılan örnek video dosyasını çekmek ve depolamak için uç cihazınıza gidin ve **yönetici haklarıyla**aşağıdaki komutları çalıştırın:  

```
mkdir /home/lvaadmin/samples
mkdir /home/lvaadmin/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
chown -R lvaadmin /home/lvaadmin/samples/  
```
