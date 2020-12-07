---
title: dosya dahil etme
description: dosya dahil etme
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 10/06/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 0b3ccc31c9159b5d7b1615add89e8fdc308bf8df
ms.sourcegitcommit: d6e92295e1f161a547da33999ad66c94cf334563
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96763458"
---
### <a name="publish-the-device-template"></a>Cihaz şablonunu yayımlama

Uygulamaya bir cihaz ekleyebilmeniz için önce cihaz şablonunu yayımlamanız gerekir:

1. **LVA Edge Gateway v2** cihaz şablonunda **Yayımla**' yı seçin.

1. **Bu cihaz şablonunu uygulama üzerinde Yayımla** sayfasında **Yayımla**' yı seçin.

**LVA Edge Gateway v2** , artık uygulamanın **cihazlar** sayfasında kullanılacak cihaz türü olarak kullanılabilir.

## <a name="migrate-the-gateway-device"></a>Ağ Geçidi cihazını geçirme

Mevcut **ağ geçidi-001** cihazı, **LVA Edge Gateway** cihaz şablonunu kullanır. Yeni Dağıtım bildiriminizi kullanmak için cihazı yeni cihaz şablonuna geçirin:

**Ağ geçidi-001** cihazını geçirmek için:

1. **Cihazlar** sayfasına gidin ve listede vurgulamak için **ağ geçidi-001** cihazını seçin.

1. **Geçir** seçeneğini belirleyin. **Geçiş** simgesi görünmüyorsa, daha fazla seçenek görmek için **...** seçeneğini belirleyin.

    :::image type="content" source="media/iot-central-video-analytics-part4/migrate-device.png" alt-text="Ağ Geçidi cihazını yeni bir sürüme geçirme":::

1. **Geçir** iletişim kutusundaki listede, **LVA Edge Gateway v2**' yi seçin ve ardından **geçir**' i seçin.

Birkaç saniye sonra geçiş tamamlanır. Cihazınız artık, özelleştirilmiş dağıtım bildiriminiz ile **LVA Edge Gateway v2** cihaz şablonunu kullanıyor.

Artık özgün **LVA Edge Gateway** cihaz şablonunu kullanan hiçbir cihaz yok. Bu cihaz şablonunu Sil:

1. **Cihaz şablonları** sayfasına gidin ve **LVA Edge Gateway** cihaz şablonunu seçin.

1. Cihaz şablonunu silmek için **Sil** ' i seçin.

### <a name="get-the-device-credentials"></a>Cihaz kimlik bilgilerini al

Cihazın IoT Central uygulamanıza bağlanmasına izin veren kimlik bilgileri gerekir. Cihaz kimlik bilgilerini al:

1. **Cihazlar** sayfasında, **ağ geçidi-001** cihazını seçin.

1. **Bağlan**’ı seçin.

1. **Cihaz bağlantısı** sayfasında, **kimlik KAPSAMı**, **cihaz kimliği** ve cihaz **birincil anahtarı** *scratchpad.txt* dosyasında bir Note olun. Bu değerleri daha sonra kullanırsınız.

1. Bağlantı yönteminin, **paylaşılan erişim imzası** olarak ayarlandığından emin olun.

1. **Kapat**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Artık **video analizi-nesne ve hareket algılama** uygulama şablonunu kullanarak bir IoT Central uygulaması oluşturdunuz, ağ geçidi cihazı için bir cihaz şablonu oluşturdunuz ve uygulamaya bir ağ geçidi cihazı eklediniz.

Video analizi-nesne ve hareket algılama uygulamasını, sanal bir sanal makineyi benzetimli video akışlarıyla çalıştıran IoT Edge modüller kullanarak denemek istiyorsanız:

> [!div class="nextstepaction"]
> [Video analizi için IoT Edge örneği oluşturma (Linux VM)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-vm.md)

**Gerçek bir** cihaz ile gerçek zamanlı olarak çalışan IoT Edge modüller kullanarak video analizi-nesne ve hareket algılama uygulamasını denemek istiyorsanız:

> [!div class="nextstepaction"]
> [Video analizi için IoT Edge örneği oluşturma (Intel NUC)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-nuc.md)
