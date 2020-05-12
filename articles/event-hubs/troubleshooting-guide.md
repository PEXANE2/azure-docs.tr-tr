---
title: Sorun giderme kılavuzu-Azure Event Hubs | Microsoft Docs
description: Bu makale, Azure Event Hubs mesajlaşma özel durumlarının ve önerilen eylemlerin bir listesini sağlar.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 01/16/2020
ms.author: shvija
ms.openlocfilehash: ab3cbdf938409f4eacffa10ae5cb20f8c36b5856
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124678"
---
# <a name="azure-event-hubs---troubleshooting-guide"></a>Azure Event Hubs-sorun giderme kılavuzu
Bu makalede, Azure EventHubs kullanırken görebileceğiniz birkaç sorun için sorun giderme ipuçları ve öneriler sunulmaktadır.

## <a name="connectivity-certificate-or-timeout-issues"></a>Bağlantı, sertifika veya zaman aşımı sorunları
Aşağıdaki adımlar *. servicebus.windows.net altındaki tüm hizmetlerde bağlantı/sertifika/zaman aşımı sorunlarını gidermenize yardımcı olabilir. 

- Veya [wget](https://www.gnu.org/software/wget/) 'e gidin `https://<yournamespacename>.servicebus.windows.net/` . IP filtrelemesi veya sanal ağ ya da sertifika zinciri sorunları olup olmadığını denetlemeye yardımcı olur (Java SDK kullanırken en yaygın olarak).

    Başarılı bir ileti örneği:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Hata iletisi örneği:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Güvenlik duvarında herhangi bir bağlantı noktasının engellenip engellenmediğini denetlemek için aşağıdaki komutu çalıştırın. Kullanılan bağlantı noktaları 443 (HTTPS), 5671 (AMQP) ve 9093 (Kafka). Kullandığınız kitaplığa bağlı olarak diğer bağlantı noktaları da kullanılır. 5671 bağlantı noktasının engellenip engellenmeyeceğini kontrol eden örnek komut aşağıda verilmiştir.

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    Linux 'ta:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Aralıklı bağlantı sorunları olduğunda, bırakılan herhangi bir paket olup olmadığını denetlemek için aşağıdaki komutu çalıştırın. Bu komut, hizmeti ile her 1 saniyede 25 farklı TCP bağlantısı kurmaya çalışacaktır. Sonra, kaç tane başarılı/başarısız olduğunu denetleyebilir ve ayrıca TCP bağlantı gecikmesini de görebilirsiniz. `psping`Aracı [buradan](/sysinternals/downloads/psping)indirebilirsiniz.

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespacename>.servicebus.windows.net:5671 -nobanner     
    ```
    , Vb. gibi diğer araçları kullanıyorsanız eşdeğer komutları kullanabilirsiniz `tnc` `ping` . 
- Önceki adımlar [Wireshark](https://www.wireshark.org/)gibi araçları kullanarak yardımcı değilse ve analiz yoksa bir ağ izlemesi elde edin. Gerekirse [Microsoft desteği](https://support.microsoft.com/) başvurun. 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Hizmet yükseltmeleri/yeniden başlatmalar ile ortaya çıkabilecek sorunlar
Arka uç hizmeti yükseltmeleri ve yeniden başlatmaları, uygulamalarınız için aşağıdaki etkiye neden olabilir:

- İstekler, geçici olarak kısıtlanabilir.
- Gelen iletilerde/isteklerde bir bırakma olabilir.
- Günlük dosyasında hata iletileri bulunabilir.
- Uygulamaların birkaç saniye boyunca hizmetle bağlantısı kesilebilir.

Uygulama kodu SDK kullanıyorsa, yeniden deneme ilkesi zaten yerleşik ve etkin durumdadır. Uygulama/iş akışına önemli bir etkisi olmadan uygulama yeniden bağlanır.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub 'ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
