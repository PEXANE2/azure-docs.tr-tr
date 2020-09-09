---
ms.openlocfilehash: 055448eacf1cc12c6d021c6571a0478cb35442ba
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89566932"
---
Dağıtım bildirimi, bir sınır cihazına hangi modüllerin dağıtıldığını tanımlar. Ayrıca, bu modüllerle ilgili yapılandırma ayarlarını tanımlar. 

Şablon dosyasından bildirim oluşturmak ve ardından Edge cihazına dağıtmak için bu adımları izleyin.

1. Visual Studio Code’u açın.
1. **Azure ıOT hub** bölmesinin yanındaki IoT Hub bağlantı dizesini ayarlamak için **diğer eylemler** simgesini seçin. Dizeyi *src/buluttan cihazdan-Console-App/appsettings.js* dosyasından kopyalayabilirsiniz. 

    ![IOT bağlantı dizesi ayarla](../../../media/quickstarts/set-iotconnection-string.png)
1. **Src/Edge/deployment.template.jsüzerinde** sağ tıklayın ve **IoT Edge dağıtım bildirimi oluştur**' u seçin.

    ![IoT Edge dağıtım bildirimini oluşturma](../../../media/quickstarts/generate-iot-edge-deployment-manifest.png)

    Bu eylem, *src/Edge/config* klasöründe *deployment.amd64.js* adlı bir bildirim dosyası oluşturmalı.
1. **Src/Edge/config/deployment.amd64.jsüzerinde**sağ tıklayın, **tek cihaz için dağıtım oluştur**' u seçin ve ardından Edge cihazınızın adını seçin.

    ![Tek bir cihaz için dağıtım oluşturma](../../../media/quickstarts/create-deployment-single-device.png)

1. IoT Hub bir cihaz seçmeniz istendiğinde, açılan menüden **LVA-Sample-Device** ' ı seçin.
1. 30 saniye sonra, pencerenin sol alt köşesinde Azure IoT Hub ' yi yenileyin. Edge cihazında artık aşağıdaki dağıtılan modüller gösterilmektedir:

    * IoT Edge (modül adı) üzerinde canlı video analizi `lvaEdge`
    * Gerçek zamanlı akış protokolü (RTSP) Simülatörü (modül adı `rtspsim` )

RTSP simülatör modülü, [canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup)çalıştırdığınızda Edge cihazınıza kopyalanmış bir video dosyası kullanarak canlı bir video akışının benzetimini yapar. 

> [!NOTE]
> Kurulum betiğimizden temin yerine kendi Edge cihazınızı kullanıyorsanız, bu hızlı başlangıç için kullanılan örnek video dosyasını çekmek ve depolamak için uç cihazınıza gidin ve **yönetici haklarıyla**aşağıdaki komutları çalıştırın:  

```
mkdir /home/lvaadmin/samples      
mkdir /home/lvaadmin/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
chown -R lvaadmin /home/lvaadmin/samples/  
```
Bu aşamada modüller dağıtılır ancak hiçbir medya grafiği etkin değildir.
