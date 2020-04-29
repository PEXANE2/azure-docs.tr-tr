---
title: Eşleme veri akışlarını parametreleştirme
description: Data Factory işlem hatlarından eşleme veri akışını Parametreleştirme hakkında bilgi edinin
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 82660cdb4ab6523bae7608fe3b071f20cb3603f8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81419179"
---
# <a name="parameterizing-mapping-data-flows"></a>Eşleme veri akışlarını parametreleştirme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)] 

Azure Data Factory veri akışlarını eşleme, parametrelerin kullanımını destekler. Daha sonra deyimlerinizin tamamında kullanabileceğiniz veri akışı tanımınızın içinde parametreler tanımlayabilirsiniz. Parametre değerleri, veri akışını Yürüt etkinliği aracılığıyla çağıran işlem hattı tarafından ayarlanabilir. Veri akışı etkinlik ifadelerindeki değerleri ayarlamak için üç seçeneğiniz vardır:

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

### <a name="passing-in-a-column-name-as-a-parameter"></a>Sütun adını parametre olarak geçirme

Ortak bir model, bir sütun adını parametre değeri olarak geçirmektir. Parametresiyle ilişkili sütuna başvurmak için `byName()` işlevini kullanın. Sütununu, gibi bir atama işleviyle ilgili türe dönüştürmeyi unutmayın `toString()`.

Örneğin, bir parametreye `columnName`göre bir dize sütununu eşlemek isterseniz, öğesinden türetilmiş bir sütun dönüşümü ekleyebilirsiniz. `toString(byName($columnName))`

![Sütun adını parametre olarak geçirme](media/data-flow/parameterize-column-name.png "Sütun adını paramete olarak geçirme")

## <a name="assign-parameter-values-from-a-pipeline"></a>İşlem hattından parametre değerleri atama

Veri akışınızı parametrelerle oluşturduktan sonra, veri akışını Yürüt etkinliğinin bulunduğu bir işlem hattından çalıştırabilirsiniz. Etkinliği işlem hattı Tuvalinize ekledikten sonra etkinliğin **Parametreler** sekmesinde kullanılabilir veri akışı parametreleri görüntülenir.

![Veri akışı parametresini ayarlama](media/data-flow/parameter-assign.png "Veri akışı parametresini ayarlama")

Parametre veri türü dize ise, parametre değerlerini ayarlamak için metin kutusuna tıkladığınızda bir işlem hattı veya bir veri akışı ifadesi girmeyi seçebilirsiniz. Ardışık düzen ifadesini seçerseniz, işlem hattı ifade paneli sunulur. Dize ilişkilendirme sözdiziminin içinde ardışık düzen işlevlerini dahil ettiğinizden emin olun `'@{<expression>}'`, örneğin:

```'@{pipeline().RunId}'```

Parametreniz dize türünde değilse, her zaman veri akışı Ifade Oluşturucusu görüntülenir. Burada, parametresinin veri türüyle eşleşen herhangi bir ifade veya sabit değer girebilirsiniz. Aşağıda, veri akışı ifadesi ve ifade oluşturucusunun bir sabit dizesi örnekleri verilmiştir:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Her eşleme veri akışı, ardışık düzen ve veri akışı ifade parametrelerinin herhangi bir birleşimini içerebilir. 

![Veri akışı parametreleri örneği](media/data-flow/parameter-example.png "Veri akışı parametreleri örneği")



## <a name="next-steps"></a>Sonraki adımlar
* [Veri akışı yürütme etkinliği](control-flow-execute-data-flow-activity.md)
* [Denetim akışı ifadeleri](control-flow-expression-language-functions.md)
