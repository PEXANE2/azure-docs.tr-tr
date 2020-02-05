---
title: Azure IoT Central’da kural ve eylem yapılandırma | Microsoft Docs
description: Bu nasıl yapılır makalesi, Azure IoT Central uygulamanızda telemetri tabanlı kuralları ve eylemleri nasıl yapılandıracağınızı bir Oluşturucu olarak gösterir.
author: vavilla
ms.author: vavilla
ms.date: 11/27/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: ca308522d0c6b9c3c5b39d73a2f4a278a5b20db8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026997"
---
# <a name="configure-rules"></a>Kuralları yapılandırma



*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

IoT Central kurallar, bağlı cihazlardan etkin olarak izlenen olaylar üzerinde tetiklenen bir özelleştirilebilir yanıt aracı olarak görev yapar. Aşağıdaki bölümlerde kuralların nasıl değerlendirildiği açıklanır.

## <a name="select-target-devices"></a>Hedef cihazları seçin

Bu kuralın ne tür cihazlarda uygulanacağını seçmek için hedef cihazlar bölümünü kullanın. Filtreler, hangi cihazların dahil edileceğini daha da iyileştirmenize olanak tanır. Filtreler cihaz kümesini filtrelemek için cihaz şablonundaki özellikleri kullanır. Filtreler bir eylem tetiklemez. Aşağıdaki ekran görüntüsünde, hedeflenen cihazların cihaz şablonu türü **soğutma**' dir. Filtre, kuralın yalnızca **üretilen durum** özelliği **Washington**'a eşit olan **soğutma** 'yi içermesi gerektiğini belirtir.

![Koşullar](media/howto-configure-rules/filters.png)

## <a name="use-multiple-conditions"></a>Birden çok koşul kullanma

Koşullar, üzerinde hangi kuralların tetikleyeceğini de tetikler. Şu anda, bir kurala birden çok koşul eklediğinizde bunlar mantıksal olarak ve Ile birlikte yapılır. Diğer bir deyişle kuralın doğru olarak değerlendirilmesi için tüm koşulların karşılanması gerekir.  

Aşağıdaki ekran görüntüsünde koşullar, sıcaklığın 70&deg; F 'den büyük olduğunu ve nem 10 ' dan küçük olduğunu denetler. Bu deyimlerden her ikisi de doğru olduğunda, kural true olarak değerlendirilir ve bir eylemi tetikler.

![Koşullar](media/howto-configure-rules/conditions.png)

## <a name="use-aggregate-windowing"></a>Birleşik Pencereleme kullan

Kurallar, Windows 'un aldığı Birleşik zaman pencerelerini değerlendirir. Aşağıdaki ekran görüntüsünde, zaman penceresi beş dakikadır. Her beş dakikada bir kural, son beş dakikalık verileri değerlendirir. Veriler yalnızca bir kez karşılık geldiği pencerede değerlendirilir.

![Atlayan pencereler](media/howto-configure-rules/tumbling-window.png)

## <a name="use-rules-with-iot-edge-modules"></a>IoT Edge modüllerle kuralları kullanma

Bir kısıtlama IoT Edge modüllerine uygulanan kurallar için geçerlidir. Farklı modüllerden gelen telemetri kuralları geçerli kurallar olarak değerlendirilmez. Örnek olarak aşağıdakileri yapın. Kuralın ilk koşulu, A modülünden bir sıcaklık telemetrisine sahiptir. Kuralın ikinci koşulu, B modülündeki bir nem telemetrisi üzerinde. İki koşul farklı modüllerden olduğundan, bu geçersiz bir koşullar kümesidir. Kural geçerli değil ve kuralı kaydetmeye çalışırken bir hata oluşturacak.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure IoT Central uygulamanızda bir kural yapılandırmayı öğrendiğinize göre şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Verilerinizi anında çözümleyin](howto-create-analytics.md)
