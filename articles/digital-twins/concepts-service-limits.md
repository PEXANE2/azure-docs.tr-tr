---
title: Azure dijital TWINS genel önizleme hizmeti sınırları | Microsoft Docs
description: Azure Digital TWINS genel önizleme hizmeti sınırlarını anlayın.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/03/2019
ms.openlocfilehash: 88a66506b5102a4efaeefe1c7033b337dff9889a
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949236"
---
# <a name="public-preview-service-limits"></a>Genel Önizleme hizmeti sınırları

Genel Önizleme sırasında Azure dijital TWINS 'in aşağıdaki geçici abonelik, örnek ve hız sınırları vardır.

Bu kısıtlamalar, yeni hizmet ve birçok özelliği hakkında bilgi edinmeye yardımcı olmak için mevcuttur.

> [!NOTE]
> Bu sınırlar, genel kullanıma (GA) göre artar veya kaldırılır.

## <a name="per-subscription-limits"></a>Abonelik başına sınırlar

Genel Önizleme sırasında, her bir Azure aboneliği tek seferde yalnızca bir Azure dijital TWINS örneği oluşturabilir veya çalıştırabilir.

> [!TIP]
> Örneğinizi silerseniz yeni bir tane oluşturabilirsiniz.

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
> 1\.000-nesne sınırını aşarsanız bir hata alırsınız ve sorgunuzu basitleştirmelidir.

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

- saniye başına 100 ileti

## <a name="next-steps"></a>Sonraki adımlar

- Bir Azure dijital TWINS örneği denemek için hızlı başlangıç bölümüne giderek [kullanılabilir odaları bulun](./quickstart-view-occupancy-dotnet.md).