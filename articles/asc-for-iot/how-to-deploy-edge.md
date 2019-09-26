---
title: IoT Edge modülü için Azure Güvenlik Merkezi 'Ni dağıtma | Microsoft Docs
description: IoT Edge 'de IoT güvenlik Aracısı için Azure Güvenlik Merkezi 'ni dağıtma hakkında bilgi edinin.
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
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: bb6a975d2a2fc2cc3e65fa8969f8b005be8b1417
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299713"
---
# <a name="deploy-a-security-module-on-your-iot-edge-device"></a>IoT Edge cihazınızda bir güvenlik modülü dağıtma


**IoT modülü Için Azure Güvenlik Merkezi** , IoT Edge cihazlarınız için kapsamlı bir güvenlik çözümü sağlar.
Güvenlik modülü, Işletim sistemi ve kapsayıcı sisteminizdeki ham güvenlik verilerini, uygulanabilir güvenlik önerileri ve uyarılara toplar, toplar ve analiz eder.
Daha fazla bilgi için bkz. [IoT Edge Için güvenlik modülü](security-edge-architecture.md).

Bu makalede, IoT Edge cihazınızda bir güvenlik modülünü dağıtmayı öğreneceksiniz.

## <a name="deploy-security-module"></a>Güvenlik modülünü dağıt

IoT Edge için bir IoT güvenlik modülü için Azure Güvenlik Merkezi 'ni dağıtmak üzere aşağıdaki adımları kullanın.

### <a name="prerequisites"></a>Önkoşullar

1. IoT Hub, cihazınızın [bir IoT Edge cihaz olarak kaydedildiğinden](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal)emin olun.

1. IoT Edge modülü için Azure Güvenlik Merkezi, IoT Edge cihazında [Auditd çerçevesinin](https://linux.die.net/man/8/auditd) yüklü olmasını gerektirir.

    - IoT Edge cihazınızda aşağıdaki komutu çalıştırarak çerçeveyi yüklemelisiniz:
   
    `sudo apt-get install auditd audispd-plugins`

    - Aşağıdaki komutu çalıştırarak Sestd 'nin etkin olduğunu doğrulayın: 
   
    `sudo systemctl status auditd`<br>
    - Beklenen yanıt:`active (running)` 
        

### <a name="deployment-using-azure-portal"></a>Azure portal kullanarak dağıtım

1. Azure portal **Market**' i açın.

1. **Nesnelerin interneti**' yi seçin, **IoT Için Azure Güvenlik Merkezi** ' ni arayın ve seçin.

   ![IoT için Azure Güvenlik Merkezi 'Ni seçin](media/howto/edge-onboarding-8.png)

1. Dağıtımı yapılandırmak için **Oluştur** ' a tıklayın. 

1. IoT Hub Azure **aboneliğini** seçin ve ardından **IoT Hub**seçin.<br>Tek bir cihazı hedeflemek için **cihaza dağıt** ' ı seçin veya birden çok cihazı hedeflemek Için **ölçeğe dağıt** ' ı seçin ve **Oluştur**' a tıklayın. Ölçekli dağıtım hakkında daha fazla bilgi için bkz. [nasıl dağıtılır](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor). 

    >[!Note] 
    >**Aynı ölçekte dağıt**' ı seçtiyseniz aşağıdaki yönergelerde **Modül Ekle** sekmesine geçmeden önce cihaz adını ve ayrıntılarını ekleyin.     

IoT için Azure Güvenlik Merkezi için IoT Edge dağıtımı oluşturmanın üç adımı vardır. Aşağıdaki bölümlerde, her birini yol. 

#### <a name="step-1-add-modules"></a>1\. adım: Modül Ekle

1. **Modül Ekle** sekmesindeki **dağıtım modülleri** alanında, **AzureSecurityCenterforIoT**' a tıklayın. 
   
1. **Adı** **azureiotsecurity**olarak değiştirin.
1. **Görüntü URI** 'sini **MCR.Microsoft.com/ascforiot/azureiotsecurity:1.0.0**olarak değiştirin.
1. **Kapsayıcı oluşturma seçenekleri** değerinin olarak ayarlandığını doğrulayın:      
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
1. **İkizi 'ın istenen özelliklerini ayarla** öğesinin seçili olduğunu doğrulayın ve yapılandırma nesnesini şu şekilde değiştirin:
      
    ``` json
    "desired": {
        "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration": {
          } 
        }
    ```

1. **Kaydet**’e tıklayın.
1. Sekmenin en altına kaydırın ve **Gelişmiş kenar çalışma zamanı ayarlarını yapılandır**' ı seçin. 
   
1. **Edge hub 'ındaki** **görüntüyü** **MCR.Microsoft.com/azureiotedge-Hub:1.0.9-RC2**olarak değiştirin.

1. **Oluşturma seçeneklerini doğrulama seçeneği** şu şekilde ayarlanır: 
         
    ``` json
    { 
    "HostConfig":{
                    "PortBindings":{
                    "8883/tcp": [{"HostPort": "8883"}],
                    "443/tcp": [{"HostPort": "443"}],
                    "5671/tcp": [{"HostPort": "5671"}]
                    }
        }
    }
    ```
1. **Kaydet**’e tıklayın.
   
1. **İleri**’ye tıklayın.

#### <a name="step-2-specify-routes"></a>2\. adım: Rota belirtme 

1. **Rotaları belirtin** sekmesinde, **azureiotsecurity** modülünden **$upstream**iletileri ileten bir yolunuz (açık veya kapalı) olduğundan emin olun. 
1. **İleri**’ye tıklayın.

    ~~~Default implicit route
    "route": "FROM /messages/* INTO $upstream" 
    ~~~

    ~~~Explicit route
    "ASCForIoTRoute": "FROM /messages/modules/azureiotsecurity/* INTO $upstream"
    ~~~

#### <a name="step-3-review-deployment"></a>3\. adım: Dağıtım gözden geçirin

- Dağıtımı **gözden geçir** sekmesinde dağıtım bilgilerinizi gözden geçirin ve ardından dağıtımı tamamladıktan sonra **Gönder** ' i seçin.

## <a name="diagnostic-steps"></a>Tanılama adımları

Bir sorunla karşılaşırsanız, kapsayıcı günlükleri IoT Edge bir güvenlik modülü cihazının durumu hakkında bilgi almanın en iyi yoludur. Bilgi toplamak için bu bölümdeki komutları ve araçları kullanın.

### <a name="verify-the-required-containers-are-installed-and-functioning-as-expected"></a>Gerekli kapsayıcıların yüklendiğini ve beklendiği gibi çalıştığını doğrulayın

1. IoT Edge cihazınızda aşağıdaki komutu çalıştırın:
    
    `sudo docker ps`
   
1. Aşağıdaki kapsayıcıların çalıştığını doğrulayın:
   
   | Name | GÖRÜNTÜYLE |
   | --- | --- |
   | azureiotsecurity | mcr.microsoft.com/ascforiot/azureiotsecurity:1.0.0 |
   | edgeHub | mcr.microsoft.com/azureiotedge-hub:1.0.9-rc2 |
   | edgeAgent | mcr.microsoft.com/azureiotedge-agent:1.0 |
   
   Gerekli en az kapsayıcı yoksa, IoT Edge dağıtım bildirimin önerilen ayarlarla hizalanıp Hizalanmadığını denetleyin. Daha fazla bilgi için bkz. [IoT Edge modülünü dağıtma](#deployment-using-azure-portal).

### <a name="inspect-the-module-logs-for-errors"></a>Hatalar için modül günlüklerini inceleyin
   
1. IoT Edge cihazınızda aşağıdaki komutu çalıştırın:

   `sudo docker logs azureiotsecurity`
   
1. Daha ayrıntılı Günlükler için aşağıdaki ortam değişkenini **azureiotsecurity** Module dağıtımına ekleyin: `logLevel=Debug`.

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için, modül yapılandırması için nasıl yapılır kılavuzuna ilerleyin. 
> [!div class="nextstepaction"]
> [Modül yapılandırması nasıl yapılır Kılavuzu](./how-to-agent-configuration.md)
