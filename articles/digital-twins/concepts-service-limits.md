---
title: Genel Önizleme hizmeti limitleri-Azure dijital TWINS | Microsoft Docs
description: Azure dijital TWINS için genel önizleme hizmeti, abonelik, örnek ve hız sınırları hakkında bilgi edinin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 5e323d8faa19ceb0712aa6183df17740ce2a0a1d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79370397"
---
# <a name="public-preview-service-limits"></a>Genel önizleme hizmet sınırları

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

Azure dijital TWINS, genel önizleme sırasında mevcut müşteriler için aşağıdaki geçici abonelik, örnek ve hız sınırlarına sahiptir. Bu kısıtlamalar, yeni hizmet ve birçok özelliği hakkında bilgi edinmeye yardımcı olmak için mevcuttur ve genel kullanıma (GA) göre arttırılacaktır.

## <a name="per-subscription-limits"></a>Abonelik başına sınırlar

Genel Önizleme sırasında, her bir Azure aboneliği tek seferde yalnızca bir Azure dijital TWINS örneği oluşturabilir veya çalıştırabilir. Örneğinizi silerseniz yeni bir tane oluşturabilirsiniz.

## <a name="per-instance-limits"></a>Örnek başına sınırlar

Sırasıyla, her Azure dijital TWINS örneği şunları içerebilir:

- Hizmet sağlama sırasında otomatik olarak oluşturulan bir Embedded **ıothub** kaynağı.
- Olay türü **Devicemessage**için tam olarak bir **EventHub** uç noktası.
- **Sensorchange**, **spacechange**, **Topologyoperation**veya **udfcustom**olay türü için en fazla üç **EventHub**, **ServiceBus**veya **eventgrid** uç noktası.

> [!NOTE]
> Genellikle yukarıdaki Azure IoT varlıklarının oluşturulması sırasında tanımlanan bazı parametreler genel önizleme sırasında gerekli değildir.
> - En son API belirtimleri için [Swagger başvuru belgelerine](./how-to-use-swagger.md) bakın.

## <a name="azure-digital-twins-management-api-limits"></a>Azure dijital TWINS yönetim API sınırları

Azure dijital TWINS yönetim API 'niz için istek oranı sınırları şunlardır:

- Azure Digital TWINS yönetim API 'sine saniye başına 100 istek.
- Tek bir Azure dijital TWINS yönetim API sorgusu tarafından döndürülen en fazla 1.000 nesne.

> [!IMPORTANT]
> 1.000-nesne sınırını aşarsanız bir hata alırsınız ve sorgunuzu basitleştirmelidir.

## <a name="user-defined-functions-rate-limits"></a>Kullanıcı tanımlı işlevlerin hız sınırları

Aşağıdaki sınırlar, Azure dijital TWINS örneğiniz için yapılan tüm Kullanıcı tanımlı işlev çağrılarının toplam sayısını ayarlar:

- saniye başına 400 istemci kitaplığı çağrısı
- 100 **SendNotification** çağrısı/saniye

> [!NOTE]
> Aşağıdaki eylemler ek hız sınırlarının geçici olarak uygulanmasına neden olabilir:
> - Topoloji nesnesi meta verilerinde yapılan düzenlemeler
> - Kullanıcı tanımlı işlev tanımında yapılan güncelleştirmeler
> - İlk kez telemetri gönderen cihazlar

## <a name="device-telemetry-limits"></a>Cihaz telemetri limitleri

Aşağıdaki sınırlar, cihazlarınızın Azure dijital TWINS örneğine gönderebileceği tüm iletilerin toplam sayısıdır:

- tüm cihazlarda saniye başına 100 ileti
-    cihaz başına saniyede 25 ileti

## <a name="next-steps"></a>Sonraki adımlar

- Bir Azure dijital TWINS örneği denemek için hızlı başlangıç bölümüne giderek [kullanılabilir odaları bulun](./quickstart-view-occupancy-dotnet.md).
