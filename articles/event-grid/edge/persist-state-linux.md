---
title: Linux'ta kalıcı durum - Azure Event Grid IoT Edge | Microsoft Dokümanlar
description: Linux'ta meta verileri kalıcı olarak sada
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 12655d2ceb4a1124376d9bddf82194472c98ebb9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77086660"
---
# <a name="persist-state-in-linux"></a>Linux'ta kalıcı durum

Olay Izgara modülünde oluşturulan konular ve abonelikler varsayılan olarak kapsayıcı dosya sisteminde depolanır. Kalıcılık olmadan, modül yeniden dağıtılırsa, oluşturulan tüm meta veriler kaybolur. Dağıtımlar ve yeniden başlatmalar arasında verileri korumak için, verileri kapsayıcı dosya sisteminin dışında kalıcı olarak devam etmiş olmanız gerekir.

Varsayılan olarak yalnızca meta veriler kalıcıdır ve olaylar gelişmiş performans için bellekte depolanır. Olay kalıcılığını da etkinleştirmek için kalıcı olaylar bölümünü izleyin.

Bu makalede, Linux dağıtımlarında kalıcılık ile Olay Izgara modülü dağıtmak için adımlar sağlar.

> [!NOTE]
>Olay Izgara modülü UID `2000` ve adı `eventgriduser`ile düşük ayrıcalıklı bir kullanıcı olarak çalışır.

## <a name="persistence-via-volume-mount"></a>Hacim montajı ile kalıcılık

 [Docker birimleri](https://docs.docker.com/storage/volumes/) dağıtımlar arasında verileri korumak için kullanılır. Docker'ın Olay Izgara modülünün dağıtımının bir parçası olarak otomatik olarak adlandırılmış bir birim oluşturmasına izin verebilirsiniz. Bu seçenek en basit seçenektir. **Bağlayıcılar** bölümünde oluşturulacak birim adı aşağıdaki gibi belirtebilirsiniz:

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
>Bağlama değerinin ikinci bölümünü değiştirmeyin. Modül içinde belirli bir konuma işaret. Linux'taki Olay Izgara modülü için **/app/metadataDb**olmalıdır.

Örneğin, aşağıdaki yapılandırma, meta verilerin kalıcı olacağı **egmetadataDbVol** biriminin oluşturulmasıyla sonuçlanır.

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

Bir ses düzeyi monte etmek yerine, ana bilgisayar sisteminde bir dizin oluşturabilir ve bu dizini monte edebilirsiniz.

## <a name="persistence-via-host-directory-mount"></a>Ev sahibi dizini montajı ile kalıcılık

Docker birimi yerine, ana bilgisayar klasörü takma seçeneğiniz de vardır.

1. Önce aşağıdaki komutu çalıştırarak ana makinede ad **eventgriduser** ve ID **2000** ile bir kullanıcı oluşturun:

    ```sh
    sudo useradd -u 2000 eventgriduser
    ```
1. Aşağıdaki komutu çalıştırarak ana bilgisayar dosya sisteminde bir dizin oluşturun.

   ```sh
   md <your-directory-name-here>
   ```

    Örneğin, aşağıdaki komutu çalıştıran myhostdir adlı bir dizin **oluşturacaktır.**

    ```sh
    md /myhostdir
    ```
1. Ardından, aşağıdaki komutu çalıştırarak bu klasörün **eventgriduser** sahibi olun.

   ```sh
   sudo chown eventgriduser:eventgriduser -hR <your-directory-name-here>
   ```

    Örneğin,

    ```sh
    sudo chown eventgriduser:eventgriduser -hR /myhostdir
    ```
1. Dizini monte etmek ve Olay Ağı modüllerini Azure portalından yeniden dağıtmak için **Binds'i** kullanın.

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
    >Bağlama değerinin ikinci bölümünü değiştirmeyin. Modül içinde belirli bir konuma işaret. Linux'taki Olay Izgara modülü için **/app/metadataDb** ve **/app/eventsDb** olmalıdır


## <a name="persist-events"></a>Olayları devam etti

Olay kalıcılığını etkinleştirmek için, önce yukarıdaki bölümleri kullanarak birim montaj veya ana bilgisayar dizini montajı yoluyla meta veri kalıcılığını etkinleştirmeniz gerekir.

Kalıcı olaylar hakkında dikkat edilmesi gereken önemli şeyler:

* Kalıcı olaylar, Etkinlik Aboneliği bazında etkinleştirilir ve bir birim veya dizin monte edildikten sonra kabul edilir.
* Olay kalıcılığı, oluşturma zamanında bir Olay Aboneliği'nde yapılandırılır ve Olay Aboneliği oluşturulduktan sonra değiştirilemez. Olay kalıcılığını geçişyapmak için Olay Aboneliğini silmeniz ve yeniden oluşturmanız gerekir.
* Kalıcı olaylar bellek işlemlerine göre hemen hemen her zaman daha yavaştır, ancak hız farkı sürücünün özelliklerine son derece bağlıdır. Hız ve güvenilirlik arasındaki denge tüm mesajlaşma sistemleri doğasında ama genellikle sadece büyük ölçekte fark olur.

Etkinlik Aboneliği'nde olay kalıcılığını `persistencePolicy` etkinleştirmek için: `true`

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
