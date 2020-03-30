---
title: Visual Studio Code'dan modülleri dağıtma - Azure IoT Edge
description: IoT Hub'ınızdan ioT Edge modülünüzü bir dağıtım bildirimi tarafından yapılandırılan IoT Edge cihazınıza itmek için Azure IoT Araçları ile Visual Studio Code'u kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/8/2019
ms.topic: conceptual
ms.reviewer: ''
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: e1b2e2a80670cf0409f8f8477563b9a209cc8706
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77209214"
---
# <a name="deploy-azure-iot-edge-modules-from-visual-studio-code"></a>Azure IoT Edge modüllerini Visual Studio Kodundan dağıtma

İş mantığınızla IoT Edge modülleri oluşturduktan sonra, bunları kenarda çalışması için aygıtlarınıza dağıtmak istersiniz. Verileri toplamak ve işlemek için birlikte çalışan birden çok modülüz varsa, bunları aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirebilirsiniz.

Bu makalede, json dağıtım bildirimi nin nasıl oluşturulacak, ardından dağıtımı bir IoT Edge aygıtına itmek için bu dosyayı kullanın. Paylaşılan etiketlerine göre birden çok aygıtı hedefleyen bir dağıtım oluşturma hakkında bilgi için Visual [Studio Code'u kullanarak ölçekte IoT Edge modüllerini dağıt'a](how-to-deploy-monitor-vscode.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğinizde bir [IoT hub'ı.](../iot-hub/iot-hub-create-through-portal.md)
* IoT Edge çalışma zamanı yüklü bir [IoT Edge aygıtı.](how-to-register-device.md#register-with-visual-studio-code)
* [Görsel Stüdyo Kodu](https://code.visualstudio.com/).
* Visual Studio Code için [Azure IoT Araçları.](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools#overview)

## <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimini yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılabildiğini, modüller arasında verilerin nasıl aktığını ve modülün istenilen özelliklerini açıklayan bir JSON belgesidir. Dağıtım bildirimlerinin nasıl çalıştığı ve bunların nasıl oluşturulabileceği hakkında daha fazla bilgi için [bkz.](module-composition.md)

Visual Studio Code kullanarak modülleri dağıtmak için, dağıtım bildirimini yerel olarak kaydedin. JSON dosyası. Yapılandırmayı cihazınıza uygulamak için komutu çalıştırdığınızda bir sonraki bölümdeki dosya yolunu kullanırsınız.

Aşağıda, örnek olarak bir modüle sahip temel bir dağıtım bildirimi verilmiştir:

   ```json
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
               }
             }
           },
           "modules": {
             "SimulatedTemperatureSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /messages/* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "SimulatedTemperatureSensor": {
         "properties.desired": {}
       }
     }
   }
   ```

## <a name="sign-in-to-access-your-iot-hub"></a>IoT hub'ınıza erişmek için oturum açın

IoT hub'ınızla işlemleri gerçekleştirmek için Visual Studio Code için Azure IoT uzantılarını kullanabilirsiniz. Bu işlemlerin işe yaraması için Azure hesabınızda oturum açmanız ve üzerinde çalıştığınız IoT merkezini seçmeniz gerekir.

1. Visual Studio Code'da **Explorer** görünümünü açın.

1. Explorer'ın alt kısmında **Azure IoT Hub** bölümünü genişletin.

   ![Azure IoT Hub bölümünü genişletme](./media/how-to-deploy-modules-vscode/azure-iot-hub-devices.png)

1. **Azure IoT Hub** **bölümündeki ...** bölümünü tıklatın. Elipsleri görmüyorsanız, başlığın üzerine gidin.

1. **IoT Hub'ı seçin.**

1. Azure hesabınızda oturum açmadıysanız, bunu yapmak için istemleri izleyin.

1. Azure aboneliğinizi seçin.

1. IoT hub'ınızı seçin.

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

Modül bilgileriyle yapılandırdığınız dağıtım bildirimini uygulayarak modülleri cihazınıza dağıtırsınız.

1. Visual Studio Code explorer görünümünde **Azure IoT Hub** bölümünü genişletin ve ardından **Aygıtdüğüm'üne** genişletin.

1. Dağıtım bildirimiyle yapılandırmak istediğiniz IoT Edge aygıtına sağ tıklayın.

    > [!TIP]
    > Seçtiğiniz aygıtın bir IoT Edge aygıtı olduğunu doğrulamak için, modül listesini genişletmek ve **$edgeHub** ve **$edgeAgent**varlığını doğrulamak için aygıtı seçin. Her IoT Edge cihazı bu iki modülü içerir.

1. **Tek Aygıt için Dağıtım Oluştur'u**seçin.

1. Kullanmak istediğiniz dağıtım bildirimi JSON dosyasına gidin ve **Kenar Dağıtım Bildirimi'ni seçin'i**tıklatın.

   ![Kenar Dağıtım Bildirimi'ni seçin](./media/how-to-deploy-modules-vscode/select-deployment-manifest.png)

Dağıtımınızın sonuçları VS Kodu çıkışında yazdırılır. Hedef aygıt çalışıyorsa ve internete bağlıysa, başarılı dağıtımlar birkaç dakika içinde uygulanır.

## <a name="view-modules-on-your-device"></a>Cihazınızdaki modülleri görüntüleme

Modülleri cihazınıza dağıttıktan sonra, tüm modülleri **Azure IoT Hub** bölümünde görüntüleyebilirsiniz. Genişletmek için IoT Edge cihazınızın yanındaki oku seçin. Şu anda çalışan tüm modüller görüntülenir.

Yeni modülleri bir aygıta yakın zamanda dağıttıysanız, **Azure IoT Hub Cihazları** bölüm üstbilgisinin üzerine gidin ve görünümü güncelleştirmek için yenileme simgesini seçin.

Modül ikizini görüntülemek ve düzeltmek için modülün adını sağ tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio Code'u kullanarak IoT Edge modüllerini ölçekte](how-to-deploy-monitor.md) nasıl dağıtılayacağım ve izleyeceğiz öğrenin
