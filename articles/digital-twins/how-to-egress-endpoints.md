---
title: Çıkış ve uç noktalar - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins'te etkinlik bitiş noktaları oluşturma yı ve nasıl çıkış yapacaklarını öğrenin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: 3803802a3d81655091d8be543ae9cb17221a98d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76511578"
---
# <a name="egress-and-endpoints-in-azure-digital-twins"></a>Azure Dijital İkizler'de çıkış ve uç noktalar

Azure Digital Twins *uç noktaları,* kullanıcının Azure aboneliğinde bir ileti veya etkinlik aracısı temsil eder. Etkinlikler ve mesajlar Azure Etkinlik Hub'larına, Azure Etkinlik Ağı'na ve Azure Hizmet Veri Gönderi konularına gönderilebilir.

Olaylar, önceden tanımlanmış yönlendirme tercihlerine göre bitiş noktalarına yönlendirilir. Kullanıcılar, her bitiş noktasının hangi *olay türlerini* alabileceğini belirtir.

Etkinlikler, yönlendirme ve olay türleri hakkında daha fazla bilgi edinmek için [Azure Digital Twins'teki Yönlendirme etkinliklerine ve iletilerine](./concepts-events-routing.md)bakın.

## <a name="events"></a>Olaylar

Olaylar, Azure iletisi ve etkinlik aracıları tarafından işlenmek üzere IoT nesneleri (aygıtlar ve sensörler gibi) tarafından gönderilir. Olaylar aşağıdaki Azure [Olay Ağı olay şeması referansı](../event-grid/event-schema.md)ile tanımlanır.

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "subject": "ExtendedPropertyKey",
  "data": {
    "SpacesToNotify": [
      "3a16d146-ca39-49ee-b803-17a18a12ba36"
    ],
    "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
    "AccessType": "Create"
  },
  "eventType": "TopologyOperation",
  "eventTime": "2018-04-17T17:41:54.9400177Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Öznitelik | Tür | Açıklama |
| --- | --- | --- |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| veri | object | Kaynak sağlayıcısına özgü olay verileri. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |

Olay Izgara olay şeması hakkında daha fazla bilgi için:

- Azure [Olay Ağı olay şeması başvurularını](../event-grid/event-schema.md)gözden geçirin.
- Azure [EventGrid Düğümü.js SDK EventGridEvent başvurularını](https://docs.microsoft.com/javascript/api/@azure/eventgrid/eventgridevent?view=azure-node-latest)okuyun.

## <a name="event-types"></a>Olay türleri

Olay türleri olayın doğasını sınıfa sınıfa ve **eventType** alanında ayarlanır. Kullanılabilir olay türleri aşağıdaki liste tarafından verilmiştir:

- TopolojiOperasyonu
- UdfCustom
- Sensör Değişimi
- SpaceChange
- DeviceMessage

Her olay türü için olay biçimleri aşağıdaki alt bölümlerde daha ayrıntılı olarak açıklanmıştır.

### <a name="topologyoperation"></a>TopolojiOperasyonu

**TopolojiOperasyon** grafik değişiklikleri için geçerlidir. **Özne** özelliği etkilenen nesnenin türünü belirtir. Aşağıdaki nesne türleri bu olayı tetikleyebilir:

- Cihaz
- DeviceBlobMetadata
- DeviceExtendedProperty
- Genişletilmiş EmlakKey
- Genişletilmiş Tip
- Depo -sunun
- Rapor
- RoleDefinition
- Sensör
- SensorBlobMeta data
- SensorExtendedProperty
- Alan
- SpaceBlobMeta data
- SpaceExtendedProperty
- Uzay Kaynağı
- SpaceRoleAssignment
- Sistem
- Kullanıcı
- UserBlobMetadata
- UserExtendedProperty

#### <a name="example"></a>Örnek

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "subject": "ExtendedPropertyKey",
  "data": {
    "SpacesToNotify": [
      "3a16d146-ca39-49ee-b803-17a18a12ba36"
    ],
    "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
    "AccessType": "Create"
  },
  "eventType": "TopologyOperation",
  "eventTime": "2018-04-17T17:41:54.9400177Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_TOPIC_NAME | Özelleştirilmiş konunun adı |

### <a name="udfcustom"></a>UdfCustom

**UdfCustom,** kullanıcı tanımlı bir işlev (UDF) tarafından gönderilen bir olaydır.
  
> [!IMPORTANT]  
> Bu olay udf'nin kendisinden açıkça gönderilmelidir.

#### <a name="example"></a>Örnek

```JSON
{
  "id": "568fd394-380b-46fa-925a-ebb96f658cce",
  "subject": "UdfCustom",
  "data": {
    "TopologyObjectId": "7c799bfc-1bff-4b9e-b15a-669933969d20",
    "ResourceType": "Space",
    "Payload": "\"Room is not available or air quality is poor\"",
    "CorrelationId": "568fd394-380b-46fa-925a-ebb96f658cce"
  },
  "eventType": "UdfCustom",
  "eventTime": "2018-10-02T06:50:15.198Z",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_TOPIC_NAME | Özelleştirilmiş konunun adı |

### <a name="sensorchange"></a>Sensör Değişimi

**SensorChange,** telemetri değişikliklerine dayalı olarak sensörün durumuna yönelik bir güncelleştirmedir.

#### <a name="example"></a>Örnek

```JSON
{
  "id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
  "subject": "SensorChange",
  "data": {
    "Type": "Classic",
    "DataType": "Motion",
    "Id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
    "Value": "False",
    "PreviousValue": "True",
    "EventTimestamp": "2018-04-17T17:46:15.4964262Z",
    "MessageType": "sensor",
    "Properties": {
      "ms-client-request-id": "c9e576b7-5eea-4f61-8617-92a57add5179",
      "ms-activity-id": "ct22YwXEGJ5u.605.0"
    }
  },
  "eventType": "SensorChange",
  "eventTime": "2018-04-17T17:46:18.5452993Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_TOPIC_NAME | Özelleştirilmiş konunun adı |

### <a name="spacechange"></a>SpaceChange

**SpaceChange,** bir alanın durumuna telemetri değişikliklerine dayalı bir güncelleştirmedir.

#### <a name="example"></a>Örnek

```JSON
{
  "id": "42522e10-b1aa-42ff-a5e7-7181788ffc4b",
  "subject": "SpaceChange",
  "data": {
    "Type": null,
    "DataType": "AvailableAndFresh",
    "Id": "7c799bfc-1bff-4b9e-b15a-669933969d20",
    "Value": "Room is not available or air quality is poor",
    "PreviousValue": null,
    "RawData": null,
    "transactionId": null,
    "EventTimestamp": null,
    "MessageType": null,
    "Properties": null,
    "CorrelationId": "42522e10-b1aa-42ff-a5e7-7181788ffc4b"
  },
  "eventType": "SpaceChange",
  "eventTime": "2018-10-02T06:50:20.128Z",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_TOPIC_NAME | Özelleştirilmiş konunun adı |

### <a name="devicemessage"></a>DeviceMessage

**DeviceMessage'ı**kullanarak, Azure Digital Twins'ten ham telemetri etkinliklerinin de yönlendirilebildiği bir **EventHub** bağlantısı belirtebilirsiniz.

> [!NOTE]
> - **DeviceMessage** yalnızca **EventHub**ile birleştirilebilir. **DeviceMessage'ı** diğer etkinlik türlerinin hiçbiriyle birleştiremezsiniz.
> - **EventHub** veya **DeviceMessage**türü kombinasyonunun yalnızca bir bitiş noktasını belirtebilirsiniz.

## <a name="configure-endpoints"></a>Uç noktaları yapılandırma

Uç nokta yönetimi, Uç Nokta API'si aracılığıyla yapılır.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Aşağıdaki örnekler, desteklenen uç noktaların nasıl yapılandırılabildiğini gösterir.

>[!IMPORTANT]
> **eventTypes** özniteliğine dikkat edin. Hangi olay türlerinin bitiş noktasına göre işleneceğini tanımlar ve böylece yönlendirmesini belirler.

Şuna karşı doğrulanmış bir HTTP POST isteği:

```URL
YOUR_MANAGEMENT_API_URL/endpoints
```

- Servis Veri Yolu olay türleri **SensorChange**, **SpaceChange**ve **TopolojiOperasyon:**

  ```JSON
  {
    "type": "ServiceBus",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME"
  }
  ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_NAMESPACE | Bitiş noktanızın ad alanı |
    | YOUR_PRIMARY_KEY | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi |
    | YOUR_SECONDARY_KEY | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | YOUR_TOPIC_NAME | Özelleştirilmiş konunun adı |

- Olay Izgara olay tiplerine Rota **SensorChange**, **SpaceChange**, ve **TopolojiOperasyon**:

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME.westus-1.eventgrid.azure.net"
  }
  ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_PRIMARY_KEY | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi|
    | YOUR_SECONDARY_KEY | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | YOUR_TOPIC_NAME | Özelleştirilmiş konunun adı |

- Olay Hub'larına Rota olay türleri **SensorChange**, **SpaceChange**ve **TopolojiOperasyon**:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_NAMESPACE | Bitiş noktanızın ad alanı |
    | YOUR_PRIMARY_KEY | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi |
    | YOUR_SECONDARY_KEY | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | YOUR_EVENT_HUB_NAME | Etkinlik merkezinizin adı |

- Olay Hub'larına rota olay türü **DeviceMessage**. `EntityPath` **ConnectionString'e** eklenmesi zorunludur:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_NAMESPACE | Bitiş noktanızın ad alanı |
    | YOUR_PRIMARY_KEY | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi |
    | YOUR_SECONDARY_KEY | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | YOUR_EVENT_HUB_NAME | Etkinlik merkezinizin adı |

> [!NOTE]  
> Yeni bir bitiş noktası oluşturulduktan sonra, bitiş noktasında olayları almaya başlamak 5 ila 10 dakika kadar sürebilir.

## <a name="primary-and-secondary-connection-keys"></a>Birincil ve ikincil bağlantı anahtarları

Birincil bağlantı anahtarı yetkisiz hale geldiğinde, sistem ikincil bağlantı anahtarını otomatik olarak dener. Bu bir yedekleme sağlar ve bitiş noktaları API aracılığıyla birincil anahtarı düzgünbir şekilde doğrulamak ve güncelleştirme olanağı sağlar.

Hem birincil hem de ikincil bağlantı anahtarları yetkisizse, sistem 30 dakikaya kadar üstel bir geri bekleme süresi girer. Olaylar tetiklenen her geri dönüş bekleme süresine bırakılır.

Sistem bekleme durumunda olduğunda, Bağlantı Noktaları API'si aracılığıyla bağlantı anahtarlarını güncelleştirmenin etkili olması 30 dakika kadar sürebilir.

## <a name="unreachable-endpoints"></a>Erişilemez uç noktalar

Bir bitiş noktası erişilemez hale geldiğinde, sistem 30 dakikaya kadar üstel bir geri bekleme süresi girer. Olaylar tetiklenen her geri dönüş bekleme süresine bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Digital Twins Swagger'ı nasıl kullanacağınızı](how-to-use-swagger.md)öğrenin.

- Azure Digital Twins'te [olayları ve mesajları yönlendirme](concepts-events-routing.md) hakkında daha fazla bilgi edinin.
