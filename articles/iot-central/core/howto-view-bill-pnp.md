---
title: Azure IoT Central uygulamasına gittiğinizde faturanızı yönetin ve deneme sürümünüzü ödeme için dönüştürün | Microsoft Docs
description: Yönetici olarak, Azure IoT Central uygulamanıza gittiğinizde faturanızı nasıl yöneteceğinizi ve deneme yanılma ücretini öde hakkında bilgi edinin
author: v-krghan
ms.author: v-krghan
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a0d12d762b675141035bebb9308a25cc71360c5c
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72949865"
---
# <a name="manage-your-bill-in-an-iot-central-application"></a>IoT Central uygulamasında faturanızı yönetme

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bu makalede, Yönetim bölümünde faturanızda Azure IoT Central uygulamanızı nasıl yöneteceğiniz ve ayrıca deneme sürümünüzü Kullandıkça Öde 'ye nasıl dönüştürebileceğiniz açıklanır.

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

Azure IoT Central uygulamasında faturanızı yönetme hakkında bilgi edindiğinize göre, önerilen sonraki adım Azure IoT Central [uygulama kullanıcı arabirimini özelleştirme](howto-customize-ui-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hakkında bilgi edindiniz.