---
title: Güvenlik aracılarını yapılandırma
description: IoT güvenlik hizmeti için Azure Güvenlik Merkezi ile kullanmak üzere IoT güvenlik aracıları için Azure Güvenlik Merkezi 'ni nasıl yapılandıracağınızı öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: f95c445a-4f0d-4198-9c6c-d01446473bd0
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: mlottner
ms.openlocfilehash: e41b7222ca89976674973e1eb700d62765b306f0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81311323"
---
# <a name="tutorial-configure-security-agents"></a>Öğretici: güvenlik aracılarını yapılandırma

Bu makalede IoT güvenlik aracıları için Azure Güvenlik Merkezi açıklanmakta ve bunların nasıl değiştirileceği ve yapılandırılacağı açıklanmaktadır.

> [!div class="checklist"]
> * Güvenlik aracılarını yapılandırma
> * İkizi özelliklerini düzenleyerek aracı davranışını değiştirme
> * Varsayılan yapılandırmayı bul

## <a name="agents"></a>Aracılar

IoT güvenlik aracıları için Azure Güvenlik Merkezi, IoT cihazlarından veri toplar ve algılanan güvenlik açıklarını azaltmak için Güvenlik eylemleri gerçekleştirir. Güvenlik Aracısı yapılandırması, özelleştirebileceğiniz bir modül ikizi özellikleri kümesi kullanılarak denetlenebilir. Genel olarak, bu özelliklere yönelik ikincil güncelleştirmeler seyrek olarak görünmez.

IoT 'nin güvenlik Aracısı ikizi yapılandırma nesnesi için Azure Güvenlik Merkezi, bir JSON biçim nesnesidir. Yapılandırma nesnesi, aracının davranışını denetlemek için tanımlayabileceğiniz denetlenebilir bir özellikler kümesidir.

Bu yapılandırma, gereken her senaryo için aracıyı özelleştirmenize yardımcı olur. Örneğin, bazı olayları otomatik olarak dışlayarak veya güç tüketimini en düşük düzeyde tutmak, bu özellikleri yapılandırarak mümkündür.

Değişiklik yapmak için IoT güvenlik aracısının Azure Güvenlik Merkezi yapılandırma [şemasını](https://aka.ms/iot-security-github-module-schema) kullanın.

## <a name="configuration-objects"></a>Yapılandırma nesneleri

IoT güvenlik aracısına yönelik her Azure Güvenlik Merkezi ile ilgili özellikler, **azureiotsecurity** modülünün istenen Özellikler bölümünde yer alan aracı yapılandırma nesnesi içinde bulunur.

Yapılandırmayı değiştirmek için **azureiotsecurity** Module ikizi Identity içinde bu nesneyi oluşturun ve değiştirin.

Aracı yapılandırma nesnesi ikizi **azureiotsecurity** modülünde yoksa, tüm güvenlik Aracısı özellik değerleri varsayılan olarak ayarlanır.

```json
"desired": {
  "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration": {
  }
}
```

## <a name="configuration-schema-and-validation"></a>Yapılandırma şeması ve doğrulama

Aracı yapılandırmanızı bu [şemayla](https://aka.ms/iot-security-github-module-schema)doğruladığınızdan emin olun. Yapılandırma nesnesi şemayla eşleşmezse bir aracı başlatılmaz.

Aracı çalışırken, yapılandırma nesnesi geçerli olmayan bir yapılandırma olarak değiştirilir (yapılandırma şemayla eşleşmez), aracı geçersiz yapılandırmayı yoksayar ve geçerli yapılandırmayı kullanmaya devam eder.

### <a name="configuration-validation"></a>Yapılandırma doğrulaması

IoT güvenlik Aracısı için Azure Güvenlik Merkezi, **azureiotsecurity** Module ikizi kimliğinin bildirilen Özellikler bölümü içinde geçerli yapılandırmasını raporlar.
Aracı tüm kullanılabilir özellikleri raporlar, bir özellik Kullanıcı tarafından ayarlanmamışsa, aracı varsayılan yapılandırmayı raporlar.

Yapılandırmanızı doğrulamak için, istenen bölümde ayarlanan değerleri, bildirilen bölümde bildirilen değerlerle karşılaştırın.

İstenen ve raporlanan özellikler arasında uyuşmazlık varsa, aracı yapılandırmayı ayrıştıramadı.

İstediğiniz özellikleri [şemaya](https://aka.ms/iot-security-github-module-schema)göre doğrulayın, hataları düzeltin ve istediğiniz özellikleri yeniden ayarlayın!

> [!NOTE]
> Aracının istenen yapılandırmayı ayrıştıramadığı durumlarda aracıdan bir yapılandırma hatası uyarısı tetiklenir.
> Uyarının hala geçerli olup olmadığını anlamak için raporlanan ve istenen bölümü karşılaştırın

## <a name="editing-a-property"></a>Özellik düzenleniyor

Tüm özel özellikler **azureiotsecurity** Module ikizi içindeki aracı yapılandırma nesnesi içinde ayarlanmalıdır.
Varsayılan özellik değerini kullanmak için yapılandırma nesnesinden özelliği kaldırın.

### <a name="setting-a-property"></a>Özellik ayarlama

1. IoT Hub değiştirmek istediğiniz cihazı bulun ve seçin.

1. Cihazınıza ve sonra **azureiotsecurity** modülü ' ne tıklayın.

1. **Modül kimliği ikizi**' na tıklayın.

1. Güvenlik modülünde değiştirmek istediğiniz özellikleri düzenleyin.

   Örneğin, bağlantı olaylarını yüksek öncelikli olarak yapılandırmak ve her 7 dakikada bir yüksek öncelikli olayları toplamak için aşağıdaki yapılandırmayı kullanın.

    ```json
    "desired": {
        "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration": {
            "highPriorityMessageFrequency": {
                "value": "PT7M"
            },
            "eventPriorityConnectionCreate": {
                "value": "High"
            }
        }
    }
    ```

1. **Kaydet**’e tıklayın.

### <a name="using-a-default-value"></a>Varsayılan değer kullanma

Varsayılan özellik değerini kullanmak için yapılandırma nesnesinden özelliği kaldırın.

## <a name="default-properties"></a>Varsayılan Özellikler

Aşağıdaki tablo IoT güvenlik aracıları için Azure Güvenlik Merkezi 'nin denetlenebilir özelliklerini içerir.

Varsayılan değerler [GitHub](https\://aka.ms/iot-security-module-default)'da uygun şemada mevcuttur.

| Name| Durum | Geçerli değerler| Varsayılan değerler| Description |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Gerekli: false |Geçerli değerler: ISO 8601 biçimindeki süre |Varsayılan değer: PT7M |Yüksek öncelikli iletiler gönderilmeden önce en uzun zaman aralığı.|
|lowPriorityMessageFrequency |Gerekli: false|Geçerli değerler: ISO 8601 biçimindeki süre |Varsayılan değer: PT5H |Düşük öncelikli iletiler gönderilmeden önce geçen en uzun süre.|
|Anlık görüntüyle |Gerektir: false|Geçerli değerler: ISO 8601 biçimindeki süre |Varsayılan değer PT13H |Cihaz durumu anlık görüntülerinin oluşturulmasına yönelik zaman aralığı.|
|Maxlocalcachesizeınbytes |Gerekli: false |Geçerli değerler: |Varsayılan değer: 2560000, 8192 ' den büyük | Bir aracının ileti önbelleği için izin verilen en fazla depolama alanı (bayt cinsinden). İletiler gönderilmeden önce cihazda iletileri depolamaya izin verilen en fazla alan miktarı.|
|Maxiletiizeınbytes |Gerekli: false |Geçerli değerler: 8192 'den büyük pozitif bir sayı, 262144 ' den az |Varsayılan değer: 204800 |Bir aracının bulut iletisine izin verilen en büyük boyutu. Bu ayar, her iletide gönderilen en fazla veri miktarını denetler. |
|eventPriority $ {EventName} |Gerekli: false |Geçerli değerler: yüksek, düşük, kapalı |Varsayılan değerler: |Aracılı oluşturulan her olayın önceliği |

### <a name="supported-security-events"></a>Desteklenen güvenlik olayları

|Olay adı| ÖzellikAdı | Varsayılan değer| Anlık görüntü olayı| Ayrıntılar durumu  |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|Tanılama olayı|eventPriorityDiagnostic| Kapalı| False| Aracıda ilgili tanılama olayları. Ayrıntılı günlük kaydı için bu olayı kullanın.|
|Yapılandırma hatası |eventPriorityConfigurationError |Düşük |False |Aracı yapılandırmayı ayrıştıramadı. Şemaya göre yapılandırmayı doğrulayın.|
|Bırakılan olay istatistikleri |eventPriorityDroppedEventsStatistics |Düşük |True|Aracıyla ilgili olay istatistikleri. |
|Bağlı donanım|eventPriorityConnectedHardware |Düşük |True |Cihaza bağlı tüm donanımların anlık görüntüsü.|
|Dinleme bağlantı noktaları|eventPriorityListeningPorts |Yüksek |True |Cihazdaki tüm açık dinleme bağlantı noktalarının anlık görüntüsü.|
|İşlem oluşturma |eventPriorityProcessCreate |Düşük |False |Cihazda oluşturma işlemini denetler.|
|İşlem sonlandır|eventPriorityProcessTerminate |Düşük |False |Cihazdaki işlem sonlandırmasını denetler.|
|Sistem bilgileri |Eventprioritysystemınformation |Düşük |True |Sistem bilgilerinin bir anlık görüntüsü (örneğin: OS veya CPU).|
|Yerel kullanıcılar| eventPriorityLocalUsers |Yüksek |True|Sistem içindeki kayıtlı yerel kullanıcıların anlık görüntüsü. |
|Oturum aç|  eventPriorityLogin |Yüksek|False|Oturum açma olaylarını cihaza (yerel ve uzak oturumlar) denetleyin.|
|Bağlantı oluşturma |eventPriorityConnectionCreate|Düşük|False|Cihaza ve cihazdan oluşturulan TCP bağlantılarını denetler. |
|Güvenlik duvarı yapılandırması| eventPriorityFirewallConfiguration|Düşük|True|Cihaz Güvenlik Duvarı yapılandırmasının (güvenlik duvarı kuralları) anlık görüntüsü. |
|İşletim sistemi temeli| eventPriorityOSBaseline| Düşük|True|Cihaz işletim sistemi taban çizgisi denetiminin anlık görüntüsü.|
|

## <a name="next-steps"></a>Sonraki adımlar

- [IoT önerileri için Azure Güvenlik Merkezi 'Ni anlama](concept-recommendations.md)
- [IoT uyarıları için Azure Güvenlik Merkezi 'Ni keşfet](concept-security-alerts.md)
- [Ham güvenlik verilerine erişme](how-to-security-data-access.md)
