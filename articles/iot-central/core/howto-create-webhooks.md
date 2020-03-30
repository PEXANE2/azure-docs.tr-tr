---
title: Azure IoT Central'da kurallar hakkında web hooks oluşturma | Microsoft Dokümanlar
description: Kurallar çıktığında diğer uygulamaları otomatik olarak bilgilendirmek için Azure IoT Central'da web hooks oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 12/02/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: d97bd7a3c6de92f22a9880040f407960d5257f6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158104"
---
# <a name="create-webhook-actions-on-rules-in-azure-iot-central"></a>Azure IoT Central'da kurallar la ilgili webhook eylemleri oluşturma

*Bu konu oluşturucular ve yöneticiler için geçerlidir.*

Webhooks, IoT Central uygulamanızı uzaktan izleme ve bildirimler için diğer uygulamalara ve hizmetlere bağlamanızı sağlar. Webhooks, IoT Central uygulamanızda bir kural tetiklendiğinde bağlandığınız diğer uygulamaları ve hizmetleri otomatik olarak bildirir. IoT Central uygulamanız, bir kural tetiklendiğinde diğer uygulamanın HTTP bitiş noktasına bir POST isteği gönderir. Yük aygıt ayrıntılarını ve kural tetikleyici ayrıntılarını içerir.

## <a name="set-up-the-webhook"></a>Webhook'u ayarlama

Bu örnekte, kurallar webhooks kullanarak ateş ettiğinde haberdar olmak için RequestBin'e bağlanırsınız.

1. [İstek Bini'ni](https://requestbin.net/)aç.

1. Yeni bir İstek Kutusu oluşturun ve **Bin URL'sini**kopyalayın.

1. Bir [telemetri kuralı](tutorial-create-telemetry-rules.md)oluşturun. Kuralı kaydedin ve yeni bir eylem ekleyin.

    ![Webhook oluşturma ekranı](media/howto-create-webhooks/webhookcreate.png)

1. Webhook eylemini seçin ve bir ekran adı sağlayın ve Geri Arama URL'si olarak Bin URL'sini yapıştırın.

1. Kuralı sakla.

Kural tetiklendiğinde, İstek Bin'de yeni bir istek görünür.

## <a name="payload"></a>Yükü

Bir kural tetiklendiğinde, telemetri, aygıt, kural ve uygulama ayrıntılarıiçeren json yükü içeren geri arama URL'sine bir HTTP POST isteği yapılır. Yük aşağıdaki gibi görünebilir:

```json
{
    "id": "<id>",
    "displayName": "Webhook 1",
    "timestamp": "2019-10-24T18:27:13.538Z",
    "rule": {
        "id": "<id>",
        "displayName": "High temp alert",
        "enabled": true
    },
    "device": {
        "id": "mx1",
        "displayName": "MXChip IoT DevKit - mx1",
        "instanceOf": "<device-template-id>",
        "simulated": true,
        "provisioned": true,
        "approved": true
    },
    "data": [{
        "@id": "<id>",
        "@type": ["Telemetry"],
        "name": "temperature",
        "displayName": "Temperature",
        "value": 66.27310467496761,
        "interfaceInstanceName": "sensors"
    }],
    "application": {
        "id": "<id>",
        "displayName": "x - Store Analytics Checkout---PnP",
        "subdomain": "<subdomain>",
        "host": "<host>"
    }
}
```

## <a name="known-limitations"></a>Bilinen sınırlamalar

Şu anda bir API üzerinden bu webhooks abone / unscribing hiçbir programlı yolu yoktur.

Bu özelliği geliştirmek için fikirleriniz varsa, önerilerinizi [Kullanıcı ses forumumuza](https://feedback.azure.com/forums/911455-azure-iot-central)gönderin.

## <a name="next-steps"></a>Sonraki adımlar

Artık web hooks'u nasıl kurup kullanacağınızı öğrendiğiniz için, önerilen bir sonraki adım [Azure Monitörü Eylem Gruplarını yapılandırmayı](howto-use-action-groups.md)araştırmaktır.
