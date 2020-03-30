---
title: Azure IoT Merkezi UI'yi özelleştirme | Microsoft Dokümanlar
description: Azure IoT merkezi uygulamanız için tema ve yardım bağlantıları nasıl özelleştirilir?
author: dominicbetts
ms.author: dobett
ms.date: 12/06/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 8f76f143b6c6a26b88b78e20d8d5d8ae1ae48553
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158053"
---
# <a name="customize-the-azure-iot-central-ui"></a>Azure IoT Merkezi UI'yi özelleştirme

Bu makalede, bir yönetici olarak, özel temalar uygulayarak ve yardım bağlantılarını değiştirerek uygulamanızın kullanıcı arasını kendi özel yardım kaynaklarınızı işaret etmek üzere nasıl özelleştirebileceğiniz açıklanmaktadır. 



Aşağıdaki ekran görüntüsü standart teonu kullanan bir sayfayı gösterir:

![Standart IoT Merkezi teması](./media/howto-customize-ui/standard-ui.png)

Aşağıdaki ekran görüntüsü, özelleştirilmiş Kullanıcı Bira sı öğelerinin vurgulandığı özel bir ekran görüntüsü kullanarak bir sayfayı gösterir:

![Özel IoT Merkezi teması](./media/howto-customize-ui/themed-ui.png)

## <a name="create-theme"></a>Tema oluşturma

Özel bir tema oluşturmak **için, İdare** **bölümündeki uygulama sayfanızı Özelleştir** bölümüne gidin:

![IoT Merkezi temalar](./media/howto-customize-ui/themes.png)

Bu sayfada, uygulamanızın aşağıdaki yönlerini özelleştirebilirsiniz:

### <a name="application-logo"></a>Uygulama logosu

Şeffaf arka plana sahip, 1 MB'dan büyük olmayan bir PNG görüntüsü. Bu logo, IoT Central uygulama başlık çubuğunda solda görüntülenir.

Logo resminizde uygulamanızın adını içeriyorsa, uygulama adı metnini gizleyebilirsiniz. Daha fazla bilgi için [başvurunuzu yönet' e](howto-administer.md#change-application-name-and-url)bakın.

### <a name="browser-icon-favicon"></a>Tarayıcı simgesi (favicon)

32 x 32 pikselden büyük olmayan, saydam arka plana sahip bir PNG görüntüsü. Bir web tarayıcısı bu resmi adres çubuğunda, geçmişte, yer imleri ve tarayıcı sekmesinde kullanabilir.

### <a name="browser-colors"></a>Tarayıcı renkleri

Sayfa üstbilginin rengini ve vurgulama düğmeleri ve diğer önemli noktaları vurgulamak için kullanılan rengi değiştirebilirsiniz. Biçimde `##ff6347`altı karakterli hex renk değeri kullanın. **HEX Value** renk gösterimi hakkında daha fazla bilgi için [HTML Renkleri'ne](https://www.w3schools.com/html/html_colors.asp)bakın.

> [!NOTE]
> **Uygulama sayfanızı Özelleştir'deki** varsayılan seçeneklere her zaman geri dönebilirsiniz.

### <a name="changes-for-operators"></a>Operatörler için değişiklikler

Bir yönetici özel bir tema oluşturursa, operatörler ve uygulamanızın diğer kullanıcıları artık **Ayarlar'da**bir tema seçemez.

## <a name="replace-help-links"></a>Yardım bağlantılarını değiştirme

Operatörlerinize ve diğer kullanıcılara özel yardım bilgileri sağlamak için, uygulama **Yardım** menüsündeki bağlantıları değiştirebilirsiniz.

Yardım bağlantılarını değiştirmek **için, İdare** bölümündeki **Yardım Ları Özelleştir** bölümüne gidin:

![IoT Merkezi yardım bağlantılarını özelleştir](./media/howto-customize-ui/help-links.png)

Ayrıca yardım menüsüne yeni girişler ekleyebilir ve varsayılan girişleri kaldırabilirsiniz:

![Özelleştirilmiş IoT Merkezi yardım](./media/howto-customize-ui/custom-help.png)

> [!NOTE]
> Her zaman **Yardım Özelleştir** sayfasındaki varsayılan yardım bağlantılarına geri dönebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Central uygulamanızda Kullanıcı Arabirimi'ni özelleştirmeyi öğrendiğinize göre, önerilen sonraki bazı adımlar şunlardır:

- [Uygulamanızı yönetme](./howto-administer.md)
- [Panonuza kutucuklar ekleme](howto-add-tiles-to-your-dashboard.md)