---
title: IoT Edge Defender-IoT-Micro-Agent ' ı dağıtma
description: IoT Edge 'da IoT güvenlik Aracısı için bir Defender dağıtımı hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2020
ms.author: mlottner
ms.openlocfilehash: e4117c3c0f1016da616a88a36a1b8c926b790c62
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103495122"
---
# <a name="deploy-a-defender-iot-micro-agent-on-your-iot-edge-device"></a>IoT Edge cihazınıza bir Defender-IoT-mikro-aracısı dağıtma

**IoT modülü Için Defender** , IoT Edge cihazlarınız için kapsamlı bir güvenlik çözümü sağlar.
Defender-IoT-Micro-Agent, ham güvenlik verilerini Işletim sisteminden ve kapsayıcı sisteminden, uygulanabilir güvenlik önerileri ve uyarılara toplar, toplar ve analiz eder.
Daha fazla bilgi için bkz. [Defender-IoT-Micro-Agent for IoT Edge](security-edge-architecture.md).

Bu makalede, IoT Edge cihazınıza bir Defender-IoT-mikro-Agent ' ı dağıtmayı öğreneceksiniz.

## <a name="deploy-defender-iot-micro-agent"></a>Defender-IoT-mikro-Aracı dağıtma

IoT Defender-IoT-Micro-Agent for IoT Edge için bir Defender dağıtmak üzere aşağıdaki adımları kullanın.

### <a name="prerequisites"></a>Önkoşullar

1. IoT Hub, cihazınızın [bir IoT Edge cihaz olarak kaydedildiğinden](../iot-edge/how-to-register-device.md#register-a-new-device)emin olun.

1. IoT Edge modülü için Defender, IoT Edge cihazında [Auditd çerçevesinin](https://linux.die.net/man/8/auditd) yüklü olmasını gerektirir.

    - IoT Edge cihazınızda aşağıdaki komutu çalıştırarak çerçeveyi yüklemelisiniz:

    `sudo apt-get install auditd audispd-plugins`

    - Aşağıdaki komutu çalıştırarak Sestd 'nin etkin olduğunu doğrulayın:

    `sudo systemctl status auditd`<br>
    - Beklenen yanıt: `active (running)`

### <a name="deployment-using-azure-portal"></a>Azure portal kullanarak dağıtım

1. Azure portal **Market**' i açın.

1. **Nesnelerin interneti**' yi seçin, **IoT Için Azure Güvenlik Merkezi** ' ni arayın ve seçin.

   :::image type="content" source="media/howto/edge-onboarding-8.png" alt-text="IoT için Defender 'ı seçin":::

1. Dağıtımı yapılandırmak için **Oluştur** ' u seçin.

1. IoT Hub Azure **aboneliğini** seçin ve ardından **IoT Hub** seçin.<br>Tek bir cihazı hedeflemek için **cihaza dağıt** ' ı seçin veya birden çok cihazı hedeflemek Için **ölçeğe dağıt** ' ı seçin ve **Oluştur**' u seçin. Ölçekli dağıtım hakkında daha fazla bilgi için bkz. [nasıl dağıtılır](../iot-edge/how-to-deploy-at-scale.md).

    >[!Note]
    >**Aynı ölçekte dağıt**' ı seçtiyseniz aşağıdaki yönergelerde **Modül Ekle** sekmesine geçmeden önce cihaz adını ve ayrıntılarını ekleyin.

IoT için Defender için IoT Edge dağıtımınızı tamamlamaya yönelik her adımı doldurun.

#### <a name="step-1-modules"></a>1. Adım: modüller

1. **AzureSecurityCenterforIoT** modülünü seçin.
1. **Modül ayarları** sekmesinde **adı** **azureiotsecurity** olarak değiştirin.
1. **Ortam değişkenleri** sekmesinde, gerekirse bir değişken ekleyin (örneğin, *hata ayıklama düzeyi* ekleyebilir ve şu değerlerden birine ayarlayabilirsiniz: "önemli", "hata", "uyarı" veya "bilgi").
1. **Kapsayıcı oluşturma seçenekleri** sekmesinde aşağıdaki yapılandırmayı ekleyin:

    ``` json
    {
        "NetworkingConfig": {
            "EndpointsConfig": {
                "host": {}
            }
        },
        "HostConfig": {
            "Privileged": true,
            "NetworkMode": "host",
            "PidMode": "host",
            "Binds": [
                "/:/host"
            ]
        }
    }
    ```

1. **Modül Ikizi ayarları** sekmesinde, aşağıdaki yapılandırmayı ekleyin:

   Module Ikizi özelliği:
   
   ``` json
     "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration"
   ```

   Module Ikizi Özellik Içeriği: 

   ```json
     {

     }
   ```
    
   Aracıyı yapılandırma hakkında daha fazla bilgi için bkz. [güvenlik aracılarını yapılandırma](./how-to-agent-configuration.md).

1. **Güncelleştir**’i seçin.

#### <a name="step-2-runtime-settings"></a>2. Adım: çalışma zamanı ayarları

1. **Çalışma zamanı ayarları**' nı seçin.
2. **Edge hub**'ı altında, **görüntüyü** **MCR.Microsoft.com/azureiotedge-Hub:1.0.8.3** olarak değiştirin.

    >[!Note]
    > Şu anda, sürüm 1.0.8.3 veya daha eski sürümlerde desteklenmektedir.

3. **Oluşturma seçeneklerini** doğrulama aşağıdaki yapılandırmaya ayarlanır:

    ``` json
    {
       "HostConfig":{
          "PortBindings":{
             "8883/tcp":[
                {
                   "HostPort":"8883"
                }
             ],
             "443/tcp":[
                {
                   "HostPort":"443"
                }
             ],
             "5671/tcp":[
                {
                   "HostPort":"5671"
                }
             ]
          }
       }
    }
    ```

4. **Kaydet**’i seçin.

5. **İleri**’yi seçin.

#### <a name="step-3-specify-routes"></a>3. Adım: yolları belirtme

1. **Rotaları belirtin** sekmesinde, **azureiotsecurity** modülünden iletileri aşağıdaki örneklere göre **$upstream** iletmek için bir yolunuz (açık veya kapalı) olduğundan emin olun. Yalnızca yol yerinde olduğunda, **İleri**' yi seçin.

   Örnek yollar:

    ```Default implicit route
    "route": "FROM /messages/* INTO $upstream"
    ```

    ```Explicit route
    "ASCForIoTRoute": "FROM /messages/modules/azureiotsecurity/* INTO $upstream"
    ```

1. **İleri**’yi seçin.

#### <a name="step-4-review-deployment"></a>4. Adım: dağıtımı Inceleme

- Dağıtımı **gözden geçir** sekmesinde, dağıtım bilgilerinizi gözden geçirin ve ardından **Oluştur** ' u seçerek dağıtımı doldurun.

## <a name="diagnostic-steps"></a>Tanılama adımları

Bir sorunla karşılaşırsanız, kapsayıcı günlükleri IoT Edge Defender-IoT-mikro aracı cihazının durumu hakkında bilgi almanın en iyi yoludur. Bilgi toplamak için bu bölümdeki komutları ve araçları kullanın.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Gerekli kapsayıcıların yüklendiğini ve beklendiği gibi çalıştığını doğrulayın

1. IoT Edge cihazınızda aşağıdaki komutu çalıştırın:

    `sudo docker ps`

1. Aşağıdaki kapsayıcıların çalıştığını doğrulayın:

   | Name | GÖRÜNTÜ |
   | --- | --- |
   | azureiotsecurity | mcr.microsoft.com/ascforiot/azureiotsecurity:1.0.2 |
   | edgeHub | mcr.microsoft.com/azureiotedge-hub:1.0.8.3 |
   | edgeAgent | mcr.microsoft.com/azureiotedge-agent:1.0.1 |

   Gerekli en az kapsayıcı yoksa, IoT Edge dağıtım bildirimin önerilen ayarlarla hizalanıp Hizalanmadığını denetleyin. Daha fazla bilgi için bkz. [IoT Edge modülünü dağıtma](#deployment-using-azure-portal).

### <a name="inspect-the-module-logs-for-errors"></a>Hatalar için modül günlüklerini inceleyin

1. IoT Edge cihazınızda aşağıdaki komutu çalıştırın:

   `sudo docker logs azureiotsecurity`

1. Daha ayrıntılı Günlükler için aşağıdaki ortam değişkenini **azureiotsecurity** Module dağıtımına ekleyin: `logLevel=Debug` .

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için, modül yapılandırması için nasıl yapılır kılavuzuna ilerleyin.
> [!div class="nextstepaction"]
> [Modül yapılandırması nasıl yapılır Kılavuzu](./how-to-agent-configuration.md)