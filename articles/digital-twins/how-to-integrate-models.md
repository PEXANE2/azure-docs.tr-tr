---
title: Sektör standardı modelleri tümleştirme
titleSuffix: Azure Digital Twins
description: Sektör standardı modellerini Azure dijital TWINS için özel DTDL ontololomi kullanarak ya da mevcut ontoloor 'leri dönüştürerek DTDL ile nasıl tümleştirileceğini anlayın
author: baanders
ms.author: baanders
ms.date: 11/04/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 3376f5d5e207a33ad39cd7506998e2ee90e084ad
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98051556"
---
# <a name="integrate-industry-standard-models-with-dtdl-for-azure-digital-twins"></a>Azure dijital TWINS için, sektör standardı modellerini DTDL ile tümleştirme

Endüstri standartlarına dayalı modelleri kullanma veya RDF veya OWL gibi standart ontology temsili kullanma, Azure dijital TWINS modellerinizi tasarlarken zengin bir başlangıç noktası sağlar. Endüstri modellerini kullanmak, standartlaştırma ve bilgi paylaşımıyla de yardımcı olur.

Azure dijital TWINS ile kullanılmak üzere, JSON-LD tabanlı [**dijital TWINS tanım dili (DTDL)**](concepts-models.md)içinde bir model temsil etmelidir. Bu nedenle, bu makalede, Azure Digital TWINS 'in kullanabilmesi için, mevcut sektör kavramlarını dtdl semantiğinin tümleştirilmesine yönelik DTDL 'de sektör standardı modellerinizi nasıl temsil ettiğini açıklanmaktadır. DTDL modeli daha sonra Azure dijital TWINS içindeki modelin gerçeği kaynağı olarak görev yapar.

Sektör standardı modellerini DTDL ile tümleştirmenin üç olası yolu vardır:
* **Benimseme**: çözümünüzü, yaygın olarak benimsenen endüstri standartlarına göre geliştirilen açık kaynaklı bir dtdl ontology ile başlatabilirsiniz. 
* **Dönüştür**: zaten mevcut modelleriniz varsa bunları dtdl 'e dönüştürmeniz gerekir.
* **Yazar**: [*nasıl yapılır: özel modelleri yönetme*](how-to-manage-model.md)bölümünde açıklandığı gıbı, kendi özel dtdl modellerinizi her zaman sıfırdan geliştirebilirsiniz.

## <a name="adopt-an-open-source-dtdl-ontology"></a>Açık kaynaklı bir DTDL ontology benimseyin

Boş bir sayfadan başlayarak açık kaynaklı bir DTDL ontology ile başlamak genellikle daha kolay. 

Örneğin, akıllı binalar, akıllı binaların modellenmesi için ortak zemin sağlayan açık kaynaklı [**Dtdl tabanlı Reatastatecore ontology**](https://github.com/Azure/opendigitaltwins-building)'ten yararlanabilir. 

Bu açık kaynaklı DTDL ontoloyüklenimi, modellerin nasıl kullanılacağı ve düzgün bir şekilde genişletileceği için en iyi yöntemleri de sağlar. 

## <a name="convert-existing-models-to-dtdl"></a>Mevcut modelleri DTDL 'ye Dönüştür

Çoğu sektör modeli ( **ontolotik** olarak da bilinir), [Owl](https://www.w3.org/OWL/), [RDF](https://www.w3.org/2001/sw/wiki/RDF)ve [RDFS](https://www.w3.org/2001/sw/wiki/RDFS)gibi Anlamsal Web standartlarını temel alır. 

Azure dijital TWINS ile bir model kullanmak için, DTDL biçiminde olmalıdır. Bu bölümde, Azure dijital TWINS ile kullanılabilmesi için RDF tabanlı modelleri DTDL 'ye dönüştürmek üzere bir **dönüştürme modeli** biçiminde genel tasarım kılavuzu açıklanmaktadır. 

Ayrıca, [BIR RDF Dönüştürücüsü için, blok şeması için doğrulanan ve yapı sektöründe diğer şemalar için Genişletilebilir **örnek dönüştürücü kodu**](#sample-converter-application)içerir. [](https://brickschema.org/ontology/)

### <a name="conversion-pattern"></a>Dönüştürme deseninin

RDF tabanlı modelleri DTDL 'ye dönüştürürken kullanılabilecek birkaç üçüncü taraf kitaplığı vardır. Bu kitaplıkların bazıları, model dosyanızı bir grafiğe eklemenize olanak tanır. Belirli RDFS ve OWL yapılarını bulmak ve bunları DTDL 'e dönüştürmek için grafik boyunca döngü yapabilirsiniz.   

Aşağıdaki tabloda, RDFS ve OWL yapılarının DTDL ile nasıl eşleştirileneceğini gösteren bir örnek verilmiştir. 

| RDFS/OWL kavram | RDFS/OWL yapısı | DTDL kavram | DTDL yapısı |
| --- | --- | --- | --- |
| Sınıflar | `owl:Class`<br>IRI soneki<br>``rdfs:label``<br>``rdfs:comment`` | Arabirim | `@type:Interface`<br>`@id`<br>`displayName`<br>`comment` 
| Alt sınıflar | `owl:Class`<br>IRI soneki<br>`rdfs:label`<br>`rdfs:comment`<br>`rdfs:subClassOf` | Arabirim | `@type:Interface`<br>`@id`<br>`displayName`<br>`comment`<br>`extends` 
| DataType özellikleri | `owl:DatatypeProperty`<br>`rdfs:label` veya `INode`<br>`rdfs:label`<br>`rdfs:range` | Arabirim Özellikleri | `@type:Property`<br>`name`<br>`displayName`<br>`schema` 
| Nesne özellikleri | `owl:ObjectProperty`<br>`rdfs:label` veya `INode`<br>`rdfs:range`<br>`rdfs:comment`<br>`rdfs:label` | İlişki | `type:Relationship`<br>`name`<br>`target` (veya yoksa atlandı `rdfs:range` )<br>`comment`<br>`displayName`<br>

Aşağıdaki C# kod parçacığı, bir RDF model dosyasının bir grafiğe nasıl yüklendiğini ve [**Dotnetrdf**](https://www.dotnetrdf.org/) kitaplığı kullanılarak dtdl 'ye nasıl dönüştürüleceğini gösterir. 

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/convertRDF.cs":::

### <a name="sample-converter-application"></a>Örnek dönüştürücü uygulaması 

Azure dijital TWINS hizmeti tarafından kullanılmak üzere RDF tabanlı model dosyasını [Dtdl 'ye (sürüm 2)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) dönüştüren örnek bir uygulama mevcuttur. Bu, [blok](https://brickschema.org/ontology/) şeması için onaylanmış ve yapı sektöründe ( [TOPOLOJI ontology (bot)](https://w3c-lbd-cg.github.io/bot/), [semantik algılayıcı ağı](https://www.w3.org/TR/vocab-ssn/)veya [BUILDINGSMART sektör temel sınıfları (ifc)](https://technical.buildingsmart.org/standards/ifc/ifc-schema-specifications/)gibi diğer şemalar için genişletilebilir.

Örnek, **Rdftodtdlconverter** adlı bir .NET Core komut satırı uygulamasıdır.

Örneği buradan edinebilirsiniz: [**Rdftodtdlconverter**](/samples/azure-samples/rdftodtdlconverter/digital-twins-model-conversion-samples/). 

Kodu makinenize indirmek için, örnek giriş sayfasındaki başlığın altındaki *ZIP 'ı indir* düğmesine basın. Bu işlem, daha sonra unzip ve keşfedebileceğiniz *RdfToDtdlConverter_sample_application_to_convert_RDF_to_DTDL.zip* adı altında bir *ZIP* dosyası indirir.

Bu örneği, dönüştürme düzenlerini bağlam içinde görmek ve kendi uygulamalarınız için kendi özel gereksinimlerinize göre model dönüştürmeleri gerçekleştiren bir yapı taşı olarak kullanmak için kullanabilirsiniz.

## <a name="validate-and-upload-dtdl-models"></a>DTDL modellerini doğrulama ve karşıya yükleme

[!INCLUDE [Azure Digital Twins: validate models info](../../includes/digital-twins-validate.md)]

Bir model dönüştürüldükten ve doğrulandıktan sonra **Azure dijital TWINS örneğinizi yükleyebilirsiniz**. Bu işlemle ilgili daha fazla bilgi için bkz. *nasıl yapılır: özel modelleri yönetme* konusunun [*modeller karşıya yükleme*](how-to-manage-model.md#upload-models) bölümü.

## <a name="next-steps"></a>Sonraki adımlar 

Dijital ikizi modellerini tasarlama ve yönetme hakkında daha fazla bilgi edinin:
 
* [*Kavramlar: özel modeller*](concepts-models.md)
* [*Nasıl yapılır: özel modelleri yönetme*](how-to-manage-model.md)
* [*Nasıl yapılır: modelleri ayrıştırma ve doğrulama*](how-to-parse-models.md)