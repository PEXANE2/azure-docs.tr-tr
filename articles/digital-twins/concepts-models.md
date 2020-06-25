---
title: Özel modeller
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS 'in ortamınızdaki varlıkları anlatmak için Kullanıcı tanımlı modelleri nasıl kullandığını anlayın.
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: conceptual
ms.service: digital-twins
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: 5aef0c9a9b0f3dbfb5ad2f4c827b1a11190e097a
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85362892"
---
# <a name="understand-twin-models-in-azure-digital-twins"></a>Azure dijital TWINS 'de ikizi modellerini anlama

[!INCLUDE [Azure Digital Twins current preview status](../../includes/digital-twins-preview-status.md)]

Azure dijital TWINS 'in temel özellikleri, kendi sözlüğünüzü tanımlayabilir ve ikizi grafınızı işletmenizin otomatik olarak tanımlanan koşullarında oluşturabilir. Bu özellik Kullanıcı tanımlı **modeller**aracılığıyla sağlanır. Modellerinizi, dünyanın bir açıklamasında adlar olarak düşünebilirsiniz. 

Model, nesne odaklı programlama dilindeki bir **sınıfa** benzer ve gerçek çalışma ortamınızdaki belirli bir kavram için veri şekli tanımlar. Modeller, adlara sahiptir ( *Oda* veya *sıcaklık algılayıcısı*gibi) ve ortamınızdaki bu varlık türünün neler yapabileceğini tanımlayan özellikler, telemetri/olaylar ve komutlar gibi öğeleri içerir. Daha sonra bu modelleri, bu tür açıklamasını karşılayan belirli varlıkları temsil eden [**dijital TWINS**](concepts-twins-graph.md) oluşturmak için kullanacaksınız.

Modeller JSON-LD tabanlı **dijital Ikizi tanım dili (DTDL)** kullanılarak yazılır.  

## <a name="digital-twin-definition-language-dtdl-for-writing-models"></a>Model yazma için dijital Ikizi tanım dili (DTDL)

Azure dijital TWINS modelleri, dijital TWINS tanım dili (DTDL) kullanılarak tanımlanmıştır. DTDL, JSON-LD ' n i n tabanlıdır ve programlama dilindeki bağımsız. DTDL, Azure dijital TWINS 'e özel değildir, ancak [ıot Tak ve kullan](../iot-pnp/overview-iot-plug-and-play.md)gibi diğer IoT hizmetlerindeki cihaz verilerini göstermek için de kullanılır. Azure dijital TWINS, DTDL *sürüm 2*kullanır.

> [!TIP] 
> DTDL kullanan hizmetlerin hepsi aynı DTDL özelliklerinin aynısını uygulamaz. Örneğin, IoT Tak ve Kullan grafik için olan DTDL özelliklerini kullanmaz, Azure dijital TWINS Şu anda DTDL komutlarını uygulamıyor. Azure dijital TWINS 'e özgü DTDL özellikleri hakkında daha fazla bilgi için [Azure Digital TWINS DTDL uygulama özellikleri](#azure-digital-twins-dtdl-implementation-specifics)' nde bu makalenin ilerleyen bölümüne bakın.

Genel olarak DTDL hakkında daha fazla bilgi için bkz. GitHub: [Digital TWINS tanım dili (DTDL)-sürüm 2](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)içindeki Özellikler belgeleri.

## <a name="elements-of-a-model"></a>Bir modelin öğeleri

Bir model tanımı içinde, üst düzey kod öğesi bir **arabirimdir**. Bu, modelin tamamını kapsüller ve modelin geri kalanı arabirim içinde tanımlanır. 

Bir DTDL model arabirimi, aşağıdaki alanlardan her birinin sıfır, bir veya bir çoğunu içerebilir:
* **Özellik** özellikleri, bir varlığın durumunu temsil eden veri alanlarıdır (birçok nesne odaklı programlama dilinde özellikler gibi). Zamana sınırlı bir veri olayı olan telemetriden farklı olarak, Özellikler depolamayı yedeklebilirler ve herhangi bir zamanda okunabilir.
* **Telemetri** -telemetri alanları ölçümleri ve olayları temsil eder ve genellikle cihaz algılayıcı ayarlarını göstermek için kullanılır. Telemetri dijital bir ikizi depolanmaz; bir yerde gönderilmek üzere bir veri olayları akışı daha gibidir. 
* **Bileşen** -bileşenler, model arabiriminizi başka arabirimlerin bir derlemesi olarak oluşturmanıza olanak tanır. Bir bileşene örnek olarak, bir *Telefon*için model tanımlarken kullanılan, önde gelen bir *Kamera* arabirimi (ve başka bir bileşen arabirimi *arkakamerası*) bulunur. İlk olarak, kendi modeli gibi *Frontcamera* için bir arabirim tanımlamanız gerekir ve ardından *Telefon*tanımlarken buna başvurabilirsiniz.

    Çözümünüzün integral bir parçası olan, ancak ayrı bir kimlik gerektirmeyen ve ikizi grafının bağımsız olarak oluşturulması, silinmesi veya yeniden düzenlenmesinin gerekli olmadığı bir şeyi betimleyen bir bileşen kullanın. Varlıkların ikizi grafiğinde bağımsız olarak var olmasını istiyorsanız, bunları farklı modellerdeki ayrı dijital ikgörüleri olarak temsil edin, *ilişkilerin* bağlanır (bkz. sonraki madde işareti).
    
    >[!TIP] 
    >Bileşenler, bir model arabirimi içindeki ilgili özelliklerin kümelerini gruplamak için de kullanılabilir. Bu durumda, her bir bileşeni arabirim içinde bir ad alanı veya "klasör" olarak düşünebilirsiniz.
* **İlişki** ilişkileri, dijital bir ikizi diğer dijital TWINS ile nasıl dahil edileceğini temsil etmenize olanak tanır. İlişkiler, *Contains* ("kat yer alan"), *cozı* ("HVAC Cozı"), *isbilledto* ("sıkıştırıcı kullanıcıya faturalandırılır") vb. gibi farklı anlam anlamlarını temsil edebilir. İlişkiler çözümün birbiriyle ilişkili varlıkların bir grafiğini sağlamasına izin verir.

> [!NOTE]
> DTDL özelliği ayrıca, dijital bir ikizi (sıfırlama komutu gibi) veya bir fanı açık veya kapalı bir komut olarak yürütülebilecek Yöntemler olan **komutları**tanımlar. Ancak, *Komutlar Şu anda Azure dijital TWINS 'te desteklenmemektedir.*

### <a name="azure-digital-twins-dtdl-implementation-specifics"></a>Azure dijital TWINS DTDL uygulama özellikleri

Bir DTDL modelinin Azure dijital TWINS ile uyumlu olması için, bu gereksinimleri karşılaması gerekir.

* Bir modeldeki tüm üst düzey DTDL öğelerinin *Interface*türünde olması gerekir. Bunun nedeni, Azure Digital TWINS model API 'Lerinin bir arabirimi ya da arabirim dizisini temsil eden JSON nesnelerini almasına yönelik olması olabilir. Sonuç olarak, en üst düzeyde başka bir DTDL öğesi türüne izin verilmez.
* Azure dijital TWINS için DTDL herhangi bir *komut*tanımlamamalıdır.
* Azure dijital TWINS yalnızca tek bir bileşen iç içe geçme düzeyine izin verir. Bu, bileşen olarak kullanılan bir arabirimin hiçbir bileşene sahip olamayacağı anlamına gelir. 
* Arabirimler, diğer DTDL arabirimleri içinde satır içi olarak tanımlanamaz; kendi kimlikleri olan ayrı en üst düzey varlıklar olarak tanımlanmalıdır. Daha sonra, başka bir arabirim bu arabirimi bir bileşen olarak veya devralma yoluyla eklemek istediğinde, KIMLIĞINE başvurabilir.

## <a name="example-model-code"></a>Örnek model kodu

İkizi tür modelleri, herhangi bir metin düzenleyicisinde yazılabilir. DTDL dili JSON söz dizimini izler, bu nedenle modelleri *. JSON*uzantısıyla depomalısınız. JSON uzantısının kullanılması, DTDL belgeleriniz için temel sözdizimi denetimi ve vurgulaması sağlamak üzere birçok programlama metin düzenleyicilerini etkinleştirir. Ayrıca, [Visual Studio Code](https://code.visualstudio.com/)Için bir [dtdl uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl) da mevcuttur.

Aşağıda, bir DTDL arabirimi olarak yazılmış tipik bir modele örnek verilmiştir. Model, her biri bir ad, yığın ve sıcaklığa sahip olan plananları açıklar. Dünya, uydu olarak Moons içerebilir ve bu da yeniler içerebilir.

```json
[
  {
    "@id": "dtmi:com:contoso:Planet;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2",
    "displayName": "Planet",
    "contents": [
      {
        "@type": "Property",
        "name": "name",
        "schema": "string"
      },
      {
        "@type": "Property",
        "name": "mass",
        "schema": "double"
      },
      {
        "@type": "Telemetry",
        "name": "Temperature",
        "schema": "double"
      },
      {
        "@type": "Relationship",
        "name": "satellites",
        "target": "dtmi:com:contoso:Moon;1"
      },
      {
        "@type": "Component",
        "name": "deepestCrater",
        "schema": "dtmi:com:contoso:Crater;1"
      }
    ]
  },
  {
    "@id": "dtmi:com:contoso:Crater;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2"
  }
]
```

Modelin alanları şunlardır:

| Alan | Description |
| --- | --- |
| `@id` | Model için bir tanımlayıcı. Biçiminde olmalıdır `dtmi:<domain>:<unique model identifier>;<model version number>` . |
| `@type` | Açıklanmakta olan bilgi türünü tanımlar. Bir arabirim için tür *arabirimdir*. |
| `@context` | JSON belgesi [bağlamını](https://niem.github.io/json/reference/json-ld/context/) ayarlar. Modeller kullanmalıdır `dtmi:dtdl:context;2` . |
| `displayName` | seçim İsterseniz modele kolay bir ad vermenizi sağlar. |
| `contents` | Kalan tüm arabirim verileri, öznitelik tanımlarının bir dizisi olarak buraya yerleştirilir. Her öznitelik `@type` , açıkladığı arabirim bilgilerinin sıralamasını belirlemek için bir (*özellik*, *telemetri*, *komut*, *ilişki*veya *bileşen*) ve ardından gerçek özniteliği tanımlayan bir özellikler kümesi (örneğin, `name` ve `schema` bir *özelliği*tanımlamak) sağlamalıdır. |

> [!NOTE]
> Bileşen*arabiriminin (Bu* örnekteki), kendisini kullanan arabirimle (*Planet*) aynı dizide tanımlandığını unutmayın. Bu şekilde, arabirimin bulunması için API çağrılarında bu şekilde tanımlanması gerekir.

### <a name="possible-schemas"></a>Olası şemalar

Dtdl başına, *özellik* ve *telemetri* özniteliklerinin şeması standart temel türler,,, `integer` `double` `string` , ve gibi `Boolean` diğer türler olabilir `DateTime` `Duration` . 

İlkel türlere ek olarak, *özellik* ve *telemetri* alanları bu karmaşık türlere sahip olabilir:
* `Object`
* `Map`
* `Enum`

*Telemetri* alanları da desteklenir `Array` .

### <a name="model-inheritance"></a>Model devralma

Bazen bir modeli daha fazla özelleştirmek isteyebilirsiniz. Örneğin, genel bir model *odası*ve özelleştirilmiş çeşitler *conferenceroom* ve *Gym*olmak yararlı olabilir. Hızlı özelleşmede DTDL devralmayı destekler: arabirimler, bir veya daha fazla arabirimden devralınabilir. 

Aşağıdaki örnek, önceki DTDL örneğinde bulunan *Planet* modelini daha büyük bir *ünalbody* modelinin alt türü olarak yeniden görüntüle. "Üst" model önce tanımlanmıştır ve ardından "alt" modeli alanını kullanarak bunu oluşturur `extends` .

```json
[
  {
    "@id": "dtmi:com:contoso:CelestialBody;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2",
    "displayName": "Celestial body",
    "contents": [
      {
        "@type": "Property",
        "name": "name",
        "schema": "string"
      },
      {
        "@type": "Property",
        "name": "mass",
        "schema": "double"
      },
      {
        "@type": "Telemetry",
        "name": "temperature",
        "schema": "double"
      }
    ]
  },
  {
    "@id": "dtmi:com:contoso:Planet;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2",
    "displayName": "Planet",
    "extends": "dtmi:com:contoso:CelestialBody;1",
    "contents": [
      {
        "@type": "Relationship",
        "name": "satellites",
        "target": "dtmi:com:contoso:Moon;1"
      },
      {
        "@type": "Component",
        "name": "deepestCrater",
        "schema": "dtmi:com:contoso:Crater;1"
      }
    ]
  },
  {
    "@id": "dtmi:com:contoso:Crater;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2"
  }
]
```

Bu örnekte, bir ad, toplu ve sıcaklığın bir adı, bir kütle ve sıcaklık katkısında *Planet*olduğunu *Ttıalgövdesi* . `extends`Bölüm bir arabirim adıdır veya arabirim adları dizisidir (uzatma arabirimine isterseniz birden çok üst modelden devralma olanağı sağlar).

Devralma uygulandıktan sonra, genişletme arabirimi tüm devralma zincirinden tüm özellikleri sunar.

Genişletme arabirimi üst arabirimlerin tanımlarından hiçbirini değiştiremez; yalnızca bunlara eklenebilir. Ayrıca, kendi üst arabirimlerinde tanımlanmış bir özelliği (özellikler aynı olarak tanımlanmış olsa bile) yeniden tanımlayamazsınız. Örneğin, bir üst arabirim bir `double` özellik *kütle*tanımlıyorsa, genişletme arabirimi de olsa bile bir *yığın*Bildirimi içeremez `double` .

## <a name="validating-models"></a>Modelleri doğrulama

DTDL 'nin geçerli olduğundan emin olmak için model belgelerini doğrulamak üzere kullanılabilecek bir örnek vardır. DTDL ayrıştırıcısı kitaplığı üzerine kurulmuştur ve dilden bağımsızdır. Buradan bulabilirsiniz: [Dtdl Doğrulayıcı örneği](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator).

Ya da, doğrudan nasıl kullanılacağına ilişkin bir örnek de dahil olmak üzere ayrıştırıcı Kitaplığı hakkında daha fazla bilgi için bkz. [nasıl yapılır: modelleri ayrıştırma ve doğrulama](how-to-use-parser.md).

## <a name="next-steps"></a>Sonraki adımlar

Bkz. Digitaltwınsmodel API 'Leri ile modelleri yönetme:
* [Nasıl yapılır: özel modelleri yönetme](how-to-manage-model.md)

Ya da dijital TWINS 'in modeller temelinde nasıl oluşturulduğuna ilişkin bilgi edinin:
* [Kavramlar: dijital TWINS ve ikizi grafiği](concepts-twins-graph.md)

