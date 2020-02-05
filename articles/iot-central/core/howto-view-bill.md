---
title: Faturanızı yönetin ve Azure IoT Central uygulamasındaki ücretsiz fiyatlandırma planından dönüştürün | Microsoft Docs
description: Yönetici olarak, faturanızı yönetme ve ücretsiz fiyatlandırma planından Azure IoT Central uygulamanızda standart bir fiyatlandırma planına taşıma hakkında bilgi edinin
author: v-krghan
ms.author: v-krghan
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 58503784f28a3ba0a6290a2209ce9fdccc14b4e4
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77023778"
---
# <a name="manage-your-bill-in-an-iot-central-application"></a>IoT Central uygulamasında faturanızı yönetme

Bu makalede, yönetici olarak Yönetim bölümünde faturanız Azure IoT Central uygulamanızı nasıl yönetebileceğinizi açıklar. Uygulamanızı ücretsiz fiyatlandırma planından standart fiyatlandırma planına nasıl taşıyabileceğinizi ve ayrıca fiyatlandırma planınızı nasıl yükselteceğinizi veya indirgetireceğinizi öğreneceksiniz.

**Yönetim** bölümüne erişmek ve bunları kullanmak Için, *yönetici* rolünde olmanız veya bir Azure IoT Central uygulaması için faturalandırmayı görüntülemenize olanak tanıyan *özel bir kullanıcı rolüne* sahip olmanız gerekir. Azure IoT Central uygulaması oluşturursanız, bu uygulama için otomatik olarak **yönetici** rolüne atanırsınız.

## <a name="move-from-free-to-standard-pricing-plan"></a>Ücretsiz 'tan standart fiyatlandırma planına taşıma

- Ücretsiz fiyatlandırma planını kullanan uygulamalar, süreleri dolmadan yedi gün boyunca ücretsizdir. Verilerin kaybolmasını önlemek için, süreleri dolmadan önce istediğiniz zaman standart bir fiyatlandırma planına taşıyabilirsiniz.
- Standart bir fiyatlandırma planı kullanan uygulamalar, uygulama başına ilk iki cihazdan ücretsiz olarak cihaz başına ücretlendirilir.

[Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/), fiyatlar hakkında daha fazla bilgi edinin.

Fiyatlandırma bölümünde, uygulamanızı ücretsiz bir standart fiyatlandırma planına taşıyabilirsiniz.

Bu self servis işlemini gerçekleştirmek için şu adımları izleyin:

1. **Yönetim** bölümündeki **fiyatlandırma** sayfasına gidin.

    ![Deneme durumu](media/howto-view-bill/freetrialbilling.png)

1. **Ücretli plana Dönüştür**' ü seçin.

    ![Deneme süresini Dönüştür](media/howto-view-bill/convert.png)

1. Uygun Azure Active Directory ve ardından ücretli bir plan kullanan uygulamanız için kullanılacak Azure aboneliğini seçin.

1. **Dönüştür**' ü seçtikten sonra uygulamanız artık ücretli bir plan kullanır ve faturalandırılmaya başlayabilirsiniz.

> [!Note]
> Varsayılan olarak, *Standart bir 2* fiyatlandırma planına dönüştürülülüyordu.

## <a name="how-to-change-your-application-pricing-plan"></a>Uygulama fiyatlandırma planınızı değiştirme

Standart bir fiyatlandırma planı kullanan uygulamalar, uygulama başına ilk iki cihazdan ücretsiz olarak cihaz başına ücretlendirilir.

Fiyatlandırma bölümünde, Azure IoT fiyatlandırma planınızı dilediğiniz zaman yükseltebilir veya indirgeyede olursunuz.

1. **Yönetim** bölümündeki **fiyatlandırma** sayfasına gidin.

    ![Deneme durumu](media/howto-view-bill/pricing.png)

1. **Planı** seçin ve yükseltmek veya alçaldüşürmek için **Kaydet** ' e tıklayın.

## <a name="view-your-bill"></a>Faturanızı görüntüleme

1. Uygun Azure Active Directory ve ardından ücretli bir plan kullanan uygulamanız için kullanılacak Azure aboneliğini seçin.

1. **Dönüştür**' ü seçtikten sonra uygulamanız artık ücretli bir plan kullanır ve faturalandırılmaya başlayabilirsiniz.

> [!Note]
> Varsayılan olarak, *Standart bir 2* fiyatlandırma planına dönüştürülülüyordu.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamasında faturanızı yönetme hakkında bilgi edindiğinize göre, önerilen sonraki adım Azure IoT Central [uygulama kullanıcı arabirimini özelleştirme](howto-customize-ui.md) hakkında bilgi edindiniz.