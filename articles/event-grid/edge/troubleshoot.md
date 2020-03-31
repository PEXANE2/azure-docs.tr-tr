---
title: Sorun Giderme - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: IoT Edge'deki Event Grid'de Sorun Giderme.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/24/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 95181d0eb23d5956b2c6af52c77f85714b107345
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73100149"
---
# <a name="common-issues"></a>Genel Sorunlar

Ortamınızda IoT Edge'de Azure Olay Kılavuzu'nu kullanarak sorunlarla karşılaşırsanız, bu makaleyi sorun giderme ve çözüm için bir kılavuz olarak kullanın.

## <a name="view-event-grid-module-logs"></a>Olay Izgara modül günlüklerini görüntüle

Sorun gidermek için Olay Izgara modülü günlüklerine erişmeniz gerekebilir. Bunu yapmak için, modülün dağıtıldığı VM'de aşağıdaki komutu çalıştırın:

Pencerelerde,

```sh
docker -H npipe:////./pipe/iotedge_moby_engine container logs eventgridmodule
```

Linux'ta,

```sh
sudo docker logs eventgridmodule
```

## <a name="unable-to-make-https-requests"></a>HTTPS isteklerini yapamamak

* Öncelikle Olay Izgara modülü gelen olduğundan emin **olun:serverAuth:tlsPolicy** **katı** veya **etkin**olarak ayarlanır.

* Modülden modüle iletişim sağlıyorsa, **4438** numaralı bağlantı noktasından arama yaptığınızdan ve modülün adının dağıtılanla eşleştiğinden emin olun. 

  Örneğin, Olay Izgara modülü ad **eventgridmodule** ile dağıtıldıysa, **https://eventgridmodule:4438**URL'niz . Kasa ve bağlantı noktası numarasının doğru olduğundan emin olun.
    
* IoT modülü olmayan bir modülden geliyorsa, örneğin dağıtım sırasında Olay Izgara bağlantı noktasının Ana Bilgisayar makinesine eşlenindiğinden emin olun,

    ```json
    "HostConfig": {
                "PortBindings": {
                  "4438/tcp": [
                    {
                        "HostPort": "4438"
                    }
                  ]
                }
     }
    ```

## <a name="unable-to-make-http-requests"></a>HTTP isteklerini yapamamak

* Önce Olay Izgara modülü gelen olduğundan emin **olun:serverAuth:tlsPolicy** **etkin** veya **devre dışı ayarlanmış**.

* Modülden modüle iletişim sağlıyorsa, **5888** numaralı bağlantı noktasında arama yaptığınızdan ve modülün adının dağıtılanla eşleştiğinden emin olun. 

  Örneğin, Olay Izgara modülü ad **eventgridmodule** ile dağıtıldıysa, **http://eventgridmodule:5888**URL'niz . Kasa ve bağlantı noktası numarasının doğru olduğundan emin olun.
    
* IoT modülü olmayan bir modülden geliyorsa, örneğin dağıtım sırasında Olay Izgara bağlantı noktasının Ana Bilgisayar makinesine eşlenindiğinden emin olun,

    ```json
    "HostConfig": {
                "PortBindings": {
                  "5888/tcp": [
                    {
                        "HostPort": "5888"
                    }
                  ]
                }
    }
    ```

## <a name="certificate-chain-was-issued-by-an-authority-thats-not-trusted"></a>Sertifika zinciri, güvenilmeyen bir makam tarafından verildi

Varsayılan olarak, Olay Izgara modülü, IoT Edge güvenlik daemon tarafından verilen sertifika ile istemcileri doğrulamak için yapılandırılır. İstemcinin bu zincire köklü bir sertifika sunduğundan emin olun.

**IoTSecurity** sınıfında [https://github.com/Azure/event-grid-iot-edge](https://github.com/Azure/event-grid-iot-edge) IoT Edge Security daemon'dan sertifikalarınasıl alınır ve giden aramaları yapılandırmak için bunu kullanırsınız.

Üretim dışı ortamsa, istemci kimlik doğrulamasını kapatma seçeneğiniz vardır. Bunun nasıl yapılacağının ayrıntıları için [Güvenlik ve Kimlik Doğrulama'ya](security-authentication.md) bakın.

## <a name="debug-events-not-received-by-subscriber"></a>Abone tarafından alınmayan Hata Ayıklama Olayları

Bunun tipik nedenleri şunlardır:

* Olay hiçbir zaman başarılı bir şekilde yayınlanmadı. Olay Izgara modülüne bir olay gönderirken 200(OK) http StatusCode alınmalıdır.

* Doğrulamak için olay aboneliğini denetleyin:
    * Uç nokta URL geçerli
    * Abonelikteki filtreler, olayın "düşmesine" neden olmaz.

* Abone modülünün çalışır durumda olup olmadığını doğrulama

* Olay Izgara modülünün dağıtıldığı VM'de oturum açın ve günlüklerini görüntüleyin.

* **Broker:logDeliverySuccess=true** ayarlayarak ve Olay Izgara modüllerini yeniden dağıtarak ve isteği yeniden deneyerek teslimat başına günlük başına açın. Teslimat başına günlüğe kaydetmenin gecikme sebebleri etkilemesi, hata ayıklama tamamlandıktan sonra önerimiz bunu komisyoncuya geri **döndürmektir: logDeliverySuccess=false** ve Olay Izgara modülü yeniden dağıtılmak.

* Ölçümleri ayarlayarak **ölçümleri açın:reportertype=console** ve Olay Izgara modüllerini yeniden dağıtın. Bundan sonraki işlemler, ölçümlerin daha fazla hata ayıklamak için kullanılabilecek Event Grid modülünün konsolunda günlüğe kaydedilmesiyle sonuçlanır. Tavsiyemiz, yalnızca hata ayıklama için ölçümleri açmak ve ölçümleri ayarlayarak kapatmak için **tamamlandığında:reportertype=none** ve Olay Izgara modüllerini yeniden dağıtmaktır.

## <a name="next-steps"></a>Sonraki adımlar

IoT Edge'de [https://github.com/Azure/event-grid-iot-edge/issues](https://github.com/Azure/event-grid-iot-edge/issues)Event Grid'i kullanarak ilgili sorunları, önerileri bildirin.