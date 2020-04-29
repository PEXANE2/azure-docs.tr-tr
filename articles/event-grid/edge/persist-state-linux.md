---
title: Linux 'ta kalıcı durum Azure Event Grid IoT Edge | Microsoft Docs
description: Linux 'ta meta verileri kalıcı hale getirme
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 12655d2ceb4a1124376d9bddf82194472c98ebb9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77086660"
---
# <a name="persist-state-in-linux"></a>Linux 'ta durumu kalıcı yap

Event Grid modülünde oluşturulan konular ve abonelikler varsayılan olarak kapsayıcı dosya sisteminde saklanır. Kalıcı olmadan, modül yeniden dağıtılırsa oluşturulan tüm meta veriler kaybedilir. Dağıtımlar ve yeniden başlatmalar genelinde verileri korumak için verileri kapsayıcı dosya sisteminin dışında kalıcı hale getirmeniz gerekir.

Varsayılan olarak yalnızca meta veriler kalıcıdır ve olaylar, daha iyi performans için bellekte depolanır. Olay kalıcılığını da sağlamak için kalıcı olaylar bölümünü izleyin.

Bu makalede, Event Grid modülünü Linux dağıtımlarında kalıcı hale getirme adımları sağlanmaktadır.

> [!NOTE]
>Event Grid modülü UID `2000` ve Name `eventgriduser`ile düşük ayrıcalıklı bir kullanıcı olarak çalışır.

## <a name="persistence-via-volume-mount"></a>Birim bağlama aracılığıyla Kalıcılık

 [Docker birimleri](https://docs.docker.com/storage/volumes/) , dağıtımları genelinde verileri korumak için kullanılır. Event Grid modülünü dağıtmanın bir parçası olarak Docker 'ın adlandırılmış bir birimi otomatik olarak oluşturmasını sağlayabilirsiniz. Bu seçenek en basit seçenektir. **Bağlar** bölümünde oluşturulacak birim adını aşağıdaki şekilde belirtebilirsiniz:

```json
  {
     "HostConfig": {
          "Binds": [
                 "<your-volume-name-here>:/app/metadataDb"
           ]
      }
   }
```

>[!IMPORTANT]
>Bağlama değerinin ikinci kısmını değiştirmeyin. Modül içindeki belirli bir konuma işaret eder. Linux üzerinde Event Grid modülü için, **/App/metadatadb**olmalıdır.

Örneğin, aşağıdaki yapılandırma, meta verilerin kalıcı olacağı, **Yumurmetadatadbvol** birimi oluşturulmasına neden olur.

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge",
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true",
    "outbound__clientAuth__clientCert__enabled=true",
    "outbound__clientAuth__clientCert__source=IoTEdge",
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ],
  "HostConfig": {
    "Binds": [
      "egmetadataDbVol:/app/metadataDb",
      "egdataDbVol:/app/eventsDb"
    ],
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

Bir birim bağlamak yerine, ana bilgisayar sisteminde bir dizin oluşturabilir ve bu dizini bağlayabilirsiniz.

## <a name="persistence-via-host-directory-mount"></a>Konak Dizin bağlama aracılığıyla Kalıcılık

Bir Docker birimi yerine, bir ana bilgisayar klasörü bağlama seçeneğiniz de vardır.

1. İlk olarak, aşağıdaki komutu çalıştırarak **eventgriduser** ve ID **2000** adlı bir kullanıcı oluşturun:

    ```sh
    sudo useradd -u 2000 eventgriduser
    ```
1. Aşağıdaki komutu çalıştırarak konak dosyası sisteminde bir dizin oluşturun.

   ```sh
   md <your-directory-name-here>
   ```

    Örneğin, aşağıdaki komutu çalıştırmak **myhostdir**adlı bir dizin oluşturur.

    ```sh
    md /myhostdir
    ```
1. Sonra, aşağıdaki komutu çalıştırarak **eventgriduser** sahibini bu klasörün sahibi yapın.

   ```sh
   sudo chown eventgriduser:eventgriduser -hR <your-directory-name-here>
   ```

    Örneğin,

    ```sh
    sudo chown eventgriduser:eventgriduser -hR /myhostdir
    ```
1. Dizini bağlamak ve Event Grid modülünü Azure portal yeniden dağıtmak için **bağlamalar** ' i kullanın.

    ```json
    {
         "HostConfig": {
            "Binds": [
                "<your-directory-name-here>:/app/metadataDb",
                "<your-directory-name-here>:/app/eventsDb",
             ]
         }
    }
    ```

    Örneğin,

    ```json
    {
          "Env": [
            "inbound__serverAuth__tlsPolicy=strict",
            "inbound__serverAuth__serverCert__source=IoTEdge",
            "inbound__clientAuth__sasKeys__enabled=false",
            "inbound__clientAuth__clientCert__enabled=true",
            "inbound__clientAuth__clientCert__source=IoTEdge",
            "inbound__clientAuth__clientCert__allowUnknownCA=true",
            "outbound__clientAuth__clientCert__enabled=true",
            "outbound__clientAuth__clientCert__source=IoTEdge",
            "outbound__webhook__httpsOnly=true",
            "outbound__webhook__skipServerCertValidation=false",
            "outbound__webhook__allowUnknownCA=true"
          ],
          "HostConfig": {
                "Binds": [
                  "/myhostdir:/app/metadataDb",
                  "/myhostdir2:/app/eventsDb"
                ],
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

    >[!IMPORTANT]
    >Bağlama değerinin ikinci kısmını değiştirmeyin. Modül içindeki belirli bir konuma işaret eder. Linux üzerinde Event Grid modülü için, **/App/metadatadb** ve **/App/eventsdb** olması gerekir


## <a name="persist-events"></a>Etkinlikleri kalıcı yap

Olay kalıcılığını etkinleştirmek için, önce yukarıdaki bölümleri kullanarak birim bağlama veya ana bilgisayar Dizin bağlama aracılığıyla meta veri kalıcılığını etkinleştirmeniz gerekir.

Kalıcı olaylar hakkında dikkat etmeniz gereken önemli noktalar:

* Kalıcı olaylar, olay başına abonelik temelinde etkinleştirilir ve bir birim veya dizin bağlandıktan sonra kabul edilir.
* Olay kalıcılığı, oluşturma zamanında bir olay aboneliğinde yapılandırılır ve olay aboneliği oluşturulduktan sonra değiştirilemez. Olay kalıcılığını değiştirmek için olay aboneliğini silip yeniden oluşturmanız gerekir.
* Kalıcı olaylar, bellek işlemlerinden neredeyse her zaman daha yavaştır, ancak hız farkı, sürücünün özelliklerine oldukça bağlıdır. Hız ve güvenilirlik arasındaki zorunluluğunu getirir tüm mesajlaşma sistemlerine sahiptir ancak genellikle büyük ölçekte fark edilebilir hale gelir.

Olay aboneliğindeki olay kalıcılığını etkinleştirmek için şu şekilde `persistencePolicy` `true`ayarlayın:

 ```json
        {
          "properties": {
            "persistencePolicy": {
              "isPersisted": "true"
            },
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "<your-webhook-url>"
              }
            }
          }
        }
 ```
