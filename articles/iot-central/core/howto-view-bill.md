---
title: Faturanızı yönetin ve Azure IoT Central uygulamasındaki ücretsiz fiyatlandırma planından dönüştürün | Microsoft Docs
description: Yönetici olarak, faturanızı yönetme ve ücretsiz fiyatlandırma planından Azure IoT Central uygulamanızda standart bir fiyatlandırma planına taşıma hakkında bilgi edinin
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 515f5f665e947ff9594cce6dbbaea9b5e0c50ebf
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95995752"
---
# <a name="manage-your-bill-in-an-iot-central-application"></a>IoT Central uygulamasında faturanızı yönetme

Bu makalede, yönetici olarak Azure IoT Central faturanızı nasıl yönetebileceğinizi açıklanmaktadır. Uygulamanızı ücretsiz fiyatlandırma planından standart bir fiyatlandırma planına taşıyabilir ve ayrıca fiyatlandırma planınızı yükseltebilir veya indirgeyede indirebilirsiniz.

**Yönetim** bölümüne erişmek için *yönetici* rolünde olmanız veya faturalandırmayı görüntülemenize izin veren *özel bir kullanıcı rolüne* sahip olmanız gerekir. Azure IoT Central uygulaması oluşturursanız, otomatik olarak **yönetici** rolüne atanırsınız.

## <a name="move-from-free-to-standard-pricing-plan"></a>Ücretsiz 'tan standart fiyatlandırma planına taşıma

- Ücretsiz fiyatlandırma planını kullanan uygulamalar, süreleri dolmadan yedi gün boyunca ücretsizdir. Verilerin kaybolmasını önlemek için bunları, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planına taşıyabilirsiniz.
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

    ![Yükseltme planını yükselt](media/howto-view-bill/pricing.png)

1. **Planı** seçip yükseltmek veya Alçaldüşürmek için **Kaydet** ' i seçin.

## <a name="view-your-bill"></a>Faturanızı görüntüleme

1. Uygun Azure Active Directory ve ardından ücretli bir plan kullanan uygulamanız için kullanılacak Azure aboneliğini seçin.

1. **Dönüştür**' ü seçtikten sonra uygulamanız artık ücretli bir plan kullanır ve faturalandırılmaya başlayabilirsiniz.

> [!Note]
> Varsayılan olarak, *Standart bir 2* fiyatlandırma planına dönüştürülülüyordu.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamasında faturanızı yönetme hakkında bilgi edindiğinize göre, önerilen sonraki adım Azure IoT Central [uygulama kullanıcı arabirimini özelleştirme](howto-customize-ui.md) hakkında bilgi edindiniz.