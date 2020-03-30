---
title: include dosyası
description: include dosyası
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 10/10/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b453a04a170764a037eed7415eaf71e5a4d37526
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844606"
---
## <a name="deploy-event-grid-iot-edge-module"></a>Olay Izgara IoT Edge modüllerini dağıtın

Modülleri bir IoT Edge aygıtına dağıtmanın çeşitli yolları vardır ve bunların tümü IoT Edge'deki Azure Olay Izgarası için çalışır. Bu makalede, Azure portalından IoT Edge'de Olay Izgarasını dağıtma adımları açıklanmaktadır.

>[!NOTE]
> Bu eğitimde, Etkinlik Izgara modüllerini kalıcılık olmadan dağıtmış olursunuz. Bu, modülü yeniden dağıttığınızda bu öğreticide oluşturduğunuz tüm konuların ve aboneliklerin silineceği anlamına gelir. Kalıcılığı nasıl düzenesin hakkında daha fazla bilgi için aşağıdaki makalelere bakın: [Linux'ta kalıcı durum](../articles/event-grid/edge/persist-state-linux.md) veya [Windows'da kalıcı durum.](../articles/event-grid/edge/persist-state-windows.md) Üretim iş yükleri için Olay Izgara modüllerini kalıcı olarak yüklemenizi öneririz.

>[!IMPORTANT]
> Bu eğitimde, Olay Izgara modülü istemci kimlik doğrulaması kapalı olarak dağıtılır ve HTTP abonelerine izin verir. Üretim iş yükleri için yalnızca HTTPS isteklerini ve istemci kimlik doğrulaması etkin leştirilmiş aboneleri etkinleştirmenizi öneririz. Olay Izgara modüllerini güvenli bir şekilde yapılandırma hakkında daha fazla bilgi için [Güvenlik ve kimlik doğrulama](../articles/event-grid/edge/security-authentication.md)sına bakın.
 
### <a name="select-your-iot-edge-device"></a>IoT Edge cihazınızı seçin

1. [Azure portalında](https://portal.azure.com) oturum açın
1. IoT Hub'ınıza gidin.
1. **Otomatik Aygıt Yönetimi** bölümündeki menüden **IoT Edge'i** seçin. 
1. Aygıtlar listesinden hedef aygıtın kimliğine tıklayın
1. **Modülleri Ayarlama**'yı seçin. Sayfayı açık tutun. Sonraki bölümdeki adımlarla devam eceksiniz.

### <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimini yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılabildiğini, modüller arasında verilerin nasıl aktığını ve modülün istenilen özelliklerini açıklayan bir JSON belgesidir. Azure portalında, JSON belgesini el ile oluşturmak yerine dağıtım bildirimi oluşturmada size yol gösteren bir sihirbaz vardır.  Üç adımı vardır: **Modülekleme,** **Rotalar belirt**ve **dağıtımı gözden geçirin.**

### <a name="add-modules"></a>Modül ekle

1. Dağıtım **Modülleri** bölümünde Ekle'yi **seçin**
1. Açılan listedeki modül türlerine göre **IoT Edge Modülünü** seçin
1. Kapsayıcının adını, görüntüsünü, kapsayıcıyı oluşturma seçeneklerini sağlayın:

[!INCLUDE [event-grid-edge-module-version-update](event-grid-edge-module-version-update.md)]

   * **Adı**: eventgridmodule
   * **Resim URI**:`mcr.microsoft.com/azure-event-grid/iotedge:latest`
   * **Konteyner Oluşturma Seçenekleri**:

    ```json
        {
          "Env": [
            "inbound__clientAuth:clientCert__enabled=false",
            "outbound__webhook__httpsOnly=false"
          ],
          "HostConfig": {
            "PortBindings": {
              "4438/tcp": [
                {
                  "HostPort": "4438"
                }
              ]
            }
          }
        }
    ```

 1. **Kaydet'i** tıklatın
 1. Rotalar bölümüne devam etmek için **İleri'yi** tıklatın

    > [!NOTE]
    > Kenar aygıtı olarak bir Azure VM kullanıyorsanız, bağlantı noktası 4438'de gelen trafiğine izin vermek için gelen bağlantı noktası kuralını ekleyin. Kuralı ekleme yönergeleri için, [bağlantı noktalarını VM'ye nasıl açacağına](../articles/virtual-machines/windows/nsg-quickstart-portal.md)bakın.


### <a name="setup-routes"></a>Kurulum rotaları

 Varsayılan yolları tutun ve inceleme bölümüne devam etmek için **Sonraki'ni** seçin

### <a name="review-deployment"></a>Dağıtımı gözden geçirme

1. İnceleme bölümü, önceki iki bölümdeki seçimlerinize göre oluşturulan JSON dağıtım bildirimini gösterir. Listedeki iki modülü gördüğünüzden onaylayın: **$edgeAgent** ve **$edgeHub.** Bu iki modül IoT Edge çalışma süresini oluşturan ve her dağıtımda varsayılan olarak gereklidir.
1. Dağıtım bilgilerinizi gözden geçirin ve **ardından Gönder'i**seçin.

### <a name="verify-your-deployment"></a>Dağıtımınızı doğrulayın

1. Dağıtımı gönderdikten sonra, IoT hub'ınızın IoT Edge sayfasına geri dönersiniz.
1. Ayrıntıları açmak için dağıtımla hedeflediğiniz **IoT Edge aygıtını** seçin.
1. Aygıt ayrıntılarında, Olay Izgara modülünün hem **dağıtımda belirtilen** hem de **aygıt tarafından bildirilen**olarak listelenmiş olduğunu doğrulayın.

Modülün cihazda başlatılması ve ardından IoT Hub'a rapor edilmesi birkaç dakika sürebilir. Güncelleştirilmiş bir durumu görmek için sayfayı yenileyin.