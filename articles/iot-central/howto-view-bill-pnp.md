---
title: Azure IoT Central uygulamasına gittiğinizde faturanızı görüntüleyin ve deneme sürenizi ödeyin. Microsoft Docs
description: Yönetici olarak, Azure IoT Central uygulamanızda Kullandıkça öde ve deneme sürümünü ücretsiz olarak nasıl görüntüleyeceğinizi öğrenin
author: v-krghan
ms.author: v-krghan
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: d27288718b7f224c78c9cf1f15d05198f628fd15
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879649"
---
# <a name="view-your-bill-in-an-iot-central-application"></a>IoT Central uygulamasında faturanızı görüntüleme

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu makalede, Yönetim bölümünde faturanızda Azure IoT Central uygulamanızı nasıl görüntüleyebileceğinizi ve ayrıca deneme sürümünüzü Kullandıkça Öde 'ye nasıl dönüştürebileceğinizi açıklar.

**Yönetim** bölümüne erişmek ve bunları kullanmak için, bir Azure IoT Central uygulaması için **yönetici** rolünde olmanız gerekir. Azure IoT Central uygulaması oluşturursanız, bu uygulama için otomatik olarak **yönetici** rolüne atanırsınız.

## <a name="view-your-bill"></a>Faturanızı görüntüleme

Faturanızı görüntülemek için, **Yönetim** bölümünde **faturalandırma** sayfasına gidin. Azure Faturalandırma sayfası, Azure IoT Central uygulamalarınızın her biri için faturanızı görebileceğiniz yeni bir sekmede açılır.

## <a name="convert-your-trial-to-pay-as-you-go"></a>Deneme sürümünüzü Kullandıkça Öde için dönüştürün

- **Deneme** uygulamaları, süresi dolmadan yedi gün boyunca ücretsizdir. Süresi dolmadan önce herhangi bir anda Kullandıkça Öde uygulamasına dönüştürülebilir.
- **Kullandıkça Öde** uygulamaları, abonelik başına ilk beş cihaz ücretsiz olarak cihaz başına ücretlendirilir.

[Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/), fiyatlar hakkında daha fazla bilgi edinin.

Faturalandırma bölümünde, deneme uygulamalarınızı Kullandıkça Öde 'e dönüştürebilirsiniz.

Bu self servis işlemini gerçekleştirmek için şu adımları izleyin:

1. **Yönetim** bölümündeki **faturalandırma** sayfasına gidin.

    ![Deneme durumu](media/howto-view-bill-pnp/freetrialbilling.png)

1. **Kullandıkça Öde seçeneğine Dönüştür**' ü seçin.

    ![Deneme süresini Dönüştür](media/howto-view-bill-pnp/convert.png)

1. Uygun Azure Active Directory ve ardından Kullandıkça Öde uygulamanız için kullanılacak Azure aboneliğini seçin.

1. **Dönüştür**' i seçtikten sonra uygulamanız artık Kullandıkça Öde uygulaması olur ve faturalandırılmaya başlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamasında faturanızı görüntüleme hakkında bilgi edindiğinize göre, önerilen sonraki adım Azure IoT Central [uygulama kullanıcı arabirimini özelleştirme](howto-customize-ui-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hakkında bilgi almanız gerekir.