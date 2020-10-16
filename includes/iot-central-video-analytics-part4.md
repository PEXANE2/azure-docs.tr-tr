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
ms.openlocfilehash: 164f5803b6e9e62447423735e98f6e4c36c73f13
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877240"
---
### <a name="add-relationships"></a>İlişki Ekle

**LVA Edge Gateway** cihaz şablonunda, **modüller/LVA sınır ağ geçidi modülü**altında **ilişkiler**' i seçin. **+ Ilişki Ekle** ' yi seçin ve aşağıdaki iki ilişkiyi ekleyin:

|Görünen Ad               |Adı          |Hedef |
|-------------------------- |------------- |------ |
|LVA Edge hareket algılayıcısı   |Varsayılanı kullan   |LVA Edge hareket algılayıcısı cihazı |
|LVA Edge nesne algılayıcısı   |Varsayılanı kullan   |LVA Edge nesne algılayıcısı cihazı |

Ardından **Kaydet**’i seçin.

:::image type="content" source="media/iot-central-video-analytics-part4/relationships.png" alt-text="İlişki Ekle":::

### <a name="add-views"></a>Görünümler ekleme

**LVA Edge Gateway** cihaz şablonu hiçbir görünüm tanımı içermez.

Cihaz şablonuna bir görünüm eklemek için:

1. **LVA sınır ağ geçidi** cihaz şablonunda, **Görünümler** ' e gidin ve cihaz kutucuğunu **görselleştirmeyi** seçin.

1. *LVA Edge Gateway cihazını* görünüm adı olarak girin.

1. Görünüme aşağıdaki kutucukları ekleyin:

    * **Cihaz bilgileri** özelliklerine sahip bir kutucuk: **cihaz modeli**, **üretici**, **işletim sistemi**, **Işlemci mimarisi**, **yazılım sürümü**, **toplam bellek**ve **toplam depolama alanı**.
    * **Boş belleği** ve **sistem sinyal** telemetrisi değerlerini içeren bir çizgi grafik kutucuğu.
    * Şu olaylarla bir olay geçmişi kutucuğu: **Kamera oluşturma**, **kamerayı silme**, **modül yeniden başlatma**, modül **başlatıldı**, **Modül durdu**.
    * **IoT Central Istemci durumu** telemetrisini gösteren bir 2x1 bilinen son değer kutucuğu.
    * **Modül durumu** telemetrisini gösteren 2x1 bilinen son değer kutucuğu.
    * **Sistem sinyal** telemetrisini gösteren 1x1 bilinen son değer kutucuğu.
    * **Bağlı kameraları** Telemetriyi gösteren 1x1 bilinen son değer kutucuğu.

    :::image type="content" source="media/iot-central-video-analytics-part4/gateway-dashboard.png" alt-text="İlişki Ekle":::

1. **Kaydet**’i seçin.

### <a name="publish-the-device-template"></a>Cihaz şablonunu yayımlama

Uygulamaya bir cihaz ekleyebilmeniz için önce cihaz şablonunu yayımlamanız gerekir:

1. **LVA sınır ağ geçidi** cihaz şablonunda **Yayımla**' yı seçin.

1. **Bu cihaz şablonunu uygulama üzerinde Yayımla** sayfasında **Yayımla**' yı seçin.

**LVA Edge Gateway** artık, uygulamadaki **cihazlar** sayfasında kullanılacak cihaz türü olarak kullanılabilir.

## <a name="add-a-gateway-device"></a>Ağ geçidi cihazı ekleme

Uygulamaya **LVA Edge Gateway** cihazı eklemek için:

1. **Cihazlar** sayfasına gidin ve **LVA Edge Gateway** cihaz şablonunu seçin.

1. **+ Yeni** seçeneğini belirleyin.

1. **Yeni bir cihaz oluştur** iletişim kutusunda cihaz adını *LVA Gateway 001*olarak değiştirin ve cihaz kimliğini *LVA-Gateway-001*olarak değiştirin.

    > [!NOTE]
    > Cihaz KIMLIĞI uygulamada benzersiz olmalıdır.

1. **Oluştur**’u seçin.

Cihaz durumu **kayıtlı**.

### <a name="get-the-device-credentials"></a>Cihaz kimlik bilgilerini al

Cihazın IoT Central uygulamanıza bağlanmasına izin veren kimlik bilgileri gerekir. Cihaz kimlik bilgilerini al:

1. **Cihazlar** sayfasında, oluşturduğunuz **LVA-Gateway-001** cihazını seçin.

1. **Bağlan**'ı seçin.

1. **Cihaz bağlantısı** sayfasında, **kimlik KAPSAMı**, **cihaz kimliği**ve cihaz **birincil anahtarı** *scratchpad.txt* dosyasında bir Note olun. Bu değerleri daha sonra kullanırsınız.

1. Bağlantı yönteminin, **paylaşılan erişim imzası**olarak ayarlandığından emin olun.

1. **Kapat**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Artık **video analizi-nesne ve hareket algılama** uygulama şablonunu kullanarak bir IoT Central uygulaması oluşturdunuz, ağ geçidi cihazı için bir cihaz şablonu oluşturdunuz ve uygulamaya bir ağ geçidi cihazı eklediniz.

Video analizi-nesne ve hareket algılama uygulamasını, sanal bir sanal makineyi benzetimli video akışlarıyla çalıştıran IoT Edge modüller kullanarak denemek istiyorsanız:

> [!div class="nextstepaction"]
> [Video analizi için IoT Edge örneği oluşturma (Linux VM)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-vm.md)

**Gerçek bir** cihaz ile gerçek zamanlı olarak çalışan IoT Edge modüller kullanarak video analizi-nesne ve hareket algılama uygulamasını denemek istiyorsanız:

> [!div class="nextstepaction"]
> [Video analizi için IoT Edge örneği oluşturma (Intel NUC)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-nuc.md)
