---
title: IoT Hub oluşturmak için Azure portal kullanın | Microsoft Docs
description: Azure portal aracılığıyla Azure IoT Hub 'ları oluşturma, yönetme ve silme. Fiyatlandırma katmanları, ölçeklendirme, güvenlik ve Mesajlaşma yapılandırması hakkında bilgiler içerir.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: robinsh
ms.openlocfilehash: c43c142b22709d42416b2dd14dfc78812970916a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84708119"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Azure portal kullanarak IoT Hub 'ı oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bu makalede [Azure Portal](https://portal.azure.com)kullanarak IoT Hub 'ları oluşturma ve yönetme açıklanır.

Bu öğreticideki adımları kullanmak için bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="change-the-settings-of-the-iot-hub"></a>IoT Hub 'ın ayarlarını değiştirme

Mevcut bir IoT Hub 'ın ayarlarını, IoT Hub bölmesinden oluşturulduktan sonra değiştirebilirsiniz.

![IoT Hub ayarlarını gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-settings-panel.png)

IoT Hub 'ı için ayarlayabileceğiniz özelliklerden bazıları şunlardır:

**Fiyatlandırma ve ölçek**: Bu özelliği, farklı bir katmana geçirmek veya IoT Hub birimi sayısını ayarlamak için kullanabilirsiniz. 

**İşlem izleme**: cihazdan buluta iletilerle veya buluttan cihaza iletilerle ilgili olaylar için günlüğe kaydetme gibi farklı izleme kategorilerini açın veya kapatın.

**IP filtresi**: IoT Hub tarafından kabul edilecek veya REDDEDILECEK bir IP adresi aralığı belirtin.

**Özellikler**: kaynak kimliği, kaynak grubu, konum vb. gibi, başka bir yere kopyalayabilmeniz ve kullanabileceğiniz özelliklerin listesini sağlar.

### <a name="shared-access-policies"></a>Paylaşılan erişim ilkeleri

Ayrıca, **Ayarlar** bölümünde **paylaşılan erişim ilkeleri** ' ne tıklayarak paylaşılan erişim ilkeleri listesini görüntüleyebilir veya değiştirebilirsiniz. Bu ilkeler, IoT Hub bağlanacak cihaz ve hizmetlere ilişkin izinleri tanımlar. 

**Ekle** ' ye tıklayarak **paylaşılan erişim ilkesi Ekle** dikey penceresini açın.  Yeni ilke adını ve bu ilkeyle ilişkilendirmek istediğiniz izinleri aşağıdaki şekilde gösterildiği gibi girebilirsiniz:

![Paylaşılan erişim ilkesi eklemeyi gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-add-shared-access-policy.png)

* **Kayıt defteri okuma** ve **kayıt defteri yazma** ilkeleri kimlik kayıt defterine okuma ve yazma erişim hakları verir. Bu izinler, arka uç bulut Hizmetleri tarafından cihaz kimliklerini yönetmek için kullanılır. Yazma seçeneğinin belirlenmesi otomatik olarak oku seçeneğini belirler.

* **Hizmet bağlanma** ilkesi, hizmet uç noktalarına erişim izni verir. Bu izin, arka uç bulut Hizmetleri tarafından cihazlardan ileti göndermek ve almak, Device ikizi ve Module ikizi verilerini güncelleştirmek ve okumak için kullanılır.

* **Cihaz bağlama** ilkesi, IoT Hub cihaz tarafı uç noktaları kullanarak ileti gönderme ve alma izinleri verir. Bu izin, cihazların bir IoT Hub 'ından ileti göndermek ve almak, cihaz ikizi ve modül ikizi verilerini güncelleştirmek ve okumak ve dosya karşıya yüklemeleri gerçekleştirmek için kullanılır.

Yeni oluşturulan ilkeyi mevcut listeye eklemek için **Oluştur** ' a tıklayın.

Belirli izinler tarafından verilen erişim hakkında daha ayrıntılı bilgi için bkz. [IoT Hub izinleri](./iot-hub-devguide-security.md#iot-hub-permissions).

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT Hub 'a yeni bir cihaz kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="message-routing-for-an-iot-hub"></a>IoT Hub 'ı için ileti yönlendirme

Merkez için yollar ve özel uç noktalar tanımladığınız Ileti yönlendirme bölmesini görmek için ileti altında **Ileti yönlendirme** **' ye tıklayın** . [İleti yönlendirme](iot-hub-devguide-messages-d2c.md) , cihazlarınızdan uç noktalarınıza verilerin gönderilme şeklini yönetmenizi sağlar. İlk adım yeni bir yol eklemektir. Ardından, yola var olan bir uç noktayı ekleyebilir veya desteklenen türlerden birini (BLOB depolama gibi) oluşturabilirsiniz. 

![İleti yönlendirme bölmesi](./media/iot-hub-create-through-portal/iot-hub-message-routing.png)

### <a name="routes"></a>Yollar

Yollar Ileti yönlendirme bölmesindeki ilk sekmedir. Yeni bir yol eklemek için +**Ekle**' ye tıklayın. Aşağıdaki ekranı görürsünüz. 

![Yeni yol eklemeyi gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-add-route-storage-endpoint.png)

Merkezinizi adlandırın. Ad, bu hub için yol listesi içinde benzersiz olmalıdır. 

**Uç nokta**için, açılan listeden bir seçim yapabilir veya yeni bir tane ekleyebilirsiniz. Bu örnekte, bir depolama hesabı ve kapsayıcısı zaten var. Bunları bir uç nokta olarak eklemek için uç nokta açılan menüsünde +**Ekle** ' ye tıklayın ve **BLOB depolama**' yı seçin. Aşağıdaki ekranda, depolama hesabının ve kapsayıcının belirtildiği yer gösterilmektedir.

![Yönlendirme kuralı için depolama uç noktası eklemeyi gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-routing-add-storage-endpoint.png)

Depolama hesabı ve kapsayıcısını seçmek için **kapsayıcı Seç** ' e tıklayın. Bu alanları seçtiğinizde, uç nokta bölmesine geri döner. Diğer alanlar için varsayılan değerleri kullanın ve depolama hesabı için uç noktayı oluşturmak ve yönlendirme kurallarına eklemek için **oluşturun** .

**Veri kaynağı**Için cihaz telemetri iletileri ' ni seçin. 

Sonra, bir yönlendirme sorgusu ekleyin. Bu örnekte, bir değeri değerine eşit olan adlı bir uygulama özelliğine sahip iletiler `level` `critical` depolama hesabına yönlendirilir.

![Yeni bir yönlendirme kuralının kaydedilmesini gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-add-route.png)

Yönlendirme kuralını kaydetmek için **Kaydet** ' e tıklayın. Ileti yönlendirme bölmesine dönersiniz ve yeni yönlendirme kuralınız görüntülenir.

### <a name="custom-endpoints"></a>Özel uç noktalar

**Özel uç noktalar** sekmesine tıklayın. Önceden oluşturulmuş özel uç noktaları görürsünüz. Buradan yeni uç noktalar ekleyebilir veya mevcut uç noktaları silebilirsiniz. 

> [!NOTE]
> Bir yolu silerseniz, bu rotaya atanan uç noktaları silmez. Bir uç noktayı silmek için özel uç noktalar sekmesine tıklayın, silmek istediğiniz uç noktayı seçin ve Sil ' e tıklayın.
>

[Başvuru IoT Hub uç noktalarında](iot-hub-devguide-endpoints.md)özel uç noktalar hakkında daha fazla bilgi edinebilirsiniz.

Bir IoT Hub 'ı için en fazla 10 özel uç nokta tanımlayabilirsiniz. 

Özel uç noktaların yönlendirme ile nasıl kullanılacağına ilişkin tam bir örnek görmek için bkz. [IoT Hub Ile ileti yönlendirme](tutorial-routing.md).

## <a name="find-a-specific-iot-hub"></a>Belirli bir IoT Hub 'ı bulun

Aboneliğinizde belirli bir IoT Hub 'ı bulmanın iki yolu vardır:

1. IoT Hub 'ının ait olduğu kaynak grubunu biliyorsanız, **kaynak grupları**' na tıklayın ve ardından listeden kaynak grubunu seçin. Kaynak grubu ekranında, IoT Hub 'ları dahil olmak üzere bu gruptaki tüm kaynaklar gösterilir. Aradığınız hub 'a tıklayın.

2. **Tüm kaynaklar**'a tıklayın. **Tüm kaynaklar** bölmesinde, varsayılan olarak kullanılacak bir açılan liste vardır `All types` . Açılan listeye tıklayın, işaretini kaldırın `Select all` . Bulun `IoT Hub` ve denetleyin. Açılır liste kutusuna tıklayarak bu kutuyu kapatın ve yalnızca IoT Hub 'larınızı gösteren girişler filtrelenecektir.

## <a name="delete-the-iot-hub"></a>IoT Hub 'ı silme

Bir IoT Hub 'ını silmek için, silmek istediğiniz IoT Hub 'ını bulun ve ardından IoT Hub 'ının adının altındaki **Sil** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Hub 'yi yönetme hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin:

* [IoT Hub ile ileti yönlendirme](tutorial-routing.md)
* [IoT Hub ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)