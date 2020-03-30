---
title: Genel önizleme hizmeti sınırları - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins için genel önizleme hizmeti, abonelik, örnek ve fiyat sınırları hakkında bilgi edinin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 5e323d8faa19ceb0712aa6183df17740ce2a0a1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370397"
---
# <a name="public-preview-service-limits"></a>Genel önizleme hizmet sınırları

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

Genel önizleme sırasında Azure Digital Twins, varolan müşteriler için aşağıdaki geçici abonelik, örneğin ve fiyat sınırlamaları na sahiptir. Bu kısıtlamalar, yeni hizmet ve birçok özelliği hakkında öğrenmeyi basitleştirmeye yardımcı olmak için vardır ve genel kullanılabilirlik (GA) tarafından artırılacaktır veya kaldırılır.

## <a name="per-subscription-limits"></a>Abonelik başına limitler

Genel önizleme sırasında, her Azure aboneliği aynı anda yalnızca bir Azure Dijital İkizler örneği oluşturabilir veya çalıştırabilir. Örneğini silerseniz, yeni bir örnek oluşturabilirsiniz.

## <a name="per-instance-limits"></a>Örnek başına sınırlar

Buna karşılık, her Azure Dijital İkizler örneği şunları yapabilir:

- Hizmet sağlama sırasında otomatik olarak oluşturulan tam olarak bir gömülü **IoTHub** kaynağı.
- Olay türü **DeviceMessage**için tam bir **EventHub** bitiş noktası .
- En fazla üç **EventHub**, **ServiceBus**, veya **EventGrid** bitiş noktaları olay türü **SensorChange**, **SpaceChange**, **TopologyOperation**, veya **UdfCustom**.

> [!NOTE]
> Genellikle yukarıdaki Azure IoT varlıklarını oluştururken tanımlanan bazı parametreler genel önizleme sırasında gerekli değildir.
> - En son API belirtimleri için [Swagger başvuru belgelerine](./how-to-use-swagger.md) başvurun.

## <a name="azure-digital-twins-management-api-limits"></a>Azure Dijital İkizler Yönetimi API sınırları

Azure Dijital İkizler Yönetimi API'niz için istek oranı sınırları şunlardır:

- Azure Dijital İkizler Yönetimi API'sine saniyede 100 istek.
- Tek bir Azure Dijital İkizler Yönetimi API sorgusu tarafından döndürülen en fazla 1.000 nesne.

> [!IMPORTANT]
> 1.000 nesne sınırını aşarsanız, bir hata alırsınız ve sorgunuzu basitleştirmeniz gerekir.

## <a name="user-defined-functions-rate-limits"></a>Kullanıcı tanımlı fonksiyonlar hız sınırları

Aşağıdaki sınırlar, Azure Dijital İkizler örneğinize yapılan kullanıcı tanımlı işlev çağrılarının toplam sayısını belirler:

- Saniyede 400 istemci kitaplığı çağrısı
- Saniyede 100 **SendNotification** çağrısı

> [!NOTE]
> Aşağıdaki eylemler ek fiyat sınırlarının geçici olarak uygulanmasına neden olabilir:
> - Topoloji nesne meta verilerine yapılan editler
> - Kullanıcı tanımlı işlev tanımında yapılan güncelleştirmeler
> - Telemetriyi ilk kez gönderen cihazlar

## <a name="device-telemetry-limits"></a>Cihaz telemetri limitleri

Aşağıdaki sınırlar, aygıtlarınızın Azure Dijital İkizler örneğinize gönderebileceği toplam ileti sayısını kaplar:

- Tüm cihazlarda saniyede 100 mesaj
-    Cihaz başına saniyede 25 ileti

## <a name="next-steps"></a>Sonraki adımlar

- Azure Digital Twins örneğini denemek [için kullanılabilir odaları bulmak için Quickstart'a](./quickstart-view-occupancy-dotnet.md)gidin.
