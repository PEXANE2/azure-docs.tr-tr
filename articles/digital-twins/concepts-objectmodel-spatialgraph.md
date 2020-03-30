---
title: Nesne modellerini ve uzamsal zeka grafiğini anlama - Azure Digital Twins | Microsoft Dokümanlar
description: Kişiler, yerler ve aygıtlar arasındaki ilişkileri modellemek için Azure Digital Twins'i kullanın
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/30/2019
ms.openlocfilehash: cafec321e7c40e27d8de731feda1103451271507
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265213"
---
# <a name="understand-digital-twins-object-models-and-spatial-intelligence-graph"></a>Dijital İkizler nesne modellerini ve uzamsal zeka grafiğini anlayın

Azure Digital Twins, fiziksel ortamların ve ilişkili cihazların, sensörlerin ve kişilerin kapsamlı sanal gösterimlerine güç veren bir Azure IoT hizmetidir. Etki alanına özgü kavramları yararlı modellerhalinde düzenleyerek geliştirmeyi geliştirir. Modeller daha sonra bir mekansal zeka grafiği içinde yer almaktadır. Bu tür kavramlar, insanlar, boşluklar ve aygıtlar arasındaki ilişkileri ve etkileşimleri sadakatle modeller.

Digital Twins nesne modelleri etki alanına özgü kavramları, kategorileri ve özellikleri açıklar. Modeller, çözümü kendi özel gereksinimlerine göre uyarlamak isteyen kullanıcılar tarafından önceden tanımlanır. Birlikte, bu önceden tanımlanmış Dijital İkizler nesne modelleri bir _ontoloji_makyaj. Akıllı bir binanın ontolojisi bölgeleri, mekanları, yerleri, ofisleri, bölgeleri, konferans salonlarını ve odak odalarını tanımlar. Bir enerji şebekesi ontolojisi çeşitli elektrik santrallerini, trafo istasyonlarını, enerji kaynaklarını ve müşterileri tanımlar. Digital Twins nesne modelleri ve ontolojileri ile farklı senaryolar ve ihtiyaçlar özelleştirilebilir.

Digital Twins nesne modelleri ve yerinde bir ontoloji ile, _bir mekansal grafik_doldurabilirsiniz. Uzamsal grafikler, bir IoT çözümüyle alakalı boşluklar, aygıtlar ve kişiler arasındaki birçok ilişkinin sanal gösterimleridir. Bu diyagram, akıllı bir binanın ontolojisini kullanan uzamsal bir grafik örneği gösterir.

[![Dijital İkizler mekansal grafik oluşturma](media/concepts/digital-twins-spatial-graph-building.png)](media/concepts/digital-twins-spatial-graph-building.png#lightbox)

Uzamsal grafik boşlukları, cihazları, sensörleri ve kullanıcıları bir araya getirir. Her biri gerçek dünyayı modelleyen bir şekilde birbirine bağlıdır. Bu örnekte, mekan 43 dört katlı, her biri birçok farklı alana sahip. Kullanıcılar iş istasyonlarıyla ilişkilidir ve grafiğin bazı bölümlerine erişim izni verilir. Bir yönetici uzamsal grafikte değişiklik yapma hakkına sahipken, ziyaretçiyalnızca belirli bina verilerini görüntüleme hakkına sahiptir.

## <a name="digital-twins-object-models"></a>Dijital İkizler nesne modelleri

Digital Twins nesne modelleri bu ana nesne kategorilerini destekler:

- **Boşluklar** sanal veya fiziksel konumlardır, `Customer` `Region`örneğin, `Venue` `Tenant`, , , ve .
- **Cihazlar,** örneğin sanal veya fiziksel ekipman `AwesomeCompany Device` `Raspberry Pi 3`parçalarıdır ve.
- **Sensörler,** örneğin olayları algılayan `AwesomeCompany Temperature Sensor` nesnelerdir `AwesomeCompany Presence Sensor`ve.
- **Kullanıcılar** yolcuları ve özelliklerini tanımlar.

Nesnelerin diğer kategorileri şunlardır:

- **Kaynaklar** bir alana eklenir ve genellikle uzamsal grafikteki nesneler tarafından kullanılmak üzere `IoTHub`Azure kaynaklarını temsil eder, örneğin.
- **Blobs** nesnelere (boşluklar, aygıtlar, sensörler ve kullanıcılar gibi) eklenir. Pandomim türü ve meta verilere sahip dosyalar olarak `maps` `pictures`kullanılırlar, örneğin, , , ve `manuals`.
- **Genişletilmiş türler,** örneğin `SpaceType` belirli özelliklere sahip varlıkları artıran genişletilebilir sayısallaştırmalardır ve. `SpaceSubtype`
- **Ontolojiler** genişletilmiş türleri bir dizi temsil, `Default` `Building`örneğin, , `BACnet`, ve `EnergyGrid`.
- **Özellik anahtarları ve değerleri** mekanların, aygıtların, sensörlerin ve kullanıcıların özel özellikleridir. Bunlar yerleşik özellikleri ile birlikte kullanılabilir, örneğin, `DeltaProcessingRefreshTime` anahtar `10` ve değer olarak.
- **Roller,** uzamsal grafikteki kullanıcılara ve aygıtlara atanan `Space Administrator` `User Administrator`izin `Device Administrator`kümeleridir, örneğin , , ve .
- **Rol atamaları,** uzamsal grafikteki bir rol ve nesne arasındaki ilişkidir. Örneğin, bir kullanıcıya veya bir hizmet sorumlusuna uzamsal grafikteki bir alanı yönetme izni verilebilir.
- **Güvenlik anahtarı depoları,** aygıtın Digital Twins ile güvenli bir şekilde iletişim kurabilmesi için belirli bir boşluk nesnesi altında hiyerarşideki tüm aygıtların güvenlik anahtarlarını sağlar.
- **Kullanıcı tanımlı işlevler** (UDF'ler) uzamsal grafik içinde özelleştirilebilir sensör telemetri işleme sağlar. Örneğin, bir UDF şunları yapabilir:
  - Sensör değeri ayarlayın.
  - Sensör okumalarına dayalı özel mantık gerçekleştirin ve çıktıyı bir alana ayarlayın.
  - Meta verileri bir boşluğa takın.
  - Önceden tanımlanmış koşullar karşılandığında bildirim gönderin. Şu anda, UDF'ler JavaScript'te yazılabilir.
- **Eşleyiciler,** belirli bir telemetri iletisi için hangi UDF'lerin yürütüldedildiğini belirleyen nesnelerdir.
- **Uç noktalar,** telemetri iletilerinin ve Dijital İkizler olaylarının yönlendirilebileceği konumlardır, örneğin , `Event Hub`, `Service Bus`ve `Event Grid`.

## <a name="spatial-intelligence-graph"></a>Uzamsal zeka grafı

Uzamsal grafik, Dijital İkizler nesne modelinde tanımlanan boşlukların, aygıtların ve kişilerin hiyerarşik grafiğidir. Uzamsal grafik kalıtım, filtreleme, geçiş, ölçeklenebilirlik ve genişletilebilirliği destekler. REST API'leri koleksiyonuyla uzamsal grafiğinizi yönetebilir ve etkileşimde olabilirsiniz.

Aboneliğinizde bir Digital Twins hizmeti dağıtsanız, kök düğümün genel yöneticisi olursunuz. Daha sonra otomatik olarak tüm yapıya tam erişim hakkı verilir. Boşluk API'sini kullanarak grafikteki boşlukları sağlama. Sensör API'sini kullanarak Cihaz API'sini ve sensörlerini kullanarak hizmet sağlama. [Açık kaynak araçları](https://github.com/Azure-Samples/digital-twins-samples-csharp) da toplu olarak grafik sağlamak için kullanılabilir.

**Grafik kalıtım**. Devralma, bir üst düğümden altındaki tüm düğümlere gelen izinler ve özellikler için geçerlidir. Örneğin, bir rol belirli bir düğümüzerinde bir kullanıcıya atandığında, kullanıcı bu rolün verilen düğüme ve altındaki her düğüme izinlerine sahiptir. Belirli bir düğüm için tanımlanan her özellik anahtarı ve genişletilmiş türü, bu düğümün altındaki tüm düğümler tarafından devralır.

**Grafik filtreleme**. Filtreleme, istek sonuçlarını daraltmak için kullanılır. Kimliklere, adlara, türlere, alt türlere, üst alana ve ilişkili alanlara göre filtre uygulayabilirsiniz. Ayrıca sensör veri türleri, özellik anahtarları ve değerleri, *geçiş*, *minLevel*, *maxLevel*, ve diğer OData filtre parametrelerine göre filtreleyebilirsiniz.

**Grafik geçişi.** Uzamsal grafiği derinliği ve genişliği yle geçebilirsiniz. Derinlik için, grafiği yukarıdan aşağıya veya aşağıdan *yukarıya,* *minLevel*ve *maxLevel*parametrelerini kullanarak çevirin. Kardeş düğümleri doğrudan bir üst alana veya onun soyundan gelenlerden birine bağlı almak için grafiği çapraz olarak ayarlayın. Bir nesneyi sorgularken, GET API'lerinin *içerir* parametresini kullanarak o nesneyle ilişkisi olan ilgili tüm nesneleri alabilirsiniz.

**Grafik ölçeklenebilirlik**. Digital Twins grafik ölçeklenebilirliğini garanti eder, böylece gerçek dünya iş yüklerinizi işleyebilir. Dijital İkizler gayrimenkul, altyapı, cihazlar, sensörler, telemetri ve daha büyük portföyleri temsil etmek için kullanılabilir.

**Grafik genişletilebilirlik**. Altta yatan Digital Twins nesne modellerini yeni türler ve ontolojilerle özelleştirmek için genişletilebilirliği kullanın. Dijital İkizler verileriniz genişletilebilir özellikler ve değerlerle de zenginlenebilir.

### <a name="spatial-intelligence-graph-management-apis"></a>Uzamsal zeka grafiği Yönetimi API'leri

[Azure portalından](https://portal.azure.com)Digital Twins'i dağıttıktan sonra, Yönetim API'lerinin [Swagger](https://swagger.io/tools/swagger-ui/) URL'si otomatik olarak oluşturulur. Azure portalında Aşağıdaki biçimde **Genel Bakış** bölümünde görüntülenir.

```plaintext
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| Adı | Şununla değiştir |
| --- | --- |
| YOUR_INSTANCE_NAME | Dijital İkizler örneğinizin adı |
| YOUR_LOCATION | Örneğiniz hangi sunucu bölgesinde barındırılır |

 Tam URL biçimi bu resimde görünür.

[![Dijital İkizler portalı Yönetim API](media/concepts/digital-twins-spatial-graph-management-api-url.png)](media/concepts/digital-twins-spatial-graph-management-api-url.png#lightbox)

Uzamsal zeka grafiklerinin nasıl kullanılacağı hakkında daha fazla bilgi için Azure Digital Twins Management API'leri gizli önizlemesini ziyaret edin.

> [!TIP]
> API özellik kümesini göstermek için Bir Swagger sneak önizlemesi sağlanır.
> [docs.westcentralus.azuresmartspaces.net/management/swagger'da](https://docs.westcentralus.azuresmartspaces.net/management/swagger)ev sahipliği yaptı.

[Swagger'ı nasıl kullanacağınız](how-to-use-swagger.md)hakkında daha fazla bilgi edinin.

Tüm API çağrıları [OAuth](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code)kullanılarak doğrulanmalıdır. API'ler [Microsoft REST API Yönergeleri yönergelerini](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)izler. Koleksiyonları döndüren API'lerin çoğu [OData](https://www.odata.org/getting-started/basic-tutorial/#queryData) sistem sorgu seçeneklerini destekler.

## <a name="next-steps"></a>Sonraki adımlar

- Aygıt bağlantısı ve Digital Twins'e telemetri iletilerinin nasıl gönderilecek hakkında bilgi edinmek için [Azure Digital Twins aygıt bağlantısı ve telemetri girişini](concepts-device-ingress.md)okuyun.

- Yönetim API sınırlamaları ve azaltmaları hakkında bilgi edinmek için [Azure Digital Twins API yönetimini ve sınırlamalarını](concepts-service-limits.md)okuyun.
