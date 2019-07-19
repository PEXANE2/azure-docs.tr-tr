---
title: Azure IoT Central uygulama panosunu yapılandırma | Microsoft Docs
description: Oluşturucu olarak, varsayılan Azure IoT Central uygulama panosunu yapılandırmayı öğrenin.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 94ad51ac11687dfe060176132e2030d61b8d4ffc
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850208"
---
# <a name="configure-the-application-dashboard"></a>Uygulama panosunu yapılandırma

**Pano** , uygulamaya erişimi olan KULLANıCıLAR uygulamanın URL 'sine gitince yüklenen sayfasıdır. Uygulamanızı oluşturmak için **contoso** veya **örnek devkits** uygulama şablonu ' nu seçtiyseniz uygulamanızın önceden tanımlanmış bir panosu vardır. **Özel uygulama** uygulaması şablonunu seçerseniz, panonuz boştur.

> [!NOTE]
> Kullanıcılar, varsayılan uygulama panosu yerine kullanmak üzere [kendi kişisel panoları da oluşturabilir](howto-personalize-dashboard.md) .

## <a name="add-tiles"></a>Kutucuk Ekle

Aşağıdaki ekran görüntüsünde, panodaki **örnek contoso** şablonundan oluşturulan bir uygulama gösterilmektedir. Uygulamanız için varsayılan panoyu özelleştirmek üzere sayfanın sağ üst kısmında **Düzenle** ' yi seçin.

!["Örnek contoso" şablonunu temel alan uygulamalar için Pano](media/howto-configure-homepage/image1a.png)

**Düzenle**' yi seçerek Pano kitaplığı panelini açar. Kitaplık, Panoyu özelleştirmek için kullanabileceğiniz kutucukları ve Pano temel öğelerini içerir.

![Pano kitaplığı](media/howto-configure-homepage/image2a.png)

Örneğin, bir cihaz için geçerli ayarların ve özellik değerlerinin seçimini göstermek üzere bir **cihaz ayarları ve Özellikler** kutucuğu ekleyebilirsiniz. Bunu yapmak için önce bir **cihaz şablonu** seçin, sonra bir **cihaz örneği**seçin. Bundan sonra, kutucuğa bir başlık verin ve görüntülenecek bir **ayar** ya da **özellik** seçin. Aşağıdaki ekran görüntüsünde, kutucuğa eklemek için seçilen ayarlar ve özellikler gösterilmektedir. Değişikliği panoya kaydetmek için **bitti** ' yi seçin.

![Ayarlar ve özellikler hakkındaki ayrıntılarla "cihaz ayrıntılarını yapılandırma" formu](media/howto-configure-homepage/image3a.png)

Artık bir operatör varsayılan uygulama panosunu görüntülediğinde, cihaz için **sıcaklık ayarla** ayarı ile yeni kutucuğu görürler:

![Kutucuk için görünen ayarlar ve özellikler içeren "Pano" sekmesi](media/howto-configure-homepage/image4a.png)

Varsayılan uygulama panosunun nasıl daha fazla özelleştirildiğini öğrenmek için Kitaplıktaki diğer döşeme türlerini keşfedebilirsiniz.

Azure IoT Central 'de kutucukları kullanma hakkında daha fazla bilgi için bkz. [Pano kutucukları kullanma](howto-use-tiles.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central varsayılan uygulama panonuzun nasıl yapılandırılacağını öğrendiğinize göre şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Görüntüleri hazırlama ve karşıya yükleme hakkında bilgi edinin](howto-prepare-images.md)
