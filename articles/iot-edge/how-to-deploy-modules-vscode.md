---
title: Visual Studio Code Azure IoT Edge 'dan modül dağıtma
description: IoT Hub bir dağıtım bildirimi tarafından yapılandırıldığı gibi, IoT Edge cihazınıza IoT Edge bir modül göndermek için Azure IoT araçlarıyla Visual Studio Code kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/8/2019
ms.topic: conceptual
ms.reviewer: ''
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: e1b2e2a80670cf0409f8f8477563b9a209cc8706
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77209214"
---
# <a name="deploy-azure-iot-edge-modules-from-visual-studio-code"></a>Visual Studio code'dan Azure IOT Edge modüllerini dağıtmak

IOT Edge modülleri, iş mantığı ile oluşturduktan sonra bunları ucuna çalışılacak cihazlarınıza dağıtmak istiyorsanız. Toplamak ve veri işlemek için birlikte çalışan birden çok modül varsa, bunları tamamını aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirin.

Bu makalede, bir JSON dağıtım bildirimi oluşturun, sonra IOT Edge cihazına dağıtım göndermek için bu dosyayı kullanma gösterilmektedir. Paylaşılan etiketlerine göre birden çok cihazı hedefleyen bir dağıtım oluşturma hakkında bilgi için, bkz. [Visual Studio Code kullanarak ölçekte IoT Edge modülleri dağıtma](how-to-deploy-monitor-vscode.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğinizdeki bir [IoT Hub 'ı](../iot-hub/iot-hub-create-through-portal.md) .
* IoT Edge çalışma zamanı yüklü [IoT Edge bir cihaz](how-to-register-device.md#register-with-visual-studio-code) .
* [Visual Studio Code](https://code.visualstudio.com/).
* Visual Studio Code için [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools#overview) .

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Dağıtım bildirimlerinin nasıl çalıştığı ve nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [IoT Edge modüllerinin nasıl kullanılabileceğini, yapılandırılacağını ve yeniden kullanıldığını anlayın](module-composition.md).

Visual Studio Code kullanarak modüllerini dağıtmak için dağıtım bildirimi olarak yerel olarak kaydedin. bir. JSON dosyası. Cihazınıza yapılandırmayı uygulamak için komutu çalıştırdığınızda, sonraki bölümde dosya yolu kullanır.

Örnek olarak bir modülü ile temel bir dağıtım bildirimi şöyledir:

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

## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınıza erişmek için oturum açın

Visual Studio Code için Azure IOT uzantıları, IOT hub'ınıza işlemleri gerçekleştirmek için kullanabilirsiniz. Bu işlemleri çalışmak Azure hesabınızda oturum açın ve üzerinde çalıştığınız IOT hub'ı seçmek gerekir.

1. Visual Studio Code ' de **Gezgin** görünümünü açın.

1. Gezgin 'in alt kısmındaki **Azure IoT Hub** bölümünü genişletin.

   ![Azure IoT Hub bölümünü Genişlet](./media/how-to-deploy-modules-vscode/azure-iot-hub-devices.png)

1. **Azure IoT Hub** bölüm üstbilgisindeki **..** . öğesine tıklayın. Üç nokta simgesini görmüyorsanız, üst bilgisinin üzerinde gezdirin.

1. **IoT Hub Seç ' i**seçin.

1. Azure hesabınızda oturum açmadınız, bunu yapmak için yönergeleri izleyin.

1. Azure aboneliğinizi seçin.

1. IOT hub'ınızı seçin.

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

Modül bilgileri yapılandırdığınız dağıtım bildirimini uygulayarak modülleri cihazınıza dağıtın.

1. Visual Studio Code Gezgini görünümünde, **Azure IoT Hub** bölümünü genişletin ve ardından **cihazlar** düğümünü genişletin.

1. Dağıtım bildirimiyle yapılandırmak istediğiniz IoT Edge cihaza sağ tıklayın.

    > [!TIP]
    > Seçtiğiniz cihazın IoT Edge bir cihaz olduğunu doğrulamak için, modül listesini genişletmek ve **$edgeHub** ve **$edgeAgent**varlığını doğrulamak için seçin. Her IoT Edge cihaz bu iki modülü içerir.

1. **Tek cihaz Için dağıtım oluştur**' u seçin.

1. Kullanmak istediğiniz dağıtım bildirimi JSON dosyasına gidin ve **kenar dağıtım bildirimini Seç**' e tıklayın.

   ![Edge dağıtım bildirimi seçin](./media/how-to-deploy-modules-vscode/select-deployment-manifest.png)

Dağıtımınızın sonuçlarını, VS Code çıktısında yazdırılır. Başarılı dağıtımlarında hedef cihaza çalışıyorsa birkaç dakika içinde uygulanan ve internet'e bağlı.

## <a name="view-modules-on-your-device"></a>Cihazınızda modülleri görüntüleme

Modülleri cihazınıza dağıttıktan sonra, bunların tümünü **Azure IoT Hub** bölümünde görüntüleyebilirsiniz. Genişletmek için IOT Edge cihazınızın yanındaki oku seçin. Çalışmakta olan tüm modülleri görüntülenir.

Yeni modülleri bir cihaza yakın zamanda dağıttıysanız, **Azure IoT Hub cihazlar** bölüm üst bilgisinin üzerine gelin ve Yenile simgesini seçerek görünümü güncelleştirin.

Bir modül, modül ikizi görüntüleyip adına sağ tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio Code kullanarak ölçekli IoT Edge modüllerini dağıtmayı ve izlemeyi](how-to-deploy-monitor.md) öğrenin
