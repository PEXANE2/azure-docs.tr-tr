---
title: Eşleme veri akışlarını parametreleştirme
description: Veri fabrikası ardışık kaynaklarından haritalama veri akışını nasıl parametreize edin
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 82660cdb4ab6523bae7608fe3b071f20cb3603f8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419179"
---
# <a name="parameterizing-mapping-data-flows"></a>Eşleme veri akışlarını parametreleştirme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)] 

Azure Veri Fabrikası'ndaki veri akışlarını eşleme parametrelerinkullanımını destekler. Veri akışı tanımınızın içindeki parametreleri tanımlayabilirsiniz ve bu parametreleri ifadeleriniz boyunca kullanabilirsiniz. Parametre değerleri, Veri Akışını Yürüt etkinliği aracılığıyla çağrı ardışık etki alanı tarafından ayarlanabilir. Veri akışı etkinlik ifadelerindeki değerleri ayarlamak için üç seçeneğiniz vardır:

* Dinamik bir değer ayarlamak için boru hattı denetimi akış ifadesi dilini kullanma
* Dinamik bir değer ayarlamak için veri akışı ifade dilini kullanma
* Statik bir gerçek değer ayarlamak için her iki ifade dilini de kullanma

Veri akışlarınızın genel amaçlı, esnek ve yeniden kullanılabilir olmasını sağlamak için bu özelliği kullanın. Veri akışı ayarlarını ve ifadelerini bu parametrelerle parametreleştirebilirsiniz.

## <a name="create-parameters-in-a-mapping-data-flow"></a>Haritalama veri akışında parametreler oluşturma

Veri akışınıza parametreler eklemek için, genel özellikleri görmek için veri akışı tuvalinin boş bölümünü tıklatın. Ayarlar bölmesinde **Parametre**adlı bir sekme görürsünüz. Yeni bir parametre oluşturmak için **Yeni'yi** seçin. Her parametre için bir ad atamanız, bir tür seçmeniz ve isteğe bağlı olarak varsayılan bir değer ayarlamanız gerekir.

![Veri Akışı parametreleri oluşturma](media/data-flow/create-params.png "Veri Akışı parametreleri oluşturma")

## <a name="use-parameters-in-a-mapping-data-flow"></a>Haritalama veri akışında parametreleri kullanma 

Parametreler herhangi bir veri akışı ifadesinde başvurulabilir. Parametreler $ ile başlar ve değişmezdir. Kullanılabilir parametrelerin **listesini, Parametreler** sekmesi altında İfade Oluşturucusu'nun içinde bulabilirsiniz.

![Veri akışı parametre ifadesi](media/data-flow/parameter-expression.png "Veri akışı parametre ifadesi")

**Yeni parametre** seçerek ve adı ve türünü belirterek hızlı bir şekilde ek parametreler ekleyebilirsiniz.

![Veri akışı parametre ifadesi](media/data-flow/new-parameter-expression.png "Veri akışı parametre ifadesi")

### <a name="passing-in-a-column-name-as-a-parameter"></a>Bir sütun adı parametre olarak geçme

Ortak bir desen parametre değeri olarak bir sütun adı geçmektir. Parametreyle ilişkili sütuna başvurmak için `byName()` işlevi kullanın. Gibi bir döküm fonksiyonu ile uygun türüne `toString()`sütun döküm unutmayın.

Örneğin, bir dize sütununa göre bir `columnName`dize eşlemi yapmak isterseniz, türetilmiş sütun dönüşüme `toString(byName($columnName))`eşit olarak ekleyebilirsiniz.

![Bir sütun adı parametre olarak geçme](media/data-flow/parameterize-column-name.png "Sütun adını paramete olarak geçirme")

## <a name="assign-parameter-values-from-a-pipeline"></a>Bir ardışık kaynaktan parametre değerleri atama

Parametrelerle veri akışınızı oluşturduktan sonra, veri akışını yürüt veri akışı etkinliği ile bir ardışık kaynaktan yürütebilirsiniz. Etkinliği boru hattı tuvalinize ekledikten sonra, etkinliğin **Parametreler** sekmesinde kullanılabilir veri akışı parametreleri sunulur.

![Veri Akışı parametresi ayarlama](media/data-flow/parameter-assign.png "Veri Akışı parametresi ayarlama")

Parametre veri türünüz dizeise, parametre değerlerini ayarlamak için metin kutusunu tıklattığınızda, bir ardışık lık veya veri akışı ifadesi girmeyi seçebilirsiniz. Ardışık hatlar ifadesini seçerseniz, ardışık hatlar ifadesi paneli ile sunulur. Örneğin, string enterpolasyon sözdizimi `'@{<expression>}'`içinde ardışık işlevler içerdiğinden emin olun:

```'@{pipeline().RunId}'```

Parametreniz tür dizesinden değilse, her zaman Veri Akışı İfade Oluşturucusu ile birlikte sunulur. Burada, parametrenin veri türüyle eşleşen istediğiniz herhangi bir ifadeyi veya gerçek değerleri girebilirsiniz. Aşağıda veri akışı ifade örnekleri ve ifade oluşturucu bir edebi dize vardır:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Her haritalama veri akışı, ardışık hatlar ve veri akışı ifade parametrelerinin herhangi bir birleşimine sahip olabilir. 

![Veri akışı parametreleri örneği](media/data-flow/parameter-example.png "Veri akışı parametreleri örneği")



## <a name="next-steps"></a>Sonraki adımlar
* [Veri akışı etkinliğini yürütme](control-flow-execute-data-flow-activity.md)
* [Kontrol akışı ifadeleri](control-flow-expression-language-functions.md)
