---
title: Azure IoT Central kurallar üzerinde Web kancaları oluşturun | Microsoft Docs
description: Kurallar başlatıldığında diğer uygulamalara otomatik olarak bildirimde bulunan Azure IoT Central Web kancaları oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 10/21/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: 77c57b8c20b3fd181cdf6ade58e478845304da8d
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72950554"
---
# <a name="create-webhook-actions-on-rules-in-azure-iot-central"></a>Azure IoT Central kurallarında Web kancası eylemleri oluşturma

*Bu konu, oluşturucular ve yöneticiler için geçerlidir.*

Web kancaları, uzaktan izleme ve bildirimler için IoT Central uygulamanızı diğer uygulama ve hizmetlere bağlamanıza olanak tanır. Web kancaları, IoT Central uygulamanızda her bir kural tetiklendiğinde bağlandığınız diğer uygulama ve hizmetlere otomatik olarak bildirim gönderir. IoT Central uygulamanız, bir kural her tetiklendiğinde diğer uygulamanın HTTP uç noktasına bir POST isteği gönderir. Yük, cihaz ayrıntılarını ve kural tetikleyicisi ayrıntılarını içerir.

## <a name="set-up-the-webhook"></a>Web kancasını ayarlama

Bu örnekte, kurallar Web kancaları kullanılarak başlatıldığında bildirim almak için RequestBin 'e bağlanırsınız.

1. [Requestbin](https://requestbin.net/)'i açın.

1. Yeni bir RequestBin oluşturun ve **BIN URL 'sini**kopyalayın.

1. [Telemetri kuralı](tutorial-create-telemetry-rules-pnp.md)oluşturun. Kuralı kaydedin ve yeni bir eylem ekleyin.

    ![Web kancası oluşturma ekranı](media/howto-create-webhooks-pnp/webhookcreate.png)

1. Web kancası eylemini seçin ve bir görünen ad girin ve bin URL 'sini geri çağırma URL 'si olarak yapıştırın.

1. Kuralı kaydedin.

Artık kural tetiklendiğinde RequestBin ' de yeni bir istek göründüğünü görürsünüz.

## <a name="payload"></a>Te

Bir kural tetiklendiğinde Telemetriyi, cihazı, kuralı ve uygulama ayrıntılarını içeren bir JSON yükü içeren geri çağırma URL 'SI için HTTP POST isteği yapılır. Yük aşağıdaki gibi görünebilir:

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

Şu anda bir API aracılığıyla bu Web kancalarını abone yapmanın/aboneliğini kaldırın bir programlama yolu yoktur.

Bu özelliği geliştirme hakkında fikirler varsa, önerilerinizi [UserVoice forumumuza](https://feedback.azure.com/forums/911455-azure-iot-central)gönderin.

## <a name="next-steps"></a>Sonraki adımlar

Web kancalarını nasıl ayarlayıp kullanacağınızı öğrendiğinize göre, önerilen sonraki adım [Azure Izleyici eylem gruplarını yapılandırmayı](howto-use-action-groups-pnp.md)araştırmaya yönelik olur.
