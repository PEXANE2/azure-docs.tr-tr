---
title: Azure IoT Central uygulamasına gerçek bir cihaz ekleme | Microsoft Docs
description: Bir işleç olarak Azure IoT Central uygulamanıza gerçek bir cihaz ekleyin.
author: sarahhubbard
ms.author: sahubbar
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 7191c98cfc522068dbea576a8f81776ae4301da8
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878176"
---
# <a name="tutorial-add-a-simulated-device-to-your-azure-iot-central-application-preview-features"></a>Öğretici: Azure IoT Central uygulamanıza sanal cihaz ekleme (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu öğreticide, Microsoft Azure IoT Central uygulamanıza sanal bir cihaz ekleme ve yapılandırma işlemleri gösterilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir sanal cihaz ekleme
> * Yapı deneyiminde sanal cihaz kullanma

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, oluşturucunun Azure IoT Central uygulamasını oluşturmak için ilk Oluşturucu öğreticisini tamamlaması gerekir:

* [Yeni bir cihaz türü belirleme](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Gerekli)

## <a name="add-a-simulated-device"></a>Sanal cihaz ekleme

Uygulamanıza gerçek bir cihaz eklemek için, [Yeni bir cihaz türü tanımlama](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) öğreticisinde oluşturduğunuz **çevresel algılayıcı** cihaz şablonunu kullanın.

1. Yeni bir cihazı operatör olarak eklemek için sol gezinti menüsünde **cihazlar** ' ı seçin. **Cihazlar** sekmesi **tüm cihazları** ve **çevresel algılayıcı** cihaz şablonunu gösterir.

1. Sanal bir çevresel algılayıcı cihazı eklemek için **+ Yeni**' yi seçin. Önerilen **CIHAZ kimliğini** kullanın veya kendi küçük BIR **Cihaz Kimliğinizi**girin. Ayrıca, yeni cihazınız için bir ad girebilirsiniz. **Benzetimli** geçişi **Açık** olarak değiştirin ve ardından **Oluştur**' u seçin.

    ![Sanal cihaz](./media/tutorial-add-device-pnp/simulated-device.png)

Artık, benzetimli verileri kullanarak cihaz şablonu için Oluşturucu tarafından oluşturulan görünümlerle etkileşime geçebilirsiniz.

## <a name="use-a-simulated-device-to-improve-views"></a>Görünümleri geliştirmek için sanal cihaz kullanma

Yeni bir sanal cihaz oluşturduktan sonra, Oluşturucu bu cihazı kullanarak cihaz şablonunun görünümlerini iyileştirmeye ve oluşturmaya devam edebilir.

1. Cihazınızın görünümünde, oluşturduğunuz sanal cihazın **CIHAZ kimliğini** kopyalayın.

1. Sol gezinti menüsünde **cihaz şablonları** sekmesini seçin ve **çevresel algılayıcı** şablonunu seçin.

1. Düzenlemek istediğiniz görünümlerden birini seçin veya yeni bir görünüm oluşturun. **Önizleme cihazını Yapılandır**öğesine tıklayın. Burada, test için yapılandırabileceğiniz gerçek bir cihazı kullanarak veya IoT Central eklediğiniz mevcut bir cihazdan bir önizleme cihazı olmadan seçim yapabilirsiniz.

1. **Çalışan bir cihazdan Seç** ' i seçin ve kopyaladığınız sanal CIHAZıN **cihaz kimliğini** girin.

1. **Uygula**'yı seçin. Artık cihaz şablonu görünümlerini oluşturma deneyiminizdeki aynı sanal cihazı görebilirsiniz. Bu görünüm, grafikler ve diğer görselleştirmeler için özellikle kullanışlıdır.

    ![Önizleme cihazını Yapılandır](./media/tutorial-add-device-pnp/configure-preview.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Yeni bir sanal cihaz ekleme
* Yapı deneyiminde sanal cihaz kullanma

Azure IoT Central uygulamanıza sanal bir cihaz bağladığınıza göre, sonraki adımlarda önerilen bazı adımları bulabilirsiniz.

Bir operatör olarak, aşağıdakileri yapmayı öğrenebilirsiniz:

> [!div class="nextstepaction"]
> [Kuralları yapılandırma](tutorial-configure-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

Cihaz geliştiricisi olarak, şunları yapmayı öğrenebilirsiniz:

> [!div class="nextstepaction"]
> [Bir Mxyonga IoT DevKit cihazını Azure IoT Central uygulamanıza bağlama](howto-connect-devkit-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)



