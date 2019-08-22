---
title: Azure IoT Central uygulamanızda cihaz gruplarını kullanma | Microsoft Docs
description: Bir operatör olarak, Azure IoT Central uygulamanızda cihaz gruplarını kullanma.
author: ellenfosborne
ms.author: elfarber
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 07f1df101442e8ae43b26cebc60c8452d475d0f3
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879753"
---
# <a name="use-device-groups-in-your-azure-iot-central-application-preview-features"></a>Azure IoT Central uygulamanızda cihaz gruplarını kullanma (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu makalede, Azure IoT Central uygulamanızda cihaz gruplarını kullanmak için nasıl bir operatör olarak kullanılacağı açıklanmaktadır.

Bir cihaz grubu, belirtilen bazı ölçütlerle eşleştiğinden, birlikte gruplanmış cihazların bir listesidir. Cihaz grupları cihazları daha küçük ve mantıksal gruplara gruplandırarak, cihazları ölçekli olarak yönetmenize, görselleştirmenize ve çözümlemenize yardımcı olur. Örneğin, bir teknisyenin sorumlu oldukları cihazları bulmasını sağlamak için Seattle 'daki tüm AIR klimaları cihazlarını listelemek üzere bir cihaz grubu oluşturabilirsiniz. Bu makalede cihaz gruplarının nasıl oluşturulacağı ve yapılandırılacağı gösterilmektedir.

## <a name="create-a-device-group"></a>Bir cihaz grubu oluşturma

Bir cihaz grubu oluşturmak için:

1. Sol gezinti menüsünde **cihaz grupları** ' nı seçin.

1. **+ Yeni**seçeneğini belirleyin.

    ![Yeni cihaz grubu](media/howto-use-device-groups-pnp/image1.png)

1. Cihaz grubunuza tüm uygulama genelinde benzersiz bir ad verin. Ayrıca, bir açıklama da ekleyebilirsiniz. Bir cihaz grubu yalnızca tek bir cihaz şablonundan cihaz içerebilir. Bu grup için kullanılacak cihaz şablonunu seçin.

1. Bir özellik, karşılaştırma operatörü ve bir değer seçerek cihaz grubu için cihazları tanımlamak üzere sorgu oluşturun. Birden çok sorgu ekleyebilirsiniz ve **Tüm** ölçütlere uyan cihazlar cihaz grubuna yerleştirilir. Oluşturduğunuz cihaz grubuna, uygulamaya erişimi olan herkes tarafından erişilebilir, böylece herkes cihaz grubunu görüntüleyebilir, değiştirebilir veya silebilir.

    ![Cihaz grubu sorgusu](media/howto-use-device-groups-pnp/image2.png)

    > [!NOTE]
    > Cihaz grubu dinamik bir sorgudur. Cihazların listesini her görüntülediğinizde, listede farklı cihazlar olabilir. Liste, şu anda sorgu ölçütlerine uyan cihazlara bağlıdır.

1. **Kaydet**’i seçin.

## <a name="analytics"></a>Analiz

Cihaz gruplarındaki çözümlemeler, sol gezinti menüsündeki ana Analiz sekmesi ile aynıdır. Analizler hakkında daha fazla bilgi için bkz. [analiz oluşturma](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda cihaz gruplarını nasıl kullanacağınızı öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Telemetri kuralları oluşturma](howto-create-telemetry-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
