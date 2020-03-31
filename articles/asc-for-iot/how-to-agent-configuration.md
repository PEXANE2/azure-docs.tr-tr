---
title: IoT aracısı için Azure Güvenlik Merkezi'ni yapılandırın| Microsoft Dokümanlar
description: IoT güvenlik hizmeti için Azure Güvenlik Merkezi'ni kullanmak üzere IoT güvenlik aracıları için Azure Güvenlik Merkezi'ni nasıl yapılandıracak larını öğrenin.
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
ms.openlocfilehash: 70396cdcaf8b6e2ac66619290eea35a7b260cd9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461258"
---
# <a name="tutorial-configure-security-agents"></a>Öğretici: Güvenlik aracılarını yapılandır

Bu makalede, IoT güvenlik aracıları için Azure Güvenlik Merkezi açıklanmaktadır ve bunları nasıl değiştirip yapılandırılabilmek için ayrıntılar açıklanmaktadır. 

> [!div class="checklist"]
> * Güvenlik aracılarını yapılandırma
> * İkiz özellikleri düzenleyerek aracı davranışını değiştirme
> * Varsayılan yapılandırmayı keşfedin

## <a name="agents"></a>Aracılar

IoT güvenlik aracıları için Azure Güvenlik Merkezi, IoT aygıtlarından veri toplar ve algılanan güvenlik açıklarını azaltmak için güvenlik eylemleri gerçekleştirir. Güvenlik aracısı yapılandırması, özelleştirebileceğiniz bir dizi modül ikiz özelliği kullanılarak denetlenebilir. Genel olarak, bu özelliklere ikincil güncelleştirmeler nadirdir.  

IoT güvenlik aracısı ikiz yapılandırma nesnesi için Azure Güvenlik Merkezi bir JSON biçimi nesnesidir. Yapılandırma nesnesi aracının davranışını denetlemek için tanımlayabileceğiniz denetlenebilir özellikler kümesidir. 

Bu yapılandırmalar, gereken her senaryo için aracıyı özelleştirmenize yardımcı olur. Örneğin, bazı olayları otomatik olarak dışlamak veya güç tüketimini en az düzeyde tutmak, bu özellikleri yapılandırarak mümkündür.  

Değişiklik yapmak için IoT güvenlik aracısı yapılandırma [şemasını](https://aka.ms/iot-security-github-module-schema) için Azure Güvenlik Merkezi'ni kullanın.  

## <a name="configuration-objects"></a>Yapılandırma nesneleri 

Her Azure Güvenlik Merkezi ioT güvenlik aracısı ile ilgili özellikler, **azureiotsecurity** modülünün istenilen özellikler bölümünde aracı yapılandırma nesnesinde bulunur. 

Yapılandırmayı değiştirmek için **azureiotsecurity** modülü ikiz kimliği içinde bu nesneyi oluşturun ve değiştirin. 

**Azureiotsecurity** modülü ikizinde aracı yapılandırma nesnesi yoksa, tüm güvenlik aracısı özellik değerleri varsayılan olarak ayarlanır. 

```json
"desired": {
  "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration": {
  } 
}
```

## <a name="configuration-schema-and-validation"></a>Yapılandırma şeması ve doğrulama 

Aracı yapılandırmanızı bu [şemaya](https://aka.ms/iot-security-github-module-schema)karşı doğruladığından emin olun. Yapılandırma nesnesi şema ile eşleşmiyorsa bir aracı başlatılacak.

 
Aracı çalışırken yapılandırma nesnesi geçersiz bir yapılandırmaya değiştirilirse (yapılandırma şema ile eşleşmiyorsa), aracı geçersiz yapılandırmayı yok sayacak ve geçerli yapılandırmayı kullanmaya devam eder. 

### <a name="configuration-validation"></a>Yapılandırma doğrulaması

Azure Güvenlik Merkezi ioT güvenlik aracısı için **azureiotsecurity** modülü ikiz kimliğinin bildirilen özellikleri bölümünde geçerli yapılandırmasını bildirir.
Aracı, bir özellik kullanıcı tarafından ayarlanmadıysa, aracı varsayılan yapılandırmayı bildirir.

Yapılandırmanızı doğrulamak için, istenilen bölümde ayarlanan değerleri bildirilen bölümde bildirilen değerlerle karşılaştırın.

İstenilen ve bildirilen özellikler arasında bir uyumsuzluk varsa, aracı yapılandırmayı ayrıştıramadı.

Şema karşı istediğiniz [schema](https://aka.ms/iot-security-github-module-schema)özellikleri doğrulayın, hataları düzeltmek ve tekrar istediğiniz özellikleri ayarlayın!

> [!NOTE]
> Aracının istenen yapılandırmayı ayrıştıraması durumunda aracıdan bir yapılandırma hatası uyarısı alınır.
> Uyarının hala geçerli olup olmadığını anlamak için bildirilen ve istenen bölümü karşılaştırın

## <a name="editing-a-property"></a>Bir özelliği düzenleme 

Tüm özel özellikler **azureiotsecurity** modülü ikiz içinde aracı yapılandırma nesnesi içinde ayarlanmalıdır.
Varsayılan özellik değerini kullanmak için özelliği yapılandırma nesnesinden kaldırın.

### <a name="setting-a-property"></a>Özellik ayarlama

1. IoT Hub'ınızda değiştirmek istediğiniz aygıtı bulun ve seçin.

1. Cihazınıza ve ardından **azureiotsecurity** modülüne tıklayın.

1. Modül **Kimlik İkiz'ine**tıklayınız.

1. Güvenlik modülünde değiştirmek istediğiniz özellikleri edin.
   
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

1. **Kaydet**'e tıklayın.

### <a name="using-a-default-value"></a>Varsayılan değer kullanma

Varsayılan özellik değerini kullanmak için özelliği yapılandırma nesnesinden kaldırın.

## <a name="default-properties"></a>Varsayılan özellikler 

Aşağıdaki tablo, IoT güvenlik aracıları için Azure Güvenlik Merkezi'nin denetlenebilir özelliklerini içerir.

Varsayılan değerler [GitHub'daki](https\://aka.ms/iot-security-module-default)uygun şemada kullanılabilir.

| Adı| Durum | Geçerli değerler| Varsayılan değerler| Açıklama |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Gerekli: false |Geçerli değerler: ISO 8601 Formatında süre |Varsayılan değer: PT7M |Yüksek öncelikli iletiler gönderilmeden önce maksimum zaman aralığı.|
|düşükÖncelikliMesaj Frekansı |Gerekli: false|Geçerli değerler: ISO 8601 Formatında süre |Varsayılan değer: PT5H |Düşük öncelikli iletilerin gönderilmesiiçin en yüksek süre.| 
|anlık Frekans |Require: false|Geçerli değerler: ISO 8601 Formatında süre |Varsayılan değer PT13H |Aygıt durumu anlık görüntüleri nin oluşturulması için zaman aralığı.| 
|maxLocalCacheSizeInBytes |Gerekli: false |Geçerli değerler: |Varsayılan değer: 2560000, 8192'den büyük | Bir aracının ileti önbelleği için izin verilen maksimum depolama alanı (baytlarda). İletiler gönderilmeden önce iletilerin aygıtta depolamasına izin verilen maksimum alan miktarı.| 
|maxMessageSizeBytes |Gerekli: false |Geçerli değerler: 8192'den büyük, 262144'ten az pozitif bir sayı |Varsayılan değer: 204800 |İletiyi bulutlamak için bir aracının izin verilen maksimum boyutu. Bu ayar, her iletide gönderilen maksimum veri miktarını denetler. |
|eventPriority${EventName} |Gerekli: false |Geçerli değerler: Yüksek, Düşük, Kapalı |Varsayılan değerler: |Aracı tarafından oluşturulan her olayın önceliği | 

### <a name="supported-security-events"></a>Desteklenen güvenlik olayları

|Olay adı| ÖzellikAdı | Varsayılan Değer| Anlık Görüntü Etkinliği| Ayrıntılar Durumu  |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|Tanılama olayı|olayÖncelikliTanı| Kapalı| False| Ajan ile ilgili tanı olayları. Ayrıntılı günlüğe kaydetme için bu olayı kullanın.| 
|Yapılandırma hatası |eventPriorityConfigurationError |Düşük |False |Aracı yapılandırmayı ayrışdırmayı başaramadı. Yapılandırmayı şema karşı doğrulayın.| 
|Bırakılan olaylar istatistikleri |olayPriorityDroppedEventsİstatistiki |Düşük |True|Aracıyla ilgili olay istatistikleri. |
|Bağlı donanım|olayÖncelikliConnectedHardware |Düşük |True |Aygıta bağlı tüm donanımların anlık görüntüsü.|
|Dinleme noktaları|eventPriorityListeningPorts |Yüksek |True |Cihazdaki tüm açık dinleme bağlantı noktalarının anlık görüntüsü.|
|İşlem oluşturma |eventPriorityProcessCreate |Düşük |False |Denetimler aygıtta oluşturma işlemini.|
|İşlem sonlandırma|olayPriorityProcessTerminate |Düşük |False |Denetimler aygıtta sonlandırma işlemini tamamlar.| 
|Sistem bilgileri |eventPrioritySystemInformation |Düşük |True |Sistem bilgilerinin anlık görüntüsü (örneğin: işletim sistemi veya CPU).| 
|Yerel kullanıcılar| olayÖncelikliLocalUsers |Yüksek |True|Sistem içinde kayıtlı yerel kullanıcıların anlık görüntüsü. |
|Oturum Aç|  olayPriorityLogin |Yüksek|False|Aygıta giriş olaylarını (yerel ve uzak oturum açmalar) denetle.|
|Bağlantı oluşturma |eventPriorityConnectionCreate|Düşük|False|Aygıta ve cihazdan oluşturulan TCP bağlantılarını denetler. |
|Güvenlik duvarı yapılandırması| olayÖncelikliFirewallConfiguration|Düşük|True|Aygıt güvenlik duvarı yapılandırmasının anlık görüntüsü (güvenlik duvarı kuralları). |
|İşletim Sistemi taban çizgisi| olayPriorityOSBaseline| Düşük|True|Aygıt işletim sistemi taban çizgisi denetiminin anlık görüntüsü.|
|
 

## <a name="next-steps"></a>Sonraki adımlar

- [IoT önerileri için Azure Güvenlik Merkezi'ni anlayın](concept-recommendations.md)
- [IoT uyarıları için Azure Güvenlik Merkezi'ni keşfedin](concept-security-alerts.md)
- [Ham güvenlik verilerine erişme](how-to-security-data-access.md)
