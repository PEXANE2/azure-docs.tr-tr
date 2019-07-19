---
title: Azure Data Factory eşleme veri akışı şeması DRFT
description: Şema kayması ile Azure Data Factory dayanıklı veri akışları oluşturun
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 562daa024985a546ffb49c4da11eace3bc81a659
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314817"
---
# <a name="mapping-data-flow-schema-drift"></a>Eşleme veri akışı şema DRFT

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Şema kayması kavramı, kaynaklarınızın genellikle meta verileri değiştirme durumdur. Alanlar, sütunlar, türler vs. anında eklenebilir, kaldırılabilir veya değiştirilebilir. Şema kayması için işlem yapılmadan veri akışınız, yukarı akış veri kaynağı değişikliklerinde değişikliklere karşı savunmasız hale gelir. Gelen sütunlar ve alanlar değiştiğinde, bu kaynak adlarına bağlı oldukları için tipik ETL desenleri başarısız olur.

Şema Driine karşı korunmak için bir veri akışı aracında tesislerin, bir veri mühendisi olarak şunları yapmasına olanak tanımak önemlidir:

* Değişebilir alan adlarına, veri türlerine, değerlere ve boyutlara sahip kaynakları tanımlama
* Sabit kodlanmış alanlar ve değerler yerine veri desenleriyle çalışkullanılabilecek dönüştürme parametreleri tanımlayın
* Adlandırılmış alanları kullanmak yerine gelen alanlarla eşleşecek desenleri anlayan ifadeler tanımlayın

## <a name="how-to-implement-schema-drift-in-adf-mapping-data-flows"></a>ADF eşleme veri akışlarında şema DRFT 'i uygulama
ADF, yürütme durumundan yürütmeye değişen esnek şemaları, veri akışlarınızı yeniden derlemenize gerek kalmadan genel veri dönüştürme mantığı oluşturabilmeniz için yerel olarak destekler.

* Kaynak dönüşümünüze "şema Drime Izin ver" i seçin

<img src="media/data-flow/schemadrift001.png" width="400">

* Bu seçeneği belirlediğinizde tüm gelen alanlar, her veri akışı yürütmesinde kaynağınızdan okunacak ve tüm akış havuza geçirilecektir.

* Yeni algılanan tüm sütunlar (düzeltebilecekler sütunları) varsayılan olarak dize veri türü olarak gelir. ADF 'nin otomatik olarak kaynaktaki veri türlerini çıkarması için kaynak dönüşümünüzün "düzeltebilecekler sütun türlerini çıkar" seçeneğini belirleyin.

* Havuz dönüşümünde tüm yeni alanları eşlemek için "otomatik eşleme" kullandığınızdan emin olun. böylece, tüm yeni alanlar, Hedefinizdeki kullanıma alınır ve bu sayede "şemadan sonra da Izin ver" olarak ayarlayın.

<img src="media/data-flow/automap.png" width="400">

* Her şey, bu senaryoya basit bir kaynak > havuz (kopya) eşlemesi ile yeni alanlar tanıtıldığında çalışacaktır.

* Bu iş akışına şema drını işleyen dönüşümler eklemek için, sütunları ada, türe ve değere göre eşleştirmek için model eşleştirmeyi kullanabilirsiniz.

* "Şema drift" ' i anlayan bir dönüşüm oluşturmak istiyorsanız türetilmiş sütunda veya toplama dönüşümünde "sütun düzeni Ekle" seçeneğine tıklayın.

<img src="media/data-flow/columnpattern.png" width="400">

> [!NOTE]
> Akış genelinde şema drını kabul etmek için veri akışınızda bir mimari karar vermeniz gerekir. Bunu yaptığınızda, kaynaklardaki şema değişikliklerine karşı koruma sağlayabilirsiniz. Ancak, veri akışınız boyunca sütunlarınızın ve türlerinizi erken bağlamayı kaybedersiniz. Azure Data Factory, şema DRI akışlarını geç bağlama akışları olarak değerlendirir, bu nedenle, dönüştürmelerinizi oluştururken sütun adları, akış genelindeki şema görünümlerinde sizin için kullanılamaz.

<img src="media/data-flow/taxidrift1.png" width="400">

Taxı Demo örnek veri akışında, TripFare kaynağı ile alt veri akışında örnek bir şema vardır. Toplama dönüşümünde, toplama alanları için "sütun deseninin" tasarımını kullandığımızda dikkat edin. Belirli sütunları adlandırmak veya konuma göre sütunları aramak yerine, verilerin değişeolabileceğini ve çalıştırmalar arasında aynı sırada görünmeyebilir olduğunu varsayıyoruz.

Bu Azure Data Factory veri akışı şeması DRFT işleme örneğinde, veri etki alanının her seyahat için fiyatlar içerdiğini bilmekle, ' Double ' türünde sütunları taradık ve topladık. Daha sonra, sütunun ne konuma ve sütunun adlandırmasına bakılmaksızın, kaynaktaki tüm çift alanlarda toplama matematik hesaplaması gerçekleştirebiliriz.

Azure Data Factory veri akışı sözdizimi, eşleşen kalıpınızdan eşleşen her sütunu temsil etmek için $ $ kullanır. Ayrıca, karmaşık dize arama ve normal ifade işlevlerini kullanarak sütun adları üzerinde de eşleştirebilirsiniz. Bu durumda, ' Double ' sütununun her bir eşleşmesinden sonra yeni bir toplu alan adı oluşturacağız ve metni ```_total``` bu eşleşen adların her birine eklememiz gerekir: 

```concat($$, '_total')```

Ardından, bu eşleşen sütunların her biri için değerleri yuvarlayacağız:

```round(sum ($$))```

Bunu, "Taxi demo" Azure Data Factory veri akışı örneğiyle test edebilirsiniz. Sonuçları etkileşimli olarak görebilmeniz için veri akışı tasarım yüzeyinin en üstündeki hata ayıklama oturumunda geçiş yapın:

<img src="media/data-flow/taxidrift2.png" width="800">

## <a name="access-new-columns-downstream"></a>Yeni sütunlara aşağı akış erişimi
Sütun desenleriyle yeni sütunlar oluşturduğunuzda, bu yeni sütunlara aşağıdaki yöntemlerle veri akışı dönüşümlerinizin ilerleyen kısımlarında erişebilirsiniz:

* Yeni sütunları konum numarasına göre tanımlamak için "byPosition" kullanın.
* Yeni sütunları adına göre tanımlamak için "byName" kullanın.
* Sütun desenleri ' nde, "ad", "akış", "konum" veya "tür" ya da bunların herhangi bir birleşimini yeni sütunlarla eşleşecek şekilde kullanın.

## <a name="next-steps"></a>Sonraki adımlar
[Veri akışı Ifade dilinde](data-flow-expression-functions.md) , sütun desenleri ve "byName" ve "byposition" gibi şema kayması için ek tesisler bulacaksınız.
