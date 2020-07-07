---
title: Eşleme veri akışlarını parametreleştirme
description: Data Factory işlem hatlarından eşleme veri akışını Parametreleştirme hakkında bilgi edinin
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 8e88e5e8a9fbe1881959c5183dc01b11ac681bdf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82780427"
---
# <a name="parameterizing-mapping-data-flows"></a>Eşleme veri akışlarını parametreleştirme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)] 

Azure Data Factory veri akışlarını eşleme, parametrelerin kullanımını destekler. Veri akışı tanımınızın içindeki parametreleri tanımlayın ve bunları deyimlerinizin tamamında kullanın. Parametre değerleri, veri akışını Yürüt etkinliği aracılığıyla çağıran işlem hattı tarafından ayarlanır. Veri akışı etkinlik ifadelerindeki değerleri ayarlamak için üç seçeneğiniz vardır:

* Ardışık düzen denetim akışı ifade dilini kullanarak dinamik bir değer ayarlayın
* Dinamik bir değer ayarlamak için veri akışı ifade dilini kullanın
* Statik değişmez değer ayarlamak için ifade dilini kullanın

Verilerinizi genel amaçlı, esnek ve yeniden kullanılabilir hale getirmek için bu özelliği kullanın. Veri akışı ayarlarını ve deyimlerini bu parametrelerle parametreleştirebilirsiniz.

## <a name="create-parameters-in-a-mapping-data-flow"></a>Eşleme veri akışında parametre oluşturma

Veri akışınıza parametreler eklemek için, genel özellikleri görmek üzere veri akışı tuvalinin boş kısmına tıklayın. Ayarlar bölmesinde, **parametre**adlı bir sekme görürsünüz. Yeni bir parametre oluşturmak için **Yeni** ' yi seçin. Her parametre için bir ad atamanız, bir tür seçmeniz ve isteğe bağlı olarak varsayılan bir değer ayarlamanız gerekir.

![Veri akışı parametreleri oluşturma](media/data-flow/create-params.png "Veri akışı parametreleri oluşturma")

## <a name="use-parameters-in-a-mapping-data-flow"></a>Eşleme veri akışında parametreleri kullanma 

Parametrelere, herhangi bir veri akışı ifadesinde başvurulabilir. Parametreler $ ile başlar ve sabittir. **Parametreler** sekmesinde, ifade oluşturucusunun içindeki kullanılabilir parametrelerin listesini bulacaksınız.

![Veri akışı parametre ifadesi](media/data-flow/parameter-expression.png "Veri akışı parametre ifadesi")

**Yeni parametre** ' i seçip adı ve türü belirterek hızlı bir şekilde ek parametreler ekleyebilirsiniz.

![Veri akışı parametre ifadesi](media/data-flow/new-parameter-expression.png "Veri akışı parametre ifadesi")

## <a name="assign-parameter-values-from-a-pipeline"></a>İşlem hattından parametre değerleri atama

Parametrelerle bir veri akışı oluşturduktan sonra, veri akışını Yürüt etkinliğinin bulunduğu bir işlem hattından çalıştırabilirsiniz. Etkinliği işlem hattı Tuvalinize ekledikten sonra etkinliğin **Parametreler** sekmesinde kullanılabilir veri akışı parametreleri görüntülenir.

Parametre değerlerini atarken, Spark türlerine göre işlem [hattı ifade dilini](control-flow-expression-language-functions.md) veya [veri akışı ifade dilini](data-flow-expression-functions.md) kullanabilirsiniz. Her eşleme veri akışı, ardışık düzen ve veri akışı ifade parametrelerinin herhangi bir birleşimini içerebilir.

![Veri akışı parametresini ayarlama](media/data-flow/parameter-assign.png "Veri akışı parametresini ayarlama")

### <a name="pipeline-expression-parameters"></a>Ardışık düzen ifade parametreleri

İşlem hattı ifade parametreleri, sistem değişkenlerine, işlevlere, işlem hattı parametrelerine ve diğer işlem hattı etkinliklerine benzer değişkenlere başvurmasına olanak tanır. **Ardışık düzen ifadesi**' ne tıkladığınızda, ifade oluşturucuyu kullanarak bir ifade girmenize izin veren bir yan gezinti açılır.

![Veri akışı parametresini ayarlama](media/data-flow/parameter-pipeline.png "Veri akışı parametresini ayarlama")

Başvurulduğunda, işlem hattı parametreleri değerlendirilir ve ardından bu değer veri akışı ifade dilinde kullanılır. Ardışık düzen ifadesi türünün veri akışı parametre türüyle eşleşmesi gerekmez. 

#### <a name="string-literals-vs-expressions"></a>Dize sabit değerleri vs ifadeleri

Dize türünde bir işlem hattı ifade parametresi atarken, varsayılan olarak tırnak işaretleri eklenir ve değer değişmez değer olarak değerlendirilir. Parametre değerini bir veri akışı ifadesi olarak okumak için, parametrenin yanındaki ifade kutusunu işaretleyin.

![Veri akışı parametresini ayarlama](media/data-flow/string-parameter.png "Veri akışı parametresini ayarlama")

Veri akışı parametresi `stringParam` değeri olan bir işlem hattı parametresine başvuruyorsa `upper(column1)` . 

- Eğer ifadesi işaretliyse, `$stringParam` Sütun1 'nin tümü büyük harf değeri olarak değerlendirilir.
- İfade işaretli değilse (varsayılan davranış), `$stringParam` olarak değerlendirilir`'upper(column1)'`

#### <a name="passing-in-timestamps"></a>Zaman damgalarına geçirme

İşlem hattı ifade dilinde, gibi sistem değişkenleri `pipeline().TriggerTime` ve `utcNow()` ' yyyy-aa-gg \' T \' HH: mm: ss biçiminde dizeler olarak dönüş zaman damgası olarak işlevler. SSSSSSZ '. Bunları zaman damgası türünde veri akışı parametrelerine dönüştürmek için, istenen zaman damgasını bir işleve dahil etmek üzere dize ilişkilendirmeyi kullanın `toTimestamp()` . Örneğin, işlem hattı tetikleme süresini bir veri akışı parametresine dönüştürmek için kullanabilirsiniz `toTimestamp(left('@{pipeline().TriggerTime}', 23), 'yyyy-MM-dd\'T\'HH:mm:ss.SSS')` . 

![Veri akışı parametresini ayarlama](media/data-flow/parameter-timestamp.png "Veri akışı parametresini ayarlama")

> [!NOTE]
> Veri akışları yalnızca 3 milisaniyeye kadar basamağı destekleyebilir. `left()`İşlev, ek basamakların kırpılıp kullanılamaz.

#### <a name="pipeline-parameter-example"></a>Ardışık düzen parametresi örneği

`intParam`String türünde bir işlem hattı parametresine başvuran bir tamsayı parametresi olduğunu varsayalım `@pipeline.parameters.pipelineParam` . 

![Veri akışı parametresini ayarlama](media/data-flow/parameter-pipeline-2.png "Veri akışı parametresini ayarlama")

`@pipeline.parameters.pipelineParam`çalışma zamanında bir değeri atandı `abs(1)` .

![Veri akışı parametresini ayarlama](media/data-flow/parameter-pipeline-4.png "Veri akışı parametresini ayarlama")

`$intParam`Türetilmiş sütun gibi bir ifadede başvuruluyorsa, return sonucunu değerlendirir `abs(1)` `1` . 

![Veri akışı parametresini ayarlama](media/data-flow/parameter-pipeline-3.png "Veri akışı parametresini ayarlama")

### <a name="data-flow-expression-parameters"></a>Veri akışı ifade parametreleri

**Veri akışı ifadesini Seç ifadesi** veri akışı ifadesi oluşturucuyu açar. Veri akışınız genelinde işlevlere, diğer parametrelere ve tanımlanmış herhangi bir şema sütununa başvurabileceksiniz. Bu ifade başvuruluyorsa olduğu gibi değerlendirilir.

> [!NOTE]
> Geçersiz bir ifade geçirirseniz veya bu dönüşümde mevcut olmayan bir şema sütununa başvuruda bulunmanız durumunda, parametre null olarak değerlendirilir.


### <a name="passing-in-a-column-name-as-a-parameter"></a>Sütun adını parametre olarak geçirme

Ortak bir model, bir sütun adını parametre değeri olarak geçirmektir. Sütun veri akışı şemasında tanımlanmışsa, doğrudan dize ifadesi olarak başvurabilirsiniz. Sütun şemada tanımlanmamışsa, `byName()` işlevini kullanın. Sütununu, gibi bir atama işleviyle ilgili türe dönüştürmeyi unutmayın `toString()` .

Örneğin, bir parametreye göre bir dize sütununu eşlemek isterseniz `columnName` , öğesinden türetilmiş bir sütun dönüşümü ekleyebilirsiniz `toString(byName($columnName))` .

![Sütun adını parametre olarak geçirme](media/data-flow/parameterize-column-name.png "Sütun adını parametre olarak geçirme")

## <a name="next-steps"></a>Sonraki adımlar
* [Veri akışı yürütme etkinliği](control-flow-execute-data-flow-activity.md)
* [Denetim akışı ifadeleri](control-flow-expression-language-functions.md)
