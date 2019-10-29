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
ms.openlocfilehash: fab9a8a8c28f2f75e7e5af69b70229c1de74c684
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72992294"
---
## <a name="deploy-event-grid-iot-edge-module"></a>Event Grid IoT Edge modülünü dağıt

IoT Edge bir cihaza modül dağıtmanın birkaç yolu vardır ve bunların hepsi IoT Edge Azure Event Grid için çalışır. Bu makalede Azure portal IoT Edge Event Grid dağıtma adımları açıklanmaktadır.

>[!NOTE]
> Bu öğreticide, Event Grid modülünü kalıcı olmadan dağıtacaksınız. Bu öğreticide oluşturduğunuz tüm konuların ve aboneliklerin, modülü yeniden dağıtırken silineceği anlamına gelir. Kalıcılığı ayarlama hakkında daha fazla bilgi için şu makalelere bakın: [Linux 'ta durumu kalıcı hale](../articles/event-grid/edge/persist-state-linux.md) getirin veya [Windows 'da durumu devam](../articles/event-grid/edge/persist-state-windows.md)ettir. Üretim iş yükleri için Event Grid modülünü Kalıcılık ile yüklemenizi öneririz.

>[!IMPORTANT]
> Bu öğreticide, Event Grid modülü istemci kimlik doğrulaması kapalı ile dağıtılacak ve HTTP abonelerine izin vermeyecektir. Üretim iş yükleri için, istemci kimlik doğrulaması etkinleştirilmiş olarak yalnızca HTTPS isteklerini ve aboneleri etkinleştirmenizi öneririz. Event Grid modülünü güvenli şekilde yapılandırma hakkında daha fazla bilgi için bkz. [güvenlik ve kimlik doğrulaması](../articles/event-grid/edge/security-authentication.md).

### <a name="select-your-iot-edge-device"></a>IoT Edge cihazınızı seçin

1. [Azure portalda](https://portal.azure.com) oturum açma
1. IoT Hub gidin.
1. **Otomatik cihaz yönetimi** bölümündeki menüden **IoT Edge** ' yi seçin. 
1. Cihaz listesinden hedef cihazın KIMLIĞINE tıklayın
1. **Modülleri Ayarlama**'yı seçin. Sayfayı açık tutun. Sonraki bölümde bulunan adımlarla devam edersiniz.

### <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimi yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılacağını, modüller arasında verilerin nasıl akacağını ve modül TWINS 'in istenen özelliklerini tanımlayan bir JSON belgesidir. Azure portal, JSON belgesini el ile oluşturmak yerine bir dağıtım bildirimi oluşturma konusunda size yol gösteren bir sihirbaza sahiptir.  Üç adım vardır: **modüller ekleme**, **rotalar belirtme**ve **dağıtımı İnceleme**.

### <a name="add-modules"></a>Modül Ekle

1. **Dağıtım modülleri** bölümünde **Ekle** ' yi seçin.
1. Açılan listedeki modül türlerinden **IoT Edge modül** ' ı seçin.
1. Kapsayıcının adını, görüntüsünü, kapsayıcı oluşturma seçeneklerini belirtin:

   * **Ad**: eventgridmodule
   * **Görüntü URI 'si**: `mcr.microsoft.com/azure-event-grid/iotedge:latest`
   * **Kapsayıcı oluşturma seçenekleri**:

    ```json
        {
          "Env": [
            "inbound:clientAuth:clientCert:enabled=false",
            "outbound:webhook:httpsOnly=false"
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

 1. **Kaydet**’e tıklayın
 1. Yönlendirmeler bölümüne devam etmek için **İleri** 'ye tıklayın

### <a name="setup-routes"></a>Kurulum yolları

 Varsayılan yolları koruyun ve gözden geçirme bölümüne devam etmek için **İleri** ' yi seçin.

### <a name="review-deployment"></a>Dağıtımı gözden geçir

1. İnceleme Bölümü, önceki iki bölümdeki seçimlerinize göre oluşturulan JSON dağıtım bildirimini gösterir. Listedeki iki modülü gördüistediğinizi onaylayın: **$edgeAgent** ve **$edgeHub**. Bu iki modül IoT Edge çalışma zamanını yapar ve her dağıtımda gerekli varsayılanlar olur.
1. Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**' i seçin.

### <a name="verify-your-deployment"></a>Dağıtımınızı doğrulama

1. Dağıtımı gönderdikten sonra IoT Hub 'ınızın IoT Edge sayfasına dönersiniz.
1. Bilgilerini açmak için dağıtıma hedeflenmiş **IoT Edge cihazı** seçin.
1. Cihaz ayrıntılarında Event Grid modülünün hem **dağıtımda** hem de **cihaz tarafından raporlanarak**listelendiğinden emin olun.

Modülün cihazda başlatılması ve sonra IoT Hub geri bildirilmesi birkaç dakika sürebilir. Güncelleştirilmiş durumu görmek için sayfayı yenileyin.