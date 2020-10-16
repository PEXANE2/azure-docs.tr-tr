---
title: Sektör standardı modellerini dönüştürme
titleSuffix: Azure Digital Twins
description: Sektör standardı (RDF/OWL) modellerini DTDL 'ye dönüştürme düzenlerini anlayın
author: baanders
ms.author: baanders
ms.date: 9/28/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: bf39bd0c787ed64d573cc2a257442b4d3bb8ab7d
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92015123"
---
# <a name="convert-industry-standard-models-to-dtdl-for-azure-digital-twins"></a>Azure dijital TWINS için sektör standardı modellerini DTDL 'e dönüştürme

Azure dijital TWINS 'deki modeller, JSON-LD tabanlı [**dijital TWINS tanım dili (DTDL)**](concepts-models.md)içinde temsil edilir. RDF veya OWL gibi bir sektör standardını temel alan Azure dijital TWINS 'in dışında mevcut modelleriniz varsa, bunları Azure dijital TWINS ile kullanmak için **DTDL 'ye dönüştürmeniz** gerekir. DTDL sürümü daha sonra Azure Digital TWINS içindeki model için Truth kaynağı olacaktır.

Bu makalede, RDF tabanlı sektörün veya özel modellerin DTDL 'ye dönüştürülmesi için bir model açıklanmaktadır. Şunları içerir:
* Çeşitli standartlara ve model türlerine uygulanabilecek **strateji düzeyinde rehberlik**
* [RDF 'e özgü dönüştürücünün **örnek kodu**](#sample-converter-application)

## <a name="industry-models"></a>Sektör modelleri  

Endüstri modellerini kullanmak, Azure dijital TWINS modelinizi tasarlarken zengin bir başlangıç noktası sağlar. Endüstri modellerini kullanmak, standartlaştırma ve bilgi paylaşımıyla de yardımcı olur. 

Bazı yaygın sektör modelleri şunları içerir:  

| Sektör dikey | Model |
| --- | --- | 
| Oluşturma/tesis yönetimi | [Reatastatecore](https://www.realestatecore.io/)<br>[BLOK şeması](https://brickschema.org/ontology/1.1/)<br>[Topoloji ontology oluşturma (BOT)](https://w3c-lbd-cg.github.io/bot/)<br>[Anlamsal algılayıcı ağı](https://www.w3.org/TR/vocab-ssn/)<br>[buildingSmart sektör temel sınıfları (IFC)](https://technical.buildingsmart.org/standards/ifc/ifc-schema-specifications/) |
| Akıllı Şehirler | [ETSı NGSı-LD](https://www.etsi.org/deliver/etsi_gr/CIM/001_099/008/01.01.01_60/gr_CIM008v010101p.pdf)<br>[Akıllı uygulamalar başvurusu (SAREF)](https://saref.etsi.org/)<br>[Fiware](https://www.fiware.org/)<br>[Açık & çevik Akıllı şehirler (OASC)](https://oascities.org/) |
| Enerji Kılavuzu | [CIM](https://cimug.ucaiug.org/) / [Iec 61968](https://en.wikipedia.org/wiki/IEC_61968)<br>Enerji ve emtia ticareti için [Adrm modelleri](http://www.adrm.com/) | 

Gereksinimlerinize bağlı olarak, aynı zamanda, sektör modellerini özelleştirmek veya genişletmek ya da sıfırdan kendi özel modelinizi geliştirmek için DTDL 'yi de kullanabilirsiniz. 

## <a name="create-and-edit-models"></a>Model oluşturma ve düzenleme

Modellemenin ilk adımı, modellerinizi oluşturmaktır. Onları DTDL 'e dönüştürmeden önce sektör standardı modellerinizi oluşturabilir ve düzenleyebilirsiniz veya bunları DTDL erken ' a dönüştürebilir ve bunları DTDL belgeleri olarak düzenleyebilirsiniz.

### <a name="with-industry-standards"></a>Endüstri standartları ile

Çoğu sektör modeli ( **ontolotik**olarak da bilinir), [Owl](https://www.w3.org/OWL/[), [RDF](https://www.w3.org/2001/sw/wiki/RDF)ve [RDFS](https://www.w3.org/2001/sw/wiki/RDFS)gibi Anlamsal Web standartlarını temel alır. 

Bazı durumlarda, OWL tabanlı model araçları 'nı kullanarak bir model oluşturmak veya düzenlemek isteyebilirsiniz. Aşağıdaki gibi bir OWL tabanlı model tasarlamak için herhangi bir sayıda model yazma aracı kullanabilirsiniz.
* [Webprotégé](https://protege.stanford.edu/products.php#web-protege) ve [Protégé Desktop](https://protege.stanford.edu/products.php#desktop-protege) popüler örneklerdir. Endüstri modellerini birden çok biçimde içeri aktarabilir, bir modeli düzenleyebilir veya genişletebilir ve modeli dışarı aktarabilirsiniz. 
* [Webvowl](http://www.visualdataweb.de/webvowl/) , modelleri görselleştirmeye yönelik başka bir popüler araçtır. 

DTDL olmayan bir sektör standardı kullanarak bir model oluşturursanız, bunu DTDL 'e dönüştürmeniz ve Azure dijital TWINS 'e yüklemeniz gerekir. 

#### <a name="common-model-file-formats"></a>Ortak model dosya biçimleri 

RDF, OWL ve RDFS, Anlamsal Web 'in temel yapı taşlarıdır. 

**RDF** , **Üçlü**biçiminde şeyleri açıklamak için kavramsal bir yapı sağlar. Üçlü üç bölümden oluşur: **Konu**, **koşul**ve **nesne**. Nesneler, URI 'lerden yapılabilir. 

Burada RDF Üçlü örnek verilmiştir:

```
<LogicalDevice> <hasCapabiity> <Temperature>
<Chiller> <locatedIn> <Level1>
<Asset> <isPartOf> <Asset>  
```

Bu örneklerin hepsi geçerli RDF 'dir, ancak son ifade anlam olarak geçersiz olur. Bu durum OWL belirtiminin resmi girdiği yerdir. **Owl** , geçerli ontology sağlamak için RDF ile yazabildiklerinizi tanımlar.

OWL 'nin kullanımıyla bir örnek aşağıda verilmiştir: 

```
<Asset> <isPartOf> <Building>
<TemperatureSensor> <isType> <Sensor>
```

**RDFS** , sınıfları tanımlamanıza ve açıklamanıza yardımcı olan ek sözlük semantiği sağlar. Örneğin, bu tür bir sınıf `rdfs:subClassOf` :

```
<Equipment> <subClassOf> <Asset>
```

Modeller, aşağıdakiler dahil olmak üzere birçok dosya biçiminde kaydedilebilir, içeri aktarılabilir ve aktarılabilir:  
* RDF/XML 
* Kaplumbağa (TTL) 
* OWL/XML 

### <a name="with-dtdl"></a>DTDL ile

Azure dijital TWINS, modelleme için JSON-LD tabanlı **dijital Ikizi tanım dili (DTDL)** kullanır. Azure Digital TWINS ile kullanılacak herhangi bir modelin, özgün olarak veya DTDL 'ye dönüştürülmesi gerekir.

DTDL 'deki modellerle çalışırken, [**DTDL extension**](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl)   dtdl modellerini yazmayı kolaylaştırmak için sözdizimi doğrulaması ve diğer özellikleri sağlayan [Visual Studio Code](https://code.visualstudio.com/)için kullanılabilir dtdl uzantısını kullanabilirsiniz.

Azure dijital TWINS modelleri ve bunların bileşenleri hakkında daha fazla bilgi edinmek için [*bkz. özel modeller*](concepts-models.md) ve [*nasıl yapılır: özel modelleri yönetme*](how-to-manage-model.md).

## <a name="convert-models-to-dtdl"></a>Modelleri DTDL 'ye Dönüştür

Azure dijital TWINS ile bir model kullanmak için, DTDL biçiminde olmalıdır. Bu bölümde, Azure dijital TWINS ile kullanılabilmesi için RDF tabanlı modellerin DTDL 'ye nasıl dönüştürüleceği ele alınmaktadır.

RDF tabanlı modelleri DTDL 'ye dönüştürürken kullanılabilecek birkaç üçüncü taraf kitaplığı vardır. Bu kitaplıkların bazıları, model dosyanızı bir grafiğe eklemenize olanak tanır. Belirli RDFS ve OWL yapılarını bulmak ve bunları DTDL 'e dönüştürmek için grafik boyunca döngü yapabilirsiniz.   

Aşağıdaki tabloda, RDFS ve OWL yapılarının DTDL ile nasıl eşleştirileneceğini gösteren bir örnek verilmiştir. 

| RDFS/OWL kavram | RDFS/OWL yapısı | DTDL kavram | DTDL yapısı |
| --- | --- | --- | --- |
| Sınıflar | `owl:Class`<br>IRI soneki<br>``rdfs:label``<br>``rdfs:comment`` | Arabirim | `@type:Interface`<br>`@id`<br>`displayName`<br>`comment` 
| Alt sınıflar | `owl:Class`<br>IRI soneki<br>`rdfs:label`<br>`rdfs:comment`<br>`rdfs:subClassOf` | Arabirim | `@type:Interface`<br>`@id`<br>`displayName`<br>`comment`<br>`extends` 
| DataType özellikleri | `owl:DatatypeProperty`<br>`rdfs:label` veya `INode`<br>`rdfs:label`<br>`rdfs:range` | Arabirim Özellikleri | `@type:Property`<br>`name`<br>`displayName`<br>`schema` 
| Nesne özellikleri | `owl:ObjectProperty`<br>`rdfs:label` veya `INode`<br>`rdfs:range`<br>`rdfs:comment`<br>`rdfs:label` | İlişki | `type:Relationship`<br>`name`<br>`target` (veya yoksa atlandı `rdfs:range` )<br>`comment`<br>`displayName`<br>

Aşağıdaki C# kod parçacığı, bir RDF model dosyasının bir grafiğe nasıl yüklendiğini ve [**Dotnetrdf**](https://www.dotnetrdf.org/) kitaplığı kullanılarak dtdl 'ye nasıl dönüştürüleceğini gösterir. 

```csharp
using VDS.RDF.Ontology; 
using VDS.RDF.Parsing; 
using Microsoft.Azure.DigitalTwins.Parser; 

//...

Console.WriteLine("Reading file..."); 

FileLoader.Load(_ontologyGraph, rdfFile.FullName); 

// Start looping through for each owl:Class 
foreach (OntologyClass owlClass in _ontologyGraph.OwlClasses) 
{ 

    // Generate a DTMI for the owl:Class 
    string Id = GenerateDTMI(owlClass); 

    if (!String.IsNullOrEmpty(Id)) 
    { 

        Console.WriteLine($"{owlClass.ToString()} -> {Id}"); 

        // Create Interface
        DtdlInterface dtdlInterface = new DtdlInterface 
        { 
            Id = Id, 
            Type = "Interface", 
            DisplayName = GetInterfaceDisplayName(owlClass), 
            Comment = GetInterfaceComment(owlClass), 
            Contents = new List<DtdlContents>() 
        }; 

        // Use DTDL 'extends' for super classes 
        IEnumerable<OntologyClass> foundSuperClasses = owlClass.DirectSuperClasses; 

        //... 
     }

     // Add interface to the list of interfaces 
     _interfaceList.Add(dtdlInterface); 
} 

// Serialize to JSON 
var json = JsonConvert.SerializeObject(_interfaceList); 

//...
``` 

## <a name="validate-and-upload-dtdl-models"></a>DTDL modellerini doğrulama ve karşıya yükleme

[!INCLUDE [Azure Digital Twins: validate models info](../../includes/digital-twins-validate.md)]

Bir model dönüştürüldükten ve doğrulandıktan sonra **Azure dijital TWINS örneğinizi yükleyebilirsiniz**. Bu işlemle ilgili daha fazla bilgi için bkz. *nasıl yapılır: özel modelleri yönetme*konusunun [*modeller karşıya yükleme*](how-to-manage-model.md#upload-models) bölümü.

## <a name="sample-converter-application"></a>Örnek dönüştürücü uygulaması 

Azure dijital TWINS hizmeti tarafından kullanılmak üzere RDF tabanlı model dosyasını [Dtdl 'ye (sürüm 2)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) dönüştüren örnek bir uygulama mevcuttur. Örnek, **Rdftodtdlconverter**adlı bir .NET Core komut satırı uygulamasıdır.

Örneği buradan edinebilirsiniz: [**Rdftodtdlconverter**](https://docs.microsoft.com/samples/azure-samples/rdftodtdlconverter/digital-twins-model-conversion-samples/). 

Kodu makinenize indirmek için, örnek giriş sayfasındaki başlığın altındaki *ZIP 'ı indir* düğmesine basın. Bu işlem, daha sonra unzip ve keşfedebileceğiniz *RdfToDtdlConverter_sample_application_to_convert_RDF_to_DTDL.zip*adı altında bir *ZIP* dosyası indirir.

Bu örneği, dönüştürme düzenlerini bağlam içinde görmek ve kendi uygulamalarınız için kendi özel gereksinimlerinize göre model dönüştürmeleri gerçekleştiren bir yapı taşı olarak kullanmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar 

Dijital ikizi modellerini tasarlama ve yönetme hakkında daha fazla bilgi edinin:
 
* [*Kavramlar: özel modeller*](concepts-models.md)
* [*Nasıl yapılır: özel modelleri yönetme*](how-to-manage-model.md)
* [*Nasıl yapılır: modelleri ayrıştırma ve doğrulama*](how-to-parse-models.md)