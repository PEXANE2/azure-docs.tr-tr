---
title: Veri işleme ve kullanıcı tanımlı işlevler - Azure Digital Twins| Microsoft Dokümanlar
description: Azure Digital Twins ile veri işleme, eşleştiriciler ve kullanıcı tanımlı işlevlere genel bakış.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2020
ms.openlocfilehash: 75ed2029582438ede43687addfd54c0a187e0120
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265187"
---
# <a name="data-processing-and-user-defined-functions"></a>Veri işleme ve kullanıcı tanımlı işlevleri

Azure Digital Twins gelişmiş işlem özellikleri sunar. Geliştiriciler, olayları önceden tanımlanmış uç noktalara göndermek için gelen telemetri iletilerine karşı özel işlevler tanımlayabilir ve çalıştırabilir.

## <a name="data-processing-flow"></a>Veri işleme akışı

Aygıtlar telemetri verilerini Azure Digital Twins'e gönderdikten sonra, geliştiriciler verileri dört aşamada işleyebilir: *doğrulama,* *eşleştirme,* *hesaplama*ve *gönderme.*

[![Azure Digital Twins veri işleme akışı](media/concepts/digital-twins-data-processing-flow.png)](media/concepts/digital-twins-data-processing-flow.png#lightbox)

1. Doğrulama aşaması, gelen telemetri iletisini sık anlaşılan [veri aktarım nesnesi](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/part-5) biçimine dönüştürür. Bu aşama aynı zamanda aygıt ve sensör doğrulaması yürütür.
1. Eşmatch aşaması, çalışacak uygun kullanıcı tanımlı işlevleri bulur. Önceden tanımlanmış eşleştirmeler, gelen telemetri iletisinden cihaz, sensör ve alan bilgilerine dayalı olarak kullanıcı tanımlı işlevleri bulur.
1. İşlem aşaması, önceki aşamada eşleşen kullanıcı tanımlı işlevleri çalıştırın. Bu işlevler, uzamsal grafik düğümlerinde sayısal değerleri okuyabilir ve güncelleyebilir ve özel bildirimler yarayabilir.
1. Gönderme aşaması, işlem aşamasındaki özel bildirimleri grafikte tanımlanan uç noktalara yönlendirir.

## <a name="data-processing-objects"></a>Veri işleme nesneleri

Azure Digital Twins'te veri işleme üç nesne tanımlamadan oluşur: *eşleştirmeler,* *kullanıcı tanımlı işlevler*ve *rol atamaları.*

[![Azure Digital Twins veri işleme nesneleri](media/concepts/digital-twins-user-defined-functions.png)](media/concepts/digital-twins-user-defined-functions.png#lightbox)

### <a name="matchers"></a>Eşleştirme

Eşleştirciler, gelen sensör telemetrisini temel alan hangi eylemlerin gerçekleştiğini değerlendiren bir dizi koşul tanımlar. Eşleşmeyi belirleme koşulları, sensörün ana aygıtından ve sensörün ana alanından gelen özellikleri içerebilir. Koşullar, bu örnekte belirtildiği gibi bir [JSON yolu](https://jsonpath.com/) karşı karşılaştırmalar olarak ifade edilir:

- Kaçan String değeri ile temsil edilen veri tipi **Sıcaklık** tüm sensörleri`\"Temperature\"`
- Limanlarında olması `01`
- Kaçan String değerine **ayarlanmış** genişletilmiş özellik anahtarı üretici olan aygıtlara ait olan`\"Contoso\"`
- Kaçan String tarafından belirtilen türdeki boşluklara ait olan`\"Venue\"`
- Hangi ebeveyn **SpaceId** torunları vardır`DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`

```JSON
{
  "id": "23535afafd-f39b-46c0-9b0c-0dd3892a1c30",
  "name": "My custom matcher",
  "spaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value Contoso and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "conditions": [
    {
      "id": "43898sg43-e15a-4e9c-abb8-2gw464364",
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "id": "wt3th44-e15a-35sg-seg3-235wf3ga463",
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "id": "735hs33-e15a-37jj-23532-db901d550af5",
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"Contoso\"",
      "comparison": "Equals"
    },
    {
      "id": "222325-e15a-49fg-5744-463643644",
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - JSON yolları vakaya duyarlıdır.
> - JSON yükü, aşağıdakiler tarafından döndürülen yük ile aynıdır:
>   - `/sensors/{id}?includes=properties,types`sensör için.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes`sensörün ana cihazı için.
>   - `/spaces/{id}?includes=properties,types,location,timezone`sensörün ana alanı için.
> - Karşılaştırmalar büyük/küçük harf duyarsız.

### <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Kullanıcı tanımlı işlev, yalıtılmış Bir Azure Dijital İkizler ortamında yürütülen özel bir işlevdir. Kullanıcı tanımlı işlevler, ham sensör telemetri iletisine alındıkça erişebilir. Kullanıcı tanımlı işlevler de uzamsal grafik ve sevk hizmetine erişebilir. Kullanıcı tanımlı işlev bir grafik içinde kaydedildikten sonra, işlevin ne zaman yürütüleceğini belirtmek için bir eşleştirme [(yukarıda](#matchers)ayrıntılı olarak) oluşturulmalıdır. Örneğin, Azure Digital Twins belirli bir sensörden yeni telemetri aldığında, eşleşen kullanıcı tanımlı işlev son birkaç sensör okumasının hareketli ortalamasını hesaplayabilir.

Kullanıcı tanımlı işlevler JavaScript'e yazılabilir. Yardımcı yöntemler, kullanıcı tanımlı yürütme ortamında grafikle etkileşime girmektedir. Geliştiriciler, sensör telemetri iletilerine karşı özel kod parçacıkları çalıştırabilir. Örneklere şunlar dahildir:

- Sensör okumasını doğrudan grafikiçindeki sensör nesnesine ayarlayın.
- Grafikteki bir boşluktaki farklı sensör okumalarına dayalı bir eylem gerçekleştirin.
- Gelen sensör okuması için belirli koşullar yerine getirildiğinde bir bildirim oluşturun.
- Bir bildirim göndermeden önce grafik meta verilerini sensör okumasına takın.

Daha fazla bilgi [için, kullanıcı tanımlı işlevlerinasıl](./how-to-user-defined-functions.md)kullanacağız'ı okuyun.

#### <a name="examples"></a>Örnekler

[Digital Twins C# örneği için GitHub repokullanıcı](https://github.com/Azure-Samples/digital-twins-samples-csharp/) tanımlı işlevlerin birkaç örneğini içerir:
- [Bu işlev,](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) bir odanın aralıkta bu değerlerle kullanılabilir olup olmadığını belirlemek için karbondioksit, hareket ve sıcaklık değerlerini arar. [Dijital İkizler için öğreticiler](tutorial-facilities-udf.md) daha ayrıntılı olarak bu işlevi keşfetmek. 
- [Bu işlev,](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/multiplemotionsensors.js) birden çok hareket sensöründen veri arar ve hiçbiri herhangi bir hareket algılamazsa alanın kullanılabilir olduğunu belirler. Dosyanın yorum bölümünde belirtilen değişiklikleri yaparak [hızlı başlatma](quickstart-view-occupancy-dotnet.md)veya [öğreticilerde](tutorial-facilities-setup.md)kullanılan kullanıcı tanımlı işlevi kolayca değiştirebilirsiniz. 

### <a name="role-assignment"></a>Rol ataması

Kullanıcı tanımlı bir işlevin eylemleri, hizmet içindeki verileri güvenli hale getirmek için Azure Digital Twins [rol tabanlı erişim denetimine](./security-role-based-access-control.md) tabidir. Rol atamaları, hangi kullanıcı tanımlı işlevlerin uzamsal grafik ve varlıklarıyla etkileşim kurmak için uygun izinlere sahip olduğunu tanımlar. Örneğin, kullanıcı tanımlı bir işlev, belirli bir alan altında grafik verilerini *oluşturma,* *OKUMA,* *GÜNCELLEME*veya *DELETE* grafik verilerini oluşturma ve kullanma iznine sahip olabilir. Kullanıcı tanımlı işlev grafikten veri istediğinde veya bir eylem emelettiğinde, kullanıcı tarafından tanımlanan bir işlevin erişim düzeyi denetlenir. Daha fazla bilgi için [Rol tabanlı erişim denetimini](./security-create-manage-role-assignments.md)okuyun.

Bir eşleştirmenin rol ataması olmayan kullanıcı tanımlı bir işlevi tetikletmesi mümkündür. Bu durumda, kullanıcı tanımlı işlev grafikteki verileri okuyamaz.

## <a name="next-steps"></a>Sonraki adımlar

- Olayları ve telemetri iletilerini diğer Azure hizmetlerine nasıl yönlendirileri hakkında daha fazla bilgi edinmek için [Rota etkinliklerini ve iletileri](./concepts-events-routing.md)okuyun.

- Eşleştirme, kullanıcı tanımlı işlevler ve rol atamaları hakkında daha fazla bilgi edinmek [için, kullanıcı tanımlı işlevleri kullanmak için Kılavuzu](./how-to-user-defined-functions.md)okuyun.

- Kullanıcı [tanımlı işlev istemci kitaplığı başvuru belgelerini gözden geçirin.](./reference-user-defined-functions-client-library.md)
