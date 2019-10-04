---
title: Veri işleme ve Kullanıcı tanımlı işlevler-Azure dijital TWINS | Microsoft Docs
description: Azure dijital TWINS ile veri işleme, eşleştiriciler ve Kullanıcı tanımlı işlevlere genel bakış.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 09/17/2019
ms.openlocfilehash: b8ea5c54afd4b1e2c212422417688e528367d44f
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949981"
---
# <a name="data-processing-and-user-defined-functions"></a>Veri işleme ve Kullanıcı tanımlı işlevler

Azure dijital TWINS, gelişmiş işlem özellikleri sunar. Geliştiriciler, önceden tanımlanmış uç noktalara olay göndermek için gelen telemetri iletilerine karşı özel işlevler tanımlayabilir ve çalıştırabilir.

## <a name="data-processing-flow"></a>Veri işleme akışı

Cihazların Azure dijital TWINS 'e telemetri verileri gönderdikten sonra, geliştiriciler verileri dört aşamada işleyebilir: *doğrulama*, *eşleşme*, *işlem*ve *dağıtım*.

[![Azure dijital TWINS veri işleme akışı](media/concepts/digital-twins-data-processing-flow.png)](media/concepts/digital-twins-data-processing-flow.png#lightbox)

1. Doğrulama aşaması, gelen telemetri iletisini, yaygın olarak anlaşılan bir [veri aktarımı nesne](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/part-5) biçimine dönüştürür. Bu aşama Ayrıca cihaz ve algılayıcı doğrulamasını yürütür.
1. Eşleştirme aşaması, çalıştırılacak uygun Kullanıcı tanımlı işlevleri bulur. Önceden tanımlı eşleştiriciler, gelen telemetri iletisinden cihaz, algılayıcı ve alan bilgilerine göre Kullanıcı tanımlı işlevleri bulur.
1. İşlem aşaması, önceki aşamada eşleşen kullanıcı tanımlı işlevleri çalıştırır. Bu işlevler, uzamsal grafik düğümlerinde hesaplanan değerleri okuyabilir ve güncelleştirebilir ve özel bildirimler yayabilir.
1. Dağıtım aşaması, işlem aşamasından gelen özel bildirimleri grafikte tanımlanan uç noktalara yönlendirir.

## <a name="data-processing-objects"></a>Veri işleme nesneleri

Azure dijital TWINS 'de veri işleme üç nesne tanımlamayı içerir: *eşleştiriciler*, *Kullanıcı tanımlı işlevler*ve *rol atamaları*.

[![Azure dijital TWINS veri işleme nesneleri](media/concepts/digital-twins-user-defined-functions.png)](media/concepts/digital-twins-user-defined-functions.png#lightbox)

### <a name="matchers"></a>Eşleştiriciler

Eşleştiriciler, gelen algılayıcı telemetrisine göre hangi eylemlerin gerçekleşmekte olduğunu değerlendiren bir koşul kümesi tanımlar. Eşleşmeyi belirleme koşulları, sensörden, sensörün üst cihazından ve sensörün üst alanıyla eşleşen özellikler içerebilir. Koşullar, bu örnekte özetlenen bir [JSON yoluna](https://jsonpath.com/) yönelik karşılaştırmalar olarak ifade edilir:

- Kaçan dize değeri ile temsil edilen veri türü **sıcaklığının** tüm sensörlerinden `\"Temperature\"`
- Bağlantı noktasında `01` olması
- Genişletilmiş özellik anahtarı **üreticisine** sahip cihazlara ait olan, @no__t kaçış dize değerine ayarlanmıştır.-1
- Kaçan dize tarafından belirtilen türdeki boşluklara ait olan `\"Venue\"`
- Üst **Spaceıd** @no__t alt öğesi olan-1

```JSON
{
  "SpaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Name": "My custom matcher",
  "Description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value GoodCorp and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"GoodCorp\"",
      "comparison": "Equals"
    },
    {
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - JSON yolları büyük/küçük harfe duyarlıdır.
> - JSON yükü, tarafından döndürülen yük ile aynıdır:
>   - algılayıcı için `/sensors/{id}?includes=properties,types`.
>   - algılayıcıyı üst cihaz için `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes`.
>   - sensör üst alanı için `/spaces/{id}?includes=properties,types,location,timezone`.
> - Karşılaştırmalar büyük/küçük harfe duyarlıdır.

### <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Kullanıcı tanımlı bir işlev, yalıtılmış bir Azure dijital TWINS ortamı içinde yürütülen özel bir işlevdir. Kullanıcı tanımlı işlevlerin, alındığı için ham algılayıcı telemetri iletisine erişimi vardır. Kullanıcı tanımlı işlevlerin de uzamsal grafik ve dağıtıcı hizmetine erişimi vardır. Kullanıcı tanımlı işlev bir grafik içinde kaydedildikten sonra, işlevin ne zaman yürütüleceğini belirtmek için bir eşleştirici ( [yukarıya](#matchers)ayrıntılı) oluşturulmalıdır. Örneğin, Azure dijital TWINS, belirli bir sensörden yeni telemetri aldığında, eşleşen kullanıcı tanımlı işlev, son birkaç algılayıcı okumasının hareketli ortalamasını hesaplayabilir.

Kullanıcı tanımlı işlevler JavaScript 'te yazılabilir. Yardımcı yöntemler Kullanıcı tanımlı yürütme ortamında grafikle etkileşime geçin. Geliştiriciler, algılayıcı telemetri iletilerine karşı özel kod parçacıkları yürütebilir. Örnekler:

- Algılayıcısı grafik içindeki algılayıcı nesnesine doğrudan okumayı ayarlayın.
- Grafikteki bir boşluk içindeki farklı algılayıcı okumaları temelinde bir eylem gerçekleştirir.
- Gelen algılayıcı okuma için belirli koşullar karşılandığında bir bildirim oluşturun.
- Bir bildirim göndermeden önce algılayıcı okumaya grafik meta verileri ekleyin.

Daha fazla bilgi için bkz. [Kullanıcı tanımlı işlevleri kullanma](./how-to-user-defined-functions.md).

#### <a name="examples"></a>Örnekler

[Dijital TWINS C# örneği için GitHub deposu](https://github.com/Azure-Samples/digital-twins-samples-csharp/) , Kullanıcı tanımlı işlevlere birkaç örnek içerir:
- [Bu işlev](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) , Aralık içinde bu değerlerle kullanılabilir bir oda olup olmadığını belirlemede karbon dioksit, hareket ve sıcaklık değerlerini arar. [Dijital TWINS öğreticileri](tutorial-facilities-udf.md) , daha ayrıntılı bilgi için bu işlevi keşfedebilir. 
- [Bu işlev](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/multiplemotionsensors.js) , birden çok hareket sensörlerinden verileri arar ve hiçbir türlü hareketi algılamazsa alanın kullanılabilir olduğunu belirler. Dosyanın açıklamalar bölümünde bahsedilen değişiklikleri yaparak [hızlı](quickstart-view-occupancy-dotnet.md)başlangıçta veya [öğreticilerde](tutorial-facilities-setup.md)kullanılan Kullanıcı tanımlı işlevi kolayca değiştirebilirsiniz. 

### <a name="role-assignment"></a>Rol ataması

Kullanıcı tanımlı bir işlevin eylemleri, hizmet içindeki verilerin güvenliğini sağlamak için Azure dijital TWINS [rol tabanlı erişim denetimine](./security-role-based-access-control.md) tabidir. Rol atamaları, uzamsal grafikle ve varlıklarıyla etkileşimde bulunmak için hangi kullanıcı tanımlı işlevlerin uygun izinlere sahip olduğunu tanımlar. Örneğin, Kullanıcı tanımlı bir işlev, belirli bir alanda grafik verileri *oluşturma*, *okuma*, *güncelleştirme*veya *silme* özelliğine sahip olabilir. Kullanıcı tanımlı işlev grafiğe veri istediğinde veya bir eylemi denediğinde Kullanıcı tanımlı bir işlevin erişim düzeyi denetlenir. Daha fazla bilgi için bkz. [rol tabanlı erişim denetimi](./security-create-manage-role-assignments.md).

Bir Eşleştirici, rol ataması olmayan kullanıcı tanımlı bir işlevin tetiklenmesi mümkündür. Bu durumda, Kullanıcı tanımlı işlev grafikten veri okuyamazsa.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer Azure hizmetlerine olay ve telemetri iletileri yönlendirme hakkında daha fazla bilgi edinmek için, [yönlendirme olaylarını ve iletilerini](./concepts-events-routing.md)okuyun.

- Eşleştiriciler, Kullanıcı tanımlı işlevler ve rol atamaları oluşturma hakkında daha fazla bilgi edinmek için [Kullanıcı tanımlı işlevleri kullanma kılavuzu](./how-to-user-defined-functions.md)' nu okuyun.

- [Kullanıcı tanımlı işlev istemci kitaplığı başvuru belgelerini](./reference-user-defined-functions-client-library.md)gözden geçirin.
