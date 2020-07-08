---
title: Azure IoT Central uygulamasının sistem durumunu izleme | Microsoft Docs
description: Operatör veya yönetici olarak, IoT Central uygulamanıza bağlı cihazların genel sistem durumunu izleyin.
author: dominicbetts
ms.author: dobett
ms.date: 05/14/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 664819b209aeb09093ce8711456b86ff4d3e8949
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84249581"
---
# <a name="monitor-the-overall-health-of-the-devices-connected-to-an-iot-central-application"></a>IoT Central uygulamasına bağlı cihazların genel sistem durumunu izleme

*Bu makale işleçler ve yöneticiler için geçerlidir.*

Bu makalede, IoT Central uygulamanıza bağlı cihazların genel durumunu değerlendirmek için IoT Central tarafından sağlanan ölçüm kümesini nasıl kullanacağınızı öğreneceksiniz.

Ölçümler, IoT Central uygulamanız için varsayılan olarak etkindir ve bunlara [Azure Portal](https://portal.azure.com/)erişebilirsiniz. [Azure izleyici veri platformu bu ölçümleri kullanıma sunar](../../azure-monitor/platform/data-platform-metrics.md) ve bunlarla etkileşimde bulunmak için çeşitli yollar sağlar. Örneğin, Azure portal, bir REST API veya PowerShell veya Azure CLı içindeki sorguları kullanabilirsiniz.

### <a name="trial-applications"></a>Deneme uygulamaları

Ücretsiz deneme planını kullanan uygulamalar ilişkili bir Azure aboneliğine sahip değildir ve Azure Izleyici ölçümlerini desteklemez. [Bir uygulamayı standart fiyatlandırma planına dönüştürebilir](./howto-view-bill.md#move-from-free-to-standard-pricing-plan) ve bu ölçümlere erişim sağlayabilirsiniz.

## <a name="view-metrics-in-the-azure-portal"></a>Azure portal ölçümleri görüntüleme

Aşağıdaki adımlarda, bazı [bağlı cihazlara](./tutorial-connect-device-nodejs.md)sahip bir [IoT Central uygulamasına](./quick-deploy-iot-central.md) sahip olduğunuz varsayılır.

Portalda IoT Central ölçümleri görüntülemek için:

1. Portalda IoT Central uygulama kaynağına gidin. Varsayılan olarak, IoT Central kaynaklar **ıotc**adlı bir kaynak grubunda bulunur.
1. Uygulamanızın ölçümlerinden bir grafik oluşturmak için **izleme** bölümünde **ölçümler** ' i seçin.

### <a name="azure-portal-permissions"></a>Azure portal izinleri

Azure portal ölçümlerine erişim, [Azure rol tabanlı erişim denetimi](../../role-based-access-control/overview.md)tarafından yönetilir. Kullanıcılara erişim izni vermek için IoT Central uygulama/kaynak grubu/aboneliğine Kullanıcı eklemek için Azure portal kullanın. IoT Central uygulamasına zaten eklendikleri halde portala bir Kullanıcı eklemeniz gerekir. Daha ayrıntılı erişim denetimi için [Azure yerleşik rollerini](../../role-based-access-control/built-in-roles.md) kullanın.

## <a name="iot-central-metrics"></a>IoT Central ölçümleri

Aşağıdaki tabloda IoT Central için şu anda kullanılabilen ölçümler açıklanmaktadır:

| Ölçüm | Ölçüm görünen adı | Birim | Toplama türü | Açıklama |
|--------|---------------------|------|------------------|-------------|
| connectedDeviceCount         | Toplam bağlı cihaz sayısı                              | Sayı  | Toplam             | IoT Central bağlı cihazların sayısı                               |
| C2D. Property. Read. Success    | IoT Central 'den başarılı cihaz özelliği okumaları    | Sayı  | Toplam             | IoT Central başlatılan tüm başarılı Özellik okuma sayısı    |
| C2D. Property. Read. Failure    | IoT Central 'den cihaz özelliği okumaları başarısız oldu        | Sayı  | Toplam             | IoT Central başlatılan tüm başarısız Özellik okuma sayısı        |
| D2C. Property. Read. Success    | Cihazlardan başarılı cihaz özelliği okumaları        | Sayı  | Toplam             | Cihazlardan başlatılan tüm başarılı Özellik okuma sayısı        |
| D2C. Property. Read. Failure    | Cihazlardan cihaz özelliği okumaları başarısız oldu            | Sayı  | Toplam             | Cihazlardan başlatılan tüm başarısız Özellik okuma sayısı            |
| C2D. Property. Update. Success  | IoT Central 'den başarılı cihaz özelliği güncelleştirmeleri  | Sayı  | Toplam             | IoT Central başlatıldığı tüm başarılı özellik güncelleştirmeleri sayısı  |
| C2D. Property. Update. Failure  | IoT Central cihaz özelliği güncelleştirmeleri başarısız oldu      | Sayı  | Toplam             | IoT Central tarafından başlatılan tüm başarısız özellik güncelleştirmelerinin sayısı      |
| D2C. Property. Update. Success  | Cihazlardan başarılı cihaz özelliği güncelleştirmeleri      | Sayı  | Toplam             | Cihazlardan başlatılan tüm başarılı özellik güncelleştirmelerinin sayısı      |
| D2C. Property. Update. Failure  | Cihazlardan cihaz özelliği güncelleştirmeleri başarısız oldu          | Sayı  | Toplam             | Cihazlardan başlatılan tüm başarısız özellik güncelleştirmelerinin sayısı          |

### <a name="metrics-and-invoices"></a>Ölçümler ve faturalar

Ölçümler, Azure IoT Central faturanızda gösterilen numaralardan farklı olabilir. Bu durum, şöyle bir nedenden dolayı meydana gelir:

- IoT Central [Standart fiyatlandırma planları](https://azure.microsoft.com/pricing/details/iot-central/) , ücretsiz olarak iki cihaz ve farklı ileti kotaları içerir. Ücretsiz öğeler faturalandırma dışında tutulurken, bunlar ölçümlerde hala sayılır.

- IoT Central, uygulamadaki her cihaz şablonu için bir test cihaz KIMLIĞI oluşturur. Bu cihaz KIMLIĞI, cihaz şablonu için **Test cihazını Yönet** sayfasında görünür. Çözüm oluşturucular, bu test cihaz kimliklerini kullanan kod oluşturarak [cihaz şablonlarını](./overview-iot-central.md#create-device-templates) yayımlamadan önce doğrulamayı seçebilirler. Bu cihazlar faturalandırma dışında tutulurken, ölçümlerde hala sayılır.

- Ölçümler cihazdan buluta iletişimin bir alt kümesini gösterebilirken, cihaz ile bulut arasındaki tüm iletişimler [faturalandırma için bir ileti olarak sayılır](https://azure.microsoft.com/pricing/details/iot-central/).

## <a name="next-steps"></a>Sonraki adımlar

Uygulama şablonlarını nasıl kullanacağınızı öğrendiğinize göre, önerilen sonraki adım [Azure portal IoT Central nasıl yöneteceğinizi öğreneceksiniz.](howto-manage-iot-central-from-portal.md)
