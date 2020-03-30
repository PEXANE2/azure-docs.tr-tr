---
title: Bir IoT Hub'ı oluşturmak için Azure portalını kullanın | Microsoft Dokümanlar
description: Azure portalı aracılığıyla Azure IoT hub'larını oluşturma, yönetme ve silme. Fiyatlandırma katmanları, ölçeklendirme, güvenlik ve ileti yapılandırması hakkında bilgi içerir.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: robinsh
ms.openlocfilehash: c43c142b22709d42416b2dd14dfc78812970916a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284739"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Azure portalını kullanarak bir IoT hub'ı oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bu makalede, [Azure portalını](https://portal.azure.com)kullanarak IoT hub'larının nasıl oluşturulup yönetilen bir şekilde açıklanmaktadır.

Bu öğreticideki adımları kullanmak için bir Azure aboneliğine ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="change-the-settings-of-the-iot-hub"></a>IoT hub'ının ayarlarını değiştirme

IoT Hub bölmesinden oluşturulduktan sonra varolan bir IoT hub'ının ayarlarını değiştirebilirsiniz.

![IoT hub'ı ayarlarını gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-settings-panel.png)

Bir IoT hub'ı için ayarlayabildiğiniz özelliklerden bazıları şunlardır:

**Fiyatlandırma ve ölçek**: Bu özelliği farklı bir katmana geçiş yapmak veya IoT Hub birimlerinin sayısını ayarlamak için kullanabilirsiniz. 

**İşlem izleme**: Aygıttan buluta iletilerle veya buluttan aygıta iletilerle ilgili etkinlikler için günlüğe kaydetme gibi farklı izleme kategorilerini açın veya kapatın.

**IP Filtresi**: IoT hub'ı tarafından kabul edilecek veya reddedilecek bir IP adresi aralığı belirtin.

**Özellikler**: Kaynak kimliği, kaynak grubu, konum gibi başka yerlerde kopyalayıp kullanabileceğiniz özelliklerin listesini sağlar.

### <a name="shared-access-policies"></a>Paylaşılan erişim ilkeleri

**Ayrıca, Ayarlar** bölümünde **Paylaşılan erişim ilkelerini** tıklatarak paylaşılan erişim ilkeleri listesini görüntüleyebilir veya değiştirebilirsiniz. Bu ilkeler, aygıtların ve hizmetlerin IoT Hub'ına bağlanması için izinleri tanımlar. 

**Paylaşılan erişim ilkesi** ekle bıçağını açmak için **Ekle'yi** tıklatın.  Aşağıdaki şekilde gösterildiği gibi, yeni ilke adını ve bu ilkeyle ilişkilendirmek istediğiniz izinleri girebilirsiniz:

![Paylaşılan erişim ilkesi eklemeyi gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-add-shared-access-policy.png)

* **Kayıt Defteri okuma** ve Kayıt Defteri **yazma** ilkeleri, kimlik kaydına erişim hakkı verir. Bu izinler, aygıt kimliklerini yönetmek için arka uç bulut hizmetleri tarafından kullanılır. Yazma seçeneğini seçmek, okuma seçeneğini otomatik olarak seçer.

* **Hizmet bağlama** ilkesi, hizmet bitiş noktalarına erişim izni verir. Bu izin, arka uç bulut hizmetleri tarafından aygıtlardan ileti gönderip almanın yanı sıra aygıt ikiz ve modül ikiz verilerini güncellemek ve okumak için kullanılır.

* **Aygıt bağlantısı** ilkesi, IoT Hub aygıt tarafındaki uç noktaları kullanarak ileti gönderme ve alma izni verir. Bu izin, aygıtlar tarafından bir IoT hub'ından ileti göndermek ve almak, aygıt ikiz ve modül ikiz verilerini güncelleştirmek ve okumak ve dosya yüklemelerini gerçekleştirmek için kullanılır.

Bu yeni oluşturulan ilkeyi varolan listeye eklemek için **Oluştur'u** tıklatın.

Belirli izinler tarafından verilen erişim hakkında daha ayrıntılı bilgi için [IoT Hub izinlerine](./iot-hub-devguide-security.md#iot-hub-permissions)bakın.

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="message-routing-for-an-iot-hub"></a>Bir IoT hub'ı için İleti Yönlendirmesi

Hub için rotalar ve özel uç noktaları tanımladığınız İleti Yönlendirme bölmesini görmek için **İleti** Yönlendirme'nin altında **İleti Yönlendirme'yi** tıklatın. [İleti yönlendirme,](iot-hub-devguide-messages-d2c.md) aygıtlarınızdan uç noktalarınıza verilerin nasıl gönderildiğini yönetmenize olanak tanır. İlk adım yeni bir rota eklemektir. Ardından, rotaya varolan bir bitiş noktası ekleyebilir veya blob depolama gibi desteklenen türlerden yenisini oluşturabilirsiniz. 

![İleti yönlendirme bölmesi](./media/iot-hub-create-through-portal/iot-hub-message-routing.png)

### <a name="routes"></a>Yollar

Routes, İleti Yönlendirme bölmesinin ilk sekmesidir. Yeni bir rota eklemek için +**Ekle'yi**tıklatın. Aşağıdaki ekranı görüyorsunuz. 

![Yeni bir rota eklemeyi gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-add-route-storage-endpoint.png)

Hub'ınızı adlandırın. Ad, o hub'ın rotaları listesinde benzersiz olmalıdır. 

**Endpoint**için açılır listeden birini seçebilir veya yeni bir tane ekleyebilirsiniz. Bu örnekte, bir depolama hesabı ve kapsayıcı zaten kullanılabilir. Bunları bitiş noktası olarak eklemek için, Bitiş Noktası açılır resminin yanına +**ekle'yi** tıklatın ve **Blob Depolama'yı**seçin. Aşağıdaki ekranda depolama hesabının ve kapsayıcının nerede belirtildiği gösterilmektedir.

![Yönlendirme kuralı için depolama bitiş noktası eklemeyi gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-routing-add-storage-endpoint.png)

Depolama hesabı ve kapsayıcıyı seçmek için **kapsayıcıyı seç'i** tıklatın. Bu alanları seçtiğinizde, Bitiş Noktası bölmesine geri döner. Alanların geri kalanı için varsayılanları kullanın ve depolama hesabı için bitiş noktası oluşturmak ve yönlendirme kurallarına eklemek için **oluşturun.**

**Veri kaynağı**için Aygıt Telemetri İletileri'ni seçin. 

Ardından, bir yönlendirme sorgusu ekleyin. Bu örnekte, eşit bir değerle `level` çağrılan bir uygulama `critical` özelliği olan iletiler depolama hesabına yönlendirilir.

![Yeni bir yönlendirme kuralını kaydetmeyi gösteren ekran görüntüsü](./media/iot-hub-create-through-portal/iot-hub-add-route.png)

Yönlendirme kuralını kaydetmek için **Kaydet'i** tıklatın. İleti Yönlendirme bölmesine geri dönersiniz ve yeni yönlendirme kuralınız görüntülenir.

### <a name="custom-endpoints"></a>Özel uç noktalar

Özel **uç noktaları** sekmesini tıklatın. Zaten oluşturulmuş özel uç noktaları görürsünüz. Buradan, yeni uç noktaları ekleyebilir veya varolan uç noktaları silebilirsiniz. 

> [!NOTE]
> Bir rotayı silerseniz, bu rotaya atanan uç noktaları silmez. Bitiş noktasını silmek için Özel uç noktaları sekmesini tıklatın, silmek istediğiniz bitiş noktasını seçin ve Sil'i tıklatın.
>

[Başvuru - IoT hub uç noktaları](iot-hub-devguide-endpoints.md)özel uç noktaları hakkında daha fazla bilgi edinebilirsiniz.

Bir IoT hub'ı için en fazla 10 özel uç nokta tanımlayabilirsiniz. 

Yönlendirme ile özel uç noktaların nasıl kullanılacağına ilgili tam bir örnek görmek için, [IoT Hub ile İleti yönlendirme](tutorial-routing.md)bölümüne bakın.

## <a name="find-a-specific-iot-hub"></a>Belirli bir IoT hub'ı bulma

Aboneliğinizde belirli bir IoT hub'ını bulmanın iki yolu şunlardır:

1. IoT hub'ına ait olduğu kaynak grubunu biliyorsanız, **Kaynak gruplarını**tıklatın ve ardından listeden kaynak grubunu seçin. Kaynak grubu ekranı, IoT hub'ları da dahil olmak üzere bu gruptaki tüm kaynakları gösterir. Aradığınız hub'a tıklayın.

2. **Tüm kaynaklar**'a tıklayın. Tüm **kaynaklar** bölmesinde, varsayılan olarak .'a `All types`gelen bir açılır liste vardır. Açılır listeye tıklayın, denetimi `Select all`kaldırın. Bul `IoT Hub` ve kontrol et. Kapatmak için açılır liste kutusuna tıklayın ve girişler yalnızca IoT hub'larınızı göstererek filtrelenir.

## <a name="delete-the-iot-hub"></a>IoT merkezini silme

Bir Iot hub'ını silmek için, silmek istediğiniz IoT merkezini bulun ve ardından IoT hub adının altındaki **Sil** düğmesini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Hub'ı yönetme hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [IoT Hub ile ileti yönlendirme](tutorial-routing.md)
* [IoT Hub ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)