---
title: Uç noktaları ve yolları yönetme (portal)
titleSuffix: Azure Digital Twins
description: Azure portal kullanarak Azure dijital TWINS verileri için uç noktaları ve olay yollarını ayarlama ve yönetme bölümüne bakın.
author: v-lakast
ms.author: v-lakast
ms.date: 7/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 7d563c7706529c6f3e280f7d138c0d6ba0dfc849
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87902224"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-portal"></a>Azure dijital TWINS 'te uç noktaları ve yolları yönetme (portal)

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

Azure dijital TWINS 'de, [olay bildirimlerini](how-to-interpret-event-data.md) aşağı akış hizmetlerine veya bağlı işlem kaynaklarına yönlendirebilirsiniz. Bu, önce olayları alabilen **uç noktalar** ayarlanarak yapılır. Daha sonra, Azure dijital TWINS tarafından oluşturulan olayların hangi uç noktalara teslim edildiğini belirten [**olay yolları**](concepts-route-events.md) oluşturabilirsiniz.

Bu makalede, [Azure Portal](https://portal.azure.com)kullanarak uç noktalar ve rotalar oluşturma işlemi adım adım açıklanmaktadır.

Ayrıca, [Eventroutes API 'leri](how-to-use-apis-sdks.md), [.net (C#) SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)veya [Azure dijital TWINS CLI](how-to-use-cli.md)ile uç noktaları ve yolları da yönetebilirsiniz. Bu makalenin Portal yerine bu mekanizmaların kullanıldığı bir sürümü için bkz. [*nasıl yapılır: uç noktaları ve yolları yönetme (API 'ler ve CLI)*](how-to-manage-routes-apis-cli.md).

## <a name="prerequisites"></a>Önkoşullar

* Bir **Azure hesabınızın** olması gerekir ( [buradan](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)birini ücretsiz olarak ayarlayabilirsiniz)
* Azure aboneliğinizde bir **Azure dijital TWINS örneği** gerekir. Zaten bir örneğiniz yoksa, [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama*](how-to-set-up-instance-scripted.md)bölümündeki adımları kullanarak bir tane oluşturabilirsiniz. Bu makalede daha sonra kullanmak için kurulum 'un aşağıdaki değerlerini kullanın:
    - Örnek adı
    - Kaynak grubu

Bu ayrıntıları, örneğinizi ayarladıktan sonra [Azure Portal](https://portal.azure.com) bulabilirsiniz. Portalda oturum açın ve Portal arama çubuğunda örneğinizin adını arayın.
 
:::image type="content" source="media/how-to-manage-routes-portal/search-field-portal.png" alt-text="Azure portal arama çubuğunun ekran görüntüsü.":::

Örneğinizin Ayrıntılar sayfasını görmek için sonuçlardan örneğinizi seçin:

:::image type="content" source="media/how-to-manage-routes-portal/instance-details.png" alt-text="ADT örnek ayrıntılarının ekran görüntüsü." border="false":::

## <a name="create-an-endpoint-for-azure-digital-twins"></a>Azure dijital TWINS için uç nokta oluşturma

Bunlar, örneğiniz için oluşturabileceğiniz desteklenen uç nokta türleridir:
* [Event Grid](../event-grid/overview.md) 
* [Event Hubs](../event-hubs/event-hubs-about.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Farklı uç nokta türleri hakkında daha fazla bilgi için bkz. [*Azure mesajlaşma hizmetleri arasında seçim yapın*](../event-grid/compare-messaging-services.md).

Bir uç noktayı Azure dijital TWINS 'e bağlamak için, uç nokta için kullanmakta olduğunuz olay Kılavuzu konusu, Olay Hub 'ı veya Service Bus zaten mevcut olması gerekir. 

### <a name="create-an-event-grid-endpoint"></a>Event Grid uç noktası oluşturma

**Önkoşul**: *özel olay* hızlı başlangıcı Event Grid [ *özel konu oluşturma* bölümündeki](../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) adımları izleyerek bir olay Kılavuzu konusu oluşturun.

Konuyu oluşturduktan sonra, Azure Digital TWINS örneğinizin [Azure Portal](https://portal.azure.com) sayfasından Azure dijital TWINS 'e bağlayabilirsiniz (örneğin adını Portal arama çubuğuna girerek örneği bulabilirsiniz).

Örnek menüsünde _uç noktalar_' ı seçin. Ardından, izleyen uç *noktalar* sayfasından *+ uç nokta oluştur*' u seçin. 

Açılan *bir uç nokta oluştur* sayfasında, ilgili radyo düğmesini seçerek _Event Grid_ türünde bir uç nokta oluşturabilirsiniz. Diğer ayrıntıları doldurun: _ad_ alanına uç noktanız için bir ad girin, açılan listeden _aboneliğinizi_ seçin ve üçüncü açılan listeden önceden oluşturduğunuz _Event Grid konu başlığını_ seçin.

Sonra, _Kaydet_' i vurarak uç noktanızı oluşturun.

:::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-event-grid.png" alt-text="Event Grid türünde bir uç nokta oluşturma ekran görüntüsü.":::

Üstteki Azure portal çubuğundaki bildirim simgesini denetleyerek uç noktanın başarıyla oluşturulduğunu doğrulayabilirsiniz: 

:::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-notifications.png" alt-text="Uç nokta oluşturulmasını doğrulama bildiriminin ekran görüntüsü." border="false":::

Ayrıca, Azure dijital TWINS örneğiniz için *uç noktalar* sayfasında, geri oluşturulan uç noktayı görüntüleyebilirsiniz.

Uç nokta oluşturma işlemi başarısız olursa, hata iletisini gözlemleyin ve birkaç dakika sonra yeniden deneyin.

Şimdi, olay Kılavuzu konusu, Azure dijital TWINS 'in içindeki bir uç nokta olarak, _ad_ alanında belirtilen ad altında kullanılabilir. Genellikle bu adı, [Bu makalede daha sonra](#event-routes)oluşturacağınız bir **olay yolunun**hedefi olarak kullanacaksınız.

### <a name="create-an-event-hubs-endpoint"></a>Event Hubs uç noktası oluşturma

**Önkoşullar**: 
* Bir _Event Hubs ad alanı_ ve bir _Olay Hub 'ı_gerekir. [*Bir olay hub 'ı oluşturma*](../event-hubs/event-hubs-create.md) hızlı başlangıcı Event Hubs adımları izleyerek bunların her ikisini de oluşturun.
* Bir _yetkilendirme kuralına_ihtiyacınız olacak. Bunu oluşturmak için, [*paylaşılan erişim imzalarını kullanarak Event Hubs kaynaklarına erişimi yetkilendirmek*](../event-hubs/authorize-access-shared-access-signature.md) Event Hubs başvurun.

[Azure Portal](https://portal.azure.com) Azure dijital TWINS örneğinizin ayrıntılar sayfasına gidin (adını Portal arama çubuğuna girerek bulabilirsiniz).

Örnek menüsünde _uç noktalar_' ı seçin. Ardından, izleyen uç *noktalar* sayfasından *+ uç nokta oluştur*' u seçin. 

Açılan *bir uç nokta oluştur* sayfasında, ilgili radyo düğmesini seçerek _Olay Hub_ 'ı türünde bir uç nokta oluşturabilirsiniz. _Ad_ alanına uç noktanız için bir ad girin. Ardından _aboneliğinizi_ve ilgili açılan kutudan önceden oluşturduğunuz _Olay Hub 'ı ad_alanınızı, _Olay Hub 'ını_ve _Yetkilendirme kuralını_ seçin.

Sonra, _Kaydet_' i vurarak uç noktanızı oluşturun.

:::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-event-hub.png" alt-text="Event Hubs türünde bir uç nokta oluşturma ekran görüntüsü.":::

Üstteki Azure portal çubuğundaki bildirim simgesini denetleyerek uç noktanın başarıyla oluşturulduğunu doğrulayabilirsiniz. 

Uç nokta oluşturma işlemi başarısız olursa, hata iletisini gözlemleyin ve birkaç dakika sonra yeniden deneyin.

Şimdi, Olay Hub 'ı Azure dijital TWINS 'in içindeki bir uç nokta olarak, _ad_ alanında belirtilen ad altında kullanılabilir. Genellikle bu adı, [Bu makalede daha sonra](#event-routes)oluşturacağınız bir **olay yolunun**hedefi olarak kullanacaksınız.

### <a name="create-a-service-bus-endpoint"></a>Service Bus uç noktası oluşturma

**Önkoşullar**: 
* Bir _Service Bus ad alanına_ ve _Service Bus konuya_ihtiyacınız vardır. [*Konular ve abonelikler oluşturma*](../service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal.md) hızlı başlangıç Service Bus adımları izleyerek bunların her ikisini de oluşturun. [*Konuya abonelik oluşturma*](../service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal.md#create-subscriptions-to-the-topic) bölümünü doldurmanız gerekmez.
* Bir _yetkilendirme kuralına_ihtiyacınız olacak. Bunu oluşturmak için Service Bus [*kimlik doğrulaması ve yetkilendirme*](../service-bus-messaging/service-bus-authentication-and-authorization.md#shared-access-signature) makalesine başvurun.

[Azure Portal](https://portal.azure.com) Azure dijital TWINS örneğinizin ayrıntılar sayfasına gidin (adını Portal arama çubuğuna girerek bulabilirsiniz).

Örnek menüsünde _uç noktalar_' ı seçin. Ardından, izleyen uç *noktalar* sayfasından *+ uç nokta oluştur*' u seçin. 

Açılan *bir uç nokta oluştur* sayfasında, ilgili radyo düğmesini seçerek _Service Bus_ türünde bir uç nokta oluşturabilirsiniz. _Ad_ alanına uç noktanız için bir ad girin. Ardından _aboneliğinizi_ve önceden oluşturduğunuz _Service Bus ad_alanınızı, _Service Bus konu başlığını_ve _Yetkilendirme kuralını_ ilgili açılan kutudan seçin.

Sonra, _Kaydet_' i vurarak uç noktanızı oluşturun.

:::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-service-bus.png" alt-text="Service Bus türünde bir uç nokta oluşturma ekran görüntüsü.":::

Üstteki Azure portal çubuğundaki bildirim simgesini denetleyerek uç noktanın başarıyla oluşturulduğunu doğrulayabilirsiniz. 

Uç nokta oluşturma işlemi başarısız olursa, hata iletisini gözlemleyin ve birkaç dakika sonra yeniden deneyin.

Artık Service Bus konu başlığı altında, _ad_ alanında belirtilen ad altında Azure dijital TWINS 'in içinde bir uç nokta olarak sunulmaktadır. Genellikle bu adı, [Bu makalede daha sonra](#event-routes)oluşturacağınız bir **olay yolunun**hedefi olarak kullanacaksınız.

## <a name="event-routes"></a>Olay yolları

Azure dijital TWINS 'den bir uç noktaya gerçek veri göndermek için bir **olay yolu**tanımlamanız gerekir. Bu yollar, geliştiricilerin sistem genelinde ve aşağı akış hizmetlerinde olay akışını üzerinde bağlantı almasına imkan tanır. Kavramlar bölümünde olay yolları hakkında daha fazla bilgi edinin [*: Azure dijital TWINS olaylarını yönlendirme*](concepts-route-events.md).

**Önkoşul**: bir yol oluşturmak için geçiş yapabilmeniz için önce Bu makalenin önceki kısımlarında açıklandığı gibi uç noktalar oluşturmanız gerekir. Uç noktalarınız kurulum tamamlandıktan sonra bir olay rotası oluşturmaya devam edebilirsiniz.

>[!NOTE]
>Son noktalarınızı dağıttıysanız, yeni bir olay yolu için kullanmayı denemeden **önce** bunların dağıtımını tamamladığınızı doğrulayın. Uç noktalar hazırlanmadığından yolu ayarlayamıyoruz, birkaç dakika bekleyip yeniden deneyin.

### <a name="create-an-event-route"></a>Olay yolu oluşturma 

Bir olay yolu tanımı şu öğeleri içerir:
* Kullanmak istediğiniz yol adı
* Kullanmak istediğiniz uç noktanın adı
* Uç noktaya hangi olayların gönderileceğini tanımlayan bir filtre
    - Bir olayın gönderilmemesi için yolu devre dışı bırakmak için bir filtre değeri kullanın`false`
    - Belirli bir filtreleme özelliği olmayan bir yolu etkinleştirmek için, bir filtre değeri kullanın`true`
    - Diğer filtre türleri hakkında daha fazla bilgi için aşağıdaki [*Olayları Filtrele*](#filter-events) bölümüne bakın.

Tek bir yol, birden fazla bildirim ve olay türünün seçili olmasını sağlayabilir.

Bir olay yolu oluşturmak için [Azure Portal](https://portal.azure.com) Azure dijital TWINS örneğinizin ayrıntılar sayfasına gidin (örneğin adını Portal arama çubuğuna girerek örneği bulabilirsiniz).

Örnek menüsünden _olay yolları_' nı seçin. Ardından, aşağıdaki *olay yolları* sayfasından *+ bir olay yolu oluştur*' u seçin. 

Açılan *bir olay yolu oluştur* sayfasında en az ' ı seçin:
* _Ad_ alanına yönlendirmenize ilişkin bir ad
* Yolu oluşturmak için kullanmak istediğiniz _uç nokta_ 

Yolun etkinleştirilmesi için, en azından **bir olay yönlendirme filtresi de eklemeniz** gerekir `true` . (Varsayılan değerinin bırakılması `false` yolu oluşturur, ancak buna hiçbir olay gönderilmez.) Bunu yapmak için, _Gelişmiş düzenleyicinin_ anahtarını etkinleştirmek üzere değiştirin ve `true` *filtre* kutusuna yazın.

:::image type="content" source="media/how-to-manage-routes-portal/create-event-route-no-filter.png" alt-text="Örneğiniz için olay yolu oluşturma ekranının ekran görüntüsü." lightbox="media/how-to-manage-routes-portal/create-event-route-no-filter.png":::

İşiniz bittiğinde, olay rotasını oluşturmak için _Kaydet_ düğmesine basın.

### <a name="filter-events"></a>Olayları filtreleme

Yukarıda açıklandığı gibi, yolların bir **filtre** alanı vardır. Yolinizdeki filtre değeri ise `false` , uç noktanıza hiçbir olay gönderilmez. 

En az filtresini etkinleştirdikten sonra `true` uç noktalar Azure dijital TWINS 'den çeşitli olaylar alır:
* Azure Digital TWINS hizmet API 'SI kullanılarak [dijital TWINS](concepts-twins-graph.md) tarafından tetiklenen telemetri
* İkizi özellik değişikliği bildirimleri, Azure dijital TWINS örneğindeki herhangi bir ikizi için özellik değişikliklerinde harekete geçirilir
* Yaşam döngüsü olayları, TWINS veya ilişkiler oluşturulduğunda veya silindiğinde tetiklenir
* Model değişiklik olayları, bir Azure dijital TWINS örneğinde yapılandırılan [modeller](concepts-models.md) eklendiğinde veya silindiğinde tetiklenir

Daha özel bir filtre tanımlayarak gönderilmekte olan olay türlerini kısıtlayabilirsiniz.

Bir olay yolu oluştururken bir olay filtresi eklemek için, *olay yolu oluşturma* sayfasının _olay yol filtresi ekle_ bölümünü kullanın. 

Bazı temel ortak filtre seçeneklerinden birini seçebilir veya kendi özel filtrelerinizi yazmak için gelişmiş filtre seçeneklerini kullanabilirsiniz.

#### <a name="use-the-basic-filters"></a>Temel filtreleri kullanma

Temel filtreleri kullanmak için, _olay türleri_ seçeneğini genişletin ve uç noktanıza göndermek istediğiniz olaylara karşılık gelen onay kutularını seçin. 

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-manage-routes-portal/create-event-route-filter-basic-1.png" alt-text="Temel filtreye sahip bir olay yolu oluşturma ekran görüntüsü. Olayların onay kutularını seçme.":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Bu, filtre metin kutusunu seçtiğiniz filtrenin metniyle otomatik olarak doldurur:

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-manage-routes-portal/create-event-route-filter-basic-2.png" alt-text="Temel filtreye sahip bir olay yolu oluşturma ekran görüntüsü. Olaylar seçildikten sonra otomatik doldurulmuş filtre metni gösteriliyor.":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

#### <a name="use-the-advanced-filters"></a>Gelişmiş filtreleri kullanma

Alternatif olarak, kendi özel filtrelerinizi yazmak için Gelişmiş Filtre seçeneğini de kullanabilirsiniz.

Gelişmiş filtre seçenekleriyle bir olay yolu oluşturmak için, _Gelişmiş düzenleyicinin_ anahtarını etkinleştirmek üzere değiştirin. Daha sonra, *filtre* kutusuna kendi olay filtrelerinizi yazabilirsiniz:

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-manage-routes-portal/create-event-route-filter-advanced.png" alt-text="Gelişmiş bir filtreye sahip bir olay yolu oluşturma ekranının ekran görüntüsü.":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Desteklenen yol filtreleri aşağıda verilmiştir. *Filtre metin şeması* sütunundaki ayrıntı, filtre kutusuna girilebilecek metindir.

[!INCLUDE [digital-twins-route-filters](../../includes/digital-twins-route-filters.md)]

[!INCLUDE [digital-twins-route-metrics](../../includes/digital-twins-route-metrics.md)]

## <a name="next-steps"></a>Sonraki adımlar

Alabilmeniz için farklı olay iletisi türleri hakkında bilgi edinin:
* [*Nasıl yapılır: olay verilerini yorumlama*](how-to-interpret-event-data.md)
