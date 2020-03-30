---
title: Azure IoT Central’da kural ve eylem yapılandırma | Microsoft Docs
description: Bu nasıl yapılandırılır makalesi, bir oluşturucu olarak Azure IoT Central uygulamanızda telemetri tabanlı kuralları ve eylemleri nasıl yapılandırabileceğinizi gösterir.
author: vavilla
ms.author: vavilla
ms.date: 11/27/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 509f9557a8128df12353ad02a7c7db02b7b42631
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158495"
---
# <a name="configure-rules"></a>Kuralları yapılandırma



*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

IoT Central'daki kurallar, bağlı aygıtlardan etkin olarak izlenen olayları tetikleyen özelleştirilebilir bir yanıt aracı olarak hizmet verir. Aşağıdaki bölümlerde kuralların nasıl değerlendirildiği açıklanmaktadır.

## <a name="select-target-devices"></a>Hedef cihazları seçin

Bu kuralın uygulanacağı aygıtların ne tür cihazlarda uygulanacağını seçmek için hedef aygıtlar bölümünü kullanın. Filtreler, hangi aygıtların eklenmesi gerektiğini daha da hassaslaştırmanızı sağlar. Filtreler, aygıt kümesini filtrelemek için aygıt şablonundaki özellikleri kullanır. Filtreler in kendileri bir eylemi tetiklemez. Aşağıdaki ekran görüntüsünde, hedeflenen cihazlar cihaz şablontürü **Buzdolabı.** Filtre, kuralın yalnızca **Üretilen Devlet** mülkiyetinin **Washington'a**eşit olduğu **Buzdolapları'nı** içermesi gerektiğini belirtir.

![Koşullar](media/howto-configure-rules/filters.png)

## <a name="use-multiple-conditions"></a>Birden çok koşul kullanma

Kurallar tetikleyicidir. Şu anda, bir kurala birden çok koşul eklediğinizde, bunlar mantıksal olarak ve birlikte olurlar. Başka bir deyişle, kuralın doğru olarak değerlendirilmesi için tüm koşulların karşılanması gerekir.  

Aşağıdaki ekran görüntüsünde, sıcaklık 70&deg; F'den büyük ve nem 10'dan az olduğunda koşullar kontrol edilir. Bu ifadelerin her ikisi de doğru olduğunda, kural doğru değerlendirir ve bir eylemi tetikler.

![Koşullar](media/howto-configure-rules/conditions.png)

## <a name="use-aggregate-windowing"></a>Toplu pencereleme kullanma

Kurallar, toplam zaman pencerelerini yuvarlanan pencereler olarak değerlendirir. Aşağıdaki ekran görüntüsünde, zaman penceresi beş dakikadır. Her beş dakikada bir kural, son beş dakikalık verileri değerlendirir. Veriler, karşılık gelen pencerede yalnızca bir kez değerlendirilir.

![Yuvarlanan Pencereler](media/howto-configure-rules/tumbling-window.png)

## <a name="use-rules-with-iot-edge-modules"></a>IoT Edge modülleri ile kuralları kullanma

Kısıtlama, IoT Edge modüllerine uygulanan kurallar için geçerlidir. Farklı modüllerden gelen telemetri kuralları geçerli kurallar olarak değerlendirilmez. Bir örnek olarak aşağıdakileri ele alalım. Kuralın ilk koşulu A modülünden gelen bir sıcaklık telemetrisidir. Kuralın ikinci koşulu, Modül B'deki nem telemetrisidir. İki koşul farklı modüllerden olduğundan, bu geçersiz bir koşul kümesidir. Kural geçerli değildir ve kuralı kurtarmaya çalışırken bir hata atar.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Merkezi uygulamanızda bir kuralı yapılandırmayı öğrendiğinize göre şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Verilerinizi anında analiz edin](howto-create-analytics.md)
