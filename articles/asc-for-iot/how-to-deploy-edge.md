---
title: IoT Edge güvenlik modüllerini dağıtın
description: IoT Edge'de IoT güvenlik aracısı için bir Azure Güvenlik Merkezi'ni nasıl dağıtılayacağım hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2020
ms.author: mlottner
ms.openlocfilehash: 4dd7ca8f926862487b9505731c0662e68ee3d7c0
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311274"
---
# <a name="deploy-a-security-module-on-your-iot-edge-device"></a>IoT Edge cihazınıza bir güvenlik modülü dağıtma

**IoT modülü için Azure Güvenlik Merkezi, IoT** Edge aygıtlarınız için kapsamlı bir güvenlik çözümü sağlar.
Güvenlik modülü, İşletim Sistemi ve Kapsayıcı sisteminizden gelen ham güvenlik verilerini eyleme geçirilebilir güvenlik önerileri ve uyarılarına toplar, toplar ve analiz eder.
Daha fazla bilgi için [IoT Edge için Güvenlik modülüne](security-edge-architecture.md)bakın.

Bu makalede, IoT Edge aygıtınızda bir güvenlik modülü nasıl dağıtıladığınızı öğreneceksiniz.

## <a name="deploy-security-module"></a>Güvenlik modüllerini dağıtın

IoT Edge için IoT güvenlik modülü için bir Azure Güvenlik Merkezi dağıtmak için aşağıdaki adımları kullanın.

### <a name="prerequisites"></a>Ön koşullar

1. IoT Hub'ınızda, cihazınızın [Bir IoT Edge aygıtı olarak kayıtlı](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal)olduğundan emin olun.

1. IoT Edge modülü için Azure Güvenlik Merkezi, IoT Edge aygıtına [AuditD çerçevesinin](https://linux.die.net/man/8/auditd) yüklenmesini gerektirir.

    - IoT Edge cihazınızda aşağıdaki komutu çalıştırarak çerçeveyi yükleyin:

    `sudo apt-get install auditd audispd-plugins`

    - Aşağıdaki komutu çalıştırarak Denetim'in etkin olduğunu doğrulayın:

    `sudo systemctl status auditd`<br>
    - Beklenen yanıt:`active (running)`

### <a name="deployment-using-azure-portal"></a>Azure portalını kullanarak dağıtım

1. Azure portalından, **Market'i**açın.

1. **Nesnelerin İnterneti'ni**seçin, ardından **IoT için Azure Güvenlik Merkezi'ni** arayın ve seçin.

   ![IoT için Azure Güvenlik Merkezi'ni seçin](media/howto/edge-onboarding-8.png)

1. Dağıtımı yapılandırmak için **Oluştur'u** tıklatın.

1. IoT Hub'ınızın Azure **Aboneliğini** seçin ve ardından **IoT Hub'ınızı**seçin.<br>Tek bir aygıtı hedeflemek **için aygıta Dağıt'ı** seçin veya birden çok cihazı hedeflemek için **Ölçekte Dağıt'ı** seçin ve **Oluştur'u**tıklatın. Ölçekte dağıtma hakkında daha fazla bilgi için nasıl [dağıtılanabildiğini](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor)öğrenin.

    >[!Note]
    >**Ölçekte Dağıt'ı**seçtiyseniz, aşağıdaki yönergelerde Modül **Ekle** sekmesine devam etmeden önce aygıt adını ve ayrıntılarını ekleyin.

IoT için Azure Güvenlik Merkezi için IoT Edge dağıtımınızı tamamlamak için her adımı tamamlayın.

#### <a name="step-1-modules"></a>Adım 1: Modüller

1. **AzureSecurityCenterforIoT** modülünü seçin.
1. Modül **Ayarları** sekmesinde, **adı** **azureiotsecurity olarak değiştirin.**
1. **Enviroment Değişkenleri** sekmesinde, gerekirse bir değişken ekleyin (örneğin, hata ayıklama düzeyi).
1. Kapsayıcı **Seçenekler Oluştur** sekmesine aşağıdaki yapılandırmayı ekleyin:

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

1. Modül **İkiz Ayarları** sekmesine aşağıdaki yapılandırmayı ekleyin:

    ``` json
      "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration"
    ```

1. **Güncelleştir**’i seçin.

#### <a name="step-2-runtime-settings"></a>Adım 2: Çalışma zamanı ayarları

1. **Çalışma Zamanı Ayarları'nı**seçin.
1. **Kenar Hub'ı** **altında, Görüntüyü** **mcr.microsoft.com/azureiotedge-hub:1.0.8.3**olarak değiştirin.
1. **Create Options'ı** doğrula aşağıdaki yapılandırmaya ayarlanır:

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

1. **Kaydet'i**seçin.

1. **Sonraki'ni**seçin.

#### <a name="step-3-specify-routes"></a>Adım 3: Rotaları belirtin

1. **Rotaları Belirt** sekmesinde, **azureiotsecurity** modülündeki iletileri aşağıdaki örneklere göre **$upstream** için iletecek bir rotanız (açık veya örtülü) olduğundan emin olun. Yalnızca rota yerindeyken **İleri'yi**seçin.

   Örnek rotalar:

    ```Default implicit route
    "route": "FROM /messages/* INTO $upstream"
    ```

    ```Explicit route
    "ASCForIoTRoute": "FROM /messages/modules/azureiotsecurity/* INTO $upstream"
    ```

1. **Sonraki'ni**seçin.

#### <a name="step-4-review-deployment"></a>Adım 4: Dağıtımı gözden geçirme

- Dağıtımı **Gözden Geçir** sekmesinde, dağıtım bilgilerinizi gözden geçirin ve dağıtımı tamamlamak için **Oluştur'u** seçin.

## <a name="diagnostic-steps"></a>Tanılama adımları

Bir sorunla karşılaşırsanız, kapsayıcı günlükleri bir IoT Edge güvenlik modülü aygıtının durumu hakkında bilgi edinmenin en iyi yoludur. Bilgi toplamak için bu bölümdeki komutları ve araçları kullanın.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Gerekli kapsayıcıların yüklü olduğunu ve beklendiği gibi çalıştığını doğrulayın

1. IoT Edge cihazınızda aşağıdaki komutu çalıştırın:

    `sudo docker ps`

1. Aşağıdaki kapsayıcıların çalıştığını doğrulayın:

   | Adı | GÖRÜNTÜ |
   | --- | --- |
   | azureiotsecurity | mcr.microsoft.com/ascforiot/azureiotsecurity:1.0.2 |
   | edgeHub | mcr.microsoft.com/azureiotedge-hub:1.0.8.3 |
   | edgeAgent | mcr.microsoft.com/azureiotedge-agent:1.0.1 |

   Gerekli minimum kapsayıcılar yoksa, IoT Edge dağıtım bildiriminizin önerilen ayarlarla uyumlu olup olmadığını kontrol edin. Daha fazla bilgi için Bkz. [IoT Edge modüllerini dağıtın.](#deployment-using-azure-portal)

### <a name="inspect-the-module-logs-for-errors"></a>Hatalar için modül günlüklerini inceleyin

1. IoT Edge cihazınızda aşağıdaki komutu çalıştırın:

   `sudo docker logs azureiotsecurity`

1. Daha ayrıntılı günlükler için **azureiotsecurity** modülü dağıtımına aşağıdaki ortam `logLevel=Debug`değişkenini ekleyin: .

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için modül yapılandırması için nasıl yapIlenler kılavuzuna devam edin.
> [!div class="nextstepaction"]
> [Modül yapılandırma nasıl yapılacağını kılavuzu](./how-to-agent-configuration.md)
