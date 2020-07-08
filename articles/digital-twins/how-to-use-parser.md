---
title: Modelleri ayrıştırma ve doğrulama
titleSuffix: Azure Digital Twins
description: DTDL modellerini ayrıştırmak için ayrıştırıcı kitaplığını nasıl kullanacağınızı öğrenin.
author: cschormann
ms.author: cschorm
ms.date: 4/10/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: b6d6e16b079f1423fd1ea812e384546ae5d84067
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85392208"
---
# <a name="dtdl-client-side-parser-library"></a>DTDL istemci tarafı ayrıştırıcı kitaplığı

Azure dijital TWINS 'deki [modeller](concepts-models.md) JSON-ld tabanlı dijital TWINS tanım DILI (dtdl) kullanılarak tanımlanır. Modellerinizi ayrıştırmak için yararlı olduğu durumlarda, NuGet.org 'de bir DTDL ayrıştırma kitaplığı, bir istemci tarafı kitaplığı olarak sağlanır: [Microsoft. Azure. DigitalTwins. Parser](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/).

Bu kitaplık, temel olarak DTDL için C# Reflection 'ın eşdeğeri olarak davranan DTDL tanımlarına model erişimi sağlar. Bu kitaplık, özellikle bir görsel veya metin düzenleyicisinde DTDL doğrulaması için herhangi bir [Azure dijital TWINS SDK 'sının](how-to-use-apis-sdks.md)bağımsız olarak kullanılabilir. Bunları hizmete yüklemeye çalışmadan önce model tanım dosyalarınızın geçerli olduğundan emin olmak için yararlıdır.

Ayrıştırıcı kitaplığını kullanmak için, bunu bir DTDL belgeleri kümesiyle birlikte sağlarsınız. Genellikle, bu model belgelerini hizmetten alırsınız, ancak istemciniz ilk yerde hizmete yüklenmeden sorumlu ise yerel olarak da kullanılabilir. 

Ayrıştırıcının kullanımı için genel iş akışı aşağıda verilmiştir:
1. Hizmetten bazı veya tüm DTDL belgelerini alın.
2. Döndürülen bellek içi DTDL belgelerini ayrıştırıcıya geçirin.
3. Ayrıştırıcı, kendisine geçirilen belge kümesini doğrular ve ayrıntılı hata bilgileri döndürür. Bu özellik, düzenleyici senaryolarında yararlı olur.
4. Belge kümesine dahil edilen modelleri çözümlemeye devam etmek için Ayrıştırıcı API 'Lerini kullanın. 

Ayrıştırıcısının özellikleri şunlardır:
* Uygulanan tüm model arabirimlerini al (arabirimin `extends` bölümünün içeriği).
* Modelde belirtilen tüm özellikleri, telemetri, komutları, bileşenleri ve ilişkileri alın. Bu komut ayrıca bu tanımlarda bulunan tüm meta verileri alır ve Devralınanlar ( `extends` bölümler) alır.
* Tüm karmaşık model tanımlarını alın.
* Bir modelin başka bir modelden atanabilir olup olmadığını belirleme.

> [!NOTE]
> [Iot Tak ve kullan (PnP)](../iot-pnp/overview-iot-plug-and-play.md) cihazları işlevlerini anlatmak için küçük bir sözdizimi değişkeni kullanır. Bu söz dizimi değişkeni, Azure dijital TWINS 'te kullanılan DTDL 'nin anlam ile uyumlu bir alt kümesidir. Ayrıştırıcı kitaplığı kullanılırken, dijital ikizi için DTDL oluşturmak üzere hangi sözdizimi çeşidinin kullanıldığını bilmeniz gerekmez. Ayrıştırıcı her zaman varsayılan olarak, hem PnP hem de Azure Digital TWINS sözdizimi için aynı modeli döndürür.

## <a name="use-the-dtdl-validator-sample"></a>DTDL doğrulayıcısı örneğini kullanma

DTDL 'nin geçerli olduğundan emin olmak için model belgelerini doğrulayabilecek örnek kod mevcuttur. DTDL ayrıştırıcısı kitaplığı üzerine kurulmuştur ve dilden bağımsızdır. Buradan bulabilirsiniz: [Dtdl Doğrulayıcı örneği](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator).

Doğrulayıcı örneği, bir DTDL dosyaları dizin ağacını doğrulamak için bir komut satırı yardımcı programı olarak kullanılabilir. Ayrıca etkileşimli bir mod sağlar. Kaynak kodda, ayrıştırıcı kitaplığının nasıl kullanılacağına ilişkin örnekler gösterilmektedir.

DTDL doğrulayıcısı örneğinin klasöründe, örneği otomatik olarak içerilen bir yürütülebilire paketleme hakkında yönergeler için *README.MD* dosyasına bakın.

Kendi kendine içerilen bir paket oluşturup, bu dosyayı yolunuza ekledikten sonra, bu komutla olan doğrulayıcısı makinenizde bir konsolda çalıştırabilirsiniz:

```cmd/sh
DTDLValidator
```

Varsayılan seçeneklerle örnek, `*.json` geçerli dizinde ve tüm alt dizinlerde bulunan dosyaları arar. Ayrıca, belirtilen dizinde ve tüm alt dizinlerde *. dtdl*uzantılı örnek aramaya sahip olmak için aşağıdaki seçeneği de ekleyebilirsiniz:

```cmd/sh
DTDLValidator -d C:\Work\DTDL -e dtdl 
```

`-i`Etkileşimli moda girmek için örneğin seçeneğini ekleyebilirsiniz:

```cmd/sh
DTDLValidator -i
```

Bu örnek hakkında daha fazla bilgi için bkz. kaynak kodu veya çalıştırma `DTDLValidator --help` .

## <a name="use-the-parser-library-in-code"></a>Koddaki ayrıştırıcı kitaplığını kullan

Ayrıca, kendi uygulamanızdaki modelleri doğrulama veya dinamik, model temelli UI, pano ve rapor oluşturma gibi şeyler için doğrudan ayrıştırıcı kitaplığını kullanabilirsiniz.

Aşağıdaki ayrıştırıcı kodu örneğini desteklemek için, bir Azure dijital TWINS örneğinde tanımlanan çeşitli modelleri göz önünde bulundurun:

> [!TIP] 
> `dtmi:com:contoso:coffeeMaker`Model, bu modeli kullanıma sunan bir PnP cihazını bağlayarak hizmete yüklendiğini belirten *yetenek modeli* sözdizimini kullanıyor.

```json
{
  "@id": " dtmi:com:contoso:coffeeMaker",
  "@type": "CapabilityModel",
  "implements": [
        { "name": "coffeeMaker", "schema": " dtmi:com:contoso:coffeeMakerInterface" }
  ]    
}
{
  "@id": " dtmi:com:contoso:coffeeMakerInterface",
  "@type": "Interface",
  "contents": [
      { "@type": "Property", "name": "waterTemp", "schema": "double" }  
  ]
}
{
  "@id": " dtmi:com:contoso:coffeeBar",
  "@type": "Interface",
  "contents": [
        { "@type": "relationship", "contains": " dtmi:com:contoso:coffeeMaker" },
        { "@type": "property", "name": "capacity", "schema": "integer" }
  ]    
}
```

Aşağıdaki kod, C# ' de bu tanımları yansıtmak için ayrıştırıcı kitaplığının nasıl kullanılacağına ilişkin bir örnek gösterir:

```csharp
async void ParseDemo(DigitalTwinsClient client)
{
    try
    {
        AsyncPageable<ModelData> mdata = client.GetModelsAsync(null, true);
        List<string> models = new List<string>();
        await foreach (ModelData md in mdata)
            models.Add(md.Model);
        ModelParser parser = new ModelParser();
        IReadOnlyDictionary<Dtmi, DTEntityInfo> dtdlOM = await parser.ParseAsync(models);

        List<DTInterfaceInfo> interfaces = new List<DTInterfaceInfo>();
        IEnumerable<DTInterfaceInfo> ifenum = 
            from entity in dtdlOM.Values
            where entity.EntityKind == DTEntityKind.Interface
            select entity as DTInterfaceInfo;
        interfaces.AddRange(ifenum);
        foreach (DTInterfaceInfo dtif in interfaces)
        {
            PrintInterfaceContent(dtif, dtdlOM);
        }

    } catch (RequestFailedException rex)
    {

    }
}

void PrintInterfaceContent(DTInterfaceInfo dtif, IReadOnlyDictionary<Dtmi, DTEntityInfo> dtdlOM, int indent=0)
{
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < indent; i++) sb.Append("  ");
    Console.WriteLine($"{sb}Interface: {dtif.Id} | {dtif.DisplayName}");
    SortedDictionary<string, DTContentInfo> contents = dtif.Contents;
    foreach (DTContentInfo item in contents.Values)
    {
        switch (item.EntityKind)
        {
            case DTEntityKind.Property:
                DTPropertyInfo pi = item as DTPropertyInfo;
                Console.WriteLine($"{sb}--Property: {pi.Name} with schema {pi.Schema}");
                break;
            case DTEntityKind.Relationship:
                DTRelationshipInfo ri = item as DTRelationshipInfo;
                Console.WriteLine($"{sb}--Relationship: {ri.Name} with target {ri.Target}");
                break;
            case DTEntityKind.Telemetry:
                DTTelemetryInfo ti = item as DTTelemetryInfo;
                Console.WriteLine($"{sb}--Telemetry: {ti.Name} with schema {ti.Schema}");
                break;
            case DTEntityKind.Component:
                DTComponentInfo ci = item as DTComponentInfo;
                Console.WriteLine($"{sb}--Component: {ci.Id} | {ci.Name}");
                dtdlOM.TryGetValue(ci.Id, out DTEntityInfo value);
                DTInterfaceInfo component = value as DTInterfaceInfo;
                PrintInterfaceContent(component, dtdlOM, indent + 1);
                break;
            default:
                break;
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Modellerinizi yazmayı tamamladıktan sonra, Digitaltwınsmodel API 'Leri ile bunları karşıya yükleme (ve diğer yönetim işlemlerini yapma) konusuna bakın:
* [Nasıl yapılır: özel modelleri yönetme](how-to-manage-model.md)