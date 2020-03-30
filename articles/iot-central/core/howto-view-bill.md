---
title: Faturanızı yönetin ve Azure IoT Central uygulamasındaki ücretsiz fiyatlandırma planından dönüştürün | Microsoft Dokümanlar
description: Yönetici olarak, faturanızı nasıl yöneteceğimiz ve ücretsiz fiyatlandırma planından Azure IoT Merkezi uygulamanızda standart bir fiyatlandırma planına nasıl geçeceğinizi öğrenin
author: v-krghan
ms.author: v-krghan
ms.date: 12/09/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 888b9f31b8082ac124a773cc489293b335d7d255
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157543"
---
# <a name="manage-your-bill-in-an-iot-central-application"></a>Faturanızı Bir IoT Merkezi uygulamasında yönetme

Bu makalede, bir yönetici olarak, yönetim bölümündeki Azure IoT Central uygulamasında faturanızı nasıl yönetebileceğiniz açıklanmaktadır. Uygulamanızı ücretsiz fiyatlandırma planından standart bir fiyatlandırma planına nasıl taşıyabileceğinizi ve fiyatlandırma planınızı nasıl yükselteceğinizi veya düşüreceğinizi öğreneceksiniz.

Yönetim bölümüne erişmek ve **Yönetim'i** kullanmak için *Yönetici* rolünde veya Azure IoT Merkezi uygulamasının faturalandırmasını görüntülemenize olanak tanıyan özel bir *kullanıcı rolüne* sahip olmalısınız. Bir Azure IoT Merkezi uygulaması oluşturursanız, otomatik olarak bu uygulama için **Yönetici** rolüne atanırsınız.

## <a name="move-from-free-to-standard-pricing-plan"></a>Ücretsizden standart fiyatlandırma planına geçiş

- Ücretsiz fiyatlandırma planını kullanan uygulamalar, süresi dolmadan önce yedi gün boyunca ücretsizdir. Veri kaybetmemek için, bunları süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planına taşıyabilirsiniz.
- Standart bir fiyatlandırma planı kullanan uygulamalar cihaz başına ücretlendirilir ve ilk iki cihaz uygulama başına ücretsiz olarak ücretlendirilir.

[Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/), fiyatlar hakkında daha fazla bilgi edinin.

Fiyatlandırma bölümünde, uygulamanızı ücretsizden standart bir fiyatlandırma planına taşıyabilirsiniz.

Bu self servis işlemini tamamlamak için aşağıdaki adımları izleyin:

1. **İdare** bölümündeki **Fiyatlandırma** sayfasına gidin.

    ![Deneme durumu](media/howto-view-bill/freetrialbilling.png)

1. **Ücretli plana Dönüştür'ü**seçin.

    ![Deneme sürümünü dönüştürme](media/howto-view-bill/convert.png)

1. Ücretli bir plan kullanan uygulamanız için kullanmak üzere uygun Azure Etkin Dizin'ini ve ardından kullanmak üzere Azure aboneliğini seçin.

1. **Convert'i**seçtikten sonra, uygulamanız artık ücretli bir plan kullanır ve faturalandırılamaya başlarsınız.

> [!Note]
> Varsayılan olarak, *Standart 2* fiyatlandırma planına dönüştürülür.

## <a name="how-to-change-your-application-pricing-plan"></a>Uygulama fiyatlandırma planınızı değiştirme

Standart bir fiyatlandırma planı kullanan uygulamalar cihaz başına ücretlendirilir ve ilk iki cihaz uygulama başına ücretsiz olarak ücretlendirilir.

Fiyatlandırma bölümünde, Azure IoT fiyatlandırma planınızı istediğiniz zaman yükseltebilir veya düşürebilirsiniz.

1. **İdare** bölümündeki **Fiyatlandırma** sayfasına gidin.

    ![Deneme durumu](media/howto-view-bill/pricing.png)

1. **Plan'ı** seçin ve yükseltme veya düşürme için **Kaydet'i** tıklatın.

## <a name="view-your-bill"></a>Faturanızı görüntüleme

1. Ücretli bir plan kullanan uygulamanız için kullanmak üzere uygun Azure Etkin Dizin'ini ve ardından kullanmak üzere Azure aboneliğini seçin.

1. **Convert'i**seçtikten sonra, uygulamanız artık ücretli bir plan kullanır ve faturalandırılamaya başlarsınız.

> [!Note]
> Varsayılan olarak, *Standart 2* fiyatlandırma planına dönüştürülür.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamasında faturanızı nasıl yöneteceğimize ilişkin bilgi edindiğinize göre, önerilen bir sonraki adım Azure IoT Central'da uygulama kullanıcı arabirimi [özelleştir](howto-customize-ui.md) hakkında bilgi edinmektir.