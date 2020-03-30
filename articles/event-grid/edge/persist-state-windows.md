---
title: Windows'da kalıcı durum - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: Windows'da durum devam
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: c2bae3bd268dba8efdf23ae314671b17a2c89420
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77086616"
---
# <a name="persist-state-in-windows"></a>Windows'da durum devam

Olay Izgara modülünde oluşturulan konular ve abonelikler varsayılan olarak kapsayıcı dosya sisteminde depolanır. Kalıcılık olmadan, modül yeniden dağıtılırsa, oluşturulan tüm meta veriler kaybolur. Dağıtımlar ve yeniden başlatmalar arasında verileri korumak için, verileri kapsayıcı dosya sisteminin dışında kalıcı olarak devam etmiş olmanız gerekir. 

Varsayılan olarak yalnızca meta veriler kalıcıdır ve olaylar gelişmiş performans için bellekte depolanır. Olay kalıcılığını da etkinleştirmek için kalıcı olaylar bölümünü izleyin.

Bu makalede, Windows dağıtımlarında kalıcılık ile Olay Kılavuz modülü dağıtmak için gereken adımları sağlar.

> [!NOTE]
>Olay Izgara modülü, Windows'da düşük ayrıcalıklı bir kullanıcı **olan ContainerUser** olarak çalışır.

## <a name="persistence-via-volume-mount"></a>Hacim montajı ile kalıcılık

[Docker birimleri](https://docs.docker.com/storage/volumes/) dağıtımlar arasında verileri korumak için kullanılır. Bir birim takmak için docker komutlarını kullanarak oluşturmanız, kapsayıcının okuyabilmesi, ona yazabilmesi ve modülü dağıtması için izin vermeniz gerekir.

1. Aşağıdaki komutu çalıştırarak bir birim oluşturun:

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine volume create <your-volume-name-here>
    ```

    Örneğin,

   ```sh
   docker -H npipe:////./pipe/iotedge_moby_engine volume create myeventgridvol
   ```
1. Aşağıdaki komutu çalıştırarak birim eşlemlerin eşettiği ana bilgisayar dizinini alın

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine volume inspect <your-volume-name-here>
    ```

    Örneğin,

   ```sh
   docker -H npipe:////./pipe/iotedge_moby_engine volume inspect myeventgridvol
   ```

   Örnek Çıktı:-

   ```json
   [
          {
            "CreatedAt": "2019-07-30T21:20:59Z",
            "Driver": "local",
            "Labels": {},
            "Mountpoint": "C:\\ProgramData\\iotedge-moby\u000bolumes\\myeventgridvol\\_data",
            "Name": "myeventgridvol",
            "Options": {},
            "Scope": "local"
          }
   ]
   ```
1. **Mountpoint** tarafından işaret edilen **Mountpoint** değere Kullanıcılar grubunu aşağıdaki gibi ekleyin:
    1. Dosya Gezgini'ni başlatın.
    1. **Mountpoint**tarafından işaret edilen klasöre gidin.
    1. Sağ tıklatın ve sonra **Özellikler'i**seçin.
    1. **Güvenlik**’i seçin.
    1. *Grup veya kullanıcı adları **altında, Edit'i**seçin.
    1. **Ekle,** Gir, `Users` **Adları Denetle'yi**seçin ve **Tamam'ı**seçin.
    1. *Kullanıcılar için İzinler*altında, **Değiştir'i**seçin ve **Tamam'ı**seçin.
1. Bu birimi bağlamak ve Azure portalından Olay Izgara modüllerini yeniden dağıtmak için **Binds'i** kullanın

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
                  "<your-volume-name-here>:C:\\app\\metadataDb"
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
   >Bağlama değerinin ikinci bölümünü değiştirmeyin. Modüldeki belirli bir konumu işaret eden bir nokta. Pencerelerde Olay Izgara modülü için, **C olmalıdır:\\uygulama\\metadataDb**.


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
                "myeventgridvol:C:\\app\\metadataDb",
                "C:\\myhostdir2:C:\\app\\eventsDb"
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

## <a name="persistence-via-host-directory-mount"></a>Ev sahibi dizini montajı ile kalıcılık

Bir ses düzeyi monte etmek yerine, ana bilgisayar sisteminde bir dizin oluşturabilir ve bu dizini monte edebilirsiniz.

1. Aşağıdaki komutu çalıştırarak ana bilgisayar dosya sisteminde bir dizin oluşturun.

   ```sh
   mkdir <your-directory-name-here>
   ```

   Örneğin,

   ```sh
   mkdir C:\myhostdir
   ```
1. Dizininizi monte etmek ve Olay Ağı modüllerini Azure portalından yeniden dağıtmak için **Binds'i** kullanın.

    ```json
    {
         "HostConfig": {
            "Binds": [
                "<your-directory-name-here>:C:\\app\\metadataDb"
             ]
         }
    }
    ```

    >[!IMPORTANT]
    >Bağlama değerinin ikinci bölümünü değiştirmeyin. Modüldeki belirli bir konumu işaret eden bir nokta. Pencerelerdeki Olay Izgara modülü için **\\C:\\app metadataDb**olmalıdır.

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
                "C:\\myhostdir:C:\\app\\metadataDb",
                "C:\\myhostdir2:C:\\app\\eventsDb"
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
## <a name="persist-events"></a>Olayları devam etti

Olay kalıcılığını etkinleştirmek için, öncelikle yukarıdaki bölümleri kullanarak birim montaj veya ana bilgisayar dizini montajı yoluyla olayların kalıcılığını etkinleştirmeniz gerekir.

Kalıcı olaylar hakkında dikkat edilmesi gereken önemli şeyler:

* Kalıcı olaylar, Etkinlik Aboneliği bazında etkinleştirilir ve bir birim veya dizin monte edildikten sonra kabul edilir.
* Olay kalıcılığı, oluşturma zamanında bir Olay Aboneliği'nde yapılandırılır ve Olay Aboneliği oluşturulduktan sonra değiştirilemez. Olay kalıcılığını geçişyapmak için Olay Aboneliğini silmeniz ve yeniden oluşturmanız gerekir.
* Kalıcı olaylar bellek işlemlerine göre hemen hemen her zaman daha yavaştır, ancak hız farkı sürücünün özelliklerine son derece bağlıdır. Hız ve güvenilirlik arasındaki denge tüm mesajlaşma sistemleri doğasında ama sadece büyük ölçekte fark olur.

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