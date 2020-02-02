---
title: Azure IoT Hub sorunlarını giderme hatası 403002 Iothubquotaaşıldı
description: 403002 Iothubquotaof hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 8312c3267e826088f34b3bffe1520703eec00bdc
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76961120"
---
# <a name="403002-iothubquotaexceeded"></a>403002 Iothubquotaaşıldı

Bu makalede, **403002 Iothubkotasına** yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

IoT Hub tüm istekleri **403002 Iothubquotaerror**ile başarısız olur. Azure portal, IoT Hub cihaz listesi yüklenmez.

## <a name="cause"></a>Nedeni

IoT Hub için günlük ileti kotası aşıldı. 

## <a name="solution"></a>Çözüm

[IoT Hub 'ındaki birim sayısını yükseltin veya artırın](iot-hub-upgrade.md) veya günlük kotanın yenilenmesi için BIR sonraki UTC gününü bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* İkizi sorguları ve doğrudan yöntemleri gibi işlemlerin kotasına nasıl dahil olduğunu anlamak için bkz. [IoT Hub fiyatlandırmasını anlama](iot-hub-devguide-pricing.md#charges-per-operation)
* Günlük kota kullanımı için izlemeyi ayarlamak için, *kullanılan ölçüm toplam ileti sayısı*ile bir uyarı ayarlayın. Adım adım yönergeler için bkz. [IoT Hub ölçümleri ve uyarıları ayarlama](tutorial-use-metrics-and-diags.md#set-up-metrics)