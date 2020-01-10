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
ms.openlocfilehash: d8c90c2a13d111d01a1e7a1bd9b63da180621ded
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772100"
---
# <a name="deploy-azure-iot-edge-modules-from-visual-studio-code"></a>Visual Studio code'dan Azure IOT Edge modüllerini dağıtmak

IOT Edge modülleri, iş mantığı ile oluşturduktan sonra bunları ucuna çalışılacak cihazlarınıza dağıtmak istiyorsanız. Toplamak ve veri işlemek için birlikte çalışan birden çok modül varsa, bunları tamamını aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirin.

Bu makalede, bir JSON dağıtım bildirimi oluşturun, sonra IOT Edge cihazına dağıtım göndermek için bu dosyayı kullanma gösterilmektedir. Paylaşılan etiketlerine göre birden çok cihazı hedefleyen bir dağıtım oluşturma hakkında bilgi için, bkz. [Visual Studio Code kullanarak ölçekte IoT Edge modülleri dağıtma](how-to-deploy-monitor-vscode.md).

## <a name="prerequisites"></a>Ön koşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki.
* Bir [IOT Edge cihazı](how-to-register-device.md#register-with-visual-studio-code) yüklü olan bir IOT Edge çalışma zamanı ile.
* [Visual Studio Code](https://code.visualstudio.com/).
* Visual Studio Code için [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools#overview) .

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).

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
                 "createOptions": "{}"
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

1. Visual Studio Code'da açmak **Gezgini** görünümü.

1. Gezgin 'in alt kısmındaki **Azure IoT Hub** bölümünü genişletin.

   ![Azure IoT Hub bölümünü Genişlet](./media/how-to-deploy-modules-vscode/azure-iot-hub-devices.png)

1. **Azure IoT Hub** bölüm üstbilgisindeki **..** . öğesine tıklayın. Üç nokta simgesini görmüyorsanız, üst bilgisinin üzerinde gezdirin.

1. Seçin **IOT hub'ını seçin**.

1. Azure hesabınızda oturum açmadınız, bunu yapmak için yönergeleri izleyin.

1. Azure aboneliğinizi seçin.

1. IOT hub'ınızı seçin.

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

Modül bilgileri yapılandırdığınız dağıtım bildirimini uygulayarak modülleri cihazınıza dağıtın.

1. Visual Studio Code Gezgini görünümünde, **Azure IoT Hub** bölümünü genişletin ve ardından **cihazlar** düğümünü genişletin.

1. Dağıtım bildirimiyle yapılandırmak istediğiniz IoT Edge cihaza sağ tıklayın.

    > [!TIP]
    > Seçtiğiniz cihazın IoT Edge bir cihaz olduğunu doğrulamak için, modül listesini genişletmek ve **$edgeHub** ve **$edgeAgent**varlığını doğrulamak için seçin. Her IoT Edge cihaz bu iki modülü içerir.

1. Seçin **tek cihaz için dağıtım oluşturma**.

1. Kullanmak istediğiniz dağıtım bildirim JSON dosyasına gidin ve tıklayın **Edge dağıtım bildirimi seçin**.

   ![Edge dağıtım bildirimi seçin](./media/how-to-deploy-modules-vscode/select-deployment-manifest.png)

Dağıtımınızın sonuçlarını, VS Code çıktısında yazdırılır. Başarılı dağıtımlarında hedef cihaza çalışıyorsa birkaç dakika içinde uygulanan ve internet'e bağlı.

## <a name="view-modules-on-your-device"></a>Cihazınızda modülleri görüntüleme

Modülleri cihazınıza dağıttıktan sonra, bunların tümünü **Azure IoT Hub** bölümünde görüntüleyebilirsiniz. Genişletmek için IOT Edge cihazınızın yanındaki oku seçin. Çalışmakta olan tüm modülleri görüntülenir.

Kısa bir süre önce yeni modülleri bir cihaza dağıtılan, üzerine **Azure IOT Hub cihazları** bölüm başlığı ve Görünümü güncelleştirmek için yenile simgesini seçin.

Bir modül, modül ikizi görüntüleyip adına sağ tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio Code kullanarak ölçekli IoT Edge modüllerini dağıtmayı ve izlemeyi](how-to-deploy-monitor.md) öğrenin
